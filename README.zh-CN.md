# PolyU Course Site（理大课程站）

把 syllabus、教学日程、作业指南、Week-1 讲义 PDF 做成**单文件双语（EN + 港式繁體）HTML 课程站**——只做结构与强调，**不改内容本身**。

![Format](https://img.shields.io/badge/Format-Agent%20Skill-2F2FE4?style=flat-square)
![Output](https://img.shields.io/badge/Output-Single--file%20HTML-2F2FE4?style=flat-square)
![Bilingual](https://img.shields.io/badge/Bilingual-EN%20%7C%20繁體-2F2FE4?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-2F2FE4?style=flat-square)

基于 [MIT License](LICENSE) 发布。

> 为 PolyU（理大）课程而做，但设计系统适用于任何单文件双语课程站；腾讯文档同步流程是理大专属。

## 功能

- 把课程文件夹（syllabus / 教学日程 / 作业说明）做成干净的**单文件 HTML 站**——Apple 风 + 理大校色，`EN | 繁` 切换，扁平可读的课程表。
- **内容保真优先**：绝不增删改实质信息（日期、比重、政策）。多源冲突时两处都保留并标明来源，绝不擅自"统一"。
- 可选**腾讯文档同步**（理大课程文件夹，保留一份可分享的线上副本），以及**成绩总览**页面模式。

## 环境要求

- 运行期零依赖——产物是自包含 HTML。skill 本体是 [Agent Skill](https://agents.md)。
- 腾讯文档同步依赖独立的 `tencent-docs` skill 与 `mcporter`；飞书上传依赖 `lark-cli` skill。

## 安装

```bash
git clone https://github.com/IcarusYe12138/polyu-course-site.git
# 然后 symlink / 拷贝进你的 skill 目录，如 ~/.agents/skills/polyu-course-site
```

## 用法

指向课程文件夹并说要哪几页（教学日程、大纲、小组/个人作业、成绩总览）。skill 套用 [`references/design-system.md`](references/design-system.md) 的设计令牌，并遵守 [`SKILL.md`](SKILL.md) 的内容保真硬规则。

## 交付与同步 —— 本地优先，由你决定

- **默认本地**：课程站是自包含 `.html`，直接给文件路径。
- **同步由你决定**：只有当你明确要放到线上时才同步——平台与权限都由你指定。理大课程文件夹用本 skill 的 [`references/tencent-docs-sync.md`](references/tencent-docs-sync.md) 流程（腾讯文档）；飞书（Lark）、Notion 也可用你自己的工具。
- **HTML 可自行部署到 Vercel**（或任何静态托管）拿公开 URL——把单文件拖到 Vercel，或用 Vercel CLI。

细节见 [`references/delivery-sync.md`](references/delivery-sync.md)。

## 仓库结构

```
SKILL.md                          # 保真硬规则、页面模式、输出清单
references/design-system.md       # CSS 令牌 + 组件约定
references/tencent-docs-sync.md   # 理大专属腾讯文档同步流程
references/delivery-sync.md       # 本地优先的交付与同步策略
```

## 致谢

布局与设计令牌均为原创。"PolyU" 及理大红的引用仅为校色匹配校园工作流；本项目为非官方作品，与香港理工大学无关。
