---
layout: single
title: "API Design Basics"
categories: Technology API
description: The term “API design” or “API architecture” refers to the process of developing a software interface that exposes backend data and application functionality for use in new applications. In this lesson, we provide an overview of the API architecture process, as a starting point for a deeper exploration of design and implementation best practices in subsequent lessons.
date: 2015-03-31T10:17:46+00:00
---

> [来源][1]
>
> 笔记：
> 1. 设计良好的API应该反映它所服务的业务目标
> 2. 设计API时应该充分考虑到组织的能力和限制，比如成本、个人能力、技术基础等
> 3. API的架构总是应该关注真实的需求，而不是典雅的技术概念

*The term “API design” or “API architecture” refers to the process of developing a software interface that exposes backend data and application functionality for use in new applications. In this lesson, we provide an overview of the API architecture process, as a starting point for a deeper exploration of design and implementation best practices in subsequent lessons.*

In the API Strategy section of this site, as well as providing a basic explanation of application programming interfaces, we explore the business drivers that lead many organizations to create APIs and we explain how these interfaces can be divided into two broad categories, based on kind of developers they target – private APIs and open APIs.

If you have taken the lessons in that section, you know that initial API design decisions should be based around the business drivers behind your API program. Additionally, you will understand how your API design process should – to a large extent – be guided by the nature of the developer audience you are targeting.

We will further explore the impact a developer audience will have on API design patterns in the next lesson. For now, we can sum up what we have learned so far like this: A well-designed API should reflect the goals of the business it is designed to serve – otherwise API design can actually hinder the objectives driving the interface’s creation.

It is also important to remember that the goals an organization sets for its API program must be chosen within the framework of the resources available to the organization and that this should guide its API design decisions. Good API design takes account of the organization’s specific strengths and limitations in terms of budget, personal skill sets and technical infrastructure. 

The larger lesson here is that API architects should always focus on real-world requirements rather than technical elegance. For example, there is a common belief that Web APIs should conform to the constraints of the REST architectural style. In fact, it does not really matter whether or not an interface is strictly “RESTful” so long as it does the job it was meant to do.

Instead, APIs should be designed around the core of practical considerations outlined in this section. Based on these considerations, there are a number of key decisions that will have to be made about the design of an API. Later lessons will explore these broad decisions before getting into the real specifics of effective API design and implementation patterns.

![Key-Considerations](http://www.apiacademy.co/sites/default/files/Key-Considerations-v5.png)



[下一章][2]


[1]: http://www.apiacademy.co/lessons/api-design/api-design-basics
[2]: /api/developer-experience/

