# PolyU Course Site — Design System

配套 `polyu-course-site`。实现时优先复制这些令牌与组件约定。

## CSS variables

```css
:root {
  --black: #000000;
  --ink: #1d1d1f;
  --ink-80: rgba(0, 0, 0, 0.8);
  --ink-48: rgba(0, 0, 0, 0.48);
  --gray: #f5f5f7;
  --white: #ffffff;
  --hairline: rgba(0, 0, 0, 0.08);
  --blue: #0071e3;
  --red: #ff3b30;          /* critical assignment emphasis — not PolyU brand */
  --link: #0066cc;
  --polyu-red: #A6192E;
  --polyu-red-bright: #E24B5A;
  --polyu-red-deep: #2A050C;
  --display: -apple-system, BlinkMacSystemFont, "SF Pro Display", "Helvetica Neue", Helvetica, Arial, sans-serif;
  --text: -apple-system, BlinkMacSystemFont, "SF Pro Text", "PingFang HK", "PingFang TC", "Hiragino Sans CNS", "Helvetica Neue", Helvetica, Arial, sans-serif;
  --max: 980px;
  --max-wide: 1080px;
  --nav-h: 48px;
}
```

## Hero

```css
.hero {
  background:
    radial-gradient(120% 90% at 8% 0%, rgba(255, 210, 214, 0.38) 0%, rgba(255, 210, 214, 0) 46%),
    linear-gradient(128deg, var(--polyu-red-bright) 0%, var(--polyu-red) 42%, var(--polyu-red-deep) 100%);
  color: #fff;
}
```

- 上方小字：课号；**作业页**写成 `CBS5406 · Group Assignment` / `個人作業`（中英），一眼区分页型。
- 下方大标题：课程名或作业名。
- 可选 `.sub` 一行说明（模式 · 课程全名）。
- 白字；不要在 Hero 里塞课表或评核摘要。

## Navigation

- Sticky；`rgba(0,0,0,.8)` + `backdrop-filter: saturate(180%) blur(20px)`。
- 左：课号；中：页内锚点；右：`EN | 繁` segmented。
- 链接触控高度跟导航条对齐（约 48px）。

## Spec rows

```css
.spec-row {
  display: grid;
  grid-template-columns: 220px 1fr;
  gap: 24px;
  padding: 22px 0;
  border-bottom: 1px solid var(--hairline);
}
```

窄屏改为单列。`dt` 用次要色；`dd` 放原文。

## Schedule

**扁平课表（硬规则）**：Course Structure / Schedule 必须是**白底 + 横线分隔**的扁平表，**禁止**圆角白框卡片感。

- 区块用 `section.alt`（白底），或去掉灰底上的 `.panel` 包裹。
- **不要**用 `border-radius` + 白底 `.panel` 把整张表包成圆角卡片（灰底上会特别明显）。
- 桌面：直接 `<div class="desk"><table class="sched">`；行间仅 `border-top: 1px solid var(--hairline)`。
- 窄屏卡片：同样扁平——`border-radius: 0`、透明底、`border-top` 分隔；不要圆角白块堆叠。

### Emphasis

```css
.mark { color: var(--red); font-weight: 600; }
.sched tr.holiday td,
.sched tr.holiday .num,
.sched tr.holiday .topic {
  color: var(--ink-48);
  font-weight: 400;
}
```

- 作业/展示：给**日期单元格**和**作业说明**加 `.mark`；展示周主题可用 `.topic.mark`（CBS503 模式）。
- **不要**把普通 Key Theories / 阅读文献整格刷红。
- 假期行：`<tr class="holiday">`；手机对应 `.card.holiday`。
- 顶栏主锚点优先：Structure / Graded / Reading（勿堆满次要章节）。

### Dual view

- Desktop：`.desk` 内表格；`.cards { display: none }`。
- `max-width: 834px`：藏表、显示卡片。卡片文案必须与表单元格一致；样式对齐扁平表，勿用圆角框。

## Assessments

Desktop：

```css
.assess-item {
  display: grid;
  grid-template-columns: minmax(140px, 1.1fr) 88px 2fr;
  gap: 24px;
  align-items: start;
}
```

Mobile（推荐，避免三行傻堆）：

```css
@media (max-width: 834px) {
  .assess-item {
    grid-template-columns: 1fr auto;
    gap: 6px 16px;
    align-items: baseline;
  }
  .assess-item > div:not(.weight):not(.bar) { grid-column: 1 / -1; }
  .assess-item .bar { grid-column: 1 / -1; }
}
```

底部细条 `width: N%` 对应评核比重，仅作视觉权重，不替代文字中的百分比。

## Bilingual markup

```html
<span class="en">…exact English…</span>
<span class="zh">…港式繁體…</span>
```

```css
html[data-lang="en"] .zh { display: none !important; }
html[data-lang="zh"] .en { display: none !important; }
```

语言键：`localStorage`；课号+页名避免多课冲突，例如 `cbs503-schedule-lang`、`cbs5406-syllabus-lang`。

整表切换（ILO / 选题列表等）：

```html
<ul class="en">…</ul>
<ul class="zh">…</ul>
```

## Source note / Rubric

```css
.source-note {
  margin: -12px 0 24px;
  font-size: 13px;
  color: var(--ink-48);
}
.rubric-wrap { overflow-x: auto; -webkit-overflow-scrolling: touch; }
.rubric {
  width: 100%;
  min-width: 720px;
  border-collapse: collapse;
  background: var(--white);
}
```

- W1 讲义并入大纲时，每个补充区块顶部加 `source-note`。
- 作业评分表：Criterion | Weight | Excellent | Good | Satisfactory | Needs Improvement。

## Type scale（参考）

| 角色 | 大约 |
|---|---|
| Hero h1 | `clamp(34px, 6vw, 56px)` weight 600 |
| Section h2 | `clamp(28px, 4vw, 40px)` weight 600 |
| Body | 17px / 1.47 |
| Table / caption | 12–14px |
| Nav | 12px |

## Anti-patterns

- 改写或「润色」源文件政策句导致意思偏移
- 把所有日期标红
- 源文件没有周课表却自编一周安排
- 大纲与讲义比重冲突时擅自「统一」成一套数字
- Hero 用黑底却无理大识别（本 skill 默认校色渐变）
- 作业页 Hero 不标明小组 / 个人
- 为移动端复制第三套文案（易与桌面漂移）
- Inter / 紫渐变 / 无意义图标行
- 成绩明细表改成徽章靠左 / 学期圆角卡片等新体系（用户明确要扁平课表）
- 成绩表 `width:100%` 却让课名列自适应撑满，造成课名与学分之间大片空白；应固定列宽比例（约 20/40/20/20）
- Overview KPI 误用 Assessments 的 `assess-item` 三列 weight 布局
