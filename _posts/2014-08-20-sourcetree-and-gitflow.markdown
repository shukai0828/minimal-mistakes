---
layout: single
title: "SourceTree里GitFlow的使用"
date: 2014-08-20 14:07:58 +0800
categories: Technology Management
---

这几天看详细了一下[Git Flow][1]的模型介绍，感觉“很好很强大”，这个开发模型利用GIT的易于分支和合并的特点，能够比较容易地将开发、发布、部署、bug修复分隔开来。

![](http://i.imgur.com/HToPAkV.png)

正准备在自己的团队内部推广使用，比较担心的是管理工作稍微繁琐一点。操作倒不复杂，只是需要头脑清醒，熟悉不同分支间的派生、合并关系和时机。没想到“正在瞌睡的时候送来了一个枕头”，正在使用的[SourceTree][2]工具天然支持Git Flow，而且把这个模型的各种操作通过菜单命令的形式提供了出来，大大减轻了使用人员的学习使用成本。

为了能让大家少走弯路，也便于我尽快熟练使用，我进行了摸索和实践，下面把具体的使用方式记录下来供参考。

<!--more-->

## 入口 ##

![](http://i.imgur.com/4h7uelE.png)

SourceTree界面顶部的工具栏按钮，从右至左第二个“GIT工作流”图标就是。

## 初始化 ##

为了便于展示，我在自己的一个Github项目上进行相关的操作。进入这个项目，然后点击刚刚的“GIT工作流”工具栏按钮。

![](http://i.imgur.com/DexaxaI.png)

建议不用做任何改动，直接“确定”即可。SourceTree会自动化进行一些操作，最明显的变化是项目代码库里自动增加了一个*develop*的分支。

![](http://i.imgur.com/17tTVb0.png)

将新创建的*develop*分支推送到远端仓库。

![](http://i.imgur.com/dEZNRGn.png)

我们到Github里看这个项目的分支关系图如下：

![](http://i.imgur.com/vMQBUHh.png)

从此，代码库里就存在了两个永久性的分支：*master*和*develop*，未来所有的开发工作都围绕这两个分支进行派生跟合并。派生和合并的时机、源分支、目标分支跟具体的开发类型有关，Gitflow里有明确的规则，如果纯粹使用命令行工具的话，需要牢记这些规则并正确执行。而SourceTree则把这些规则用具体功能自动化实现了，这样就能减少人为的失误，至少在我刚开始手动完成这些操作的时候，失误的几率还是挺高的。

从初始化的第一个界面中，还有三类分支的命名规则：feature、release、hotfix，这就是未来承接具体开发工作的分支类型，从名称中就能准确把握他们的用途。

## 创建分支 ##

上面提到，项目里有两个永久的分支：*master*和*develop*。这两个分支也被称为“历史性”分支，在其后的开发工作中，Gitflow模型支持在feature、release、hotfix分支上折腾，这样也有效避免了不同类型的开发工作在代码层级的耦合和干扰。

这三个分支的用途、派生来源分支和合并目标分支如下：

**feature**，功能开发分支，用于承接具体功能需求的开发

 - 派生于*develop*
 - 合并于*develop*

**hotfix**，bug修复分支，用于解决线上运行环境发现的bug

 - 派生于*master*
 - 合并于*master*、*develop*

**release**，版本发布分支，用于完成发布准备的

 - 派生于*develop*
 - 合并于*master*、*develop*

跟“历史性”分支相反，这三类分支都是短期分支，针对他们的工作内容完成后，一般都要进行删除。工作内容完成的标识有两个：开发完成、合并完成，缺一不可。

### hotfix ###

正式环境正在运行的项目发现了一个bug，需要创建hotfix分支进行bug修复，在已经做过Gitflow初始化的项目上点击工具栏上的“Gitflow”按钮，出现如下窗口：

![](http://i.imgur.com/kxUv8Zw.png)

但有时候我们已经在库里创建了一个还未完成的分支，就会看到这个窗口：

![](http://i.imgur.com/qBxGzok.png)

点击“建立新的修复补丁”：

![](http://i.imgur.com/Tc42jkp.png)

输入自己想要的分支名称，“确定”即可创建，从预览图中可以看到分支派生来源以及派生出来的分支信息。这里有一点要注意，hotfix分支的名称在最终合并到*master*时会自动变成标签，而一般来说，*master*的标签往往标识版本号，比如1.0.0，所以这个分值名称最好能够按照版本规则来命名，比如1.0.1。

hotfix分支创建完成后，在分支目录结构里能够看到它：

![](http://i.imgur.com/8r1g04g.png)

经过一系列艰苦卓越的工作，这个bug修复完成了，就要合并到*master*和*develop*，在SourceTree里，只要通过点击“Gitflow”工具栏即可指导你完成：

![](http://i.imgur.com/7ndxlDy.png)

点击第一个“完成修复补丁”：

![](http://i.imgur.com/VcQpvuQ.png)

这个操作默认“删除分支”，将把本地的修复分支删除掉，因为它的使命已经完成。合并时如果出现冲突，还是需要开发人员自行解决的。这时能够在“预览”位置看到这个hotfix分支的合并关系示意图。

合并后将代码库推送到远端，这时在Github上可以看到这些分支的关联关系：

![](http://i.imgur.com/yTfR9Nv.png)

中间的蓝色分支既是这个hotfix分支，从图中可以看到这个分支先后被合并到了*master*和*develop*上。因为我没有把这个分支提交到Github，所以只有图形示意，没有类似*master*和*develop*的名称指示。

### feature ###

当接到具体的功能需求时，开发人员需要派生feature分支，在SourceTree上，派生的工作相当简单，只要在已经做过Gitflow初始化的项目上点击工具栏上的“Gitflow”按钮，出现如下窗口：

![](http://i.imgur.com/kxUv8Zw.png)

点击第一个按钮“建立新的功能”：

![](http://i.imgur.com/tF9WRGW.png)

这里，“功能名称”的值不像hotfix分支那样需要考虑发布版本号的规则，可以用能够清晰描述功能特征的文字。

创建完成后，SourceTree的项目目录树就出现了这个新分支，如下：

![](http://i.imgur.com/c048xXb.png)

现在就可以在这个分支上修修补补了，经过一系列艰苦卓越的工作，这个功能开发完成了，就要合并到*develop*。功能分支只能合并到*develop*，为新的*release*分支做准备，

在SourceTree里，只要通过点击“Gitflow”工具栏即可指导你完成：

![](http://i.imgur.com/24yR5cz.png)

![](http://i.imgur.com/U13kA6q.png)

“删除分支”和“预览”和上面hotfix分支里提到的效果是一样，所不同的是，这里没有“推送变更到远程仓库”的选项。有没有这个选项的差别不大，可能是为了体现分支的推进的紧急程度吧。

合并完成后并将本地库推送到远端Github，这时看Github的分支关系网络图如下：

![](http://i.imgur.com/JMK442h.png)

因为我在做的这个示例操作的时候，并没有再对*master*分支进行过派生和合并，所以看到*master*的指示停在原地，后面并入*develop*的黑色分支既是功能分支。

### release ###

所有特性的开发工作都是为了产品功能的更替，当一个或多个特性开发完成，可以进行发布了，就要准备创建release分支。

![](http://i.imgur.com/P3lXRAe.png)

Release分支是为上线做准备的，它的命名也要遵守项目的版本命名规则，这个名字在最终合并到*master*时会自动变成版本标签。

这个release分支也是测试工作的目标对象，，经过一系列艰苦卓越的测试、调整工作，这个release完成了，达到了可上线的状态，就要合并到*master*和*develop*。

![](http://i.imgur.com/43mp0VS.png)

![](http://i.imgur.com/BkNDCMt.png)

如图“预览”所示，这个release的名称会自动成为*master*的标签。

这一次，我有意没有删除这个分支，并把它推送到了Github，在Github上的分支关系网络图中，能够明确地看到release的起点和终点，它最终被合并到了两个“历史性”分支中了。

![](http://i.imgur.com/pEUVTKJ.png)

## 收尾 ##

三类临时性分支中，hotfix和release的结果都要合并到*master*和*develop*中，为什么？因为它们的修改结果持续影响这后续的开发和维护，必须合并以保证代码的一致性。

至此，SoureTree的Gitflow应用教程已经完成，如果你没有认识到这个特性很有用，我只能说：好吧，你的开发工作还没有复杂到一个程度，一个必须要规避代码干扰、保证并行推进的程度。

对于小型项目和团队来说，基于GIT的[中心式协作模型][3]和[特性分支模型][4]就足够了；本协作模型适合中型、大型项目和团队；对于更大型的团队和项目来说，除了这个协作模型，还有一个[交叉型协作模型][5]可供管理使用。



[1]: /blog/2014/08/09/a-successful-git-branching-model/
[2]: http://www.sourcetreeapp.com/
[3]: /blog/2014/08/12/git-workflows-centralized/
[4]: /blog/2014/08/13/git-workflows-feature-branch/
[5]: /blog/2014/08/14/git-workflow-forking/
