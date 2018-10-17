---
title: git命令记录
date: 2018-09-05 13:14:32
tags: git 
categoties: 技术
description: git入门命令
---
git入门命令
<!--more-->

什么是Git，GitHub？

&nbsp;&nbsp;&nbsp;&nbsp;纯属个人理解，如有雷到，见谅见谅。。。。  
&nbsp;&nbsp;&nbsp;&nbsp;说的通俗一点，GitHub就是一个来存放**文本文件**的地方（类比网盘），它是远程服务器我们的电脑是客户端。两个地方是不是的同步一下数据来保证我们两个地方保存的东西都一样。既然如此我们就直接使用平时的网盘来保存我们的代码何必使用git或者github呢。因为它有版本控制功能，它记录了我们对**文本文件**的操作流水账，比如：加了哪些东西，在那些个文件中加的，在哪里做了修改等等，我们可以把不同的操作结果叫做不同的版本，每个版本有自己的版本号，我们可以在版本号列表中选择恢复到任何记录在案的版本。

刚刚下载安装git后需要设置连接用户：
``` git
git config --global user.name
git config --golbal user.email
```
创建版本库，即使之可以被版本管理：

    git init   //初始化，git可管理

往简单说只需三步你就可以将本地库同步到远程：  
```
1. git add   //此命令表示将更改提交到暂存区
2. git commit -m "此次提交的说明性文字"  //表示将更改提交到仓库
3. git push //此命令为省略，只是来表示为push命令，新手切勿直接使用，将本地提交到远程
```
到此会有疑问，什么暂存区什么仓库，如下说明：   
**工作区**：就是你在电脑上看到的目录。  
**版本库**：就是该目录下的 **.git**目录，在不懂的情况下不要随意改动。  
**暂存区**： 暂存区存放与版本库中，即在 **.git**目录中。  
*补充*：**.git** 中也存放着默认创建的master分支，和指向master的指针HEAD。

**git status**------查看有什么被修改：  
用来显示工作目录和暂存区的状态，不显示已经提交(git commit)到历史中的信息

    git status  //闲着没事就 git status 一下  


你会看到有三类（个人理解）：  
1.Changes to be committed  **:**  所有的都准备好了，可以被 git commit 了。  
2.Changes not staged for commit:  你修改过文件，需要git add一下。  
&ensp;&ensp;&ensp;&ensp;&ensp;就像是：喂，我知道你做过修改，过来给我报备一下，**我要记录在案**  
3.Untracked files: 表示这些文件好没有被版本库记录，如果要记录，你需要git add一下。

**git diff + 文件名**   
查看该文件做了什么修改。 本人没用过，不太会。。。

版本管理命令：  
```
git log  //按时间的由近到远的显示提交记录

git reset -hard HEAD^  //恢复到上一个版本

git reset -hard HEAD~N //恢复到上N个版本

git reflog //显示每次提交的版本号（为一串数字，唯一存在）
```

查看你的本地仓库与远程库的关系：  
```
git remote
git remote -vv
```
查看本地分支的追踪关系：  
```
git branch -vv //查看本地分支与哪些分支存在追踪关系
```
创建并切换分支：  
`git checkout -b 新分支的名字 //代表创建分支并切换到该分支`  
该命令相当于  
`git branch 新分支名字`
`git checkout 新分支名字`

与分支有关的命令：
-  查看有哪些分支： git branch
-  删除分支：git branch -d 分支名

合并分支：  
`git merge 分支 //将指定分支合并到当前分支，一般搭配--no-ff 参数来使用 结果是禁用fast forward`   
`还有一点就是会保存下被删除分支的版本号信息，方便来恢复`



















参考地址：<https://www.yiibai.com/git/>