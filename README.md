# LCM Theme — 轻质混凝土 / SIP Formwork 静态站点

静态化后的 LCM 企业站点主题（原 WordPress 站点导出），纯 HTML + Tailwind（CDN），无构建步骤。
Deployed on Cloudflare Pages (project `lcm`).

---

## 修改内容（2026-10-18）

### 1. 图片去水印
- 原图右下角带有灰色「LCM」水印（水印已烧录在 JPEG 像素里，无法用 CSS 去除）。
- 通过**裁切**去除水印：大面积图裁掉右侧 2% + 底部 20%，小缩略图裁掉右侧 29% + 底部 43%（以 600px 宽度为界）。
- 共处理 **80 张被引用的图片**，全部使用 `object-cover` 展示，版式未受影响。
- 重新生成 `wp-content/uploads/2020/01/cast_house-24-1024x768.jpg`（母图 1568×960 → 派生图 1024×627）。

### 2. 图片去重（全站唯一）
- 重建各页面的「Technical Process Gallery」，每页 3–5 张**主题匹配且互不重复**的照片（首页 5 张）。
- 追加 8 张新素材：`benefit-8.jpg`、`application-11/12/13.jpg`、`mixer-13/14/15/16.jpg`。
- 首页首屏示意图 `application-1.jpg` 替换为实拍图 `cast_wall-5.jpg`。
- 结果：**11 个页面共 80 个图片文件，页面内 0 重复、跨页面 0 重复**。

### 3. 移除灰色叠加层
- 删除覆盖在每张 gallery 缩略图上的灰色描边 `<span class="... ring-inset ring-black/10">`，全站剩余 0 处。

### 4. 路径修复
- 修正首页 `mixer-11.jpg` 的路径为 `/wp-content/uploads/2019/12/mixer-11.jpg`。
- 全站 **125 处**图片引用（`src` / `srcset` / `imagesrcset` / lightbox `href` / og:image）统一为根路径，且大小写与实际文件名完全匹配。

### 5. 首屏 srcset
- 首屏大图改为 `cast_house-24`，`srcset` 描述符由过期的 `1600w/1536w` 改为实际的 **1024w / 1568w**。
- `<img>` 的 `width="1568" height="960"` 与 preload 描述符对齐，消除 CLS。

### 6. 视频
- 审计全部 YouTube 嵌入：**14 个视频 ID 全部有效**（通过 oEmbed 校验），每个页面 1 个且互不相同，`/videos` 页 14 个全部唯一。
- 移除 TikTok / YouTube Shorts 等失效或重复链接，0 处剩余。

### 7. 部署与工程化
- 部署至 Cloudflare Pages 项目 `lcm`：<https://main.lcm-5c4.pages.dev>
- 站点页面 11/11 返回 200，`/zzz-not-real` 正确返回 404，工具目录（`/.tools/*`）302 回首页。
- 新增 `.gitignore`（排除 `.tools/`、`.wrangler/`、`.clinerules` 等本地工具与部署缓存）与 `.gitattributes`（`eol=lf`，避免 Windows 换行污染）。

---

## 页面清单

| 页面 | 文件 | 图片数 | 视频 |
| --- | --- | --- | --- |
| Home | `index.html` | 10 | 1 |
| Application | `application.html` | 10 | 1 |
| Advantages | `advantages.html` | 7 | — |
| SIP Formwork | `sip-formwork.html` | 7 | 1 |
| Casting a House | `casting-a-house.html` | 7 | 1 |
| Foaming Technology | `foaming-technology.html` | 11 | 1 |
| Hollow Molds | `hollow-molds.html` | 7 | 1 |
| External Wall Panels | `external-wall-panels.html` | 7 | 1 |
| Videos | `videos.html` | 0 | 14 |
| FAQ | `faq.html` | 7 | — |
| Contact | `contact.html` | 7 | — |
| 404 | `404.html` | — | — |

`robots.txt`、`_redirects`（Cloudflare Pages 重写规则）、`style.css`（历史遗留，当前页面未引用）一并保留。

---

## 部署

```powershell
# Cloudflare Pages（生产分支 main）
npx wrangler pages deploy . --project-name=lcm --branch=main

# 可选：同步 HTML 到内网 NAS 镜像
Copy-Item -Path .\*.html -Destination <nas-share>\lcm\ -Force
```

---

## 维护约定

1. **新增图片请勿再带水印**：现有图片已经过裁切去水印，不要再次裁切（会丢失画面）。
2. **gallery 图片需全站唯一**：新增照片前先确认未被其他页面使用。
3. **每个页面最多 1 个视频**，`/videos` 页的视频 ID 不得重复。
4. 图片引用一律使用根路径（`/wp-content/...`），文件名大小写需与实际文件一致（Pages 区分大小写）。

---

## English summary

Static LCM theme (11 pages + 404). All referenced photos were cropped to remove the baked-in grey “LCM” watermark, galleries were rebuilt so that every page uses unique, topic-matched photos (0 duplicates site-wide), grey overlay rings were removed, the hero `srcset` was regenerated for the real 1024/1568 px assets, and every YouTube embed was validated and de-duplicated (14 valid IDs). Deployed to Cloudflare Pages project `lcm`.
