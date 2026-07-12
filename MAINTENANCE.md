# 网站维护指南（jinjieliu.com）

本站基于 [PRISM](https://github.com/xyjoey/PRISM) 模板（Next.js 静态导出）。**内容和代码完全分离**：日常改动只需要编辑 `content/`（英文）和 `content_zh/`（中文）里的文本文件，不用碰代码。

## 改什么 → 动哪个文件

| 想改的东西 | 文件位置 |
|---|---|
| 新论文、改论文信息 | `content/publications.bib`（中英文共用一份） |
| 加一条动态 News | `content/news.toml` 和 `content_zh/news.toml` |
| 头衔、邮箱、社交链接、"最近更新"日期 | `content/config.toml` 和 `content_zh/config.toml` |
| 自我介绍（首页正文） | `content/bio.md` 和 `content_zh/bio.md` |
| 研究兴趣列表 | `content/about.toml` 和 `content_zh/about.toml` |
| 奖项 / 教学 / 学术服务 | 对应的 `awards.toml` / `teaching.toml` / `services.toml`（两个目录各一份） |
| 简历页 | `content/cv.md` 和 `content_zh/cv.md` |
| 换头像 | 替换 `public/profile.jpg`（建议正方形，200 KB 以内） |
| 网站图标 | `public/favicon.svg` |
| 配色、字体 | `src/app/globals.css`（唯一需要碰"代码"的场景，改 CSS 变量即可） |

## 加一篇新论文（最常见操作）

打开 `content/publications.bib`，照抄一个现有条目改内容：

```bibtex
@article{liu2027example,
  selected = {true},                 % 加这行 = 出现在首页"代表作"（首页最多显示 5 条）
  title = {论文标题},
  journal = {期刊名},                % 会议论文用 @inproceedings + booktitle
  author = {Jinjie Liu and 合作者全名},
  year = {2027},
  volume = {12},
  pages = {1--10},
  doi = {10.xxxx/xxxxx},
  description = {一句话介绍（列表页显示）},
  abstract = {摘要（展开后显示，可省略）},
  keywords = {关键词1, 关键词2}
}
```

注意：每个字段行末的**逗号不能漏**，花括号要配对，否则整页论文列表可能渲染失败。

## 加一条动态

在 `content/news.toml`（英文）和 `content_zh/news.toml`（中文）**开头**各加一段（新的放最上面）：

```toml
[[news]]
date = "2027-01"
content = "支持 **加粗**、*斜体* 和 [链接](https://example.com) 的 Markdown 写法。"
```

## 改完之后：预览与发布

```bash
# 1. 本地预览（在仓库根目录）
npm run dev          # 浏览器打开 http://localhost:3000，右上角可切中英文/明暗模式

# 2. 确认无误后提交
git add -A
git commit -m "更新xxx"

# 3. 发布
git push             # Cloudflare Pages 会自动构建，2-3 分钟后 jinjieliu.com 更新
```

首次部署或换机器时先装依赖：`npm ci`（需要 Node 22+）。

## 注意事项

- **双语同步**：`content/` 和 `content_zh/` 是镜像关系，改英文时记得同步改中文（`publications.bib` 除外，共用一份）。
- **编码**：所有文件保存为 UTF-8（无 BOM），中文才不会乱码。
- **字体**：正文 Inter，标题 Source Serif 4（经 `src/app/layout.tsx` 里的 `<link>` 从 Google Fonts 加载）；中文标题自动切换为思源宋体系（`globals.css` 里 `html[data-locale^="zh"]` 那段，**不要删**）。不要在 CSS 里用 `@import url(...)` 加字体，会导致 `npm run dev` 报 CSS 解析错误。
- **配色**：Nature 编辑风 = 纸白底 + 墨黑字 + masthead 红 `#e2231a`。改色只需要动 `globals.css` 顶部 `:root/.light` 和 `.dark` 两组变量。
- **`_source/` 目录**：存放 CV 源文件、头像原图等素材，已被 `.gitignore` 排除，不会发布到网上。

## 部署架构

GitHub 仓库（FewStars/jinjieliu.com）→ Cloudflare Workers 构建（git 接入，push 到 main 自动构建部署）→ 自定义域名 jinjieliu.com（DNS 在 Cloudflare）。

- 构建命令：`npm run build`（静态导出到 `out/`）；部署命令：`npx wrangler deploy`
- 仓库根目录的 `wrangler.jsonc` 声明了静态资产模式（`assets.directory = ./out`）。**不要删这个文件**，否则 wrangler 会误判为 SSR 应用去套 OpenNext 适配器，构建必挂。
- Node 版本由 `.node-version` 指定（22）。
