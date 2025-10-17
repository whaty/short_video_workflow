# 电子书构建指南

## 📚 如何生成电子书

本书支持多种格式输出，可以根据需要选择合适的方式。

---

## 方案1：GitBook（推荐）

### 安装 GitBook CLI

```bash
# 安装 Node.js（如果还没安装）
# 访问 https://nodejs.org/

# 安装 GitBook CLI
npm install -g gitbook-cli

# 验证安装
gitbook -V
```

### 构建电子书

```bash
# 进入 book 目录
cd book

# 安装插件
gitbook install

# 本地预览
gitbook serve

# 访问 http://localhost:4000 查看效果

# 生成静态网站
gitbook build

# 输出目录：_book/
```

### 部署到 GitHub Pages

```bash
# 1. 生成静态文件
gitbook build

# 2. 创建 gh-pages 分支
git checkout --orphan gh-pages

# 3. 复制 _book 内容到根目录
cp -r _book/* .

# 4. 提交并推送
git add .
git commit -m "Deploy GitBook"
git push origin gh-pages

# 5. 在 GitHub 仓库设置中启用 GitHub Pages
# Settings → Pages → Source: gh-pages branch
```

---

## 方案2：mdBook（Rust工具）

### 安装 mdBook

```bash
# macOS
brew install mdbook

# 或使用 cargo
cargo install mdbook
```

### 创建配置文件

在 `book/` 目录创建 `book.toml`:

```toml
[book]
title = "短视频赚钱实战手册"
author = "短视频工作流"
description = "从0到月入过万的系统化方法论"
language = "zh-CN"

[build]
build-dir = "_book"

[output.html]
git-repository-url = "https://github.com/whaty/short_video_workflow"
```

### 构建

```bash
cd book

# 本地预览
mdbook serve

# 生成静态文件
mdbook build
```

---

## 方案3：Pandoc（生成PDF）

### 安装 Pandoc

```bash
# macOS
brew install pandoc

# 安装 LaTeX（用于生成PDF）
brew install --cask mactex
```

### 生成 PDF

```bash
cd book

# 合并所有 Markdown 文件
cat cover.md preface.md how-to-use.md \
    part1/*.md part2/*.md part3/*.md part4/*.md \
    epilogue.md > full-book.md

# 生成 PDF
pandoc full-book.md \
    -o "短视频赚钱实战手册.pdf" \
    --pdf-engine=xelatex \
    -V CJKmainfont="PingFang SC" \
    --toc \
    --toc-depth=2 \
    -V geometry:margin=1in \
    -V fontsize=12pt
```

### 自定义 PDF 样式

创建 `metadata.yaml`:

```yaml
---
title: "短视频赚钱实战手册"
subtitle: "从0到月入过万的系统化方法论"
author: "短视频工作流"
date: "2025年10月"
lang: zh-CN
documentclass: book
geometry: margin=1in
fontsize: 12pt
mainfont: "PingFang SC"
---
```

然后生成：

```bash
pandoc metadata.yaml full-book.md \
    -o "短视频赚钱实战手册.pdf" \
    --pdf-engine=xelatex \
    --toc \
    --toc-depth=2
```

---

## 方案4：Typora（可视化）

### 使用 Typora

1. 下载安装 Typora：https://typora.io/
2. 打开 `book/` 目录中的 Markdown 文件
3. 选择 `文件` → `导出` → `PDF`
4. 自定义样式和布局
5. 导出

### 优势

- 可视化编辑
- 所见即所得
- 导出美观
- 操作简单

---

## 方案5：在线文档平台

### 语雀

1. 注册语雀账号：https://www.yuque.com/
2. 创建知识库
3. 导入 Markdown 文件
4. 设置权限（公开/付费）
5. 分享链接

### Notion

1. 注册 Notion 账号：https://www.notion.so/
2. 创建 Page
3. 导入 Markdown 文件
4. 设置分享权限
5. 发布为网站

---

## 推荐方案

### 免费开源版

```
GitBook + GitHub Pages
- 免费托管
- 自动更新
- 在线阅读
- 搜索功能
```

### 付费售卖版

```
PDF（Pandoc生成）+ 语雀/Notion
- PDF：下载版本
- 语雀：在线阅读 + 持续更新
- 可设置付费阅读
```

### 混合方案

```
1. GitHub开源：建立影响力
2. GitBook在线：免费阅读
3. PDF精美版：付费下载
4. 语雀高级版：付费订阅 + 社群
```

---

## 文件结构

```
book/
├── README.md              # 首页
├── SUMMARY.md             # 目录
├── book.json              # GitBook配置
├── BUILD.md               # 本文件
├── cover.md               # 封面
├── preface.md             # 前言
├── how-to-use.md          # 使用说明
├── part1/                 # 第一部分
│   ├── chapter1.md
│   ├── chapter2.md
│   └── chapter3.md
├── part2/                 # 第二部分
│   ├── chapter4.md
│   ├── chapter5.md
│   └── chapter6.md
├── part3/                 # 第三部分
│   ├── chapter7.md
│   ├── chapter8.md
│   └── chapter9.md
├── part4/                 # 第四部分
│   ├── chapter10.md
│   ├── chapter11.md
│   └── chapter12.md
├── appendix/              # 附录
│   ├── keywords.md
│   ├── tools.md
│   └── data-template.md
└── epilogue.md            # 结语
```

---

## 常见问题

### Q1：GitBook 安装失败？

```bash
# 尝试使用 nvm 管理 Node.js 版本
nvm install 10
nvm use 10
npm install -g gitbook-cli
```

### Q2：中文 PDF 乱码？

```bash
# 确保安装了中文字体
# 使用 xelatex 引擎
# 指定中文字体
pandoc input.md -o output.pdf \
    --pdf-engine=xelatex \
    -V CJKmainfont="PingFang SC"
```

### Q3：如何更新内容？

```bash
# 1. 修改 Markdown 文件
# 2. 重新构建
gitbook build

# 3. 提交更新
git add .
git commit -m "Update content"
git push
```

---

## 下一步

1. 选择合适的方案
2. 按照步骤构建
3. 测试效果
4. 发布上线

**开始构建你的电子书！**
