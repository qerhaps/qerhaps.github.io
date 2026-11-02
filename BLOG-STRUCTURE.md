# 博客原型架构

Hexo 8.1.2 + Butterfly 5.7.0，源码在 `main` 分支，构建产物推送到 `gh-pages`。

## 目录结构

```
E:\blog\
├── _config.yml                 站点总配置（标题、URL、永久链接、部署目标）
├── _config.butterfly.yml       主题配置（导航、头像、搜索、夜间模式、注入）
├── _config.landscape.yml       旧主题残留配置，可安全删除
├── package.json                依赖与脚本
├── .gitignore                  忽略 node_modules / public / db.json
│
├── scaffolds\                  新建文章的模板
│   ├── post.md                 文章模板（含 categories / tags / cover / top_img）
│   ├── page.md                 独立页面模板
│   └── draft.md                草稿模板
│
├── source\                     ── 内容源，所有写作都在这里
│   ├── _posts\                 文章正文
│   │   ├── tech\               技术类
│   │   │   └── butterfly-setup.md
│   │   ├── life\               生活类
│   │   │   └── first-post.md
│   │   ├── notes\              笔记类
│   │   │   └── markdown-guide.md
│   │   └── draft\              未发布草稿（render_drafts: false，不会生成）
│   │       └── .gitkeep
│   │
│   ├── _data\                  主题读取的数据文件
│   │   └── link.yml            友链列表，对应 /link/ 页面
│   │
│   ├── about\index.md          关于页        → /about/
│   ├── tags\index.md           标签总览      → /tags/       （type: tags）
│   ├── categories\index.md     分类总览      → /categories/ （type: categories）
│   ├── link\index.md           友链页        → /link/       （type: link）
│   │
│   ├── css\custom.css          自定义样式，通过 inject 注入每个页面
│   └── img\                    站点图片资源 → /img/xxx
│       └── .gitkeep
│
├── themes\butterfly\           主题本体，不要改（升级会被覆盖）
│   ├── _config.yml             主题原始配置，仅作参考
│   ├── layout\                 pug 模板
│   ├── source\                 stylus 样式与内置资源
│   └── languages\              多语言包
│
└── public\                     构建产物，已 gitignore，不要手改
```

## 内容与分类的关系

物理目录（`_posts/tech/`）只影响文件组织，**分类来自 front-matter 的 `categories` 字段**。两者建议保持一致，方便维护。

新建文章时目录层级会进入 URL：

```
2026/11/02/tech/butterfly-setup/   ← 目录 tech 被带进了永久链接
```

如果不希望 URL 带目录，可以在 `_config.yml` 里改 `permalink`，例如 `:year/:month/:day/:title/` 配合把文章平铺在 `_posts/` 根下，或引入 `hexo-abbrlink` 生成固定短链。

## 新建内容

```bash
npx hexo new post "文章标题"          # 进 _posts/，模板用 scaffolds/post.md
npx hexo new draft "草稿标题"         # 进 _posts/draft/，不生成页面
npx hexo new page "页面名"            # 进 source/页面名/index.md
```

## 常用命令

| 命令 | 作用 |
| --- | --- |
| `npx hexo clean` | 清掉 `public/` 和 `db.json`，改配置后必做 |
| `npx hexo generate` | 生成静态文件到 `public/` |
| `npx hexo server` | 本地预览，默认 <http://localhost:4000> |
| `npx hexo deploy` | 推送到 `gh-pages` 分支 |

## 写作流程

1. `npx hexo new post "标题"`
2. 写正文，填 `categories` / `tags` / `description`
3. `npx hexo server` 本地看效果
4. `npx hexo clean && npx hexo generate`
5. 源码提交 `main`，产物 `npx hexo deploy` 推 `gh-pages`

## 网络与部署通道

本机代理会拦截 TLS，HTTPS 方式 `git push` 会报 `CRYPT_E_NO_REVOCATION_CHECK` / `unable to get local issuer certificate`。已改用 **SSH over 443**：

`~/.ssh/config`
```
Host github.com
  HostName ssh.github.com
  Port 443
  User git
```

远端地址统一为 `git@github.com:qerhaps/qerhaps.github.io.git`，站点 `_config.yml` 的 `deploy.repo` 也指向该地址。不要改回 HTTPS。

验证通道：

```bash
ssh -T git@github.com    # 应返回 Hi qerhaps!
```

## 关键配置位置

| 想改什么 | 去哪 |
| --- | --- |
| 站点标题 / 副标题 / 作者 | `_config.yml` |
| 导航菜单 | `_config.butterfly.yml` → `menu` |
| 社交图标 | `_config.butterfly.yml` → `social` |
| 首页打字机文案 | `_config.butterfly.yml` → `subtitle.sub` |
| 头像 / favicon | `_config.butterfly.yml` → `avatar` / `favicon` |
| 夜间模式 | `_config.butterfly.yml` → `darkmode` |
| 本地搜索 | `_config.butterfly.yml` → `search.use: local_search` |
| 自定义 CSS | `source/css/custom.css`（已通过 `inject.head` 引入） |
| 友链数据 | `source/_data/link.yml` |
