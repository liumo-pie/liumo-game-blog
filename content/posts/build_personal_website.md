+++
draft = false
date = 2025-08-26T14:28:29+08:00
title = "构建个人博客网站全流程回顾"
description = "个人博客搭建"
slug = ""
authors = ["liumo"]
tags = ["webbsite","blog"]
categories = ["summary"]
externalLink = ""
series = []
+++
# 个人网站搭建实用指南
## 静态网站生成器

基于各个语言的生成器都有，基于我记录博客的需求，我选中比较合适内容展示的Hugo  
### 安装和使用
1. [下载Hugo Extended版本](https://github.com/gohugoio/hugo/releases)无需搭建go环境，可直接使用
2. 解压后将hugo.exe的目录添加到系统的环境变量。
3. 打开vscode，使用终端（快捷键为ctrl+`）
4. 终端导航到自己目标位置 cd
5. 创建Hugo站点 hugo new site my-godot-learning-blog
6. cd进入项目目录
7. 选中自己想要的主题了，[Hugo主题网站](https://themes.gohugo.io/)，我选中了coder，为了后续可以对主题进一步的开发和利用。拉取分支之后指定自己的分支为子模块。