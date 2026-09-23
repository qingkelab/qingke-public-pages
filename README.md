# qingke-public-pages

青稞社区**公开发布仓库**：只放已经定稿、可以公开的静态 HTML 文章。

私人材料（研究简报、证据矩阵、来源库、草稿 Markdown 与 SVG 源文件、benchmark 产物）一律留在私有仓库，不进这里。

## 结构

```text
index.html                    文章入口页（含 <!-- articles --> 插入标记）
article/<三位序号>/index.html  单篇文章（自包含内联样式）
article/<三位序号>/images/     该文章的图片（可选）
.github/workflows/pages.yml   GitHub Pages 部署
.nojekyll                     关闭 Jekyll，按原始静态文件发布
```

## 怎么加一篇文章

推荐直接用解读项目的发布脚本（会自己算编号、写页面、插首页入口）：

```bash
# 在 qingke-jiedu 仓库里
node scripts/publish-article.js --dir output/<id> --repo ~/Documents/qingke-public-pages            # 先看计划（dry-run）
node scripts/publish-article.js --dir output/<id> --repo ~/Documents/qingke-public-pages --yes      # 落盘
node scripts/publish-article.js --dir output/<id> --repo ~/Documents/qingke-public-pages --yes --push --pr
```

约定：

- 编号 = 现有 `article/<NNN>` 最大值 +1，不复用、不重排；`--number` 撞到已发布编号会被拒绝（除非 `--force`）。
- 首页入口插在 `<!-- articles -->` 标记之后（**不要删这个标记**）；仓库用卡片样式，脚本会跟着生成卡片。
- 正文里的本地图片会改写成 `images/xxx.png`；仍指向外链的图（如 arXiv CDN）保持原样。
- 每页页尾带一份可折叠的「数字核验表」（若发布目录里有 `deepread.fact-check.md`）。

手工方式：在 `article/<NNN>/index.html` 放一个自包含页面，然后在 `index.html` 的标记后加一条 `<li><a class="card" href="article/<NNN>/">…</a></li>`。

## 部署

推送到 `main` 即触发 `.github/workflows/pages.yml`，用 GitHub 官方 Pages actions 发布整个仓库根目录。

- 站点地址：`https://qingkelab.github.io/qingke-public-pages/`

## 约定

- 图片走公开地址时保留远程 URL，不把私有仓库的源文件复制进来。
- 正文用自包含 HTML（内联样式），便于单独打开与迁移。
- 文章页不放草稿痕迹、不放内部审计元数据。
