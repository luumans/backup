title: Git Workflows
date: 2017-08-29 14:11:20
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
# 概要
随着团队不断的壮大，业务流程迭代。代码工作流的规范是显而易见的。为了保证开发速度，我们不断改进完善这个发布流程，让这个过程更简单、高效。
这篇指南以大家在SVN中已经广为熟悉使用的集中式工作流作为起点，循序渐进地演进到其它高效的分布式工作流，还介绍了如何配合使用便利的Pull Request功能，体系地讲解了各种工作流的应用。
在阅读过程中，请记住这些工作流是指导原则，而不是具体规则。我们想向您展示什么是可能的，因此您可以混合和匹配来自不同工作流的方面，以满足您的个人需求。

常见问题：
1. 我们以使用SVN的工作流来使用Git有什么不妥？
1. 如何控制开发版本？
1. Git方便的branch在哪里，团队多人如何协作？冲突了怎么办？如何进行发布控制？
1. 经典的master-发布、develop-主开发、hotfix-bug修复如何避免代码不经过验证上线？
1. 如何在GitHub上面与他人一起协作，star-fork-pull request是怎样的流程？
<!-- more -->



# 集中式工作流

集中式工作流以中央仓库作为项目所有修改的单点实体。相比 SVN 缺省的开发分支 trunk ，Git 叫做master，所有修改提交到这个分支上。本工作流只用到 master 这一个分支。
![集中式工作流](http://wiki.jikexueyuan.com/project/git-workflow-tutorial/images/git-workflow-svn.png)

## 工作方式
要发布修改到正式项目中，开发者要把本地 master 分支的修改『推』到中央仓库中。这相当于 svn commit 操作，但 push 操作会把所有还不在中央仓库的本地提交都推上去。


![push](http://wiki.jikexueyuan.com/project/git-workflow-tutorial/images/git-workflow-svn-push-local.png)

# Gitflow工作流
<!-- ![Git Workflows](http://nvie.com/img/git-model@2x.png) -->
Gitflow工作流通过为功能开发、发布准备和维护分配独立的分支，让发布迭代过程更流畅。严格的分支模型也为大型项目提供了一些非常必要的结构。
<!-- ![Gitflow](http://wiki.jikexueyuan.com/project/git-workflow-tutorial/images/git-workflows-gitflow.png) -->

## 特点
1. 健壮的用于管理大型项目的框架
1. 分支分配明确
1. 功能分支，在做准备、维护和记录发布也使用各自的分支

## 工作方式
Gitflow工作流仍然用中央仓库作为所有开发者的交互中心。和其它的工作流一样，开发者在本地工作并push分支到要中央仓库中。

分支命名：`master`版本、`develop`开发、`release-*`发布、`feature-*`功能、`hotfix-*`修复
>特点

```
├── master
	├── hotfix-*
	├── release-*
	├── develop
		├── hotfix-*
		├── release-*
		├── feature-*
```

![维护分支](https://wac-cdn.atlassian.com/dam/jcr:21cf772d-2ba5-4686-8259-fcd6fd2311df/05.svg?cdnVersion=fn)


### master版本分支
![](https://wac-cdn.atlassian.com/dam/jcr:e3bd4199-27ac-4bac-a5d2-3ff0fdb112d3/01.svg?cdnVersion=fn)
正式发布历史分支：用于管理发布的版本，发布Tag

### develop开发分支
![](https://wac-cdn.atlassian.com/dam/jcr:2e2315b0-d79a-403f-a981-4cb94599df1f/02.svg?cdnVersion=fn)
功能的集成分支：用于开发项目

### feature功能分支
![](https://wac-cdn.atlassian.com/dam/jcr:9f149cef-f784-43de-8207-3e7968789a1f/03.svg?cdnVersion=fn)
功能分支：每个新功能位于一个自己的分支，这样可以push到中央仓库以备份和协作。
新功能提交应该从不直接与master分支交互，

#### 案例
小红和小明开始各自的功能开发。他们需要为各自的功能创建相应的分支。新分支不是基于master分支，而是应该基于develop分支进行开发
![](https://wac-cdn.atlassian.com/dam/jcr:57829b6b-1e6d-40ea-a15a-1a2fe6bf80f6/07.svg?cdnVersion=fn)

##### 创建feature分支

```
<!-- develop分支 -->
$ git checkout develop

<!-- 创建dev分支并切换 -->
$ git checkout -b feature-dev
Switched to branch 'feature-dev'

=>
<!-- 以develop创建dev分支并切换 -->
git checkout -b feature-dev develop
```

##### 编辑、暂存、提交
```
$ git status
$ git add
$ git commit -m "branch"
[feature-dev fec145a] branch
 1 file changed, 1 insertion(+)
$ git push
```

##### 完成合并
```
<!-- 拉取develop代码并合并 -->
$ git pull origin develop
$ git checkout develop

<!-- 合并指定分支到feature-dev分支 -->
$ git merge feature-dev
$ git push

<!-- 删除分支 -->
$ git branch -d feature-dev
```

### release发布分支
![](https://wac-cdn.atlassian.com/dam/jcr:3555a856-675e-453a-b49d-ba60667809e1/04.svg?cdnVersion=fn)
发布分支：用于发布准备的专门分支。
一旦develop分支上有了做一次发布（或者说快到了既定的发布日）的足够功能，就从develop分支上fork一个发布分支。新建的分支用于开始发布循环，所以从这个时间点开始之后新的功能不能再加到这个分支上 —— 这个分支只应该做Bug修复、文档生成和其它面向发布任务。
一旦对外发布的工作都完成了，发布分支合并到master分支并分配一个版本号打好Tag。另外，这些从新建发布分支以来的做的修改要合并回develop分支。

### hotfixes修复分支
![](https://wac-cdn.atlassian.com/dam/jcr:21cf772d-2ba5-4686-8259-fcd6fd2311df/05.svg?cdnVersion=fn)
维护分支：用于生成快速给产品发布版本。
这是唯一可以直接从master分支fork出来的分支。修复完成，修改应该马上合并回master分支和develop分支（当前的发布分支），master分支应该用新的版本号打好Tag。


Gitflow 工作流没有用超出功能分支工作流的概念和命令，而是为不同的分支分配一个很明确的角色，并定义分支之间如何和什么时候进行交互。 除了使用功能分支，在做准备、维护和记录发布也使用各自的分支。 当然你可以用上功能分支工作流所有的好处：Pull Requests 、隔离实验性开发和更高效的协作。

# Forking工作流
Forking工作流是分布式工作流，充分利用了Git在分支和克隆上的优势。可以安全可靠地管理大团队的开发者（developer），并能接受不信任贡献者（contributor）的提交。

# 冲突解决

## 大幅度

- [Git工作流指南：Gitflow工作流](http://blog.jobbole.com/76867/#comment-156726 "")
- [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows "")

# 拓展

- [Git 工作流指南](http://wiki.jikexueyuan.com/project/git-workflow-tutorial/ "")
- [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/ "成功的Git分支模型")
- [改进合作 Git 工作流：自动提取、合并提交](https://tech.meituan.com/improving-git-flow_squashing-commits.html "")
- [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows "工作流比较")
- [my-git](https://github.com/xirong/my-git/ "有关 git 的学习资料")
- []( "")
- []( "")
- []( "")
- []( "")
- []( "")