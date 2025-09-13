+++ 
draft = false
date = 2025-09-10T16:09:23+08:00
title = "学习用godot制作视觉小说"
description = "仿逆转裁判"
slug = ""
authors = ["liumo"]
tags = []
categories = []
externalLink = ""
series = []
+++
# 用godot制作视觉小说
## 补充知识
1. 设置git的代理。使用v2rayN。
 ```git config --global http.proxy 'socks5://127.0.0.1:7890'```

### 前提情要
- 如何在知道目标效果的情况下反推实现
1. 需要具备拆分思想，例如一个简单的角色控制器
- 移动效果->CharacterBody2D
- 动画效果->AnimationPlayer or AnimatedSprite  
- 碰撞检测 ->CollisionShape2D
- 特效->GPUParticles2D
- AudioStreamPlayer
1. 回归到界面，也是拆分背景和角色等。由内到外层的进行拆分 （以逆转裁判为例） 
- 背景
- 角色-有动画效果  
1. 需要有AnimatedSprite节点
2. 需要固定在界面底部-control组件  
- 固定显示-ui-control
1. 如果是ui组件自己就有这个属性  
2. 如果是非ui节点就需要在上面添加一个control的父节点 
- 对话系统-label button  
1. 不知道panel和panelcontainer是什么差别什么时候选什么
2. 主题的复用和主题的覆盖  
3. 插件dialogue的使用  
   - 安装插件
   - 新建文件夹
   - 在角色脚本里面导入使用
   - 可操作性场景的添加（将dialogue添加为export）
   - 用tool保存为场景，在场景里面进行目标效果的交互
   - Q:我现在想要用dialogue manager插件实现换人物显示的效果，我该怎么传递我的对话文本里面的讲话的人名，我是把角色场景加到balloon这个场景里面，还是把角色场景加到主场景里面，加到ballon里面很方便调用，可以用onready，如果加到主场景里面，对话框和角色节点都作为主场景的子节点，要怎么把对话框角色人名信息传给角色节点更改动画
   - _unhandled_input和_input是什么关系
- 主题一般都放在主节点里面，可以有很多类型的主题，后续的主题要修改的时候只要覆盖就好了
1. 富文本  
2. polygon多边形绘制
- 存储系统  
- 对话界面显示-ui

1. 更改屏幕大小对应缩放背景  
   -使用节点canvaslayer
   -把texturerect的锚点改成全屏就可以了
2. 更改节点大小对应缩放背景