---
title: Hexo博客搭建指南
categories:
- 指北
---

# Hexo博客搭建指南

我的搭建Hexo博客之路
<!-- more -->

## 安装Node.js

Node.js下载地址：[Download | Node.js (nodejs.org)](https://nodejs.org/en/download/)

下载后进行安装，安装后检测是否安装完成：

`node -v`

node.js会同时安装npm，检测：

`npm -v`

## 安装Hexo

使用npm命令安装Hexo：

`npm install -g hexo-cli`

## 初始化博客

`hexo init blog`

也可以直接从已有的github仓库拉取代码

## 配置博客

_config.yml文件，为主题配置文件

如下为我的配置

```yaml
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: Jettisons' Blog
subtitle: 'Hello,world'
description: 'Jettison'
keywords:
author: Jettison
avatar: /0.jpg
language: en
timezone: 'Asia/Shanghai'

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yoursite.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: false
  preprocess: true
  line_number: true
  tab_replace: ''

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## updated_option supports 'mtime', 'date', 'empty'
updated_option: 'mtime'

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: 'git'
  repo: https://@github.com/Jettisons/Jettisons.github.io.git
  branch: master

```

配置文件编写完成后

安装Git部署插件

`npm install hexo-deployer-git --save`

## 网站上线

`hexo clean`

`hexo g -d`

完成后便可以输入https://jettisons.github.io/登录自己的博客

## 编写文章

文章放在source目录下的_post文件夹中，为md格式的文件