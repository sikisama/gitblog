<!--
author: zhangxuefeng
date: 2016-06-07
title: Git使用指南
tags: Git
category:tool
status: publish
summary: 总结一些git常用的命令
-->
## 一.创建与添加
    1.git init //将一个目录初始化为git仓库
建立一个空的仓库，目录下会多一个.git的隐藏文件夹，用来跟踪管理版本库。
    
    2.git add //添加文件到缓存
将文件添加至缓存，多个文件用空格分隔

**git add .** 或 **git add  \*** 可以递归的添加当前目录的所有文件


    3.git commit -m 'comments' //记录缓存内容的快照
将（已缓存的/已add的）文件提交到仓库，生成快照

    4.git commit -a 
自动将在提交前已记录、修改的文件放入缓存区(跳过add)

    5.git clone [url] [new_name] //复制一个Git仓库
    eg. git clone https://github.com/sikisama/sikisama.github.io
    or
    git clone git://github.com/sikisama/sikisama.github.io


## 二、查看与修改

    1.git status / git status -s //查看工作目录与缓存状态

查看代码在缓存与当前工作目录的状态。带-s参数显示简短的结果，第一栏是缓存（add之后），第二栏是工作目录（add之前）。

用于查看上一次提交之后(commit)有什么被修改或临时提交的（add）。

    2.git diff  //尚未缓存的改动
显示上次提交快照（commit）之后尚未缓存(add)的所有修改。
> 执行完git status 再跑一下 git diff 是好习惯
    
    3.git diff --cached   //已缓存的改动
显示接下来将要写入快照的内容（after add before commit ）
    
    4.git diff HEAD
查看已缓存与未缓存的改动，也就是工作目录与上一次提交(commit)的更新区别，无视缓存。
    
    5.git diff --stat //显示摘要
显示摘要而非整个diff

    6.git reset HEAD -- file //已缓存与未缓存的改动
**取消**已经缓存（add）的内容，将缓存区恢复到做出修改之前的样子。
    
    //回退到之前的若干个版本(commit)
    git reset --hard HEAD^/HEAD~[n]
    git reset --hard [commit_id]
    

    7.git rm 
将文件从缓存区**移除**，如果要在目录中保留，使用
    
    git rm --cached
    
    
    8.git mv  //git rm --cached orig;mv orig new;git add new
        
## 三、远程仓库

>  使用 git fetch 更新你的项目，使用 git push 分享你的改动。 你可以用 git remote 管理你的远程仓库。

    1.git remote /git remote -v //罗列添加和删除远端仓库的别名
当你需要与远端仓库同步的时候，不需要使用它详细的链接。Git 储存了你感兴趣的远端仓库的链接的别名或者昵称。

    2.git remote add [alias] [url]//为项目添加一个远程仓库
    
    
    
    3.git remote rm [alias] //删除现存的某个别名
    
    
    4.git fetch //从远端下载新分支与数据
    
想要同步远端信息，首先 git fetch [alias] 告诉git去获取数据，然后执行 git merge [alias]/[branch]
    
    5.git pull //从远端仓库提取数据并尝试合并到当前分支,在git fetch 后 git merge 
    
    6.git push [alias] [branch]//推送你的新分支到某个远端仓库
会将你的[branch]分支推送成[alias]远端上的[branch]分支。
> git push [alias] [branch] 将你的本地改动推送到远端仓库。 如果可以的话，它会依据你的 [branch] 的样子，推送到远端的 [branch] 去。 如果在你上次提取、合并之后，另有人推送了，Git 服务器会拒绝你的推送，知道你是最新的为止。

**修正这个问题:执行 git fetch github; git merge github/master，然后再推送。**
    
    

## 四、分支与合并

> 执行 git branch [branchname] 来创建分支，用 git checkout [branchname] 命令切换到该分支。当切换分支的时候，git会用该分支最后提交的快照来替换工作目录的内容。使用 git merge 来合并分支。

    1.git branch //列出、创建与管理工作上下文
    
    2.git checkout [branchname] //切换到新的分支上下文
    
    3.git branch [branchname] //切换到新的分支上下文
git将还原你的工作目录到你创建分支时的样子——可以把它看做一个记录你当前进度的书签。    

    4.git checkout -b [branchname] //创建新分支并立即切换到它
    
    5.git branch -d [branchname] //删除分支
    
    6.git merge [branchname] //将分支合并到当前分支
    

**合并冲突**
不同分支修改了相同区块的代码，就会出现冲突。使用git diff 可以显示冲突的双方和冲突解决的方法。使用git add告诉git文件中的冲突已经解决。

    7.git log //显示一个分支中提交的更改记录
    
    8.git log --online //查看紧凑简介的版本，--graph 选项查看历史中出现的分支和合并
    
    9.git log --online master ^dev //在不想看到的分支前放一个 ^
    
    10.git tag //给历史记录中某个重要的一点打上标签
    
    git tag -a [v] [sha]
    

参考资料

- [git参考手册](http://codeigniter.org.cn/)
- [git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
    