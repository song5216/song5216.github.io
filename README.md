# 宋家强 的个人博客

基于 Hugo 静态网站生成器构建的个人技术博客，采用 PaperMod 主题并进行深度定制。

🌐 **在线访问**: [https://song5216.github.io](https://song5216.github.io)

## ✨ 特性

- 🎨 **自定义 Hero 首页** - 精心设计的着陆页，支持多按钮配置
- 🔍 **全文搜索** - 基于 Fuse.js 的模糊搜索
- 💬 **Giscus 评论** - 基于 GitHub Discussions 的评论系统
- 🌓 **深色/浅色模式** - 自动跟随系统主题
- 📱 **响应式设计** - 完美适配移动端
- 📂 **分类 & 标签** - 灵活的内容组织
- 📖 **阅读时间 & 字数统计**
- 🔗 **文章目录 (TOC)**

## 🛠 技术栈

| 技术 | 说明 |
|------|------|
| [Hugo](https://gohugo.io/) | Go 语言编写的静态网站生成器 |
| [PaperMod](https://github.com/adityatelange/hugo-PaperMod) | 简洁优雅的 Hugo 主题 |
| [Giscus](https://giscus.app/) | 基于 GitHub Discussions 的评论系统 |
| [Fuse.js](https://fusejs.io/) | 轻量级模糊搜索库 |
| [GitHub Pages](https://pages.github.com/) | 静态网站托管 |
| [GitHub Actions](https://github.com/features/actions) | CI/CD 自动部署 |

## 📁 项目结构

```
blog/
├── archetypes/          # 文章模板
├── content/             # 内容目录
│   ├── posts/           # 博客文章
│   ├── about.md         # 关于页面
│   ├── projects.md      # 项目页面
│   ├── links.md         # 友链页面
│   └── search.md        # 搜索页面
├── layouts/             # 自定义布局
│   ├── _default/        # 默认模板覆盖
│   └── partials/        # 组件模板
│       ├── hero.html    # 首页 Hero 区域
│       ├── comments.html# Giscus 评论
│       └── ...
├── static/              # 静态资源
│   └── css/custom.css   # 自定义样式
├── themes/PaperMod/     # PaperMod 主题
└── config.yaml          # Hugo 配置文件
```

## 🚀 Quick Start

### 环境要求

- [Hugo Extended](https://gohugo.io/installation/) >= v0.112.0
- Git

### 1. 克隆项目

```bash
git clone https://github.com/song5216/song5216.github.io.git blog
cd blog
```

### 2. 初始化主题子模块

```bash
git submodule update --init --recursive
```

### 3. 启动本地服务

```bash
hugo server -D
```

访问 http://localhost:1313 预览博客

### 4. 创建新文章

```bash
hugo new posts/my-first-post.md
```

编辑 `content/posts/my-first-post.md`：

```yaml
---
title: "我的第一篇文章"
date: 2025-01-20
draft: false
tags: ["Hugo", "博客"]
categories: ["技术"]
description: "这是我的第一篇博客文章"
---

正文内容...
```

### 5. 构建生产版本

```bash
hugo --minify
```

输出目录为 `public/`

## ⚙️ 配置说明

核心配置位于 `config.yaml`：

```yaml
# 基础配置
baseURL: "https://song5216.github.io/"
title: "宋家强 的个人博客"
theme: "PaperMod"

# Hero 区域配置
params:
  hero:
    title: "宋家强 的技术世界"
    subtitle: "记录学习 · 分享思考 · 探索技术"
    buttons:
      - text: "开始阅读"
        url: "/posts/"
      - text: "关于我"
        url: "/about/"
        class: "secondary"
```

## 📝 License

MIT License
