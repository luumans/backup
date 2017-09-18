title: Git速查手册
date: 2017-08-19 14:11:20
description: 
categories:
- Tool
tags:
- Git
toc: true
author:
comments:
original:
permalink: 
---
# 安装
## Linux
打开控制台，然后通过包管理安装，在Ubuntu上命令是：
```
sudo apt-get install git-all
```

## Windows
推荐使用git for
windows，它包括了图形工具以及命令行模拟器。

## OS X
最简单的方式是使用homebrew安装，命令行执行
```
brew install git
```
如果你是在是先用图形工具的话，那么推荐你使用Github desktop,Sourcetree。但我还是推荐你使用命令行，下面的内容就都是命令行的。
<!-- more -->

- []( "")
- []( "")
- []( "")
- []( "")
- []( "")

# Git 术语
| 术语 | 定义 |
| -----|-----|
| 仓库（Repository）| 一个仓库包括了所有的版本信息、所有的分支和标记信息。在Git中仓库的每份拷贝都是完整的。仓库让你可以从中取得你的工作副本。 |
| 分支（Branches）| 一个分支意味着一个独立的、拥有自己历史信息的代码线（code line）。你可以从已有的代码中生成一个新的分支，这个分支与剩余的分支完全独立。默认的分支往往是叫master。用户可以选择一个分支，选择一个分支执行命令git checkout branch. |
| 标记（Tags）| 一个标记指的是某个分支某个特定时间点的状态。通过标记，可以很方便的切换到标记时的状态，例如2009年1月25号在testing分支上的代码状态 |
| 提交（Commit）| 提交代码后，仓库会创建一个新的版本。这个版本可以在后续被重新获得。每次提交都包括作者和提交者，作者和提交者可以是不同的人 |
| 修订（Revision）| 用来表示代码的一个版本状态。Git通过用SHA1 hash算法表示的id来标识不同的版本。每一个 SHA1 id都是160位长，16进制标识的字符串.。最新的版本可以通过HEAD来获取。之前的版本可以通过"HEAD~1"来获取，以此类推。 |

# 创建
## 新建仓库
```
<!-- 在当前目录新建一个Git代码库 -->
$ git init

<!-- 新建一个目录，将其初始化为Git代码库 -->
$ git init [project-name]
```

## 复制远程仓库
```
<!-- 下载一个项目和它的整个代码历史 -->
$ git clone [url]
```

# 配置
## 配置账号信息
Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。
```
<!-- 设置提交代码时的用户信息 -->
$ git config --global user.name "My Name"
$ git config --global user.email myEmail@example.com

<!-- 显示仓库的Git配置 -->
$ git config --list

<!-- 编辑Git配置文件 -->
$ git config -e [--global]
```
配置好这两项，用户就能知道谁做了什么，并且一切都更有组织性了不是吗？

### 生成SSH秘钥
用于上传到你对应的github账号
```
$ ssh-keygen -t rsa -C "mail@gmail.com"
<!-- 这的密码不是我们GitHub的密码，而是Git SSH的密码 -->

<!-- 打开Git生成的密码文件，将其复制到GitHub上 -->
$ vim ~/.ssh/id_rsa.pub

<!-- 验证GitHub SSH是否成功 -->
$ ssh -T git@github.com
```

# 案例
## 提交流程

![提交流程](http://img.blog.csdn.net/20161219162011600?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDM0NjMwMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```
$ git pull
$ git start
$ git add
$ git commit -m ""
$ git push
```

# 修改与提交
## 修改
```
<!-- 添加指定文件到暂存区 -->
$ git add [file1] [file2] ...

<!-- 添加指定目录到暂存区，包括子目录 -->
$ git add [dir]

<!-- 添加当前目录的所有文件到暂存区 -->
$ git add .

<!-- 添加每个变化前，都会要求确认 -->
<!-- 对于同一个文件的多处变化，可以实现分次提交 -->
$ git add -p

<!-- 删除工作区文件，并且将这次删除放入暂存区 -->
$ git rm [file1] [file2] 

<!-- 停止追踪指定文件，但该文件会保留在工作区 -->
$ git rm --cached [file]

<!-- 改名文件，并且将这个改名放入暂存区 -->
$ git mv [file-original] [file-renamed]
```

## 提交
```
<!-- 提交暂存区到仓库区 -->
$ git commit -m [message]

<!-- 提交暂存区的指定文件到仓库区 -->
$ git commit [file1] [file2] ... -m [message]

<!-- 提交工作区自上次commit之后的变化，直接到仓库区 -->
$ git commit -a

<!-- 提交时显示所有diff信息 -->
$ git commit -v

<!-- 使用一次新的commit，替代上一次提交 -->
<!-- 如果代码没有任何新变化，则用来改写上一次commit的提交信息 -->
$ git commit --amend -m [message]

<!-- 重做上一次commit，并包括指定文件的新变化 -->
$ git commit --amend [file1] [file2] 
```

# 分支与标签
## 分支
```
<!-- 列出所有本地分支 -->
$ git branch

<!-- 列出所有远程分支 -->
$ git branch -r

<!-- 列出所有本地分支和远程分支 -->
$ git branch -a

<!-- 新建一个分支，但依然停留在当前分支 -->
$ git branch [branch-name]

<!-- 新建一个分支，并切换到该分支 -->
$ git checkout -b [branch]

<!-- 新建一个分支，指向指定commit -->
$ git branch [branch] [commit]

<!-- 新建一个分支，与指定的远程分支建立追踪关系 -->
$ git branch --track [branch] [remote-branch]

<!-- 切换到指定分支，并更新工作区 -->
$ git checkout [branch-name]

<!-- 切换到上一个分支 -->
$ git checkout -

<!-- 建立追踪关系，在现有分支与指定的远程分支之间 -->
$ git branch --set-upstream [branch] [remote-branch]

<!-- 合并指定分支到当前分支 -->
$ git merge [branch]

<!-- 选择一个commit，合并进当前分支 -->
$ git cherry-pick [commit]

<!-- 删除分支 -->
$ git branch -d [branch-name]

<!-- 删除远程分支 -->
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

## 标签
```
<!-- 列出所有tag -->
$ git tag

<!-- 新建一个tag在当前commit -->
$ git tag [tag]

<!-- 新建一个tag在指定commit -->
$ git tag [tag] [commit]

<!-- 删除本地tag -->
$ git tag -d [tag]

<!-- 删除远程tag -->
$ git push origin :refs/tags/[tagName]

<!-- 查看tag信息 -->
$ git show [tag]

<!-- 提交指定tag -->
$ git push [remote] [tag]

<!-- 提交所有tag -->
$ git push [remote] --tags

<!-- 新建一个分支，指向某个tag -->
$ git checkout -b [branch] [tag]
```



# 查看信息
```
<!-- 显示有变更的文件 -->
$ git status

<!-- 显示当前分支的版本历史 -->
$ git log

<!-- 显示commit历史，以及每次commit发生变更的文件 -->
$ git log --stat

<!-- 搜索提交历史，根据关键词 -->
$ git log -S [keyword]

<!-- 显示某个commit之后的所有变动，每个commit占据一行 -->
$ git log [tag] HEAD --pretty=format:%s

<!-- 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件 -->
$ git log [tag] HEAD --grep feature

<!-- 显示某个文件的版本历史，包括文件改名 -->
$ git log --follow [file]
$ git whatchanged [file]

<!-- 显示指定文件相关的每一次diff -->
$ git log -p [file]

<!-- 显示1行日志 -n为n行  -->
$ git log -5

<!-- 显示过去5次提交 -->
$ git log -5 --pretty --oneline

<!-- 显示所有提交过的用户，按提交次数排序 -->
$ git shortlog -sn

<!-- 显示指定文件是什么人在什么时间修改过 -->
$ git blame [file]

<!-- 显示暂存区和工作区的差异 -->
$ git diff

<!-- 显示暂存区和上一个commit的差异 -->
$ git diff --cached [file]

<!-- 显示工作区与当前分支最新commit之间的差异 -->
$ git diff HEAD

<!-- 显示两次提交之间的差异 -->
$ git diff [first-branch]...[second-branch]

<!-- 显示今天你写了多少行代码 -->
$ git diff --shortstat "@{0 day ago}"

<!-- 显示某次提交的元数据和内容变化 -->
$ git show [commit]

<!-- 显示某次提交发生变化的文件 -->
$ git show --name-only [commit]

<!-- 显示某次提交时，某个文件的内容 -->
$ git show [commit]:[filename]

<!-- 显示当前分支的最近几次提交 -->
$ git reflog
```



# 远程同步
```
<!-- 下载远程仓库的所有变动 -->
$ git fetch [remote]

<!-- 显示所有远程仓库 -->
$ git remote -v

<!-- 显示某个远程仓库的信息 -->
$ git remote show [remote]

<!-- 增加一个新的远程仓库，并命名 -->
$ git remote add [shortname] [url]

git pull <远程主机名(origin)> <远程分支名>:<本地分支名>

<!-- 取回远程仓库的变化，并与本地分支合并 -->
$ git pull [remote] [branch]

<!-- 上传本地指定分支到远程仓库 -->
$ git push [remote] [branch]

<!-- 强行推送当前分支到远程仓库，即使有冲突 -->
$ git push [remote] --force

<!-- 推送所有分支到远程仓库 -->
$ git push [remote] --all
```

# 撤销
```
<!-- 恢复暂存区的指定文件到工作区 -->
$ git checkout [file]

<!-- 恢复某个commit的指定文件到暂存区和工作区 -->
$ git checkout [commit] [file]

<!-- 恢复暂存区的所有文件到工作区 -->
$ git checkout .

<!-- 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变 -->
$ git reset [file]

<!-- 重置暂存区与工作区，与上一次commit保持一致 -->
$ git reset --hard

<!-- 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变 -->
$ git reset [commit]

<!-- 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致 -->
$ git reset --hard [commit]

<!-- 重置当前HEAD为指定commit，但保持暂存区和工作区不变 -->
$ git reset --keep [commit]

<!-- 新建一个commit，用来撤销指定commit -->
<!-- 后者的所有变化都将被前者抵消，并且应用到当前分支 -->
$ git revert [commit]

<!-- 暂时将未提交的变化移除，稍后再移入 -->
$ git stash
$ git stash pop
```

# 其他
```
<!-- 生成一个可供发布的压缩包 -->
$ git archive
```


# 拓展
- [my-git](https://github.com/xirong/my-git "有关 git 的学习资料")
- [廖雪峰Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 "讲解过于复杂，而且还有很多广告")
- [Git-it - GitHub](http://jlord.us/git-it/ "一位女员工写的 Git 教程")
- [Learn Git Branching](http://learngitbranching.js.org/?demo "是一个git仿真沙盒")
- [github快速入门](http://www.jianshu.com/p/da9bc509b1d2)
- [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html "")
- [用 Git 钩子进行简单自动部署](https://aotu.io/notes/2017/04/10/githooks/ "")
- [git-recipes](https://github.com/geeeeeeeeek/git-recipes/wiki "")
- [专为设计师而写的GitHub快速入门教程](http://www.ui.cn/detail/20957.html "")
- []( "")
- []( "")
- []( "")

## 工作流
- [Git 工作流](https://juejin.im/search?query=Git%20%E5%B7%A5%E4%BD%9C%E6%B5%81 "")
- [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows "")
- [Git工作流指南：Gitflow工作流](http://blog.jobbole.com/76867/#comment-156726 "")
- [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/ "")
- [改进合作 Git 工作流：自动提取、合并提交](https://tech.meituan.com/improving-git-flow_squashing-commits.html "")


## 速查表
- [Git索引](http://backlogtool.com/git-guide/cn/reference/ "")
- [Git指令速查表](https://www.git-tower.com/blog/git-cheat-sheet-cn "")

## 图解
- [猴子都能懂的Git入门](http://backlogtool.com/git-guide/cn/ "")
- [图解Git](http://marklodato.github.io/visual-git-guide/index-zh-cn.html#conventions "")




