# PolyU Course Site → 腾讯文档同步

> **范围：仅 PolyU 理大课程。** 求职 / 瀚泊 / 个人作品等场景不要用本文。

配套 `polyu-course-site`。用户说「同步腾讯文档 / 更新腾讯文档 / 上传课程网站」时执行。

## 对照表

每个理大课程根目录应有 `腾讯文档同步.md`（**不要上传到腾讯文档**）：

| 字段 | 含义 |
|---|---|
| 本地根目录 | Desktop 上的课程文件夹 |
| 云端 `folder_id` | 课程总文件夹 + `Basic` / `Assignment-*` / `LectureNotes` 等 |
| 已同步文件表 | 本地相对路径 · MD5 · `file_id` · 分享链接 |

参考：

- `path/to/CBSxxxx/腾讯文档同步.md`（每个课程根目录一份）
- 跨课总览（如成绩总览）可放在首页文件夹 **PolyU-Overall**（`folder_id=<folder_id>`）；本地对照表可写在 `path/to/腾讯文档同步-PolyU-Overall.md`

## 何时更新

扫描本地课程文件（跳过：`腾讯文档同步.md`、`.DS_Store`、`Icon\r`、`_` 开头预览目录）。

| 情况 | 动作 |
|---|---|
| 新文件 | 导入 → move 到同名子文件夹 → `set_privilege(2)` → 写入对照表 |
| MD5 变了 | 重新导入 → move → 删旧 `file_id` → `set_privilege(2)` → 更新对照表与链接 |
| MD5 未变 | 跳过 |
| 本地已删、云端还在 | **默认不删云端**；回复里列出，等用户确认 |

> 腾讯文档**无法原地覆盖** HTML page：每次更新都会产生**新** `file_id` / URL；旧版进回收站。

## 导入链路（HTML → `/page/`）

工具：`mcporter call tencent-docs …`（需外网，依赖独立的 tencent-docs skill）。辅助脚本：`import_file.sh`（由上述 tencent-docs skill 提供）。

```
1. bash import_file.sh <local.html>
   → pre_import + PUT COS → FILE_KEY / TASK_ID / MD5 / SIZE
2. manage.async_import
3. 轮询 manage.import_progress（约 3s 一次）至 progress=100 且有 file_id / file_url
4. manage.move_file → target_folder_id = 对照表子文件夹
5. manage.set_privilege → policy=2（所有人可查看）
6. manage.delete_file → 旧 file_id（仅更新场景）
7. 改对照表：MD5、file_id、链接、上次同步日期
```

## 权限与链接

| 要做 | 不要做 |
|---|---|
| `policy=2` 所有人可查看 | 承诺自动「发布网站」 |
| 交付 `https://docs.qq.com/page/…` | 当作已发布的 `*.page.qq.com` 正式发布链接 |
| 回复格式见下 | 把理大流程套到非理大文件夹 |

```
【腾讯文档】{文档标题}
{https://docs.qq.com/page/...}
```

说明：MCP **没有**「发布」接口。`{id}.page.qq.com` 与 `/page/` 同源形态，但「所有人可查看」≠ UI 正式发布态。用户若必须要发布链接，由其在网页端点发布，或等官方 API。

## 新建课程云端脚手架

若课程尚无腾讯文档文件夹：

1. `manage.search_file` / `manage.folder_list` 确认是否已有。
2. 在合适父级（如 PolyU-Overall）`manage.create_file`（`file_type=folder`，标题 ≤36 字）。
3. 再建 `Basic` / `Assignment-Group` / `Assignment-Individual` / `LectureNotes` / `ReadingList`。
4. 写好本地 `腾讯文档同步.md` 再导入文件。

## 完成后回复

- 新增 / 更新 / 跳过的文件名
- 每条分享链接（已确认 `get_privilege` → `policy=2` 更稳妥）
- 若 URL 相对上次变更：一句说明「腾讯文档无法覆盖，链接已更新」
