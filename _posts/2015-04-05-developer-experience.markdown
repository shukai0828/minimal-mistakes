---
layout: single
title: "The Developer Experience"
categories: Technology API
description: One of the key principles of good API design is that an interface must provide a seamless and user-friendly developer experience (DX) if it is to facilitate the creation of applications that add value to the API owner’s business. In this lesson, we explain the importance of a focus on DX and provide an overview of developer-focused API design principals.
date: 2015-04-01T01:22:54+00:00
---


> [来源][1]
>
> 笔记：
> 1. 设计良好的API一定能够给开发人员提供无缝、用户友好的体验
> 2. 要保证使用API的开发人员能够轻易地利用API暴露出来的功能
> 3. 提升API的可用性能够带来更高的使用率、更低的集成成本
> 4. API设计人员应该站在目标开发人员的视角考虑如何设计，其它关键概念
>   a. 易于查错，比如：提供有用的错误提示
>   b. 简化变更管理，比如：支持API版本策略
>   c. 提升直观的信息，比如：提供日志供查看
>   d. 建立信任，比如：提供长期稳定的服务
>   e. 提供信息充分、易于使用的文档，尽量降低学习成本
> 5. 建立开发者社区，让API使用者能够互相帮助


*One of the key principles of good API design is that an interface must provide a seamless and user-friendly developer experience (DX) if it is to facilitate the creation of applications that add value to the API owner’s business. In this lesson, we explain the importance of a focus on DX and provide an overview of developer-focused API design principals.*

----

Product designers understand that focusing on how their creations will be used in the real world is a key step in building products that consumers will actually want. For example, by taking a user-centric approach, designers of mobile devices and operating systems have massively increased the popularity of their products.

This focus on improved usability is also central to good API design. While traditional product designers focus on the user experience, an API designer strives to improve the developer experience (DX). Developers must be able to easily leverage the functionality an API exposes if that API is going to enable the creation of truly valuable new Web and mobile apps.

Shifting the focus of design from technical aspects of the backend systems an API exposes to the manner in which that API will be consumed by developers can greatly improve the experience of using it. Ultimately, improving an API’s usability will result in higher adoption rates and lower integration costs – both of which are important goals for any API program.

There are many strategies and solutions that can be used to make these improvements. A good general rule of thumb is that API designers should try to create their interfaces from the perspective of their target developers. More specifically, there are a number of key goals that an API designer hoping to create an excellent DX must be aware of:

Facilitating troubleshooting (e.g. providing useful error messages)
Simplifying change management (e.g. implementing an API versioning strategy)
Improving visibility (e.g. providing log and usage access)
Establishing trust (e.g. communicating a sense of stability and longevity)
Perhaps the most important area of focus for DX-friendly API design is minimizing the learning curve, making it simple for developers to create powerful applications, fast. There is a great deal that API design can do here but – to be truly effective – these efforts must be integrated into a broader scheme for educating developers and growing a self-supporting dev community.

![DX-Considerations](http://www.apiacademy.co/sites/default/files/DX-Considerations-v5.png)

Providing comprehensive, easy-to-use documentation is vital to minimizing the learning curve. Also, engaging directly with the developer community (e.g. via online forums) can be a highly effective educational strategy. This kind of direct engagement also helps to increase adoption and minimize guesswork involved in attempting to meets developers’ specific usability needs. 

To conclude, it should be clear that – alongside key business goals – the needs of developers represent a fundamental driver of good API design decisions. Broad decisions will be driven by the specific nature of the developer audience (e.g. is it internal or external?) while more granular decisions will be driven by the need to provide an excellent DX.



[下一章][2]


[1]: http://http://www.apiacademy.co/lessons/api-design/developer-experience
[2]: /api/web-api-architectural-styles/