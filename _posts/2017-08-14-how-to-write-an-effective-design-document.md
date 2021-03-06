---
layout: single
date:   2017-08-14 19:37:57
title:  "如何写有效的设计文档 "
header:
  overlay_image: http://7xil47.com1.z0.glb.clouddn.com/common_title_documents.jpg
  overlay_filter: 0.5
categories:
  - Development
  - Documents
toc: true
toc_label: 目录
toc_icon: icon-docs
---

**How to Write an Effective Design Document**

Day by day, programmers are able to get more done in less time. With today’s high level languages, development environments, tools and the “rapid application development” mindset, both programmers and managers have become accustomed to extremely fast development cycles. Programmers are now more inclined to jump directly into development, fearing that every hour they are not writing code will result in an hour of overtime the weekend before the deadline.

> 使用今天的高级开发语言、开发环境、开发工具以及快速应用开发的理念，程序员和管理者已经习惯于尽可能缩短研发周期。现在的程序员更倾向于直接开始开发。

The process of designing before coding is becoming outdated. Documenting designs is becoming even rarer. Many developers have never written a design document and cringe at the idea of doing so. Many who are required to, typically generate a lot of interaction diagrams and class diagrams, which don’t often express the developer’s thought process during the design phase. This article will discuss how to do write an effective design document concisely with no special tools, and without needing to know UML. It will also discuss why a well written design document is one of the most valuable tools a developer can have when entering a new project.

> 在开始编码之前进行设计过程的步骤被摒弃了，将设计结论形成文档的动作越来越少。设计文档通常需要记录较多的交互图和类图，但这些图形通常并不总是能够表达开发人员在设计阶段的的设计思想。

> 本篇文章主要讨论了如何书写简洁有效的设计文档，不需要使用特殊的工具，不需要掌握UML。本文也讨论了在进入一个新的项目时，书写良好的设计文档为什么能成为开发人员的一个最有价值的工具。

## 为什么要写设计文档？ 

Why write a design document?

A design document is a way for you to communicate to others what your design decisions are and why your decisions are good decisions. Don’t worry if your design is not UML compliant and don’t worry if you didn’t use a special modeling tool to create it. The biggest factor that determines if your design document is good is whether or not it clearly explains your intentions.

> 设计文档记录你的设计决策是什么，为什么它是良好的决策，验证设计文档是否良好的最大因素是该文档是否清晰地阐述了你的设计意图。

This presents a problem, however. In order to convey design decisions, you have to consider the audience that you are writing for. A peer developer will understand why a well-crafted class abstraction is a good design, however your manager will probably not. Because your peer developers and your manager have different concepts of what makes a design good, there is a need for two design documents; one for peer developers and one for managers. Each document serves a different and equally valuable purpose as you begin your project development.

> 有时候需要针对不同的受众书写不同的设计文档，通常是同事和上级管理者。由于他们在判断设计决策时的视角不一样，因此有必要输出两个设计文档：一个给开发工程师，一个给管理者，这两个文档的目的一致，但表达方式各异。

If this seems like too much work, it’s not. This article will show you how to do this through documentation reuse.

> 输出两份文档并不会带来太多的额外工作。

## 如何做好设计？

What makes a good design?

A design will typically be considered good if it fulfills the requirements in a meaningful way. If any aspect of the design cannot be justified, then it is probably worth reevaluating. Many programmers try to incorporate design patterns into their work, and they often add unnecessary complexity. You should be able to list at least one compelling reason, related to the requirements, for why a design decision was made. That reason must then be documented. If you can’t come up with a clear reason for a design decision, then it is probably not adding value.

> 设计一定要满足需求，如果在某一方面看起来不很合理，就可能需要重新评估。如果不能为设计决策提供清晰的理由，那么这个决策可能就没什么价值。

Diagrams are a great tool for visualizing your design, but they cannot convey the motivation behind your design decisions. That is why it is so important to let diagrams supplement your design document, not be your design document.

> 图形能够让你的设计思想看起来非常直观，不过他们不能表达设计决策背后的动机，图形是设计文档的重要补充，但是不能代替设计文档本身。

In addition, it is also extremely important to document any benefits that result from a design decision. By doing so, others who read your document will understand what value they can gain. Likewise, any associated risks must also be documented. More often than not, other programmers have faced the same risks and may have helpful pointers or solutions that you may not have thought about. By listing these items, you also get others to think about what the potential risks could be as well. Teammates will often be able to see potential pitfalls that you didn’t see when you created your design. It is much easier to rearrange some boxes in a diagram than it is to rewrite hundreds of lines of code when an assumption fails or when you hit an unforeseen snare during coding. A good design document minimizes unexpected complications by addressing them before the code is written.

> 将设计带来的好处记录下来尤为重要，同时设计决策带来的风险也应该被记录下来。

Finally, a document will provide you, your manager and your team with a common vocabulary for talking about the project. A design document can be a powerful tool for a manager because it gives them a view into the project that they don’t normally have the technical expertise to see. By listing the benefits you give your manager tangible items that describe why your design is sound. By documenting the risks of your design before development, you pass the responsibility of that risk to your manager, which is where it belongs.

> 文档将为您、您的经理和您的团队提供一个关于项目的通用词汇表。设计文档对于管理者来说也是一个强大的工具，因为它给管理者提供了一个视图，让他们即使不具备技术上的专业知识，也能够对项目有深入的了解。通过列出这些有形的内容，能够让你的经理知道你的设计为什么是合理的。同时在开发开始之前，记录风险并传达到上级。

Lastly, the design document is a written contract between you, your manager and your team. When you document your assumptions, decisions, risks, etc, you give others a chance to say, “Yes, this is exactly what I expect.” Once your document passes that stage, it becomes a baseline for limiting changes in project scope. Obviously, requirements are going to change sometimes, but with a baseline document you have the power to say that no change in scope is due to a misunderstanding of the requirements.

> 当你记录了你的假设、决定、风险等信息后，你就让别人有机会说“是的，这就是我想要的”。

## 写给工程师 的文档

Writing for a Peer Developer

The goal of a peer developer design document is to make sure that your ideas are valid and that your approach works with what others are doing. When developers don’t communicate their plans, disaster is sure to strike when modules or classes begin to interact. The following items describe a general guideline for writing this type of document:

> 给开发人员撰写的设计文档的目的是确保他们按照你的想法落实设计决策

**Section 1 – State the purpose of your project/sub-system:** In this section, write a few paragraphs that describe what the project or sub-system does. What is the problem it is trying to solve? Why does it need to exist? Who will use it? By answering these questions, you establish the scope of your design. If you find it hard to write a few paragraphs in this section, then you probably don’t understand the domain as much as you should. If you can’t fit your description within a few paragraphs, then perhaps the scope is too large. Use this section as a tool to verify that the scope of your design is reasonable.

> 第一节 - 陈述项目或模块的目的：在本节写几段描述项目或子系统要完成的工作，以及试图要解决的问题。这个部分的内容能够验证您设计的范围是否合理。

**Section 2 – Define the high level entities in your design:** High level entities are objects, or groups of objects, that constitute major constructs of your design. Good examples of entities are a data access layer, a controller object, a set of business objects, etc… Figure 1 shows an example of a .

> 第二节 - 定义高级别的实体：高级别的实体可以是对象或对象组，它们是设计的关键部件。比较好的实体示例可以是数据获取层、控制器对象、一组业务对象等等。下图展示了一个实体关系示例：

![](http://7xil47.com1.z0.glb.clouddn.com/2017-08-14_how-to-write-an-effective-design-document1.jpg)

Figure 1

In this section, explain in a few sentences what each entity does. The descriptions don’t have to be verbose, just enough to explain what each block’s purpose is. Be sure to describe your reasoning for defining the entities in your diagram and what their roles are.

> 在本节解释每个实体的用途，内容不用太多，只要能够说清每个方块的用途即可。一定要描述清楚图中定义实体的理由以及它们的角色是什么。

**Section 3 – For each entity, define the low level design:** This section is where your objects and object relationships are defined. For each object (or set of objects) define the following:

> 第三节 - 从如下几个方面定义每个实体的详细设计

**用途**

Usage

Describe in a paragraph how the object is used and what function it serves. If an object will interface with an external object or system, it is a good idea to show the interface for the object. Most importantly, you must again describe your thought process for defining the object as you did. List the benefits and risks. If an object provides an encapsulation, describe in a sentence why the encapsulation adds value. Use your descriptions to give meaning to the diagrams. They don’t have to be verbose, just enough to get the point across.

> 说明对象的用途和功能，明确可能的接口，说明为什么要这么设计，列出优点和缺点；说明封装的带来的价值。

**配置**

Configuration

If your object needs any special configuration or initialization, this is a good place to describe it. If not, this section can be left out.

> 如果需要特殊的配置和初始化，要能够详细描述

**对象**

Model

Figure 2 shows an example of a to supplement the System Security entity from figure 1. It is not perfect UML, but has some aspects of UML. Most importantly, it describes the design.

> 下图是图1的详细设计示例

![](http://7xil47.com1.z0.glb.clouddn.com/2017-08-14_how-to-write-an-effective-design-document2.jpg)

Figure 2

Don’t worry about perfection in your models, but be sure to describe exactly what is going on in the diagram. Here, two concrete security objects derive from a base security object, and a security factory will create one or the other for a client depending on the security model of the system.


**交互**

Interaction

This is also a good section for interaction diagrams. An interaction diagram shows how a set of objects or entities communicate with each other to perform a complex task. Figure 3 shows an example of an to show how a user might log in. It uses objects from the various entities shown in figure 1.

> 交互图，下图是一个示例

![](http://7xil47.com1.z0.glb.clouddn.com/2017-08-14_how-to-write-an-effective-design-document3.jpg)

Figure 3

Again, this diagram is not perfect UML, but it explains the communication sequence to accomplish a complex task. Interaction diagrams are most useful when you want to diagram how an object in your system will communicate with an object in another subsystem. This type of diagram will let the other developer verify that the interaction is correct.

> 交互图在说明复杂的通讯过程时非常有帮助

**Section 4 – Benefits, assumptions, risks/issues:** In this section, make a list of 5-6 top benefits of the design, a list of ALL known risks/issues and a list of ALL assumptions. Some of this may simply be rehashing what you wrote in a previous section of the document. What’s important is getting all of these items into one section so that the reader doesn’t have to read the whole document to understand what the benefits, risks and assumptions are.

> 第四节 - 列出五六条本设计的好处，已知的风险和问题，假设的条件。能够尽快的给阅读者一个直观的认识。

Never remove anything from this section! As risks become non-risks, document that they are now non-risks and why they became non-risks. Never erase them from the document. The same holds true for assumptions. You should be able to look at this section and know instantly what the current risks are to your design.

## 写个领导的文档

Writing for a Manager

The goal of a design document for your manager is to make sure that your manager understands what the main entities of the system are, what the benefits are and, most importantly, what the risks are. The document is your chance to show that you understand the requirements and that you have come up with a plan to meet those requirements.

If you have written the peer developer document well, then writing the manager’s document is simple, because it is just made up of sections 1, 2 and 4. By dividing the peer developer document up as described previously, the parts which are typically not meaningful to a manager have been contained in a single section of the document which may be removed.

> 上述文档的第1、2、4节秀给领导就够了。

## 结论

Conclusion

The hardest part of writing a design document has nothing to do with the writing. The difficult part is working through a logical design before you get to coding. Once you have a vision of how the objects and entities are arranged, writing the details is easy. In addition, it should not require anything more than a word processor and a simple shape painting program. The positive difference that spending a week on this task can make is unbelievably rewarding in the end. As the adage goes, “If you fail to plan, then you plan to fail.”

> 如果你没有计划好，那么你的计划就会失败。

[原文出处](http://blog.slickedit.com/2007/05/how-to-write-an-effective-design-document/ "How to Write an Effective Design Document")
