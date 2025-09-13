+++ 
draft = false
date = 2025-09-13T07:50:52+08:00
title = "git基础使用"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
# git的基础命令
## 使用代理的链接方式
1. 使用的是v2aryN，代理命令为  (有人说根本没有https，之后看下)
   `git config --global http.proxy 'socks5://127.0.0.1:10808`  
2. 使用的是clash，代理命令为
   `git config --global http.proxy 'http://127.0.0.1:7890'`  
3. 使用系统设置（还没试过）
   `set https_proxy=127.0.0.1:10808`  
4. 清除系统设置  
   `git config --global unset http.proxy`  

## 基本项目运转流程
万能的命令 git <指令> -h  
可以查看各种命令的基础参数和用法
一个-为短命令，也就是简写，--为长命令，也就是全名，[-no]这种为可选内容，--[no-]option：代表一对开关：--option 和 --no-option，<占位符>一般是参数信息，在help文档里面的占位符里面的显示会告诉你是要传什么类型的参数进去
### 起始点为本地有项目  
1. 初始化本地仓库
    `git init`
2. 把代码提交到暂存区
    `git add .`
3. 提交到本地仓库，并添加提交信息
    `git commit -m "提交信息"`
4. 关联远程仓库地址(需要事先在github上面进行创建)
    `git remote add origin <远程仓库的url>`
5. 现在是切换到了远程仓库的分支里面进行操作了
    `git add .`
6. 提交到远程仓库
    `git commit -m "提交信息"`
7. 推送到远程仓库
    `git push -u origin main`
### 本地拉分支进行修改
1. 查看本地所有分支，和上面一样可以在这个分支上面进行暂存和提交
   `git branch`
2. 创建并切换分支
   `git checkout -b <新分支名称>`
3. 创建远程分支并建立跟踪关系
   `git push -u origin <远程分支名称>`
4. 后续只需要进行push和pull就可以了
   
### 对比和学习

好的，完全理解！命令行虽然强大，但看代码差异确实不直观。下面我为你总结完整流程，并重点介绍如何**用命令行调用直观的图形化工具**来查看代码，两者结合才是最佳方案。

### 一、核心流程与命令总结（命令行）

首先，这是查看更改和历史的核心流程，记住这个顺序：

| 你的目的 | 核心命令 | 补充命令 |
| :--- | :--- | :--- |
| **1. 我想知道发生了什么**（有哪些提交？） | `git log --oneline --graph --all` | `git shortlog -sn` |
| **2. 我想看某次提交改了啥**（具体代码？） | `git show <commit-hash>` | `git log -p -1 <commit-hash>` |
| **3. 我想比较两个状态**（工作区/暂存区/分支） | `git diff` | `git diff --staged` `git diff main..feature` |
| **4. 我想追踪一个文件的历史** | `git log -p -- <file-path>` | `git blame <file-path>` |

---

### 二、如何变得直观：命令行启动图形化工具

你不需要离开命令行，只需要输入几个命令就能召唤出非常直观的图形界面。

#### 方案一：使用 Git 内置的图形化工具 —— `gitk`

这是最简单、最直接的方法，**所有版本Git都自带**。

```bash
# 1. 查看当前仓库的完整历史（最常用）
gitk --all

# 2. 查看某个文件的历史
gitk --all path/to/your/file.cs

# 3. 查看最近100次提交
gitk -n 100
```
**`gitk` 界面直观在哪？**
*   **上半部分**：清晰的提交历史树状图，分支合并一目了然。
*   **下半部分**：点击任意提交，立刻显示该提交的详细信息：作者、时间、注释。
*   **右下部分**：**高亮显示代码差异**，红色是删除的，绿色是新增的，看得清清楚楚。



#### 方案二：使用 `git difftool` 和 `git mergetool`

这些命令允许你用自己喜欢的图形化对比工具（如VS Code）来查看差异和解决冲突。

**1. 首先，设置你喜欢的工具（以 VS Code 为例）：**
```bash
# 设置默认的diff工具为vscode
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'

# 设置默认的merge工具为vscode
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

**2. 然后，就可以愉快地使用了：**
```bash
# 用vscode查看工作区和暂存区的差异（代替 git diff）
git difftool

# 用vscode查看暂存区和上次提交的差异（代替 git diff --staged）
git difftool --staged

# 用vscode比较两个分支（代替 git diff main..feature）
git difftool main..feature

# 用vscode解决合并冲突（超级好用！）
git mergetool
```
这样会用VS Code打开对比视图，左右分栏，颜色高亮，非常直观。

#### 方案三：配置更直观的 `git log`（终端内优化）

如果你就是想呆在终端里，也可以让输出更直观：

```bash
# 创建一个强大的git log别名，一次搞定
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"

# 以后只需要输入以下命令，就能看到彩色的、带分支线的、包含相对时间的日志
git lg
```


---

### 三、终极推荐工作流（命令行 + 图形化）

结合两者，你的高效工作流应该是这样的：

1.  **在命令行里快速定位**：
    ```bash
    # 快速浏览一下提交历史，找到我感兴趣的那个提交的哈希值（比如 a1b2c3d）
    git log --oneline --graph -10
    ```

2.  **用图形化工具深入查看**：
    ```bash
    # 详细查看这个提交的具体改动，用图形界面看代码差异
    git show a1b2c3d
    # 或者直接用gitk查看整个历史
    gitk --all
    ```

3.  **解决冲突或对比分支时**：
    ```bash
    # 直接召唤VS Code来进行直观的对比和合并
    git difftool main..my-branch
    git mergetool
    ```

### 总结

| 场景 | 推荐命令 | 优点 |
| :--- | :--- | :--- |
| **快速浏览历史** | `git lg` (自定义别名) 或 `git log --oneline --graph` | 速度快，不离终端 |
| **详细分析提交** | `gitk --all` | **最直观**，历史树+代码diff一目了然 |
| **对比代码差异** | `git difftool` (需配置VS Code) | 用你最熟悉的编辑器高亮对比 |
| **解决合并冲突** | `git mergetool` (需配置VS Code) | **救命神器**，图形化解决冲突不再头疼 |

**给你的建议：**
现在就试试 `gitk --all`，它是让你直观理解项目历史的最佳入门工具。之后再去配置 `git difftool` 和 `git mergetool` 到VS Code，这将极大提升你的开发体验。