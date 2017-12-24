---
layout: single
date:   2017-12-23 21:47:10
title:  "BitKeeper姻缘了断"
header:
  overlay_image: http://7xil47.com1.z0.glb.clouddn.com/common_title_git.jpg
  overlay_filter: 0.5
categories:
  - Development
  - GIT
---

纯转载，本文应该写在2005年之前，讲述了Linux团队和Bitkeeper的爱恨情仇，以及GIT的诞生。GIT因Bitkeeper启发而起，以Bitkeeper为目标仿效，最终几乎完胜Bitkeeper，纵观前因后果，GIT可称“枭雄”。

-------------

维持数年的BitKeeper与Linux的关系最终还是落入了好莱坞明星婚姻式的结局。他们曾经相得益彰，最后却走到这个遗憾的地步。kerneltrap这篇[Feature: No More Free BitKeeper](http://kerneltrap.org/node/4966)做了一个完整的回顾。（与原文有改编）

![Larry McVoy](http://7xil47.com1.z0.glb.clouddn.com/bitkeeper.jpg)

1999年12月，Linux PowerPC项目首先开始使用BitKeeper－－这个非开源但是有条件免费的版本控制工具。到了2002年2月，Linux之父Linus Torvalds已经决定开始用它来管理Linux内核代码主线－－Linus对BitKeeper的评价是the best tool for the job－－这是个最后在free和open source社区甚至之外都引起了广泛瞩目的举动。

![Linus Torvalds](http://7xil47.com1.z0.glb.clouddn.com/LinusTorvalds.jpg)

BitMover是BitKeeper的开发厂商，创始人和CEO是Larry McVoy。Larry期望BitKeeper 能帮助Linus 免于陷入不断加重的Linux内核管理工作中－－事实上，自从Linus 3年前开始使用BitKeeper之后，Linux的开发步伐加快了两倍。

![Larry McVoy](http://7xil47.com1.z0.glb.clouddn.com/Larry_McVoy.jpg)

Free Software这个短语中的Free可以被理解成“自由”，好像Free as in Freedom中的Free－－或者仅仅是“免费”，Free as in Free Beer中的Free。BitKeeper是按照后一种定义免费可用的。允许free/open source软件开发者不用付费就能使用这个工具－－前提是这样一个协议：这个免费工具的真正使用者不能同时开发其竞争产品。换句话说，这个工具可以免费使用freelyused，不过不能被随便克隆freely cloned。当然同时，BitMover还有一个更高级的BitKeeper版本是商业产品，需要购买。这两个版本都是BitMover的知识产权。

也有人一直反对Linus使用BitKeeper－－想想看，Linux是free/open source软件的旗舰产品，却在使用一个非free/open source软件－－GNU的传奇人物，那个看上去和耶稣有几分相似的大胡子Richard Stallman就是其中之一。他严厉地批评Linus决定使用非自由（free as in freedom）软件。然而，大多数人都承认，目前也没有哪个free工具具有BitKeeper那样的功能－－它提供了真正的分布式开发能力。这些年来，对BitKeeper的一些功能进行反向工程的举动引起了BitMover的多次注意和警告，最近两次这样的动作则最终导致了BitMover决定终止结束免费BitKeeper产品的开发和应用。

![Richard Stallman](http://7xil47.com1.z0.glb.clouddn.com/RichardStallman.jpg)

－－**Best Tool For The Job**

Linus 使用BitKeeper后不久，他就宣称BitKeeper是Best Tool For The Job。和大多数需要需要中心仓库来保存主副本master copy的版本控制工具不同－－我想作者是指CVS，SubVersion等－－BitKeeper允许真正的分布式开发，每个人都拥有自己的主副本master copy。此外，大多数版本控制工具把单个文件，甚至同一文件的一个部分看作独立的实体。如果A和B文件都做了修改，它们允许只合并B文件。而在BitKeeper里，这是不允许的－－要应用B的变更，你必须也先应用A的变更－－这就是BitKeeper的概念，这个设计保证了两个仓库相互间是完全的复制品。

Larry McVoy说Linus刚开始用BitKeeper时也觉得十分不适亦有抱怨。使用BitKeeper之前，Linus的做法是逐条细看每个patch，可能得自己提取出他需要的部分－－有了BitKeeper后，这个工作就更困难了。但是，最终结果是，一些子系统的维护者被赋予了更大的信任，而Linus就能一改逐行检查patch为逐个分类地检查了。Linus更信任的维护者只需要很少的审查，而那些信任稍低些的维护者的代码就要多留心。这个过程保证了代码的高质量，同时也成倍地加快了Linux的内核开发进程。

－－**Free Versus Free**

尽管有生产率上的提高，有人仍然坚持反对Linux选用非open source或者非free as in freedom产品－－所以，提供一个类BitKeeper的open source产品的动作很严肃地开始了。这又继而导致了Linux内核邮件列表上的激烈争论，Larry试图解释他很高兴让Linus和其他开发者在内核开发过程中使用他们的产品－－但是谁也不能对其进行反向工程。最近，两个这样的反向工程项目让BitMover开始怀疑/审视自己商业模式中的free部分。更坏的是，其中一个项目是由OSDL－－影响颇大的open source团体同时也是Linus Torvalds 的老板－－“无意”间赞助的。
Larry 说，OSDL资助的一个无关项目正在对BitKeeper协议进行反向工程。双方大约7周之前开始讨论如何处理这一状况，他们甚至已经达成了口头约定－－涉及反向工程的该项目成员应当停止其举动。然而，那之后，反向工程的行为又开始了。尽管OSDL并没有为反向工程直接买单－－他们资助的项目确实是有其明确的，与BitKeeper无关的，目的的－－但是事实是，他们也确实雇佣了一个在开发竞争产品的雇员，这是免费的BitKeeper许可所不允许的。 Larry说，OSDL本来有机会解决这个问题的，可是他们只是耸耸肩，说it’s not my problem。这成了压垮骆驼的最后一根稻草。考虑到眼下的情况和此前各种举动对每年要花费50万美元进行免费BitKeeper的开发和支持的BieMover造成的伤害，他们最终作出了一个困难的决定，计划逐步停止免费版本的支持和应用。

－－**What Went Wrong**

Larry 在解释这个决定时说“这是open source社区面临的真正问题，没有比这更失败的了”。作为一个长期的open source狂热者和BitMover的CEO，“我们作为一个商业组织已经尽可能地做到了你能想像到的最大程度的open source友好”。那些使BitMover终止免费产品的举动可能也会使其他公司打消让其产品支持Linux的想法。Larry还说，以前在SUN他们 有句话“it’s the apps, stupid”，意思是，那些“我的OS比你的OS要好”的争论和梦话都毫无意义，让你的平台上有尽可能比其他平台多的应用才是硬道理－－这对Linux 也适用。问题的关键是，不会有公司对把他们的应用移植到某个平台感兴趣－－如果这个平台上的家伙们的既定目标和已有记录表明他们一发现有用的东西就对它做反向工程的话。从某种程度上说，open source世界需要做出选择：要么接受商业软件并且承认商业公司有资格从自己的劳动中获利的事实，要么组织起来，开始提供“所有”应用程序的open source版本应对挑战。

Larry说，我们的状况是，没有机会改变open source社区的举动。和海军陆战队不同，open source社区更喜欢用‘不是我的问题’来忽视那些坏苹果（鸵鸟？），而海军陆战队则会为几个坏家伙的行为惩罚整个团队，然后很快坏家伙就会不见。可能 有一天这种状况会改善，不过到那一天之前我们不得不按照对生意有意义的原则行事－－而让open source guys使生意无法继续显然是没有意义的。
有些人会把终止免费BitKeeper视作它的失败，Larry可不这么想。Linus 也说，使用BitKeeper的3年已经对工作流程产生了深远的影响。他不再是关注于逐个patch的变更，而是和其他受信任的开发者一道在更高更有效率的层次上工作。
这不是个容易的决定，Larry说，我想提供帮助，然而部分open source社区的攻击让他感到heartbreaking－－只是因为我不能提供一种politically correct的方法来帮助大家。You have no idea how miserable that has been。

－－**What Next**

在此后3个月，BitMover打算逐步终止免费的BitKeeper产品。已经有人提供了一些资金帮助一些核心的内核开发者购买商业版本的 license－－不过Linus Torvalds不在其中。Larry 表示，“如果Linus和Andrew （我想他是说Andrew Morton）到其他地方，我们很愿意为他们提供license”－－弦外之音的背景是这两人现在都是OSDL的雇员。

除此之外，这次事件牵扯到的所有人也都明白，BitMover为支持免费产品而得到的回报也会没有了。开始时，作为提供服务和产品的回报，BitMover获得了足够的市场占有率和bug报告。Linus对单方面交易不感兴趣－－即便他在胜利者一方－－仍然决定从BitKeeper迁移，现在他正在寻找替代品。

Larry提到内核代码树仍将由BitKeeper追踪，许多内核开发者专为此目的购买了商业版的license。其中包括很多对Linux提供 contribution的大公司雇员，比如IBM, Intel, HP, Nokia 和 Sun，还有很多小公司亦然。

至于使用什么工具替代BitKeeper管理内核开发，目前还没有决定。现在已测试的几个工具中，没有一个被选中，至少没有哪个提供了足够的性能和功能。Larry提到Linus试用了monotone，但是导入不包含版本信息的文件就花了2个小时，太慢了－－Darcs也是。不过最终，总会选用某个工 具，然后开发之使其满足内核开发者的需要。

Larry 认为这个工具只需要在Linux运行即可，不需要支持其他平台例如Solaris或者Windows，并且因此可以利用某些Linux特有的特性，做到运行如飞。实际上，Linus已经开始编写一个利用Linux性能特性的非常简单的工具－－这个努力得到了Larry的辅助，他说这对BitMover也有益，因为一个Linux-only的工具不会是BitKeeper的竞争者－－它的最大市场在Windows。

－－**BitKeeper versus the Open Source Replacement**

随着BitKeeper的退出，肯定有些其他的项目打算进入并取代其Linux内核版本控制工具的位置。当被问到是不是担心这最终会导致产生一个和BitKeeper竞争的项目时，Larry说，当然担心－－除非他们是白痴。不过，他也指出由于一些原因，这种状况不大可能出现。在维护两个产品的过程中，他很惊奇地认识到open source社区的需求和商业社区的是如此的不同。纵然有些重叠，不过两个社区完全是按两个方向前进的。

一个例子就是二进制管理。在Linux内核中没有多少二进制文件，对这些文件的修改就更少。Larry举例说，比如有个jpeg图像作为logo，大约一年才更新一次，没有多少需求期望实现复杂的二进制管理。相反，在商业社区，二进制管理是至关重要的。例如某人在追踪一个1M的word文档，它可能经过了几百个版本，会占用1G的空间。因为这项功能在商业用户中很常见，BitKeeper就很重视改善这种功能，显然，Linux内核相关的open source解决方案就不会有这种需求。

此外，Larry说，其实BitKeeper没有什么杀手级的功能killer feature使其如此优秀。BitKeeper只是成百上千的使其如此优异的小功能的集合。他说他希望将来会有可行的open source解决方案出现，在那之前，商业版BieKeeper的用户会把各种功能带入open source社区。其实，在讨论要替代BieKeeper的工具应该具备那些功能特性时，wishlist很快就成了BieKeeper的功能列表。

－－**Last Free BitKeeper Release**

今年2月Linux内核邮件列表的一封信件中，Larry讨论了目前免费产品中的16位限制。目前内核代码树主线中有大约64000个changeset，以后的开发会很快超过这个限制。出于这个原因，BieMover可能会提供免费BieKeeper的最后一个release，允许内核开发者完成最终的转换。到7月底，期望能完成迁移，继而结束免费产品并着重于商业版。

由于免费和商业版本有巨大的需求差异，放弃免费版本的决定将对商业产品有益。Larry说，按1:10的比例算，我想我们是4。当然其他产品是3或者更低，只是他们自己不承认。我们已经有了一个功能特性路线图，将使我们达到8的程度。今后3到5年我们会着重于实现这些特性。

**Summary**

使用BieKeeper管理Linux的3年对Linux开发过程十分有益。它带来了良好的源码控制习惯，为内核开发者提供了更有效的管理分布式开发的方法。

关于是Free as in Freedom 还是 Free as in Free Beer的争论离结束的日子还远得很，我们可以肯定，当从BitKeeper迁移出的时候，在Linux内核邮件列表上又会有长长的争论。今天还没有明显的替代品，不过已经有项目开展起来打算满足内核开发者要求的时候，这个目标就只是时间问题了。既然内核曾经由一个高质量的源码控制产品维护，那么将来应该也会如此，只要有需要，就会有解决方法。柏拉图的话最适用，necessity is the mother of invention，创造来自需求。


 
from http://zhouxiaohu.blogbus.com/logs/1115748.html

————————————————————————————

BitKeeper 是a 软件 工具为 修正控制 (配置管理 SCM 等) 计算机 原始代码． 一个老练分配系统, BitKeeper 主要竞争反对其它专业系统譬如 合理的ClearCase 并且 强迫地． BitKeeper 由BitMover Inc. 生产, 私有公司根据 旧金山, 加利福尼亚 并且拥有 CEO 拉里・McVoy, 谁早先设计了 TeamWare．

BitKeeper 修造在许多TeamWare 概念。 它的关键卖点是分布开发小组能保留他们自己的地方来源贮藏库和仍然工作与中央贮藏库的舒适。

BitKeeper 是闭合来源产品和通常被卖或被出租(作为支持包裹一部分) 对中等或大公司。 精确费用随单独顾客变化, 但每开发商费用估计是一千美元。


BitKeeper, Linux 和自由版本为开放来源项目
BitMover 过去经常为确定提供存取对于系统 开放来源 或 免费软件 项目, 最著名(和有争议) 是原始代码 Linux 仁． 执照为BitKeeper 的”社区” 版本考虑到开发商免费使用工具为开放来源或免费软件项目, 假设那些开发商没有参加一个竞争的工具的发展(譬如 CVS, GNU 曲拱, 颠覆 或 ClearCase) 为BitKeeper 他们的用法的期间加上一年。 这个制约申请是否竞争的工具是开放自由或业主。 这个BitKeeper 的版本并且要求, 关于变动的某一阶信息被存放在计算机服务器由Bitmover 管理(万维网。openlogging 。org), 使它不可能使社区版本用户开办项目对Bitmover 是未察觉的加法。

决定被做出 2002 年 使用BitKeeper 为Linux 仁发展是一有争议一个。 一些, 著名地 GNU 创建者 理查・Stallman, 对私有的工具的表达的关心被使用在旗舰任意射出。 当项目负责人 Linus Torvalds 并且其它核心开发商采取了BitKeeper, 数锁上开发商(包括Linux 退伍军人 阿伦・考克斯) 拒绝做如此, 援引Bitmover 执照, 并且讲的关心, 项目割让一些控制对一位私有的开发商。 缓和这些关心, Bitmover 增加了允许有限的interoperation 在Linux BitKeeper 服务器的门户(由Bitmover 维护) 并且开发商之间使用CVS 和颠覆。 在这加法以后, flamewars1 偶尔地发生了在 Linux 仁邮寄的名单, 经常涉及关键仁开发商和Bitmover CEO 拉里・McVoy, 谁并且是Linux 开发商。

在4月 2005 年, BitMover 宣布, 它会停止提供BitKeeper 的版本免费对社区, 给作为原因努力 安德鲁”Tridge” Tridgell, 开发商被雇用 OSDL 在一个无关的项目, 开发会显示metadata 的客户(数据关于修正, 可能包括区别在版本之间) 代替唯一较新版本。 能看metadata 和比较通过版本是所有版本控制系统的当中一个核心特点但对任何人不是可利用的没有一个商业BitKeeper 执照, 极大使多数Linux 仁开发商感到不便。 虽然BitMover 决定提供自由商业BitKeeper 准许对一些仁开发商, 它拒绝给或卖执照对任何人由OSDL 使用, 包括 Linus Torvalds 并且 安德鲁・Morton, 安置OSDL 开发商在同样位置其它仁开发商是。 git 项目开始了, 目的在于成为Linux 仁的来源配置管理软件。

支持的末端对于”自由用途” 版本正式地是 7月1 日, 2005 年 并且用户被要求交换对商业版本或改变版本管理系统那时。 商业用户并且被要求不生产任何竞争的工具: 在 10月, 2005 年, McVoy 与一名顾客联系使用comercially 被准许的BitKeeper 要求顾客中止的雇员贡献对 Mercurial 项目, GPL 来源管理工具。 Bryan O’Sullivan, 雇员反应了, “避免冲突的任何可能的悟性, 我志愿了对的拉里只要我继续使用BitKeeper 的商业版本, 我对发展Mercurial 不会贡献。”

from  http://wikipedia.qwika.com/en2zh/BitKeeper