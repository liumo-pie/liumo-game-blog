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
## 一.静态网站生成器

基于各个语言的生成器都有，基于我记录博客的需求，我选中比较合适内容展示的Hugo  
### 安装和使用
1. [下载Hugo Extended版本](https://github.com/gohugoio/hugo/releases)无需搭建go环境，可直接使用
2. 解压后将hugo.exe的目录添加到系统的环境变量。
3. 打开vscode，使用终端（快捷键为ctrl+`）
4. 终端导航到自己目标位置 cd
5. 创建Hugo站点 hugo new site my-godot-learning-blog
6. cd进入项目目录
7. 选中自己想要的主题了，[Hugo主题网站](https://themes.gohugo.io/)，我选中了coder，为了后续可以对主题进一步的开发和利用。拉取分支之后指定自己的分支为子模块。
8. fork作者的主题 [hugo-coder](https://github.com/luizdepra/hugo-coder.git)
9. 添加这个子模块，初始化git仓库`git init`  
`git submodule add https://github.com/liumo-pie/hugo-coder.git themes/hugo_coder`
子模块的改动会直接影响到子模块原先的项目，一般需要提交两次，一次提交到原项目，一次提交到现在的项目。
10. 现在可以使用模板来快速的创建自己的博客了。
    
## 二.主题的使用
### 主题配置文件hugo.toml
这里可以配置网站的基础信息。例如网站url还有title等信息。具体的配置参数信息可以在[原主题](https://github.com/luizdepra/hugo-coder/blob/main/docs/configurations.md)找到指引。或者可以参考我的[配置思路](https://liumo-pie.github.io/liumo-game-blog/content/posts/hugo_toml_value).

### hugo的基本结构
- content:存放所有的博客文章
   -content和界面的互动设置。
    -hugo.toml  
    ```

    ```
- themes:安装的主题就在这里
- statis:静态资源
- archetypes：文章模板
- hugo.toml：主配置文件
- public：构建输出目录
  
在content目录下创建posts目录。新建md文件就能写自己的markdown。

### 上传git和自动化操作
1. 创建新仓库 Create a new repository
2. bash 
   ```git remote add origin https://github.com/liumo-pie/liumo-game-blog.git```
3. 实现自动化的页面部署。保证每次推送到main分支都直接部署到网站上并进行页面渲染。
   -
  

