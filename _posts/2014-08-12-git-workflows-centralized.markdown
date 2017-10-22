---
layout: single
title: "Git中心式工作流程"
date: 2014-08-12 06:34:11 +0800
categories: Technology GIT
---

![](http://i.imgur.com/4dKRz7H.png)

切换到分布式的版本控制系统可能会让人望而却步，不过使用即使使用Git也不用改变团队现存的工作流程，开发团队仍然可以使用跟Subersion一模一样的操作完成项目开发。

不过Git却有很多超越SVN的优点，能够增强你的开发流程。首先，Git让每个开发者拥有整个项目的本地拷贝，这个孤立的环境使开发者的能够独立地工作，不用过早关注项目的其他代码变化。开发者们能够提交代码到本地仓库，如非必要，完全可以不必花费精力考虑上游的开发。

<!--more-->

其次，Git提供了强健的分支和合并模型，跟SVN不同，Git的分支机制保证在不同代码库之间集成代码、共享变更时是非常安全的，其机制是“fail-safe”的。

## 它是如何工作的 ##

跟Subversion一样，中心式工作流程有一个中心代码仓库，其作用是作为项目代码变更的唯一入口。不过默认的开发分支名称不再是SVN里的*trunk*，而是*master*，所有的变更都会被提交到这个分支上，该流程不需要任何其它*master*之外的分支。

刚开始的时候，开发者“克隆”中心代码库到本地，在自己的本地项目中修改文件、提交变更，就像使用SVN一样。不过，这些新提交的变更却被存储到本地了，完全跟中心代码库隔离了。这样就开发者能够推迟同步变更到上游，直到他们认为时机合适为止。

为了发布变更到正式的项目，开发者“推送”（push）自己本地的*master*分支到中心代码仓库，这个操作等同于*svn commit*，不同的是，该操作是将原本不存在于中心*master*分支的本地提交记录添加到中心仓库上。

![](http://i.imgur.com/QWMMzgx.png)

## 管理冲突 ##

中心代码库代表正式的项目，应该严肃对待，它的提交历史应该正式且稳定。如果开发人员的本地提交跟中心代码库有偏离，Git将会拒绝推送这些变更，因为该行为将覆盖正式的提交内容。

![](http://i.imgur.com/12300wp.png)

在开发人员发布自己的特性代码之前，需要从中心代码或获取最新的代码记录，而且要将自己的变更rebase到这些最新的代码提交记录之前，这样做就好像是在宣告“我要把自己的变更添加到别人的工作成果之上”。操作结果是完美的线性历史，跟传统的SVN工作流一样。

如果本地变更跟上游的提交结果有直接的冲突，Git会暂停rebase操作，让开发者手动解决冲突。比较好的事Git使用*git status*和*git add*命令完成解决冲突和产生提交的工作，经验较少的开发者能够比较容易地管理自己的合并工作。另外，如果开发者陷入非常难于解决的问题时，Git也很容易放弃整个rebase操作而让开发者进行其他尝试或寻求帮助。

## 示例 ##

下面我们逐步介绍典型的小型团队如何使用这个工作流程，假设这个团队里有John和Mary两个开发人员，他们将在不同的特性上工作，最后通过中心代码库共享自己的代码。

### 初始化中心代码库 ###

![](http://i.imgur.com/CFxZYh6.png)

首先在一台服务器上创建一个中心代码库，如果是全新的项目，可以初始化空的代码仓库，否则需要从现存的Git或SVN代码库导入。

中心代码库应当是有空的代码库（不应当有工作目录），创建方法如下：

    ssh user@host
    git init --bare /path/to/repo.git

确保使用有效的SSH用户名、主机IP或域名，`/path/to/repo.git`表示代码库的位置，代码库名称后的`.git`扩展名表示它是一个空白的代码库。

### 开发人员从中心库克隆代码 ###

![](http://i.imgur.com/nYjOLin.png)

中心库创建完成后，每个开发人员通过*git clone*命令创建本地代码拷贝：

    git clone ssh://user@host/path/to/repo.git

克隆代码仓库时，Git自动添加一个被称为*origin*的快捷方式，它指向“父级”代码库，开发人员未来需要和它交互。

### John在他自己的特性上工作 ###

![](http://i.imgur.com/oB10aED.png)

在John的本地代码库中，他可以使用标准的Git提交方式开发特性：edit、stage、commit。如果还不熟悉暂存区（stageing area）的概念，可以尝试做一次提交：

    git status # View the state of the repo
    git add # Stage a file
    git commit # Commit a file

由于这些命令仅仅创建了本地提交记录，John能够不受限制地重复这些操作而不用担心对中心库产生影响。这对大型的特性开发很有帮助，有助于将其拆分成更简单、更原子化的区块。

### Mary在她自己的特性上工作 ###

![](http://i.imgur.com/OxB4aMY.png)

同时，Mary正在她自己的特性上进行开发，使用的也是自己的本地代码库，利用同样的edit、stage、commit操作，跟John一样，她不用关心中心代码库的事情，也不用考虑John正在做的工作，他们的本地代码库都是私有的。

### John发布他自己的特性 ###

![](http://i.imgur.com/NglI0uw.png)

一旦John完成了自己的特性，他应该把本地提交记录发布到中心代码仓库，以便于其他成员访问，使用*git push*命令完成发布：

    git push origin master

*origin*是中心代码仓库的远程连接，是在John克隆它的时候被创建出来的，*master*参数告诉Git创建*orgin*的*master*分支，看起来似乎是John的本地master分支。由于中心库在John克隆后并没有任何更新，这次发布将不会导致冲突，*push*也如期完成自己的任务。

### Mary试图发布她的特性 ###

![](http://i.imgur.com/B6IxFZZ.png)

在John顺利地发布自己的变更到中心库后，Mary的push操作将会有什么结果呢？她也使用了同样的命令进行推送：

    git push origin master

但是由于她的本地历史记录已经跟中心库发生了偏离，Git将拒绝这个请求，并提示如下信息：

    error: failed to push some refs to '/path/to/repo.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
    hint: before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

这就阻止了Mary覆盖中心库的正式记录，她需要把John的更新pull到自己的代码库，把这些更新集成到她自己的本地变更中，继续尝试push。

### Mary rebase到John提交记录的顶端 ###

![](http://i.imgur.com/wW59OK5.png)

Mary可以使用*git pull*命令将上游变更整合到自己的代码库中，该命令有点象SVN的update，拉取完整的上游变更到Mary的本地库，并试图整合到她的本地提交记录中：

    git pull --rebase origin master

选项*--rebase*让Git在同步中心库完成后将Mary的提交记录移动到*master*分支的顶端，如下图所示：

![](http://i.imgur.com/wAl1UeX.png)

如果忘记这个选项，*pull*操作仍然能够工作，不过当别人想要从中心库同步时，都会收到不必要的“merge commit”提示。对于这个流程来说，rebase而不是产生合并请求会更好。

### Mary解决了一个合并冲突 ###

![](http://i.imgur.com/Xh50het.png)

*rebase*操作每执行一次，只把一个本地提交记录转移到更新过的*master*分支，也就是说需要一个接一个地捕捉合并冲突而不是一次性地整体解决它们，这样做有助于保持项目提交历史比较整洁，也更加容易定位bug，在必要时对项目进行回滚产生的影响也很小。

如果Mary和John工作在不相关联的特性上，*rebase*操作基本不会产生冲突；反之，Git将会暂停当前提交的rebase操作，并输出如下信息：

    CONFLICT (content): Merge conflict in <some-file>

![](http://i.imgur.com/9Rg8kY3.png)

好消息是Git可以让任何人解决自己的合并冲突，在这个示例中，Mary运行*git status*命令查看问题原因，发生冲突的文件将会在Unmerged paths区域出现：

    # Unmerged paths:
    # (use "git reset HEAD <some-file>..." to unstage)
    # (use "git add/rm <some-file>..." as appropriate to mark resolution)

    # both modified: <some-file>

然后Mary可以按照自己的意愿进行修改，一旦完成，就可以暂存这些文件并让*git rebase*继续完成后面的工作：

    git add <some-file> 
    git rebase --continue

这就是全部的工作，Git将会继续处理下一个提交，并重复这个过程直到产生新的冲突。

要是你目前为止还没有任何概念，比着急，执行如下命令就能回到你运行*git pull --rebase*前的状态：

    git rebase --abort

### Mary成功发布了她的特性 ###

![](http://i.imgur.com/p1QvqlC.png)

在Mary完成跟中心库的同步工作后，她就能成功地发布自己的变更了：

    git push origin master

## 何去何从 ##

从这个过程可以看出，仅仅用Git的几个命令就能替换掉Subversion，让团队脱离SVN很不错，不过目前为止还没有真正利用上Git分布式的特点。

如果你的团队很享受中心式工作流程，不过还想简化这个过程的协作投入，绝对值得去探索一下“[特性分支工作流][1]”的好处。通过为每个特性使用独立的分支，就可以在把特性集成到正式项目之前进行深入的讨论，决策如何处理新的附加要求。



[1]: https://www.atlassian.com/git/workflows#!workflow-feature-branch