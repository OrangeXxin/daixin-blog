---
title: "使用 Hugo + Cloudflare Pages 搭建个人博客"
date: 2026-09-02
draft: false
summary: "从零开始，用 Hugo 静态站点生成器和 Cloudflare Pages 免费搭建一套个人博客的完整教程。"
tags: ["教程", "Hugo", "Cloudflare"]
---

## 为什么选 Hugo + Cloudflare Pages

个人博客的搭建方案有很多，之所以选这套组合：

- **Hugo**：基于 Go 的静态站点生成器，构建速度快到离谱（几百篇文章也是秒级），Markdown 写作体验好，主题生态成熟
- **Cloudflare Pages**：免费额度对个人博客完全够用，全球 CDN 加速，自带 HTTPS，支持持续集成（Git 推代码自动构建发布）

**成本：0 元。** 域名也可以先用免费的 `*.pages.dev` 子域名，后续再考虑绑定自己的域名。

## 第一步：安装 Hugo

Hugo 分为标准版和扩展版（Extended），**建议安装扩展版**，很多主题依赖它（例如 SCSS 编译）。

### Windows

推荐用 `winget` 安装：

```powershell
winget install Hugo.Hugo.Extended
```

或者直接去 [Hugo Releases](https://github.com/gohugoio/hugo/releases) 下载 `hugo_extended_<版本>_windows-amd64.zip`，解压后把 `hugo.exe` 所在目录加入系统 PATH。

### macOS

```bash
brew install hugo
```

验证安装：

```bash
hugo version
```

## 第二步：创建站点

```bash
hugo new site blog
cd blog
```

这会生成如下目录结构：

```
blog/
├── archetypes/   # 内容模板
├── assets/       # 构建资源
├── content/      # 文章内容（Markdown）
├── data/         # 数据文件
├── layouts/      # 自定义模板（一般用不到）
├── static/       # 静态文件
├── themes/       # 主题目录
└── hugo.toml     # 站点配置
```

## 第三步：安装主题

这里使用最流行的 **PaperMod** 主题：

```bash
git clone --depth 1 https://github.com/adityatelange/hugo-PaperMod themes/PaperMod
```

如果网络访问 GitHub 不稳定，也可以直接下载 zip 包解压到 `themes/PaperMod` 目录。

然后编辑 `hugo.toml`，设置主题并写入配置：

```toml
baseURL = "https://yourblog.pages.dev/"
languageCode = "zh-cn"
title = "我的博客"
theme = "PaperMod"

[menu.main]
  [[menu.main]]
    identifier = "archives"
    name = "归档"
    url = "/archives/"
    weight = 1

[params]
  description = "个人技术博客"
  ShowReadingTime = true
```

## 第四步：写文章

```bash
hugo new content posts/my-first-post.md
```

编辑 `content/posts/my-first-post.md`，在 front matter 中把 `draft: true` 改为 `draft: false`：

```markdown
---
title: "我的第一篇文章"
date: 2026-09-02
draft: false
---

正文使用 Markdown 书写……
```

本地预览：

```bash
hugo server -D
```

浏览器打开 `http://localhost:1313` 即可实时预览。

## 第五步：部署到 Cloudflare Pages

### 方式一：直接上传（无需 Git）

构建站点：

```bash
hugo --minify
```

构建产物在 `public/` 目录。然后在 Cloudflare Dashboard：

1. 进入 **Workers & Pages** → **创建** → **Pages**
2. 选择 **直接上传**，把 `public/` 目录拖进去
3. 上传后即可通过 `https://<项目名>.pages.dev` 访问

### 方式二：Git 集成（推荐，自动构建）

把博客仓库推到 GitHub/GitLab，然后：

1. **Workers & Pages** → **创建** → **Pages** → **连接到 Git**
2. 选择仓库
3. 构建设置：
   - **构建命令**：`hugo --minify`
   - **构建输出目录**：`public`
   - **环境变量**：`HUGO_VERSION` = `0.165.0`

之后每次 `git push`，Cloudflare 都会自动重新构建并发布。

> 提示：Cloudflare Pages 的默认 Hugo 版本可能较旧，通过环境变量 `HUGO_VERSION` 指定版本可确保构建一致。

## 后续优化方向

- 绑定自定义域名并开启 HTTPS
- 配置评论系统（如 giscus / waline）
- 接入站点统计（如 umami / 百度统计）
- 提交站点地图到搜索引擎（Google / Bing）

## 总结

Hugo + Cloudflare Pages 是一套**零成本、极速、易维护**的个人博客方案。写作只用 Markdown，发布只需一条命令或一次 `git push`，把精力留给内容本身。

开始你的博客之旅吧！
