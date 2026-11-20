---
name: polyu-course-site
description: >-
  Builds PolyU course single-file bilingual HTML sites (EN + 港式繁體) from syllabus /
  teaching-schedule / assignment guidelines / Week-1 lecture PDFs—structure and
  emphasis only, no content invention. Also syncs those HTML pages to Tencent Docs
  for PolyU courses only. Use when the user mentions polyu-course-site, course site,
  课程站, syllabus/assignment HTML, CBS course webpage, or 同步腾讯文档 for a PolyU
  course folder.
---

# PolyU Course Site

把理大课程资料做成**单文件 HTML**课程站（可多页：大纲 / 小组作业 / 个人作业）。

> **不改内容本身；只通过结构、层级与强调，让信息更简洁明了。**

| 参考实现 | 路径 |
|---|---|
| 教学日程（经典） | `path/to/CBSxxxx_Teaching_Schedule_2026-27.html` |
| 大纲 + 作业页 | `path/to/CBSxxxx-syllabus.html` 与 `path/to/CBSxxxx-assignment.html` |
| 成绩总览（扁平课表明细） | `path/to/_grades.html`；云端 PolyU-Overall |

设计令牌：[references/design-system.md](references/design-system.md)  
腾讯文档（仅理大）：[references/tencent-docs-sync.md](references/tencent-docs-sync.md)

---

## When to use

- 教学日程 / syllabus / 作业指南 → HTML
- Apple 风 + 理大校色 + EN｜繁
- `polyu-course-site` / 课程站 / course site
- 理大课程文件夹内「同步 / 更新腾讯文档」（见文末；**非理大场景勿用**）

---

## Hard rules（内容保真）

1. **禁止增删改实质信息**：时间、地点、讲师、邮箱、书目、周次、题目、阅读、作业说明、评核比重、守则、链接——英文侧保持原文。
2. **允许的事**：分节、表格/卡片、spec rows、字重/颜色强调、导航锚点、响应式、双语 UI、来源小注（如 “From Week 1 Introduction”）。
3. **中文**：港式繁體；专名 / 引用 / 课号 / 电邮 / URL / Essay 1 等可保留英文。
4. **英文**：UK spelling 仅用于**自拟 UI 标签**；源正文不要「纠正」拼写。
5. **不要**擅自加「本周」高亮、假数据、装饰统计、额外政策；**不要**在源文件没有周课表时自编周课表。
6. **多源冲突**（如正式大纲 20% vs 讲义 30%）：两处都保留原文 + 标明来源；**禁止**自行统一或「纠正」。
7. 交付：自包含 `.html`（CSS/JS 内嵌）；回复含 `MEDIA:/absolute/path.html`。

---

## Sources（读什么）

按优先级取证，用户点名的文件优先：

| 类型 | 常见位置 | 用途 |
|---|---|---|
| 正式大纲 | `Basic/*syllabus*.docx` / PDF | 科目字段、ILO、正式评核、书目 |
| 教学日程 | `Basic/*Schedule*.pdf` | 周次、放假、作业发布/截止 |
| 作业指南 | `Assignment-Group/`、`Assignment-Individual/` | 要求、rubric、比重 |
| Week 1 讲义 | `LectureNotes/*W1*.pdf` | Course structure、How you're graded、purpose/expectations（用户要求纳入时） |

文件夹脚手架（理大课程惯例）：`Basic` / `Assignment-Group` / `Assignment-Individual` / `LectureNotes` / `ReadingList`。

---

## Workflow

```
Task Progress:
- [ ] 1. 读源文件（pdftotext / docx 文本 / Read），按原章节列出全部信息
- [ ] 2. 标出「作业关键日期」与「放假/暂停」行（仅强调，不改字）
- [ ] 3. 选定页型（大纲 / 日程 / 作业）并写单文件 HTML
- [ ] 4. 双语：.en / .zh（或 ul.en / ul.zh）+ 顶部 EN|繁；localStorage 键用课号+页名
- [ ] 5. 桌面 + 窄屏各扫一眼；仅修挤的区块
- [ ] 6. 回用户：路径 + 强调了什么 + 未改什么原文
- [ ] 7. （可选）用户要求同步腾讯文档 → 见 references/tencent-docs-sync.md
```

后续小改：**外科式编辑**现有 HTML，禁止整文件重写。

---

## Page types

### A. Teaching schedule（经典）

骨架：Overview → Schedule → Assessments → Conduct / Integrity / SEN / Resources  
Kickers 与源 PDF 章节编号一致。

### B. Syllabus（正式大纲 ± Week 1 补充）

建议阅读顺序（比机械夹在「1 与 2 之间」更顺）：

1. Overview（科目编号 / 学分等 spec rows）
2. 若纳入 W1：Purpose → Expectations → Course Overview → **Course Structure** → How You're Graded（讲义快照）
3. Objectives → ILOs → Subject Synopsis → Methodology
4. Assessments（**正式大纲**评核；可与 W1 快照并存）
5. GenAI / Effort / Reading

W1 区块加 `source-note`（From Week 1 Introduction）。作业截止行用 `.mark`。

### C. Assignment（小组 / 个人）

- Hero **顶部**标明类型：`CBS5406 · Group Assignment` / `個人作業`（中英都要一眼可读）
- Overview spec rows：比重、模式、人数、**Key date(s)**（有则标红）
- 提案结构 / Options / GenAI / Rubric 表（`.rubric-wrap` + `.rubric`）
- 关键页关键日期应与大纲课表 / W1 一致；只加有出处的日期

---

## Structure without changing content

| 源内容 | 手法 |
|---|---|
| 上课时间 / 讲师 / 教科书 / 作业元数据 | Spec rows |
| 周课表 | **扁平**桌面表 + 扁平手机列表（同一文案）；**禁止**圆角框包裹整表 |
| 放假 / suspended | `.holiday` 整行变灰 |
| due / released / presentation / Submit… | **日期** `.mark`；作业说明 `.mark`；展示周主题可用 `.topic.mark`（对齐扁平课表模式） |
| 评核比重 | 标题 \| 大号 % \| 说明 + 细条 |
| 书目 / URL | 列表或 resource 行；引用不译 |
| 整段列表双语 | `ul.en` + `ul.zh`（整表切换）亦可 |

### 强调纪律

- **扁平课表**：白底横线表。灰底上不要套 `.panel` 圆角白框；Structure / Schedule 用 `section.alt` 或去掉 panel。
- **红（扁平课表模式）**：只标 **日期** + **作业/展示节点**（released / due / Presentation / Submit…）。主题若本身就是展示周，可用 `.topic.mark`；普通理论／阅读文献保持中性色。
- **灰**：假期、停课、非教学周。
- **不要**把整行 Key Theories 全文刷红；**不要**把普通阅读文献标红。
- 默认**不要**「本周」自动高亮。

### 成绩总览 / 课程明细（MABCC 等 transcript 页）

参考：`path/to/_grades.html`（满意版）。

- **默认保持扁平四列表**：代码｜课程名称｜学分｜等级；学期用组标题行 + GPA，**不要**擅自改成彩色等级徽章靠左、灰底学期卡片、两行 meta 等「新视觉体系」。
- **列宽用固定比例**（`table-layout: fixed`），推荐约 **20% / 40% / 20% / 20%**，避免课名列吞满整行、学分等级被甩到最右侧留大片空白。
- **Overview / 目前進度**：用 **spec rows**（标签｜大数字 + 说明）+ 单独进度条；**不要**套 Assessments 的三列 weight 布局（会把 `25 / 31`、Credit 挤扁）。
- 用户说「不满意 / 回到之前」时：**外科式回到上一满意结构**，禁止再发明第三套交互语言。

### 顶栏导航（精简）

大纲 / 课程站页顶栏**只保留主任务锚点**，避免一排十几个链接：

1. **Course Structure**（或 Schedule）
2. **Graded**（或 Assessments）
3. **Reading**

品牌课号仍可点回 Hero。其余章节（Overview、ILOs、Methodology…）留在正文，靠滚动到达即可——**不要**全部塞进 sticky nav。

---

## Visual language

- Apple 资讯站留白 + 系统字体；**PolyU 砖红只用于 Hero**（`#E24B5A` → `#A6192E` → `#2A050C`）。
- 底 `#f5f5f7`；正文 `#1d1d1f`；链接 `#0066cc`；强调 `#ff3b30`。
- 字体：`-apple-system` / SF Pro + `PingFang HK` / `PingFang TC`。
- **禁止**：紫渐变、SaaS 三卡片、无意义玻璃堆、emoji。

细节见 design-system.md。

---

## Bilingual（EN + 繁）

```html
<span class="en">…</span><span class="zh">…</span>
<!-- 或 -->
<ul class="en">…</ul><ul class="zh">…</ul>
```

```css
html[data-lang="en"] .zh { display: none !important; }
html[data-lang="zh"] .en { display: none !important; }
```

```js
localStorage.setItem("cbs5406-syllabus-lang", next); // 课号+页名，避免多课冲突
```

导航右上角 **EN｜繁** segmented，勿下拉。

---

## Responsive（外科式）

- Schedule：`≤834px` 藏表、显示卡片。
- Assessments：窄屏「标题 + %」同行，说明与进度条整行在下。
- Spec / Resources：可单列。触控 ≥ 44px。

---

## Output checklist

- 作业节点：日期 + 作业说明已标红（理论列保持中性，除非主题本身即展示周）
- 课表为扁平横线表（无圆角框）；放假已变灰；未自编无出处课表
- 顶栏仅 Structure / Graded / Reading（或同等主锚点），未堆满次要章节
- 多源比重冲突已并存并标明来源（若适用）
- 作业页 Hero 已标明小组 / 个人
- EN｜繁可切换且 localStorage 记得住
- 单文件可双击打开；回复含 `MEDIA:` 绝对路径

---

## 腾讯文档（仅 PolyU 课程）

**交付与同步原则：本地优先，同步由你决定。** 默认产物是自包含 `.html`，本地交付即可。只有当你明确要把课程页放到线上时，才走下面的腾讯文档流程——是否同步、目标课程文件夹、权限（如"所有人可查看"）都由你指定；HTML 产物也可自行部署到 Vercel 等静态托管。下列腾讯文档步骤仅在目标是理大课程文件夹时启用：

> 各平台细节与取舍见 [references/delivery-sync.md](references/delivery-sync.md)。

用户说「同步腾讯文档 / 更新腾讯文档 / 上传课程网站」且目标是**理大课程文件夹**时：

1. 读该课程根目录 `腾讯文档同步.md`（若无则按现有云端文件夹结构创建对照表）。
2. 严格按 [references/tencent-docs-sync.md](references/tencent-docs-sync.md) 执行。
3. 默认：所有人可查看（`policy=2`）+ `docs.qq.com/page/` **分享链接**。
4. **不要**承诺自动「发布链接」/ `*.page.qq.com`（MCP 无发布接口）。
5. **不要**把此流程套到求职、瀚泊、个人作品等非理大场景。
