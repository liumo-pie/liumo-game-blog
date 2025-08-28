+++ 
draft = false
date = 2025-08-19T14:28:29+08:00
title = "上手用markdown记录godot项目学习流程"
description = "基本的markdown语法学习"
slug = ""
authors = ["liumo"]
tags = ["godot","game"]
categories = ["summary"]
externalLink = ""
series = []
+++
# 进行markdown的基本语法学习
## 标题 段落 换行 强调

标题的对应标志是#号，几级标题对应几个#号。想要分段就直接的用空行进行分段，这样就会自动的分段了。

这样就是第二段。在一句话里面我想要进行强制换行，只需要  
打两个空格并且点击enter就能实现强制换行了，但是我不懂空格的作用是什么所以我现在是
直接打了一个enter来进行比对。还是需要打两个空格并且enter

我又进行了一次分段，这次想要实现的是强调效果。强调有三个等级，分别是斜体，粗体还有粗斜体。强调的对应标志是*，有几个*号就是几级。_斜体_还有**粗体** 还有最后的 ***粗斜体***。这个检测的有点奇怪，等下在网站上看下效果。

## 有序、无序列表 链接（行内&引用）图片
1. 苹果
2. 香蕉
3. 梨
数字加点号（有序列表）
- 项目1
- 项目2
    - 子项目（空格或者tab）
- 项目三

行内式，这是一个[链接的显示名称](https://www.example.com)括号里面显示的是链接地址。
这是一个[引用式链接][1]和另一个[引用式链接][Github]。

[1]:https://www.example.com
[Github]:https://www.github.com 

图片的引用各式和链接几乎一样，只是前面加了一个！
！[图片名称](https://example.com/cat.jpg )

***
## 代码 引用 表格 任务列表
`printf()`
```python
def hello_world():
    print("hello,world")
```

>大于号表示引用
>>不知道拿来干什么的

表格还不会
| ui | 人物 | 场景 |
| :- | :-:| -: |
| 靠左 | 居中 | 靠右 |


任务列表
- [x] 学会markdown的基本用法
- [] 学会godot的基本用法