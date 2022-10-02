---
title: Hexo
urlname: label
date: 2021-05-09 12:06:08
update: 2021-05-09 12:06:08
tags:
  - Hexo
categories: Hexo
---

# Hexo

## 1. 概述

快速、简洁且高效的博客框架。

Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

### 1.1. 特点

- 超快速度
  `Node.js` 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。
- 支持 `Markdown`
  Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。
- 一键部署
  只需一条指令即可部署到 GitHub Pages, Heroku 或其他平台。
- 插件和可扩展性
  强大的 API 带来无限的可能，与数种模板引擎（EJS，Pug，Nunjucks）和工具（Babel，PostCSS，Less/Sass）轻易集成

```nodejs
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```

### 1.2. 安装

#### 1.2.1. 安装前提

- Node.js (Node.js 版本需不低于 10.13，建议使用 Node.js 12.0 及以上版本)
- Git

#### 1.2.2. 安装 Hexo

使用 npm 安装 Hexo：

```nodejs
npm install -g hexo-cli
```

## 2. 建站

### 2.1. 初始化

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件：

```nodejs
hexo init <folder>
cd <folder>
npm install
```

新建完成后，指定文件夹的目录如下：

```bash
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

- \_config.yml
  网站的配置信息，您可以在此配置大部分的参数。
- package.json
  应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。
- scaffolds
  - 模版文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。
  - Hexo 的模板是指在新建的文章文件中默认填充的内容。
- source
  - 资源文件夹是存放用户资源的地方。
  - 除 `_posts` 文件夹之外，开头命名为 \_ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。
  - Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。
- themes
  主题文件夹，Hexo 会根据主题来生成静态页面。

## 3. 配置

### 3.1. 网站

| 参数        | 描述                           |
| ----------- | ------------------------------ |
| title       | 网站标题                       |
| subtitle    | 网站副标题                     |
| description | 网站描述                       |
| keywords    | 网站的关键词。支持多个关键词。 |
| author      | 您的名字                       |
| language    | 网站使用的语言                 |
| timezone    | 网站时区                       |

### 3.2. 网址

| 参数                       | 描述                                                                             | 默认值                    |
| -------------------------- | -------------------------------------------------------------------------------- | ------------------------- |
| url                        | 网址, 必须以 http:// 或 https:// 开头                                            |                           |
| root                       | 网站根目录                                                                       | url's pathname            |
| permalink                  | 文章的 永久链接 格式                                                             | :year/:month/:day/:title/ |
| permalink_defaults         | 永久链接中各部分的默认值                                                         |                           |
| pretty_urls                | 改写 permalink 的值来美化 URL                                                    |                           |
| pretty_urls.trailing_index | 是否在永久链接中保留尾部的 index.html，设置为 false 时去除                       | true                      |
| pretty_urls.trailing_html  | 是否在永久链接中保留尾部的 .html, 设置为 false 时去除 (对尾部的 index.html 无效) | true                      |

#### 3.2.1. 网站存放在子目录

如果您的网站存放在子目录中，例如 `http://example.com/blog`，则请将您的 url 设为 `http://example.com/blog `并把 root 设为 `/blog/`。

#### 3.2.2. pretty_urls

```yml
# 比如，一个页面的永久链接是 http://example.com/foo/bar/index.html
pretty_urls:
  trailing_index: false
# 此时页面的永久链接会变为 http://example.com/foo/bar/
```
