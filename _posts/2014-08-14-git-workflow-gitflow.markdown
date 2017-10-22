---
layout: single
title: "Gitflow工作流程"
date: 2014-08-14 08:50:45 +0800
categories: Technology GIT
---

![](http://i.imgur.com/WjL6EpT.png)

[Gitflow工作流程][1]来源于Vincent Driessen的网站[nvie][2]。

这个工作流程围绕项目发布定义了一个严格的模型，它比[特性分支工作流程][3]复杂很多，为更大型的项目提供了强劲的管理框架。

本流程不会使用多于特性分支流程使用到的概念或命令，它为不同的分支赋予了特定的任务，并定义了他们之间交互的时机和方式。不同于特性分支流程，它是用彼此独立的分支完成准备、维护和记录发布的工作。当然我们仍然可以继续使用特性分支流程的种种好处：拉取请求、隔离体验、高效协同。

<!--more-->

## 它是如何工作的 ##

Gitflow流程仍然使用中心代码仓库作为所有开发人员的通讯中心，同时，开发人员仍然在本地代码库工作并把分支推送到中心代码库，他们间的唯一不同是项目的分支结构不一样。

### 历史性分支 ###

跟以前介绍的单一*master*分支流程不同，本流程使用两个分支记录项目的历史，*master*分支保存了官方的发布历史，*develop*用来集成不同的特性分支。这个流程可以很方便地在*master*分支上打标签标识版本号。

![](http://i.imgur.com/9rPQXSD.png)

本流程剩余的工作围绕这个着两个分支的特性展开工作。

### 特性分支 ###

每个新特性的代码只能存在于它所属的分支里，该分支会被推送到中心代码库用于备份或协同。特性分支不从*master*分支派生，而是从*develop*分支派生，新特性开发完成后何必会*develop*，特性分支永远不能直接跟*master*发生联系。

![](http://i.imgur.com/dd8q84K.png)

请注意，无论出于任何意图和目的，特性分支流程把特性分支同时当成了*develop*分支，但是Gitflow工作流程又前进了一步。

### 发布分支 ###

![](http://i.imgur.com/4f1iY64.png)

一旦*develop*收录了足够多的特性可以进行发布了，就要从*develop*派生一个发布分支。创建这个分支后就进入了发布工作的生命周期，除了基于本发布分支的bug修复、文档生成以及其它相关工作外，本分支不再接受任何新的特性。发布分支准备就绪后，发布分支将被合并到*master*分支并在其上打上带版本号的标签。另外，发布分支需要被合并进*develop*，以便把发布分支创建后产生的变更加入*develop*分支。

使用专门的分支进行发布的准备便于一个团队专注于当前的发布工作，而另一个团队继续在其它特性上继续开发为下次发布做准备。这个方式创造了清晰的开发重点（比如，很容易说“本周我们准备发布版本4.0”，也能在代码库的分支结构里切实看到这个重点）。

通用规则：

- 派生于: *develop*
- 合并到: *master*
- 命名规则: release-* or release/*

### 维护分支 ###

![](http://i.imgur.com/E3eOEBw.png)

维护分支（也成为热修复分支）被用来快速地为生产环节打补丁，这个分支只能从*master*分支派生，一旦修复工作完成，该分支将被同时合并回*master*和*develop*分支（如果已经有了一个发布分支，也要合并到发布分支中），然后*master*分支需要用新的版本号打上标签。

使用专门的bug修复开发分支，让开发团队在定位问题的时候不会被其他工作打断，或者不会被新的发布周期所阻碍。可以把维护分支当作热发布分支，它和*master*直接配合。

## 示例 ##

下面的示例演示了这个流程如果管理单个发布周期，这里假设你已经创建了中心代码仓库。

### 创建开发分支 ###

![](http://i.imgur.com/tlsLmYP.png)

第一件事情是补充一个*develop*分支，可以让一个开发人员从本地创建一个空的*develop*分支，然后推送到服务器：

    git branch develop
    git push -u origin develop

这个分支将会包含项目的完整历史，反而*master*只会包含缩减版本的内容，其他开发人员现在应当克隆中心库并创建*develop*的开发分支：

    git clone ssh://user@host/path/to/repo.git
    git checkout -b develop origin/develop

每个人现在都有了一个此历史性分支的本地拷贝。

### Mary和Jhon开始新的特性 ###

![](http://i.imgur.com/4XVr3GE.png)

Jhon和Mary工作在不同的特性上，他们都需要创建独立的分支开发各自的特性。这一次他们的特性分支必须从*develop*派生：

    git checkout -b some-feature develop

他们两个都向自己的分支提交了一些变更：

    git status
    git add <some-file>
    git commit

### Mary完成了特性的开发 ###

![](http://i.imgur.com/FLMOrjP.png)

在做了几个变更并提交之后，Mary认为她的特性已经完成了，如果她的团队在使用“拉取请求”，此时非常适合开启一个合并到*develop*的请求，否则的话她就可以把特性分支合并到本地*develop*并推送到中央代码库，像这样：

    git pull origin develop
    git checkout develop
    git merge some-feature
    git push
    git branch -d some-feature

第一个命令保证*develop*分支在合并本特性之前是最新的（注意：特性分支永远不要直接合并到*master*中），解决合并时可能碰到的冲突。

### Mary开始准备一个发布 ###

![](http://i.imgur.com/yzCKH1o.png)

当Jhon仍然在自己的特性上工作的时候，Mary开始准备她的第一个官方发布，她使用新的分支封装发布的准备工作，这一步同时建立了发布的版本号：

    git checkout -b release-0.1 develop

这个分支用来清理本次发布可能碰到的问题、完成测试、更新文档，以及其它需要的任何发布准备工作，这是一个专改进发布结果的分支。

一旦Mary把这个分支推送到中心库，本次发布的特性就固定下来了，任何还在*develop*中但没有准备妥当的功能只能延期到下一个发布周期了。

### Mary完成了发布 ###

![](http://i.imgur.com/JIBvr5h.png)

发布工作一旦准备就绪，Mary把发布分支合并到*master*和*develop*分支，然后删除本发布分支。把发布分支合并回开发分支相当重要，因为对发布分支的调整需要体现到后续的特性开发中。再一次，这是一个发起“拉取请求”进行代码评审的好机会。

    git checkout master
    git merge release-0.1
    git push
    git checkout develop
    git merge release-0.1
    git push
    git branch -d release-0.1

发布分支扮演了特性开发（*develop*)和公开发布（*master*）间的缓冲区的角色，只要想*master*合并内容，都应当对其打上标签以便于引用：

    git tag -a 0.1 -m "Initial public release" master
    git push --tags

Git提供了几个钩子脚本，当代码仓库内特定的事件发生时，对应的钩子脚本就能被执行。因此可以利用钩子完成自动发布，当向中心库的*master*分支推送内容或推送标签时，触发钩子完成部署。

### 最终用户发现了一个bug ###

![](http://i.imgur.com/uFgTdyT.png)

发布了当前版本后，Mary和John一起继续开发其它特性，为下一次发布作准备。突然，一个最终用户提交了一个当前已发布版本的一个bug，为了定位这个bug，Mary（或Jhon）基于*master*创建了一个维护分支，进行了相当一部分的工作，提交了不少的变更，然后把这个维护分支直接合并到*master*。

    git checkout -b issue-#001 master
    # Fix the bug
    git checkout master
    git merge issue-#001
    git push

跟发布分支一样，维护分支包含的重要更新也需要被包含到*develop*分支中，需要把维护分支合并进来，合并后就可以把维护分支删除了：

    git checkout develop
    git merge issue-#001
    git push
    git branch -d issue-#001

## 何去何从 ##

截至到现在，你肯定已经很熟悉“[中心式工作流程][4]”、“[特性分支工作流程][5]”和“Gitflow工作流程”，也掌握住了本地代码库、push/pull模式以及Git强大的分支和合并模型。

这里展示的工作流程是一个非常简单的示例，主要说明这个流程里的各方面工作，这些工作都不难而且很快就能完成，因此不要害怕去适应一个工作流程的方方面面，主要目标是让Git为你所用，而不是其它。



[1]: http://nvie.com/posts/a-successful-git-branching-model/
[2]: http://nvie.com/
[3]: /blog/2014/08/13/git-workflows-feature-branch/
[4]: /blog/2014/08/13/git-workflows-feature-branch/
[5]: /blog/2014/08/14/git-workflow-gitflow/
