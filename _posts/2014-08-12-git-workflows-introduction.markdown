---
layout: single
title: "Git典型工作流程介绍"
date: 2014-08-12 06:09:22 +0800
categories: Technology GIT
---

基于Git的开发流程有很多，本系列文章主要介绍几个企业级开发团队最常用的流程。请注意，这里设计出来流程说明只是一个指导性的原则，而不是真实的规则。向大家展示这些流程能够达到的目标，便于大家结合自己的实际情况对这些流程环节进行组合，形成最适合自己的新流程。

![](http://i.imgur.com/JSQSiVz.png)

<!--more-->

## 中心式工作流程 ##

![](http://i.imgur.com/4dKRz7H.png)

也称为SVN风格流程，如果开发人员对Subversion的操作方式很熟悉的话，中心式工作流程能够让团队既能体验Git的好处，又不用改变团队的工作协作方式。这个流程通常也是开发团队向GIT工作流程转换的第一站。

[详情»][1]

## 特性分支工作流程 ##

![](http://i.imgur.com/oXIMTmB.png)

特性分支工作流程基于中心式工作流程，用专门的分支把新特性封装起来，在把变更集成到正式项目之前，使用“拉取请求”发起对变更的讨论。（译者注：这里的Pull request不是指git里的pull操作，而是依托于Bitbuckt的一个特性，在不同开发人员间进行即时沟通）

[详情»][2]

## Gitflow工作流程 ##

![](http://i.imgur.com/hpYK26b.png)

Gitflow工作流程通过使用独立的特性开发分支、发布准备分支和维护分支简化了发布的周期，这是一个严格的分支模型，它向大型的项目管理添加了必要的协作结构。

[详情»][3]

## 交叉型工作流程 ##

![](http://i.imgur.com/JjgMoVy.png)

交叉型流程是分布式的工作流程，利用Git分支和克隆的性能优势，提供了安全、可依赖的途径管理大型团队的开发成员、接受来自未经验证的贡献者的提交。

[详情»][4]

## 拉取请求 ##

![](http://i.imgur.com/LfjOZxq.png)

拉取请求是Bitbucket的一个特性（经调查，Gitlab、Github都支持这个特性），可以让开发人员的协作更加容易，它们提供了一个友好的网页界面让开发者在集成代码到正式库之前对其进行讨论。

[详情»][5]

## 来源 ##

[Git Workflows][0]

[0]: https://www.atlassian.com/git/workflows
[1]: /git/git-workflows-centralized/
[2]: /git/git-workflows-feature-branch/
[3]: /git/git-workflow-gitflow/
[4]: /git/git-workflow-forking/
[5]: /git/git-workflow-pull-requests/
