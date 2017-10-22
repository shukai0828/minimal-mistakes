---
layout: single
title: "便捷地部署网站代码"
date: 2014-08-07 18:37:10 +0800
categories: Technology GIT
---

> 现在当我部署代码的时候，都会想起来我原来的部署方式是多么简陋。想到还有一些开发人员直接在生产环境使用命令行方式（比如VI/VIM）修改线上代码、或者通过FTP方式直接上传代码到线上，都让我不寒而栗。

就在不久以前，我跟小伙伴并排而坐进行开发，经常问对方“你在编辑这个文件么？”，或者问“上次我备份这个文件是什么时候？”。小伙伴经常回答“没有，我在编辑另外一个。”，或者“好像上周我门备份过了”。

那时我就在想，一定有更好的方式，一定有！

<!--more-->

## 使用GIT ##

在进行自动部署之前，让我们谈谈代码的“备份”。

首先保证我们在使用[GIT][1]，使用GIT的put操作就能完成备份工作。如果你担心直接使用GIT这个步子迈的有点大，可以先看看这个[入门指南][2]。一句话，GIT能够记录对项目所做的任何变化，通过GIT的“commit”方法保存个人的开发进展。可以把GIT想象成运输代码的“卡车”，从一个地方运输到另外一个地方。

> 可以在[这里][3]找到更多的git资源。
> 
> 读读“[成功的Git分支模型][4]”也很不错。

## Git不是 Github ##

很多开发人员刚开始搞不清git和github的区别，请注意：Git ≠ Github。

Github是一个激动人心的云服务，引领了开源和社交化编码的时代，其上被标记为“Public”的项目能够被复制出来或者直接在其上更改。

在开源项目之外，Github也是完美的部署工具，它提供了将项目部署到网站的服务。如果说Git是运输代码的卡车的话，Github则是运输行为的集散中心。

Github让我们能够在一个团队里共享代码、异步协同工作，允许我们通过它的界面对代码进行评审和编辑，也允许我们在其上创建任务并分配给同一项目内的其他伙伴。

> 如果不喜欢Github，也可以试试[Bitbucket][5]和[Beanstalk][6]。

## 基础的部署流程 ## 

从一个比较基础的开发流程说起吧，关键环节如下：

> 该流程假定你已经有了本地工作环境、一个Github账号和一个可以运行的测试服务器。


**Localhost <-> 推送到Github <- 在服务器上从Github上Pull**

我们可以对这个过程详细说明：

1. 将代码仓库pull到本地环境（使用Github、Gitbox、Tower等客户端工具）
2. 创建新的代码文件或者对现有文件进行修改
3. 将本地代码仓库push到Github
4. 登录到测试服务器并进入到合适的目录
5. 初始化git并添加这个Github代码仓库
6. 执行git的pull操作将代码pull到该目录
7. 通过浏览器查看代码修改的结果

## 部署到“云” ##

如果你跟我一样使用最原始的方法完成代码的部署工作，也一定自己做了很多服务器的搭建、Apache或Nginx的配置、安装PHP等方面的工作，同时也获得了一系列特殊的技能。真棒！不过不要再这么做了，这些工作靠一台机器就能完成。

使用[Forge][7]就能达到这个目的。

Forge提供神奇而便宜的服务，它不仅仅允许你通过[Digitial Ocean][8]、[Rackspace][9]和[AWS][10]配置和部署服务器，而且能够让你配置服务器使其将推送到Github上的改动自动部署到服务器上。如果你在使用Wordpress或者开发简单的HTML活着基于PHP的项目，Forge确实很适合你。

现在我们看看使用Forge部署服务器后的自动部署代码的流程：

**Localhost change -> Push to Github || Server is Auto-magicly™ updated by Forge.**

通过这个流程，你仍然必须建立自己的代码仓库、建立自己的本地环境、部署自己的服务器，但是一旦完成这些工作，对网站的更新只需要把变更push到Github就可以了。

## 其他自动部署方式 ##

Forge不是唯一的自动部署服务，还有很多其他选择：

- [Beanstalk][11]
- [FTPloy][12]
- [Deploy][13]
- [Springloops][14]
- [Wercker][15]
- [Octopus Deploy(.net)][16]
- [Capistrano(Ruby)][17]
- [Bamboo + Phing (PHP)][18]

如果自动部署不符合你的要求，或者你仍然依赖“post-hook”的部署方式，这里有一个简单的“PHP git 部署脚本”。

## 来源 ##

[Stop deploying websites the hard way!][0]

[0]: https://medium.com/@adammccombs/stop-deploying-websites-the-hard-way-2c499eab85ed
[1]: http://git-scm.com/ "git"
[2]: http://rogerdudler.github.io/git-guide/index.zh.html
[3]: http://bradfrostweb.com/blog/post/gitgithub-resources/
[4]: /blog/2014/08/09/a-successful-git-branching-model/
[5]: https://bitbucket.org/
[6]: http://beanstalkapp.com/
[7]: https://forge.laravel.com/
[8]: https://www.digitalocean.com/
[9]: http://www.rackspace.com/
[10]: http://aws.amazon.com/
[11]: http://beanstalkapp.com/
[12]: http://ftploy.com/
[13]: https://www.deployhq.com/
[14]: http://www.springloops.io/
[15]: http://wercker.com/
[16]: https://octopusdeploy.com/
[17]: http://capistranorb.com/
[18]: https://www.atlassian.com/software/bamboo