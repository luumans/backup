title: Git处理情况
date: 2016-01-19 14:11:20
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
　　** Git处理情况：**
<!-- more -->
[](http://deweixu.me/2016/11/05/how-git-works/ "")

这篇文章解释了git的工作原理，它会使你更深入的理解git，更好的使用它来控制项目的版本。
本文重点介绍了支持Git的图形结构，以及该图形的属性指示Git行为的方式。从基础开始，同时有实例讲解，根据实例建立一个更真实的模型，让你更好地理解git
做了什么。

## 创建项目

```
~
$ mkdir alpha
~
$ cd alpha
```

项目目录是alpha

```
~/alpha
$ mkdir data
~/alpha
$ printf 'a' > data/letter.txt
```

到目录alpha下创建了一个名为data的目录，在里面创建了一个名为letter.txt的文件，其中的内容是一个字符a，alpha目录结构如下：
```
alpha
└── data
└── letter.txt
```

## 初始化仓库

```
~/alpha
$ git init
Initialized empty Git repository
```

git init使当前目录变成了Git仓库，为此，它创建了一个.git目录并向其中写入了一些文件。这些文件定义了关于Git配置和项目历史的一切，它们只是普通文件。 用户可以使用文本编辑器或shell来读取和编辑它们。 这就是说，用户可以像他们的项目文件一样轻松地阅读和编辑他们项目的历史。
现在alpha目录的结构就像下面这样


alpha
├── data
| └── letter.txt
└── .git
├── objects
etc...

.git
目录及其内容归Git
系统所有，所有其他的文件统称为工作副本，归用户所有。

## 添加文件

```
~/alpha
$ git add data/letter.txt
```

运行上面的命令，有两个效果。首先，它在.git/objects/目录中创建了一个新的blob文件。这个blob文件包含data/letter.txt的压缩内容。 它的名称通过文件内容的Hash
(应该是用的sha1)得到。取一段文本的Hash值意味着运行一个程序，将其内容变成一块较小的文本，这块文本是原始内容的唯一标识。例如，Git将aes转换为2e65efe2a145dda7ee51d1741299f848e5bf752e
，前两个字符用作对象数据库中的目录的名称：.git/objects/2e/。 散列的其余部分用作保存所添加文件的内容的blob文件的名称：.git/objects/2e/65efe2a145dda7ee51d1741299f848e5bf752e
。git add将文件添加到Git并将其内容保存到objects目录中。 如果用户从工作副本中删除data/letter.text，它的内容在Git中仍然是安全的。

其次，git add将文件添加到索引。 索引是一个列表，其中包含Git已被告知要跟踪的每个文件。 它作为一个文件存储在.git/index。 文件的每一行将被跟踪的文件映射到其内容的哈希
。 这是运行git add命令后的索引：data/letter.txt 2e65efe2a145dda7ee51d1741299f848e5bf752e

创建一个包含内容1234的文件data/number.txt

```
~/alpha
$ printf '1234' > data/number.txt
```

目录结构变成了下面这样：

```
alpha
└── data
└── letter.txt
└── number.txt
```

添加文件到Git

```
~/alpha
$ git add data
```

git add
命令创建一个包含data/number.txt
内容的blob
对象。 它为指向blob
的data/number.txt
添加一个索引条目。 这是git add
命令第二次运行后的索引：


data/letter.txt 2e65efe2a145dda7ee51d1741299f848e5bf752e
data/number.txt 274c0052dd5408f8ae2bc8440029ff67d79bc5c3

只有数据目录中的文件被列在索引中，虽然用户运行了git add data
。 数据目录data
不单独列出。


```
~/alpha
$ printf '1' > data/number.txt
~/alpha
$ git add data
```

当最初创建data/number.txt时，想要输入内容1，而不是1234.他们进行更正并将文件再次添加到索引。 此命令将使用新内容创建一个新的blob。 并且它更新data/number.txt
的索引条目以指向新的blob。

## git commit

```
~/alpha
$ git commit -m 'a1'
[master (root-commit) 774b54a] a
```

进行a1提交，Git打印了这次提交的相关信息。
commit命令有三个步骤。 创建一个树形图来表示正在提交的项目版本的内容。 创建一个提交对象。 将当前分支指向新的提交对象。

## 创建树形图

Git通过从索引创建树图来记录项目的当前状态。 此树图记录项目中每个文件的位置和内容。该图由两种类型的对象组成：blob和树。Blob是通过git add
存储的。 它们表示文件的内容。在commit时存储树。 树表示工作副本中的目录。下面是记录新提交的data目录的内容的树对象：

```
100664 blob 2e65efe2a145dda7ee51d1741299f848e5bf752e letter.txt
100664 blob 56a6051ca2b02b04ef92d5150c9ef600403cb1de number.txt
```

第一行记录展示了data/letter.txt
文件的信息。 第一部分是文件的权限。 第二部表示此条目的内容由blob
而不是树
表示。 第三部分描述了blob
的Hash
。 第四部分描述文件的名称。第二行当然就是文件data/number.txt
文件的信息。
下面是alpha
的树对象：
1

040000 tree 0eed1217a2947f4930583229987d90fe5e8e0b74 data

alpha
树对象只包含了一个指向data
树指针。(译著：如果alpha
目录下还有一个文件， alpha
树对象就还会多一行，就是指向多出文件的blob
对象)
![`a1` commit object pointing at its tree graph](http://upload-images.jianshu.io/upload_images/1064727-617c76769ffc1d0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在上面的图中，root
树指向data
树。 data
树指向data/letter.txt
和data/number.txt
的blob
。

## 创建一个提交对象
git commit
在创建树图后创建一个提交对象
。 提交对象
只是.git/objects/
中的另一个文本文件：



5

tree ffe298c3ce8bb07326f888907996eaa48d266db4
author Mary Rose Cook <mary@maryrosecook.com> 1424798436 -0500
committer Mary Rose Cook <mary@maryrosecook.com> 1424798436 -0500

a1

第一行指向树图。 Hash
是表示工作副本的根的树对象。 也就是alpha
目录。 最后一行是提交消息。
![`a1` commit object pointing at its tree graph](http://upload-images.jianshu.io/upload_images/1064727-805a1858eb60d644.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 将当前分支指向新的提交
最后，commit命令将当前分支指向新的提交对象。哪个是当前分支？ .git/HEAD
文件记录了当前分支：
1

ref: refs/heads/master

这说明HEAD
指向master
, master
是主分支。HEAD
和master
都是refs
。 ref
是Git
用来标识特定提交的标签。
表示master
引用的文件不存在，因为这是对仓库的第一次提交。 Git
在.git/refs/heads/master
下创建文件，并将其内容设置为提交对象的哈希值：
1

74ac3ad9cde0b265d2b4f1c778b283a6e2ffbafd

(如果你在阅读时输入这些Git
命令，你的a1
提交的哈希值
将不同于我的哈希值
。 内容对象（如blob和树）总是散列为相同的值。 提交不会，因为它们包括创建者的日期和名称。)
添加HEAD
和master
到树图：
![`HEAD` pointing at `master` and `master` pointing at the `a1` commit](http://upload-images.jianshu.io/upload_images/1064727-623273180a0e5fd7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
HEAD
指向master
，就像提交之前一样。 但maste
r现在存在并指向新的提交对象a1
。

## 再一次commit
下面是a1
提交后的Git
结构图。 包含工作副本和索引。
![`a1` commit shown with the working copy and index](http://upload-images.jianshu.io/upload_images/1064727-a5fc68b7929e4de9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
工作副本，索引和a1
提交都具有与data/letter.txt
和data/number.txt
相同的内容。 索引和HEAD
提交都使用Hash
来引用blob
对象，但是工作副本内容作为文本存储在不同的地方。
1

~/alpha
$ printf '2' > data/number.txt

将data/number.txt
的内容设置为2
.这会更新工作副本，但索引
和HEAD
不变。
![`data/number.txt` set to `2` in the working copy](http://upload-images.jianshu.io/upload_images/1064727-2ae69aac417eb56a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
1

~/alpha
$ git add data/number.txt

将文件添加到Git
。 这会向objects
目录添加一个包含2
的blob
。 它指向新blob
的data/number.txt
的索引条目。
![`data/number.txt` set to `2` in the working copy and index](http://upload-images.jianshu.io/upload_images/1064727-c1f28bc9af2b1b54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


~/alpha
$ git commit -m 'a2'
[master f0af7e6] a2

提交的步骤与之前相同。
首先，创建一个新的树形图来表示索引的内容。
data/number.txt
的索引条目已更改。 旧的数据树不再反映data
目录的索引状态。 必须创建一个新的data
树对象：


100664 blob 2e65efe2a145dda7ee51d1741299f848e5bf752e letter.txt
100664 blob d8263ee9860594d2806b0dfd1bfd17528b0ba2a4 number.txt

新数据树与旧数据树的哈希值不同。 必须创建一个新的根树以记录此Hash
值：
1

040000 tree 40b0318811470aaacc577485777d7a6780e51f0b data

其次，创建一个新的提交对象。



5
6

tree ce72afb5ff229a39f6cce47b00d1b0ed60fe3556
parent 774b54a193d6cfdd081e581a007d2e11f784b9fe
author Mary Rose Cook <mary@maryrosecook.com> 1424813101 -0500
committer Mary Rose Cook <mary@maryrosecook.com> 1424813101 -0500

a2

提交对象的第一行指向新的根树对象。 第二行指向a1
：新提交的父级。要找到父提交，要跟着HEAD
和master
来掌握并发现a1
的提交哈希。
最后，master分支文件的内容被设置为新提交的hash
值。
![`a2` commit](http://upload-images.jianshu.io/upload_images/1064727-6325d80af059308a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Git graph without the working copy and index](http://upload-images.jianshu.io/upload_images/1064727-c88c228e16534979.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
内容存储为对象树。 这意味着只有diffs存储在对象数据库中。 看看上面的图表。 a2 commit
重用了在a1
提交之前创建的blob
。 类似地，如果提交中整个没有变，则其树以及其下的所有blob
和树可以被重用。 一般来说，提交的内容更改很少。 这意味着Git
可以在小的空间中存储大的提交历史。
每个提交都有一个父级。 这意味着存储库可以存储项目的历史记录。
refs
是提交历史的一部分或另一部分的入口点。 这意味着提交可以被赋予有意义的名称。 用户将他们的工作组织到对他们的项目有意义的谱系中，具体的参考如fix-for-bug-376
。 Git
使用符号引用，如HEAD
，MERGE_HEAD
和FETCH_HEAD
来支持操作提交历史记录的命令。
objects
目录中的节点是不可变的。 这意味着内容被编辑，而不是被删除。 每一次添加的内容和每次提交的对象都是在目录中.
refs
是可变的。 因此，ref
的含义可以改变。 master
指向的提交可能是当前项目的最佳版本，但是，很快，它将被更新的更好的提交取代。

## -commit)Check out a commit


~/alpha
$ git checkout 37888c2
You are in 'detached HEAD' state...

使用Hash
值checkout``a2
的提交(如果你在运行这些git命令，这里的hash
值要换成你自己的，使用git log
查看)
checkout
 有四个步骤：
获取a2
提交，并获取指向它的树图

它将树形图中的文件条目写入工作副本。 这将导致没有更改。 工作副本已经具有被写入其中的树图的内容，因为HEAD
已经通过master
指向a2
提交。

将树图中的文件条目写入索引。 这也导致没有变化。 索引已经具有a2
提交的内容。

HEAD
的内容设置为a2
提交的哈希：

1

f0af7e62679e144bb28c627ee3e8f7bdb235eee9

将HEAD
的内容设置为Hash
值会使存储库处于分离的HEAD
状态。 注意在下面的图表中，HEAD
直接指向a2
提交，而不是指向master
。
![Detached `HEAD` on `a2` commit](http://upload-images.jianshu.io/upload_images/1064727-44192f5251d538c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




~/alpha
$ printf '3' > data/number.txt
~/alpha
$ git add data/number.txt
~/alpha
$ git commit -m 'a3'
[detached HEAD 3645a0e] a3

将data/number.txt
的内容设置为3
，并提交更改。 Git
去HEAD
得到a3
提交的父级。 而不是找到一个分支ref，它找到并返回a2
提交的哈希。
Git
更新HEAD
直接指向新的a3
提交的哈希。 存储库仍处于分离的HEAD
状态。 它不在一个分支上，因为没有提交指向a3
或其一个后代。 这意味着它很容易丢失。
![`a3` commit that is not on a branch](http://upload-images.jianshu.io/upload_images/1064727-6e568376142e1e97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 创建分支
1

~/alpha
$ git branch deputy

创建一个新分支deputy
。 这只是在.git/refs/heads/deputy
创建一个新文件，其中包含HEAD
指向的哈希, 也就是a3
提交的哈希。
分支只是refs, refs只是文件。 这意味着Git分支是轻量级的。

deputy
分支的创建将新的a3
提交安全地放置在分支上。 HEAD
仍然分离，因为它仍然直接指向一个提交。
![`a3` commit now on the `deputy` branch](http://upload-images.jianshu.io/upload_images/1064727-c4b85ca812ab3ae6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 切换分支


~/alpha
$ git checkout master
Switched to branch 'master'

切换到了master
分支
获取a2
提交，并将master
指向获取提交点的树图。

树形图中的文件条目替换工作副本的文件。 这将使data/number.txt
的内容设置为2。

将树图中的文件条目写入索引。 这会将data/number.txt
的条目更新为2个blob
的散列。

改变HEAD
的值

1

ref: refs/heads/master

![`master` checked out and pointing at the `a2` commit](http://upload-images.jianshu.io/upload_images/1064727-342054932f3890bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## )切换到与工作副本不兼容(有改变)的分支



5
6
7

~/alpha
$ printf '789' > data/number.txt
~/alpha
$ git checkout deputy
Your changes to these files would be overwritten
by checkout:
data/number.txt
Commit your changes or stash them before you
switch branches.

将data/number.txt
的内容设置为789
， 当checkout
到deputy
时，Git
报了一个错误。
HEAD
指向master
，master
指向a2
，其中data/number.txt
的内容是2
。deputy
指向a3
，其中data/number.txt
的内容是3
。data/number.txt
在工作副本的内容为789
，所有这些版本都不同，差异必须解决。
Git
可以使用要切换分支中提交的版本替换掉工作副本中的版本，这样可以避免数据丢失。
Git
可以合并工作副本的版本和要切换分支中的版本，但这很复杂。
所以Git
报了一个错误，不能切换分支。

3

~/alpha
$ printf '2' > data/number.txt
~/alpha
$ git checkout deputy
Switched to branch 'deputy'

把data/number.txt
的内容变回2
时，便切换成功了。
![`deputy` checked out](http://upload-images.jianshu.io/upload_images/1064727-c47efc60986c19bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 合并祖先


~/alpha
$ git merge master
Already up-to-date.

将主分支master
和并到deputy
分支。和并两个分支实际上是合并两个提交。第一个提交指向deputy
，它是接收者。第二个提交指向master
，它是提交者。可以理解为把master
提交到deputy
。对于这个合并，git
什么也没有做，因为两个分支的内容是一样的。
图中的一系列的提交可以看成是对存储库的一系列更改。这也就意味着，在合并中，如果提交者(master
)是接收者(deputy
)的祖先，git
将什么也不做，因为这些变化已经存在。

## 合并后代


~/alpha
$ git checkout master
Switched to branch 'master'

切换到分支master

![`master` checked out and pointing at the `a2` commit](http://upload-images.jianshu.io/upload_images/1064727-b5c3e8ae0c66fff7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


~/alpha
$ git merge deputy
Fast-forward

合并deputy
到master
。Git
发现接受者的a2
提是提交者a3
的祖先，它可以做快进合并。
获得提交者的提交a3
并提供指向它的树图，将树图中的文件条目写入工作副本和索引。快进
是指master
指向a3
。
![`a3` commit from `deputy` fast-forward merged into `master`](http://upload-images.jianshu.io/upload_images/1064727-4da2fab3ec32a358.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在合并中，如果提交者(deputy
分支上的a3
提价)是接收者(master
上的a2
提价)的后代，则历史记录不改变。 已经有一系列提交描述了要做出的改变(接收者和提交者之间的提交序列
)。 虽然Git历史没有改变，Git图确实改变。 HEAD指向的具体引用被更新为指向提交者(master
指向a3
)。

## 合并来自两个不同谱系的分支




~/alpha
$ printf '4' > data/number.txt
~/alpha
$ git add data/number.txt
~/alpha
$ git commit -m 'a4'
[master 7b7bd9a] a4

把number.txt
的内容设置为4
，并提交到master




5
6

~/alpha
$ git checkout deputy
Switched to branch 'deputy'
~/alpha
$ printf 'b' > data/letter.txt
~/alpha
$ git add data/letter.txt
~/alpha
$ git commit -m 'b3'
[deputy 982dffb] b3

切换到deputy
分支，把data/letter.txt
的内容设置为b
，并提交到deputy
。
![`a4` committed to `master`, `b3` committed to `deputy` and `deputy` checked out](http://upload-images.jianshu.io/upload_images/1064727-9681d95abd83f7dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
提交可以共享父级，这意味着可以在提价的历史中创建新的谱系。
提交可以有多个父级。 这意味着单独的谱系可以通过具有两个父的提交来合并：合并提交。


~/alpha
$ git merge master -m 'b4'
Merge made by the 'recursive' strategy.

合并master
到deputy

Git
发现接收者b3
和提供者a4
在不同的谱系中。 它做一个合并提交。 这个过程有八个步骤。
Git
将提交者的提交的哈希写入到alpha/.git/MERGE_HEAD
文件。 这个文件的存在告诉Git
在合并中。

Git
查找基本提交：接收者和提交者提交的最近的祖先的共同点。

![`a3`, the base commit of `a4` and `b3`](http://upload-images.jianshu.io/upload_images/1064727-bc9298ebf5ad5e0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
提交有父级别。 这意味着可以找到两个谱系起始点。 Git
从b3
向后跟踪，找到所有的祖先，从a4
向后寻找所有的祖先。 它找到两个谱系共享的最近的祖先a3
。 这是基本提交。
Git
从接收者和提交者提交的树图生成基本的索引。

Git
生成一个diff
，它将接收者提交和提交者提交对基础提交所做的更改合并。 此diff
是指向更改的文件路径列表：添加，删除，修改或冲突。

Git
获取出现在base
，receiver
或giver
索引中的所有文件的列表。 比较较索引条目以决定对文件做出的更改。 它将一个相应的条目写入diff
。 在这种情况下，diff
有两个条目。
第一个条目是是data/letter.txt
。 文件内容在base
和receiver
中不同。 但是在base
和giver
中是一样的。 Git
看到内容被reviceer
修改，但是没有被giver
修改。 data/letter.txt
的diff
条目是一个修改，而不是冲突。
diff
中的第二个条目是data/number.txt
。 在这种情况下，文件内容在base
和receiver
中是相同的，并且在giver
中是不同的。 data/letter.txt
的diff
条目也是一个修改。
可以找到合并的base
提交。 这意味着，如果一个文件只是从receiver
或提giver
的base
改变，Git
可以自动解决该文件的合并。 这减少了用户必须做的工作。
由diff
中的条目指示的更改将应用于工作副本。 data/letter.txt
的内容设置为b
，data/number.txt
的内容设置为4
。

由diff
中的条目指示的更改将应用于索引。 data/letter.txt
的条目指向b
 blob
，data/number.txt
的条目指向4
 blob
。

更新索引：




5
6
7

tree 20294508aea3fb6f05fcc49adaecc2e6d60f7e7d
parent 982dffb20f8d6a25a8554cc8d765fb9f3ff1333b
parent 7b7bd9a5253f47360d5787095afc5ba56591bfe7
author Mary Rose Cook <mary@maryrosecook.com> 1425596551 -0500
committer Mary Rose Cook <mary@maryrosecook.com> 1425596551 -0500

b4

注意：这次提交有两个父级

将当前分支deputy
 分支指向新的提交。

![`b4`, the merge commit resulting from the recursive merge of `a4` into `b3`](http://upload-images.jianshu.io/upload_images/1064727-27f29fb87fff04a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 合并来自不同谱系的两个提交，这两个提交都修改同一个文件
切换到master
分支，并把deputy
合并到master
，快进
到b4
，现在master
和deputy
都指向同一个提交
![`deputy` merged into `master` to bring `master` up to the latest commit, `b4`](http://upload-images.jianshu.io/upload_images/1064727-6aea1c3e0f49984a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



5
6

~/alpha
$ git checkout deputy
Switched to branch 'deputy'
~/alpha
$ printf '5' > data/number.txt
~/alpha
$ git add data/number.txt
~/alpha
$ git commit -m 'b5'
[deputy bd797c2] b5

切换到deputy
分支，把data/number.txt
的内容设置为5
，并提交。



5
6

~/alpha
$ git checkout master
Switched to branch 'master'
~/alpha
$ printf '6' > data/number.txt
~/alpha
$ git add data/number.txt
~/alpha
$ git commit -m 'b6'
[master 4c3ce18] b6

切换到master
分支，把data/number.txt
的内容设置为6
，并提交。
![`b5` commit on `deputy` and `b6` commit on `master`](http://upload-images.jianshu.io/upload_images/1064727-3719b7ad2c921674.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




~/alpha
$ git merge deputy
CONFLICT in data/number.txt
Automatic merge failed; fix conflicts and
commit the result.

将deputy
合并到master
。存在冲突，并且合并已暂停。冲突合并的过程遵循与未冲突合并的过程相同的前六个步骤：设置.git/MERGE_HEAD
，查找base
，生成base
，receiver
，giver
的索引，创建diff
，更新工作副本和更新索引。由于冲突，不采取第七提交步骤和第八更新ref
步骤。让我们再次看看这些步骤，发生了什么。
Git
将giver
提交的哈希写入.git/MERGE_HEAD
文件。

![`MERGE_HEAD` written during merge of `b5` into `b6`](http://upload-images.jianshu.io/upload_images/1064727-d658ac38e88cefec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Git
找到base
提交b4

Git
从接收者和提交者提交的树图生成基本的索引。

Git
生成一个diff
，它将接收者提交和提交者提交对基础提交所做的更改合并。 此diff
是指向更改的文件路径列表：添加，删除，修改或冲突。

在这种情况下，diff
只包含一个条目：data/number.txt
。 该条目被标记为冲突，因为data/number.txt
的内容在接收者，提供者和base
中是不同的。
由diff
中的条目指示的更改将应用于工作副本。 对于冲突区域，Git
将两个版本写入工作副本中的文件。 data/number.txt
的内容设置为：




5

<<<<<<< HEAD
6
=======
5
>>>>>>> deputy

由diff
中的条目指示的更改应用于索引。 索引中的条目通过其文件路径和阶段的组合成唯一标识。 未冲突文件的条目具有阶段0
.在此合并之前，索引如下所示，其中0
是阶段值：



0 data/letter.txt 63d8dbd40c23542e740659a7168a0ce3138ea748
0 data/number.txt 62f9457511f879886bb7728c986fe10b0ece6bcb

在合并diff
被写入索引之后，索引如下所示：




0 data/letter.txt 63d8dbd40c23542e740659a7168a0ce3138ea748
1 data/number.txt bf0d87ab1b2b0ec1a11a3973d2845b42413d9767
2 data/number.txt 62f9457511f879886bb7728c986fe10b0ece6bcb
3 data/number.txt 7813681f5b41c028345ca62a2be376bae70b7f61

在阶段0
的data/letter.txt
的条目与在合并之前相同。 在阶段0
的data/number.txt
的条目被去掉了。 它有三个新的条目。 阶段1
的条目具有base
 data/number.txt
内容的散列。 阶段2
的条目具有receiver
 data/number.txt
内容的散列。 阶段3
的条目具有giver
 data/number.txt
内容的散列。 这三个条目的存在告诉Git
 data/number.txt
是冲突的。合并就暂停了。


~/alpha
$ printf '11' > data/number.txt
~/alpha
$ git add data/number.txt

通过将data/number.txt
的内容设置为11
来合成两个冲突版本的内容。他们将文件添加到索引。 Git
添加一个包含11
的Blob
。添加一个冲突的文件告诉Git
冲突已解决。 Git
从索引中删除阶段1
,2
和3
的data/number.txt
条目。 在阶段0
的data/number.txt
的条目中添加新blob
的散列。 该索引现在为：


0 data/letter.txt 63d8dbd40c23542e740659a7168a0ce3138ea748
0 data/number.txt 9d607966b721abde8931ddd052181fae905db503



~/alpha
$ git commit -m 'b11'
[master 251a513] b11

提交。 Git
在存储库中看到.git/MERGE_HEAD
，告诉它合并正在进行。 然后检查索引并发现没有冲突。 就创建一个新的提交b11
，以记录解析的合并的内容。 z最后会删除.git/MERGE_HEAD
文件。 这将完成合并。

将当前分支master
指向新的提交。

![`b11`, the merge commit resulting from the conflicted, recursive merge of `b5` into `b6`](http://upload-images.jianshu.io/upload_images/1064727-a23d6fc7e71236c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 移除一个文件
下面的图包括提交历史、最近提交的树和blob
以及工作副本和索引：
![The working copy, index, `b11` commit and its tree graph](http://upload-images.jianshu.io/upload_images/1064727-21408360670787df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


~/alpha
$ git rm data/letter.txt
rm 'data/letter.txt'

告诉Git
删除data/letter.txt
。 该文件从工作副本中删除。 该条目从索引中删除。
![After `data/letter.txt` `rm`ed from working copy and index](http://upload-images.jianshu.io/upload_images/1064727-e45ea7801d1e32a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


~/alpha
$ git commit -m '11'
[master d14c7d2] 11

提交。 作为提交的一部分，一如既往，Git构建一个表示索引内容的树形图。 data/letter.txt
不包括在树图中，因为它不在索引中。
![`11` commit made after `data/letter.txt` `rm`ed](http://upload-images.jianshu.io/upload_images/1064727-fb9bc0ec36c2bfd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 复制存储库


~/alpha
$ cd ..
~
$ cp -R alpha bravo

将alpha/
存储库的内容复制到bravo/
目录。 这将产生以下目录结构：



5
6
7

~
├── alpha
| └── data
| └── number.txt
└── bravo
└── data
└── number.txt

现在bravo
目录中有另一个Git
图：
![New graph created when `alpha` `cp`ed to `bravo`](http://upload-images.jianshu.io/upload_images/1064727-cfed5ae7c7a613c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 将存储库链接到另一个存储库


~
$ cd alpha
~/alpha
$ git remote add bravo ../bravo

移回到alpha
存储库。 他们将bravo
设置为alpha
上的远程存储库。 这会在alpha/.git/config
文件中添加：


[remote "bravo"]
url = ../bravo/


## 从远程获取分支



5

~/alpha
$ cd ../bravo
~/bravo $ printf '12' > data/number.txt
~/bravo $ git add data/number.txt
~/bravo $ git commit -m '12'
[master 94cd04d] 12

进入bravo
存储库。 将data/number.txt
的内容设置为12
，并将更改提交到bravo
上的master
。
![`12` commit on `bravo` repository](http://upload-images.jianshu.io/upload_images/1064727-beb64d9401d7bea0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



5

~/bravo $ cd ../alpha
~/alpha
$ git fetch bravo master
Unpacking objects: 100%
From ../bravo
* branch master -> FETCH_HEAD

进入alpha
存储库。 从bravo
获取master
到alpha
。 这个过程有四个步骤。
Git
获取master
在bravo
上指向的提交的哈希。 这是12
提交的哈希。

Git
提供了12
提交所依赖的所有对象的列表：提交对象本身，其树图中的对象，12
提交的祖先提交和它们的树图中的对象。 它从此列表中删除alpha
对象数据库已有的对象。 它将其余部分复制到alpha/.git/objects/
。

将alpha/.git/refs/remotes/bravo/master
下的具体ref
文件的内容设置为12提交的哈希值。

将alpha/.git/FETCH_HEAD
的内容设置为：

1

94cd04d93ae88a1f53a4646532b1e8cdfbc0977f branch 'master' of ../bravo

下图表示了fetch
命令从bravo
获取了master
的12
提交
![`alpha` after `bravo/master` fetched](http://upload-images.jianshu.io/upload_images/1064727-9e6136cf919e14a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
对象是可以复制的，这意味着可以在存储库之间共享历史记录。
存储库可以存储远程分支引用，如alpha/.git/refs/remotes/bravo/master
， 这意味着存储库可以在本地记录在远程存储库上分支的状态。 在获取时是正确的，但如果远程分支改变，它将过期。

## 合并FETCH_HEAD

3

~/alpha
$ git merge FETCH_HEAD
Updating d14c7d2..94cd04d
Fast-forward

合并FETCH_HEAD
, FETCH_HEAD
只是另一个ref
。 解析了12
提交，giver
。 master
开始指向11
提交。 Git
做一个快进合并，并将master
指向在12
提交。
![`alpha` after `FETCH_HEAD` merged](http://upload-images.jianshu.io/upload_images/1064727-bdbf2a8a4db46282.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 从远程分支Pull


~/alpha
$ git pull bravo master
Already up-to-date.

将bravo
的master
拉到alpha
。 Pull
是fetch and merge FETCH_HEAD
的缩写。

## Clone一个存储库

3

~/alpha
$ cd ..
~
$ git clone alpha charlie
Cloning into 'charlie'

移动到上面的目录。 clone
 alpha
到charlie
。 clone
到charlie
具有与生成bravo
存储库的cp
类似的结果。 Git
创建一个名为charlie
的新目录。 它将charlie
作为一个Git仓库，将alpha
添加为远程仓库被称为origin
，获取源并合并FETCH_HEAD
。

## Push分支到远程分支



5

~
$ cd alpha
~/alpha
$ printf '13' > data/number.txt
~/alpha
$ git add data/number.txt
~/alpha
$ git commit -m '13'
[master 3238468] 13

返回到alpha
仓库，把data/number.txt
的内容设置为13
，并提交。
1

~/alpha
$ git remote add charlie ../charlie

设置alpha
的远程仓库为charlie




5

~/alpha
$ git push charlie master
Writing objects: 100%
remote error: refusing to update checked out
branch: refs/heads/master because it will make
the index and work tree inconsistent

push
 master
到charlie
.
13
提交所需的所有对象都复制到charlie
。
此时，推送过程停止。 Git
告诉我们出了什么问题。 它拒绝推送到远程分支。 这是有道理的, 因为推送将更新远程索引和HEAD
。 这将导致混乱，如果有人正在编辑远程的工作副本。(这也有其他的解决办法，可以google一下)
此时，可以创建一个新的分支，将13
提交合并到其中，并将该分支推送到charlie
。但是我们想要一个类似GitHub
那样的中央仓库，无论什么时候都可以push
 pull
。(中央仓库为什么可以？因为在初始化仓库的时候使用的是git init --bare
, 初始化成一个裸存储库，远程仓库应该都要这么初始化。)

## Clone 一个裸仓库

3

~/alpha
$ cd ..
~
$ git clone alpha delta --bare
Cloning into bare repository 'delta'

移动到上面的目录。 将delta
 clone
为裸存储库。 这是一个有两个区别的普通clone
。 配置文件指示存储库是裸的。 通常存储在.git
目录中的文件存储在存储库的根目录中如下：



5

delta
├── HEAD
├── config
├── objects
└── refs

![`alpha` and `delta` graphs after `alpha` cloned to `delta`](http://upload-images.jianshu.io/upload_images/1064727-3fa95cae9aa8d8ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Push分支到裸存储库

```
~
$ cd alpha
~/alpha
$ git remote add delta ../delta
```

返回到alpha存储库。 将delta设置为alpha上的远程存储库。

```
~/alpha
$ printf '14' > data/number.txt
~/alpha
$ git add data/number.txt
~/alpha
$ git commit -m '14'
[master cb51da8] 14
```

将data/number.txt的内容设置为14，并将更改提交到alpha上的master

![`14` commit on `alpha`](http://upload-images.jianshu.io/upload_images/1064727-c6c82edc910832cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




~/alpha
$ git push delta master
Writing objects: 100%
To ../delta
3238468..cb51da8 master -> master

push
 master
到delta
，有3个步骤
master
分支上的14
提交所需的所有对象都从alpha/.git/objects/
复制到delta/objects /
。

delta/refs/heads/master
被更新为指向14
提交。

alpha/.git/refs/remotes/delta/master
设置为指向14
提交。 alpha
具有delta
的状态的最新记录.

![`14` commit pushed from `alpha` to `delta`](http://upload-images.jianshu.io/upload_images/1064727-9d968d38eb0b02e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


