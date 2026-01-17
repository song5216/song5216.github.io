# Song5216 的个人博客

基于 [Hugo](https://gohugo.io/) 和 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题搭建的个人博客网站，部署在 GitHub Pages 上。

## 功能特性

- ✅ 中文语言支持
- ✅ PaperMod 主题
- ✅ Giscus 评论系统
- ✅ 搜索功能（Fuse.js）
- ✅ 分类和标签系统
- ✅ GitHub Actions 自动部署
- ✅ 响应式设计
- ✅ 暗黑模式支持

## 本地开发

### 前置要求

1. 安装 [Hugo Extended](https://gohugo.io/installation/)（必须使用 Extended 版本）
   ```bash
   # macOS
   brew install hugo
   ```

2. 安装 Git

### 初始化项目

1. 克隆仓库（如果还没有）
   ```bash
   git clone https://github.com/song5216/song5216.github.io.git
   cd song5216.github.io
   ```

2. 初始化 Git 子模块（PaperMod 主题）
   ```bash
   git submodule update --init --recursive
   ```

   如果还没有添加主题，使用：
   ```bash
   git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
   ```

3. 启动本地开发服务器
   ```bash
   hugo server -D
   ```

   访问 http://localhost:1313 查看网站

## 创建新文章

使用 Hugo 命令创建新文章：

```bash
hugo new posts/文章标题.md
```

编辑文章时，记得将 front matter 中的 `draft: false` 设置为 `false` 来发布文章。

### 文章 Front Matter 示例

```yaml
---
title: "文章标题"
date: 2025-01-17T10:00:00+08:00
draft: false
tags: ["标签1", "标签2"]
categories: ["分类"]
description: "文章描述"
---
```

## 配置 Giscus 评论系统

1. 在 GitHub 仓库中启用 Discussions：
   - 进入仓库 Settings → General → Features
   - 勾选 "Discussions"

2. 访问 [giscus.app](https://giscus.app/) 配置评论系统：
   - 输入仓库名称：`song5216/song5216.github.io`
   - 选择分类
   - 获取 `data-repo-id` 和 `data-category-id`

3. 更新 `layouts/partials/comments.html`：
   - 将 `YOUR_REPO_ID` 替换为实际的 repo-id
   - 将 `YOUR_CATEGORY_ID` 替换为实际的 category-id

4. 在 `config.yaml` 中确保评论系统已启用：
   ```yaml
   params:
     comments: true
     commentSystem: "giscus"
   ```

## 部署

网站通过 GitHub Actions 自动部署：

1. 推送代码到 `main` 分支
2. GitHub Actions 会自动构建 Hugo 网站
3. 构建完成后自动部署到 GitHub Pages

### 手动部署（可选）

如果需要手动部署：

```bash
# 构建网站
hugo --minify

# 进入构建输出目录
cd public

# 初始化 Git（如果还没有）
git init
git remote add origin https://github.com/song5216/song5216.github.io.git

# 提交并推送
git add .
git commit -m "Deploy site"
git push -u origin main
```

### 启用 GitHub Pages

1. 进入仓库 Settings → Pages
2. 在 "Source" 部分选择 "GitHub Actions"
3. 保存设置

网站将在几分钟后可通过 https://song5216.github.io 访问

## 项目结构

```
blog/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions 自动部署配置
├── archetypes/
│   └── default.md              # 文章模板
├── content/
│   ├── posts/                  # 文章目录
│   └── search.md               # 搜索页面
├── layouts/
│   └── partials/
│       └── comments.html       # Giscus 评论系统集成
├── static/                     # 静态资源目录
├── themes/
│   └── PaperMod/              # PaperMod 主题（git submodule）
├── config.yaml                 # Hugo 主配置文件
├── .gitignore                  # Git 忽略文件
└── README.md                   # 项目说明文档
```

## 自定义配置

主要配置文件是 `config.yaml`，你可以修改：

- 网站标题和描述
- 社交链接
- 主题参数
- 菜单项
- 等等

更多配置选项请参考：
- [Hugo 文档](https://gohugo.io/documentation/)
- [PaperMod 主题文档](https://github.com/adityatelange/hugo-PaperMod/wiki)

## 许可证

MIT License

## 致谢

- [Hugo](https://gohugo.io/) - 静态网站生成器
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod) - Hugo 主题
- [Giscus](https://giscus.app/) - 评论系统
