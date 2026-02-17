# song5216.github.io（Hugo + PaperMod）

这是一个基于 **Hugo（Extended）** 构建并通过 **GitHub Pages** 自动部署的个人技术博客站点。你可以把它理解成：

- `content/`：文章与页面（类似后端的“数据源”）
- `layouts/`：模板覆盖与组件化（类似后端的“视图层/渲染层”）
- `config.yaml`：站点配置与功能开关（类似后端的“应用配置”）
- GitHub Actions：自动构建生成静态文件并发布到 Pages（类似 CI/CD）

在线地址：<https://song5216.github.io>

---

## 1. 你需要先知道的核心概念（后端视角）

### 1.1 Hugo 是什么

Hugo 是一个静态网站生成器：

- 输入：Markdown 内容 + 配置 + 主题模板
- 处理：本地/CI 中执行 `hugo`
- 输出：纯静态 HTML/CSS/JS（在本项目里输出到 `public/`）

静态站点的优点：部署简单、访问快、安全面小；缺点：动态能力需要依赖第三方（如评论/搜索/统计）。

### 1.2 PaperMod 是什么

PaperMod 是 Hugo 的一个主题。本仓库用 Git Submodule 的方式引入，位于：

- `themes/PaperMod/`
- 子模块声明：`.gitmodules`

本项目会在 `layouts/` 目录中覆盖部分主题模板，以实现自定义首页 Hero、评论组件等。

### 1.3 GitHub Pages 部署是如何工作的

工作流文件：`.github/workflows/deploy.yml`

触发条件：

- `main` 分支 push
- 或手动触发（workflow_dispatch）

流水线做的事情：

1. 拉取代码（包含 submodules）
2. 安装 Hugo（Extended）
3. 执行构建：`hugo --minify --baseURL "${{ steps.pages.outputs.base_url }}/"`
4. 将 `public/` 作为构建产物上传并发布到 GitHub Pages

你日常只需要关心：

- 写内容（`content/`）
- 调整配置（`config.yaml`）
- 必要时改模板/样式（`layouts/`、`static/`）

---

## 2. 5 分钟本地跑起来（推荐路径）

### 2.1 安装依赖

- Hugo Extended（建议版本 >= 0.112，CI 用 latest）
- Git

Mac（示例）：

```bash
brew install hugo
```

> 注意：需 Extended 版本，否则部分主题/SCSS 可能不可用。

### 2.2 克隆项目并初始化主题子模块

```bash
git clone https://github.com/song5216/song5216.github.io.git blog
cd blog
git submodule update --init --recursive
```

### 2.3 启动本地预览

```bash
hugo server -D
```

访问：<http://localhost:1313>

参数说明：

- `-D`：包含草稿（front matter 中 `draft: true`）

---

## 3. 项目结构速览

```text
.
├── config.yaml                 # 站点配置（标题/菜单/主题参数/评论/搜索等）
├── archetypes/                 # hugo new 时的文章模板
├── content/                    # 内容（文章/页面）
│   ├── posts/                  # 博客文章
│   ├── about.md                # 关于页
│   ├── projects.md             # 项目页
│   ├── links.md                # 友链页
│   └── search.md               # 搜索页
├── layouts/                    # 覆盖 PaperMod 的自定义模板（优先级高于主题）
│   ├── _default/               # list/single/terms 等页面布局
│   └── partials/               # 可复用组件（hero/comments/toc 等）
├── static/                     # 静态资源（直接拷贝到站点根目录）
│   └── css/custom.css          # 自定义样式（推荐在这里加样式）
├── themes/PaperMod/            # 主题（Git Submodule）
└── public/                     # 构建输出（本地/CI 生成；一般不手改）
```

### 3.1 `public/` 要不要提交？

本仓库当前包含 `public/` 目录（且有 `.gitkeep`）。但实际发布由 GitHub Actions 构建并上传产物完成。

建议的维护策略：

- 本地不要手动修改 `public/`
- 以 `content/` / `layouts/` / `static/` / `config.yaml` 为真实源代码

如果你希望仓库更“干净”，可以考虑把 `public/` 从版本控制中移除（需要调整 `.gitignore` 与历史文件），但这属于可选重构，不影响现有部署。

---

## 4. 日常操作指南

### 4.1 新建文章

```bash
hugo new posts/my-first-post.md
```

编辑 `content/posts/my-first-post.md` 的 Front Matter（示例）：

```yaml
---
title: "我的第一篇文章"
date: 2025-01-20
draft: true
tags: ["Hugo", "博客"]
categories: ["技术"]
description: "这是一段摘要描述"
---
```

发布文章：将 `draft: true` 改为 `draft: false`。

### 4.2 新建页面（About/Projects/Links 等）

做法与文章类似，放到 `content/` 根目录即可，例如：

- `content/about.md`
- `content/projects.md`

### 4.3 修改菜单

修改 `config.yaml` 中的 `menu.main`：

- `identifier`：唯一标识
- `name`：显示名称
- `url`：路由（Hugo 会基于内容生成对应路径）
- `weight`：排序（越小越靠前）

### 4.4 修改首页 Hero（按钮/标题）

修改 `config.yaml` 中的 `params.hero`：

- `title` / `subtitle`
- `buttons`：数组，支持 `text` / `url` / `class`

对应模板在 `layouts/partials/hero.html`。

### 4.5 增加工具（SOP）

本项目的“工具”入口页为：`/tools/`，对应内容文件：`content/tools.md`（通过 front matter 的 `tools:` 列表驱动卡片展示）。

#### Step 1：在工具聚合页增加一个卡片入口

编辑 `content/tools.md`，在 `tools:` 下新增一项：

```yaml
tools:
  - name: "工具名称"
    url: "/tools/your-tool/"
```

- `name`：卡片展示文案
- `url`：跳转地址
  - 站内工具：`/tools/xxx/`
  - 外链工具：`https://...`（会自动新开标签页）

#### Step 2：新增工具页面（若为站内 HTML/JS 工具）

在 `static/` 下新增工具目录与入口文件：

- `static/tools/your-tool/index.html`

Hugo 会将其映射为站点路径：

- `/tools/your-tool/`

> 实践建议：复制一个已有工具目录（如 `static/tools/compute-length/`）再改内容，确保资源路径与交互逻辑可复用。

#### Step 3：本地验证

启动本地预览：

```bash
hugo server -D
```

检查：

- 工具聚合页是否出现新卡片：`http://localhost:1313/tools/`
- 点击卡片能否正常跳转到工具页：`http://localhost:1313/tools/your-tool/`

### 4.6 修改样式

优先在 `static/css/custom.css` 增量覆盖，不建议直接改主题 CSS。

原因：

- 主题升级更安全
- 自定义更可控

### 4.6 评论系统（Giscus）

本项目启用了 Giscus（基于 GitHub Discussions）。配置入口：

- `config.yaml` → `params.comments: true`
- `config.yaml` → `params.commentSystem: "giscus"`
- 具体渲染逻辑在 `layouts/partials/comments.html`

如果你需要更换仓库/分类/映射策略，通常需要同时调整：

- Giscus 后台配置（giscus.app）
- 模板里的脚本参数

### 4.7 搜索

本项目启用了首页 JSON 输出用于搜索：

- `config.yaml` → `outputs.home` 包含 `JSON`
- 主题侧通过 Fuse.js 读取 `index.json` 实现前端搜索

---

## 5. 构建与发布

### 5.1 本地构建

```bash
hugo --minify
```

输出目录：`public/`

### 5.2 线上自动发布

只要你把改动 push 到 `main`，GitHub Actions 就会自动：

- 构建站点
- 发布到 GitHub Pages

工作流文件：`.github/workflows/deploy.yml`

---

## 6. 常见问题（FAQ）

### Q1：本地启动时报错：找不到主题 / 页面样式不对？

通常是忘了初始化子模块：

```bash
git submodule update --init --recursive
```

### Q2：为什么我改了 `static/css/custom.css` 没生效？

- 先确认本地是否在跑 `hugo server`，并刷新页面（必要时强刷）
- 检查 `public/` 里是否生成了对应资源（本地 server 会在内存中提供）
- 如果你在 `layouts/` 覆盖了 head 引入逻辑，确认 `custom.css` 仍被加载

### Q3：内容路由是怎么来的？

Hugo 通常按 `content/` 的路径生成 URL：

- `content/posts/hello.md` → `/posts/hello/`
- `content/about.md` → `/about/`

可以通过 Front Matter / 配置进一步定制（如 `slug`、`url`）。

---

## 7. 建议的工作流（适合后端同学）

1. 拉取最新 `main` 并更新子模块
2. `hugo server -D` 本地预览
3. 写/改内容：`content/`
4. 调整配置：`config.yaml`
5. 涉及样式：优先改 `static/css/custom.css`
6. 涉及页面结构：改 `layouts/` 的覆盖模板
7. push 到 `main` 触发自动发布

---

## 8. License

MIT License
