# Delivery & Sync — local-first, you decide where it goes

This note backs the delivery-sync principle stated in `SKILL.md` (before the Tencent Docs section).

## Principle: local-first, sync is opt-in

- **Default = local.** A course site is delivered as a self-contained `.html` (CSS/JS inline). Hand over the file path and it opens in any browser.
- **Sync only on request.** Only when the user explicitly wants the course page online do you sync — and even then, the user picks the platform and the permission.
- **If unspecified, keep it local.** Never upload a course page as a side effect of building it.

## Where you *can* put it (all optional)

| Target | When it fits | Notes |
|---|---|---|
| Tencent Docs | A **PolyU course folder** (institutional habit) | Use the skill's own `references/tencent-docs-sync.md` flow. Set `policy=2` (anyone can view) only on request. |
| Feishu / Lark Drive | Share with a Lark workspace | Upload the single HTML; overwrite in place with your Lark tool (e.g. `lark-cli`) to preserve the URL. |
| Notion | Embed in a course-notes page | Attach/embed the file; Notion won't render raw HTML inline. |
| Vercel | You want a public URL for the course site | Drop the single HTML file onto Vercel (New Project → upload a static file) or use the Vercel CLI. You run this step yourself. |

## Why local-first

- Source materials (syllabus, assignment briefs) are often internal — publish only what you intend to be public.
- A single self-contained HTML is portable; keeping it local first avoids leaking unreleased course info.
