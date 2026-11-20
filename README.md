# PolyU Course Site

Build **single-file bilingual (EN + 港式繁體) HTML course sites** from a syllabus, teaching schedule, assignment guidelines, and Week-1 lecture PDFs — structure and emphasis only, **no content invention**.

![Format](https://img.shields.io/badge/Format-Agent%20Skill-2F2FE4?style=flat-square)
![Output](https://img.shields.io/badge/Output-Single--file%20HTML-2F2FE4?style=flat-square)
![Bilingual](https://img.shields.io/badge/Bilingual-EN%20%7C%20繁體-2F2FE4?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-2F2FE4?style=flat-square)

Released under the [MIT License](LICENSE).

> Built for PolyU (理大) courses, but the design system works for any single-file bilingual course site. The Tencent Docs sync flow is PolyU-specific.

## What it does

- Turns a course folder (syllabus / teaching schedule / assignment briefs) into a clean **single-file HTML site** — Apple-style layout with the PolyU palette, an `EN | 繁` toggle, and a flat, readable schedule.
- **Fidelity-first**: it never adds, drops, or rewrites substantive info (dates, weights, policies). When two sources conflict, both are shown with their sources — it never "reconciles" them for you.
- Optional **Tencent Docs sync** for PolyU course folders (preserves a shareable online copy), and a **grades-overview** page pattern.

## Requirements

- None at runtime — the output is a self-contained HTML file. The skill itself is an [Agent Skill](https://agents.md).
- The Tencent Docs sync path depends on the separate `tencent-docs` skill and `mcporter`; Feishu upload depends on the `lark-cli` skill.

## Install

```bash
git clone https://github.com/IcarusYe12138/polyu-course-site.git
# then symlink / copy into your agent's skill folder, e.g. ~/.agents/skills/polyu-course-site
```

## Usage

Point the skill at a course folder and say which pages you want (teaching schedule, syllabus, group/individual assignment, grades overview). It applies the design tokens in [`references/design-system.md`](references/design-system.md) and follows the hard fidelity rules in [`SKILL.md`](SKILL.md).

## Delivery & sync — local-first, you decide

- **Default = local.** The course site is a self-contained `.html` — hand over the file path.
- **Sync is your call.** Only when you explicitly want it online do you sync — and you pick the platform and permission. For a PolyU course folder, use the skill's [`references/tencent-docs-sync.md`](references/tencent-docs-sync.md) flow (Tencent Docs). Feishu (Lark) and Notion work too via your own tools.
- **You can deploy the HTML to Vercel** (or any static host) for a public URL — drag the single file onto Vercel, or use the Vercel CLI.

Details: [`references/delivery-sync.md`](references/delivery-sync.md).

## Repository layout

```
SKILL.md                          # fidelity rules, page patterns, output checklist
references/design-system.md       # CSS tokens + component conventions
references/tencent-docs-sync.md   # PolyU-specific Tencent Docs sync flow
references/delivery-sync.md       # local-first delivery & sync policy
```

## Credits

Layout and design tokens are original. "PolyU" and the PolyU red are referenced for color-matching a school workflow; this project is unofficial and not endorsed by The Hong Kong Polytechnic University.
