+++ 
draft = true
date = 2025-09-14T18:41:41+08:00
title = "尝试用mcp接入视觉小说"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
# 本地部署mcp
mcp提供了pc本地接入了模型的效果，使得模型可以访问本地文件，操作本地文件，可以构建本地的自主操作。
## 拉取模型部署到本地
1. 安装ollama，[download链接](https://ollama.com/download/windows)
2. 到[hugging face](https://huggingface.co/models)上面查找目标模型
3. 拉取模型并且部署到本地,这个部分不是在ollama里面进行的，是在命令行里面进行的，这一步就实现了模型的本地部署，可以在命令行里面调用自己本地的模型进行聊天
   `ollama pull <模型名称>` `ollama run <模型名称>`
4. 安装mcp的服务端。
   - mcp的服务端需要通过pip下载，首先要安装[Python](https://www.python.org/downloads/)
   - 最好在虚拟环境进行执行，安装虚拟环境工具`pip3 install virtualenv`
   - 创建虚拟环境`python3 -m venv mcp-venv`
   - 激活该虚拟环境`.\mcp-venv\Scripts\activate`,linux和windows有所不同。
     - ![激活后的界面](content/posts/website/image_import_ai/0DF132FD6EF89B2847BC9B5516AF9C16.png)
  