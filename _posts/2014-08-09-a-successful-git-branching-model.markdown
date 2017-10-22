---
layout: single
title: "成功的Git分支模型"
date: 2014-08-09 16:50:58 +0800
categories: Technology GIT
---

事实证明Git的分支模型设计非常成功，今天我在这里对其进行解读，不涉及任何项目开发上的细节，仅仅谈论Git的分支策略和发布管理。

![](http://i.imgur.com/HToPAkV.png)

<!--more-->

上图充分说明了Git在我们自己项目上进行源代码管理的作用。

## Why git? ##

在网上有非常多关于GIT和集中式源代码管理系统的对比和讨论，作为开发人员，我更喜欢GIT。Git确实改变了开发人员进行代码合并和分支的思考方式，在经典的CVS和SVN的世界里，合并/分支操作总是让人有点发怵，这些操作都尽量少做为妙。

但如果使用Git，这些操作相当简单和廉价，可以被认为是日常核心工作流程之一。在CSV和SVN的书籍里面，分支和合并的说明总是在后面的章节被提及（为高级用户而设），但是在Git的[参考书][1]里，这些操作说明在第三章就被提及了（基本知识）。

由于Git的分支和合并非常简单易用，具备可重用性，这些操作不再令人谈虎色变，分支跟合并行为真正变成了版本控制工具的助力手段而不是其它。

说了这么多Git工具的好处，让我们把重点放到基于Git的开发模型上来。我将在这里讲解的开发模型不再是团队成员必须遵守的一套软件开发过程的步骤，原来的这个步骤的目的是使软件开发过程变得易于管理。

## 分布而不是集中 ##

我们使用的、在这个分支模型下工作良好的代码仓库有一个“真正”的中心库，需要注意到的是这个中心库被认为是唯一的中央仓库（由于Git是DVCS类型，从技术层面来看是没有所谓的“中心”参考的），我们将把这个中央仓库作为“源”（**origin**），这个名字对所有的Git用户来说都很熟悉。

![](http://i.imgur.com/TmyLTr2.png)

每个开发人员从origin拉取（pull）或向origin推送（push）代码。但除了从中心pull或push之外，每个开发人员还可以从其他团队成员那里pull变更。比如，当两个以上的开发人员同时在一个大型的新特性上进行开发时，能够直接从对方那里获取代码变更时很有用处的，这样避免过早地向**origin**推送不成熟的代码。在上图中，同时有几个子团队，分布是Alice和Bob、Alice和David、Clair和David。

Alice定义一个Git的remote，命名为**bob**，指向Bob的本地仓库，反之依然，从技术角度看这样做并没有什么特殊的意义。但结合后面对分支模型的解读，我们就发现这些分支的灵活创建魅力无穷。

## 主要分支 ##

![](http://i.imgur.com/zUipe3M.png)

上图揭示了这个开发模型的核心，中央仓库永久保存了两个主要分支：

- **master**
- **develop**

每个Git用户都很熟悉存在于**origin**的**master**分支，跟**master**分支平行的另外一个分支被称为**develop**分支。

我们把**origin/master**作为反映“生产就绪”（*production-ready*）状态的**HEAD**所在的主要分支。

我们把**origin/develop**作为反映“下次发布”状态的**HEAD**所在的主要分支，这里保存着最新交付的开发变化。有的人也把它称为“集成分支”，作为每日自动构建的代码源。

当**develope**分支的源代码趋于稳定而且可以进行发布，该分支的所有代码变更应当合并回**master**分支，并被打上标签，该标签通常是一个发布版本号。

因此，每当变更被合并回**master**分支，一个新的生产环境版本同时被定义出来了。我们建议在这一点上要严格管理，这样的话，每次在**master**上的提交操作，理论上我们都能够使用Git的钩子脚本自动构建和部署软件到生产服务器。

## 辅助分支 ##

在主要分支**master**和**develop**之外，我们的开发模型使用了各种辅助分支来帮助团队成员完成并行的开发任务，比如跟踪特性的开发、为生产环境的发布做好准备、协助快速解决生产环境的问题等。跟主要分支相比，这些辅助分支的生命周期很短暂，最终应当被删除。

我们可能使用的辅助分支类型如下：

- Feature（特性）分支
- Release（发布）分支
- Hotfix（热修复）分支

每个辅助性分支都有特定的目的，并遵循严格的规则，哪个分支是它的源分支，哪个分支是它的合并目标都要明确定义。稍后我们再逐一详细说明。

从技术视角来看，这类分支并没有什么特殊的地方，它们只是被我们按照用途进行了分类，它们仍然是Git的简单的分支。

### Feature（特性）分支 ###

![](http://i.imgur.com/dgQ8CF2.png)

- 必须来自于: **develop**
- 必须合并到: **develop**
- 分支命名规范: feature-*

特性分支（有时也被称为主题分支）被用来为未来的发布开发新特性，新特性的开发工作开始时，这个特性是否能够在目标发布里被包含进去还不明确。特性分支的意义是随着开发工作一直存在，但最终会被合并到**develope**分支里（新特性被将来的发布收纳）或被**develop**分支忽略（也许新特性的体验并不好）。

特性分支应该仅仅存在于开发人员的代码仓库里，不应该放入**origin**。

#### 创建特性分支 ####

开始一个新特性时，请从**develop**分支创建特性分支：

    $ git checkout -b myfeature develop
    Switched to a new branch "myfeature"

#### 在develop分支上合并完成的特性分支 ####

开发完成的特性可以被合并到**develop**分支，以便添加到新的发布中去：

    $ git checkout develop
    Switched to branch 'develop'
    $ git merge --no-ff myfeature
    Updating ea1b82a..05e9557
    (Summary of changes)
    $ git branch -d myfeature
    Deleted branch myfeature (was 05e9557).
    $ git push origin develop

选项**--no-ff**的作用是在合并时创建一个新的提交对象，不使用该选项的话，合并行为会执行“快进”（fast-forward）操作。该选项避免丢失特性分支存在过的历史记录，以及在其上的提交信息记录。比较下图：

![](http://i.imgur.com/lhwVwAS.png)

在后一种情况下，从Git历史中是不可能轻易看到哪些提交的对象里实现了一个特性，你不得不通读日志信息进行判断。此时，恢复一个完整的特性（比如一组commit）是让人很头痛的，相反，如果使用**--no-ff**选项则能够很轻易地达到这个目的。

使用这个选项会创建额外少许的空的提交对象，不过这样做却物超所值。

不幸的是，我还没有发现一个使**--no-ff**成为默认选项的**git merge**方式，不过我们真的应该这么做。

### 发布分支 ###

- 必须来自于: **develop**
- 必须合并到: **develop** 和 **master**
- 分支命名规范: release-*

发布分支为新产品发布的准备提供支持，也允许进行较小的bug修复、为发布准备元数据（版本号、构建日期等），在release分支做这些工作，**develop**分支得以保持“整洁”以接受下一个发布的新特性。

何时从**develop**分支创建release分支？当开发工作进展到可以进行发布的状态，至少所有的目标特性都被合并到**develop**分支中了，才可以创建release分支。更未来的新发布的目标特性则不用合并，它们必须等到当前的release分支创建后才允许合并到**develop**中。

确切地说，在创建release分支的时候，这个发布会被赋予一个版本号。此时，**develop**分支开始反映下一个发布的代码变化，但还不清楚下一个发布会最终变为0.3还是1.0，直到新的release分支被创建为止，release版本号的数字依赖项目的版本命名规则在创建发布分支时确定。

#### 创建发布分支 ####

发布分支从**develop**分支上创建。比如，版本1.1.5是当前产品的已发布版本，而且我们即将进行一次大的发布。**develop**的状态也为发布做好了准备，我们决定新发布的版本号是1.2，因此我们给从**develop**上创建出来的release分支用新的版本号进行命名：

    $ git checkout -b release-1.2 develop
    Switched to a new branch "release-1.2"
    $ ./bump-version.sh 1.2
    Files modified successfully, version bumped to 1.2.
    $ git commit -a -m "Bumped version number to 1.2"
    [release-1.2 74d9424] Bumped version number to 1.2
    1 files changed, 1 insertions(+), 1 deletions(-)

创建新的分支并切换到其上后，我们增加了版本号。这里的bump-version.sh是虚构的shell脚本，用来改变工作目录下的部分文件来反映新版本的。（也可以手动进行改动，主要目的就是改变一下反映版本变化的文件），然后提交增加的版本号。

这个新分支可能会存在一段时间，直到本分支的目标达成为止。在此其间，bug修复的结果将提交到本分支（而不是**develop**分支）。在这个分支上添加大的新特性被严格禁止，新特性必须被合并到开发分支，等待下一个发布的到来。

#### 终结发布分支 ####

当发布分支的状态达到真正的发布状态，需要执行一些动作。首先，将发布分支合并到**master**（请牢记：**master**上的每一个提交都被定义为一个新的发布）。然后，在**master**上的提交必须被打上标签，以便未来对这个历史版本进行引用。最后，需要将在这个发布分支上所做的改动合并回**develop**分支，以使未来的发布也包含了这些bug修复的内容。

先在Git中操作头两步：

    $ git checkout master
    Switched to branch 'master'
    $ git merge --no-ff release-1.2
    Merge made by recursive.
    (Summary of changes)
    $ git tag -a 1.2

本发布已经完成，并打上标签供未来引用。
> You might as well want to use the -s or -u <key> flags to sign your tag cryptographically.

为了留住在release分支中所做的改变，我们需要将release分支合并回**develop**，在Git中如下操作：

    $ git checkout develop
    Switched to branch 'develop'
    $ git merge --no-ff release-1.2
    Merge made by recursive.
    (Summary of changes)

这一步可能导致冲突（或更严重，因为我们已经更改了版本号），出现冲突的话就修复并提交吧。

此时我们的工作真的就做完了，可以将release分支移除了，因为我们不在需要它了。

    $ git branch -d release-1.2
    Deleted branch release-1.2 (was ff452fe).

### 热修复分支 ###

![](http://i.imgur.com/lTGX9yO.png)

- 分支来自于: **master**
- 必须合并到: **develop** 和 **master**
- 分支命名规范: hotfix-*

Hotfix分支跟release分支非常相似，都是为新的产品发布做准备，不过hotfix分支是计划外的操作。这些分支用于响应来自生产环境不可预期的状态，需要尽快对这些状态进行修复。当严重的bug出现时在生产环境时，也需要立即解决。Hotfix分支是从**master**上相应标签处分支出来的，这个标签对应着当前生产环境的版本。

此类分支的实质是让其他团队成员的工作能够继续，而由另外专门的人员准备进行快速的生产环境修复。

#### 创建热修复分支 ####

Hotfix分支从**master**分支上创建。比如，假设版本1.2是当前产品的发布版本，正在运行但是由于出现了bug导致服务出错，但是在**develop**上的变更还没有稳定下来，因此迫切需要创建hotfix分支修复问题：

    $ git checkout -b hotfix-1.2.1 master
    Switched to a new branch "hotfix-1.2.1"
    $ ./bump-version.sh 1.2.1
    Files modified successfully, version bumped to 1.2.1.
    $ git commit -a -m "Bumped version number to 1.2.1"
    [hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1
    1 files changed, 1 insertions(+), 1 deletions(-)

创建分支后，不要忘记在某些文件上标记版本号。

然后，修复bug并提交修复的结果，也许需要多次提交才能修复完成。

    $ git commit -m "Fixed severe production problem"
    [hotfix-1.2.1 abbe5d6] Fixed severe production problem
    5 files changed, 32 insertions(+), 17 deletions(-)

#### 终结热修复分支 ####

修复工作完成后，修复的结果需要合并回**master**。同时需要合并回**develop**，这是为了这个bug在下一个发布时也得到了修复。这些操作跟release分支结束后的操作完全类似。

首先，更新**master**并未这个发布打上标签：

    $ git checkout master
    Switched to branch 'master'
    $ git merge --no-ff hotfix-1.2.1
    Merge made by recursive.
    (Summary of changes)
    $ git tag -a 1.2.1

> You might as well want to use the -s or -u <key> flags to sign your tag cryptographically.

然后，在**develop**中包含修复的结果：

    $ git checkout develop
    Switched to branch 'develop'
    $ git merge --no-ff hotfix-1.2.1
    Merge made by recursive.
    (Summary of changes)

有一个特殊情况，当生产环境对应的release分支还存在的话，修复的结果需要合并到对应的release分支中来，而不用合并到**develop**。release分支的终结合并操作会最终将修复结果体现到**develop**中。（如果在**develop**中的工作需要立即包含这个bug修复的结果，而且登不上release分支的终结，你也可以安全地合并这个修复到**develop**中）

最后，删除这个临时的分支。

    $ git branch -d hotfix-1.2.1
    Deleted branch hotfix-1.2.1 (was abbe5d6).

## 总结 ##

这个分支模型并没有什么特别让人吃惊的新概念，本文开头的那个“巨大图片”在我们的项目中起到了巨大的作用，它构建了一个简洁的智力模型，让团队成员易于理解，让他们在共识的分支和发布流程中协同工作。

## 来源 ##

[A successful Git branching model][0]

[0]: http://nvie.com/posts/a-successful-git-branching-model/
[1]: http://git-scm.com/book
