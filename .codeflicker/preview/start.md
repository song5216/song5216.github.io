# Hugo 本地预览启动指南

## 项目概述
本仓库是 Hugo（主题 PaperMod）构建的静态博客站点。你新增了“工具”页面与相关卡片样式，可通过 `hugo server` 本地预览验证。

## . - Hugo Site

### 快速启动

```bash
# 在仓库根目录执行
hugo server -D
```

**启动后访问**：http://localhost:1313

验证项：
- 工具页：`http://localhost:1313/tools/`
- 检查卡片样式是否生效：确认页面加载了 `static/css/custom.css`（如需可强刷浏览器缓存）

```yaml
subProjectPath: .
command: hugo server -D
cwd: .
port: 1313
previewUrl: http://localhost:1313
description: Hugo 本地预览（包含草稿 -D），用于验证 /tools/ 页面与卡片样式
```
