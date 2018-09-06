---
title: git命令记录
date: 2018-09-05 13:14:32
tags: git 
categoties: 技术
description: git入门命令
---
git入门命令
<!--more-->

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
























参考地址：<https://www.yiibai.com/git/>