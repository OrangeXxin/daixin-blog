# 代鑫的博客

使用 **Hugo + Cloudflare Pages** 搭建的个人博客。

## 项目结构

```
blog/
├── content/          # 文章内容（Markdown）
│   ├── about.md      # 关于页
│   ├── archives.md   # 归档页
│   └── posts/        # 博客文章
├── themes/PaperMod/  # 主题
├── static/           # 静态资源
├── hugo.toml         # 站点配置
├── wrangler.toml     # Cloudflare Pages 配置
└── public/           # 构建产物（git 已忽略）
```

## 本地使用

```powershell
# 本地预览（含草稿）
hugo server -D

# 构建站点（产物输出到 public/）
hugo --minify

# 新建文章
hugo new content posts/my-post.md
```

预览地址：`http://localhost:1313`

## 部署到 Cloudflare Pages

### 方式一：Git 集成（推荐，自动部署）

1. 把本项目推到 GitHub（或 GitLab）
2. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com) → **Workers & Pages** → **创建** → **Pages** → **连接到 Git**
3. 选择博客仓库，构建设置：
   - **框架预设**：Hugo
   - **构建命令**：`hugo --minify`
   - **构建输出目录**：`public`
   - **环境变量**：`HUGO_VERSION` = `0.165.0`（避免使用默认旧版本）
4. 保存后 Cloudflare 会自动构建发布，访问地址 `https://<项目名>.pages.dev`

之后每次 `git push` 都会自动重新部署。

### 方式二：wrangler CLI 直接上传

```powershell
# 安装 wrangler
npm install -g wrangler

# 登录 Cloudflare
wrangler login

# 构建并部署
hugo --minify
wrangler pages deploy public --project-name daixin-blog
```

首次部署时如果项目不存在，wrangler 会提示创建，也可先执行：
`wrangler pages project create daixin-blog --production-branch main`

## 发布前必改

- `hugo.toml` 中的 `baseURL`：改为你的实际域名（默认 `https://yourblog.pages.dev/`）
- `hugo.toml` 中的社交链接（GitHub / 邮箱）
- `content/about.md` 中的联系方式

## 写文章流程

1. `hugo new content posts/文章名.md`
2. 编辑 front matter 把 `draft: false`
3. 用 Markdown 写正文
4. `git add -A && git commit -m "..." && git push`（Git 集成模式自动发布）
