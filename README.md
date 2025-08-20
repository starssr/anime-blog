# 动漫风格博客模板

一个专为动漫爱好者打造的个人博客模板，基于现代化的前端技术栈构建。它集成了 Live2D 角色动画和 particles.js 粒子背景效果，为用户提供沉浸式的动漫风格体验。这个项目为动漫文化爱好者提供了一个美观且实用的博客解决方案。

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Node Version](https://img.shields.io/badge/node-%3E%3D16.0.0-brightgreen.svg)
![Yarn Version](https://img.shields.io/badge/yarn-%3E%3D1.22.0-blue.svg)

## 🚀 部署

你可以通过一键点击快速部署到 EdgeOne Pages：

[![Deploy to EdgeOne](https://cdnstatic.tencentcs.com/edgeone/pages/deploy.svg)](https://console.cloud.tencent.com/edgeone/pages/new?template=anime-blog-demo1)

[更多模板](https://edgeone.ai/pages/templates)

## ✨ 特性

- 🎨 动漫风格 UI 与 Live2D 角色动画
- ✨ 动态粒子背景效果
- 📱 响应式设计，支持所有设备
- 📝 基于 Markdown 的内容管理
- 🎯 SEO 优化
- 💬 支持多种评论系统

## ⚠️ 重要说明

本项目仅供**私人部署**使用。如需商业用途，请参考以下项目的许可条款：
- [hexo-theme-redefine](https://github.com/EvanNotFound/hexo-theme-redefine)
- [Live2D](https://www.live2d.com/en/terms/)

## 🚀 快速开始

### 环境要求

- Node.js >= 16.0.0
- Yarn >= 1.22.0

### 安装步骤

1. 克隆项目
```bash
git clone https://github.com/your-username/anime-blog-template.git
cd anime-blog-template
```

2. 安装依赖
```bash
yarn install
```

3. 启动开发服务器
```bash
yarn dev
```

4. 构建生产版本
```bash
yarn build
```

## 📝 内容管理

### 文章编写

在 `source/_posts` 目录下创建 Markdown 文件，使用以下格式：

```markdown
---
title: 文章标题
date: 2024-03-21 12:00:00
tags:
  - 标签1
  - 标签2
categories:
  - 分类1
---

文章内容...
```

### 草稿管理

- 将未完成的文章放在 `source/_drafts` 目录
- 使用 `yarn draft` 命令预览草稿

## ⚙️ 配置说明

### 网站配置

在 `config/site.yml` 中配置：

```yaml
title: 我的动漫博客
subtitle: 分享动漫文化
description: 一个关于动漫文化的个人博客
keywords: 动漫,ACG,文化
author: Your Name
language: zh-CN
timezone: Asia/Shanghai
```

### 主题配置

在 `config/theme.yml` 中配置：

```yaml
# 主题颜色
theme_color: "#FF69B4"

# 导航菜单
menu:
  - name: 首页
    path: /
  - name: 归档
    path: /archives
  - name: 标签
    path: /tags
  - name: 关于
    path: /about
```

## 📦 项目结构

```
.
├── public/          # 构建输出目录
├── source/          # 博客源文件
│   ├── _posts/     # 博客文章
│   ├── _drafts/    # 草稿文章
│   └── assets/     # 静态资源
├── themes/         # 主题文件
└── config/         # 配置文件
```

## 🚀 部署

支持多种部署方式：

- **GitHub Pages**
  - 将 `公共` 目录内容推送到 `gh-pages` 分支
  - 在仓库设置中启用 GitHub Pages

- **Vercel**
  - 连接 Vercel 账号
  - 选择 `public` 作为输出目录
  - 自动部署

- **EdgeOne**
  - 上传静态文件到 EdgeOne
  - 配置 CDN 加速

## ❓ 常见问题

### 1. 如何修改主题样式？
主题样式文件位于 `themes/default/assets/css/` 目录下，可以直接修改相应的 CSS 文件。

### 2. 如何添加新的页面？
在 `source` 目录下创建对应的 Markdown 文件，并在 `config/theme.yml` 中的 `menu` 配置中添加相应的菜单项。

### 3. 如何添加评论系统？
在 `config/theme.yml` 中配置 Gitalk 或 Valine 等评论系统的参数。

## 🤝 贡献指南

欢迎提交 Pull Request 来改进这个项目。在提交之前，请确保：

1. 代码符合项目的编码规范
2. 添加必要的测试
3. 更新相关文档

## 📚 参考项目

本项目参考并使用了以下项目的资源：

- [hexo-theme-redefine](https://github.com/EvanNotFound/hexo-theme-redefine) - 一个功能丰富的 Hexo 主题
- [Live2D Widget](https://github.com/stevenjoezhang/live2d-widget) - 网页平台 Live2D 小部件

如果你喜欢这个项目，请考虑给这些优秀的项目点个 star！ 
