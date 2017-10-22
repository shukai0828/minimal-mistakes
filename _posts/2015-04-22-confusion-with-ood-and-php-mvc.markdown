---
layout: single
title: "面向对象设计方法论在PHP MVC模式下的困惑和释疑"
categories: Technology OOD
description: 最近在看面向对象设计，同时结合PHP MVC框架进行对比，有一些困惑，有一部分也许有了答案。
date: 2015-04-22T03:10:26+00:00
---

**困惑1：在MVC框架下，面向对象设计（OOD）关注的是Model层么？**

不是的，PHP的MVC模式提供了完整的应用开发框架，但对具体业务的设计是面向业务领域的。应用开发框架的优势是提供常用用户、授权、安全检查、数据库调度等方面的成熟工具，使开发人员得以把精力放置在业务领域的分析和设计上。

业务领域的分析和设计，也称为“领域建模”。

或者说，MVC的Model应该是领域模型，但是目前常见的MVC框架显然把Model混同数据操作层了。

[这篇文章][4]说“the model = database equation is **incorrect**”。

**困惑2：既然OOD的重点是“领域建模”，那么分析和设计的结果如何切入MVC体系？**

> “尽管OO技术可以用于所有级别，但本书对OOA/D的介绍着重于核心应用逻辑，或‘领域’层。”——《UML模式和应用（第三版）》P.147。

在OOA/D着重于“领域层”的语境下，在MVC框架已经提供了成熟的通用工具情况下，OOA/D在PHP MVC体系下的工作应该着眼于业务逻辑。

在MVC框架下，Model的行为更像是数据存取层，或者实现O/R Mapping的工作。反而业务逻辑的处理都倾向于在Controller里实现，但因为View、Controller、Model存在框架级别的固有关系，OOD的结果比较难于切入MVC体系。

经过一些调研，需要引入Business Layer的概念支撑“领域建模”的结果，但不是完全的领域建模。[这篇文章][3]说“Business Layer是part of the model(mvc)，不是Domain Model里的Model”。[这篇文章][4]也在建议Business Layer。

[这篇文章][5]说MVC和Model（Domain Objects、Data Mappers、Services）的通讯应当只依赖Services。这样做有四种好处：

1. 有助于强化单一职责原则
2. 逻辑变化时能够提供额外的调整余地
3. 控制器能够更加简单
4. 提供清晰的结构蓝图

[这篇文章][1]说可以引入“实体类（Entity）”概念，处理业务领域的逻辑，同时还有DAO的行为，里面出现的示例明显看到有DAO类的操作。

[这篇文章][2]说MVC里面的Model不是Domain Model，Domain Model包含问题域，实现业务逻辑的；不过他又同时说MVC里的Model应该是ViewModel，是视图用到的数据组织层。尽管可以在ViewModel实现业务逻辑，但会违反OO设计的原则，比如SRP。ViewModel定义了如何为View层交换数据，ViewModel可以访问Domain Model。




[1]: http://stackoverflow.com/questions/3029952/ddd-and-mvc-difference-between-model-and-entity
[2]: https://r.je/view-helpers.html
[3]: http://programmers.stackexchange.com/questions/193410/the-service-class-in-mvc
[4]: http://www.devshed.com/c/a/php/php-service-layers-modeling-domain-objects/
[5]: http://stackoverflow.com/questions/5863870/how-should-a-model-be-structured-in-mvc

