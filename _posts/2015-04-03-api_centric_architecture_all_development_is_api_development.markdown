---
layout: single
title: "API-Centric Architecture: All Development is API Development"
categories: Technology API
description: Developers and architects often think of APIs as a continuation of the integration-based architectures that have long been in use within enterprise IT. But this is a narrow view.
date: 2015-04-02T02:45:29+00:00
---

> [来源][0]

Developers and architects often think of APIs as a continuation of the integration-based architectures that have long been in use within enterprise IT. But this is a narrow view.

This post is the first in a series that will examine the broader reality, where APIs have become a foundational technology for the development of robust and scalable enterprise applications. We will examine some of the important implications of the movement to the API-centric architecture that is underway within enterprise application development.

## All development is API development

> 应用所处的中心位置，在前端为其他外部客户端提供API，在后端集成内部系统，为平级应用开放数据和逻辑，应用内部由一系列API组件服务组成。
>
> MVC设计模式和页面模板技术，decade，1990s

Today’s software is built as services. Rather than using web frameworks that invoke services and produce web pages, today's applications are built by consuming and producing APIs. Applications have embraced APIs on the front end for connecting to rich clients; on the backend for integrating with internal systems; on the sides for enabling other applications to access their internal data and processes; and within, as applications are composed of a set of component services, linked together with APIs.

The majority of enterprise software development efforts are focused on building web applications using web frameworks. Java Enterprise Edition, Microsoft .NET, Ruby-on-Rails, PHP, and a variety of other technologies are used to implement web applications using model-view-controller (MVC) design patterns and page template technologies. For a decade, starting in the late 1990s, an entire generation of developers has spent the majority of its time building applications in this model.

## APIs move to the front end

> 前端技术、生态环境的变化，single page app的出现


The advent of Web 2.0 brought a renaissance to rich client development. The incompatibilities and inconsistent implementations of JavaScript began to ease as users updated to the latest browsers. At the same time, renewed competition in the browser market drove more innovation and acceleration of the implementation of web standards.

- HTML5 and JavaScript made it possible for much of the interaction to run in the browser rather than to be orchestrated via page-based flows.

- Web application developers found it necessary to create APIs to allow communication between the browser and the server.

Over time, these techniques accumulated, with more and more functionality that had previously been implemented in page templates becoming replaced with API calls. The end-result is what's now called the “single page app,” or SPA. At this point, no user interactions are serviced via page templates. A single web page is served and all interactions are handled via JavaScript, which generates a user interface via HTML5 techniques and libraries such as jQuery. Nearly every user interaction generates API calls to support these interactions.

![api integration](https://blog.apigee.com/sites/blog/files/api%20integration1.png)

As this model of web development became common, traditional page-based web analytics was hampered, but developers soon found that moving to API-based interaction analytics gives them greater abilities to capture and analyze web usage. JavaScript enables detailed observation of where app users spend time and what they are doing within the applications. These analytics can be transmitted to analytics APIs in a constant stream of interaction data.

## Mobile forces the issue

The mobile app represents a return to the more strongly delineated client-server model of development, where all interactions happen client-side and a constant networked communications mechanism between client and server must be maintained.

For mobile apps, the API-driven UX becomes non-optional. Unlike the web application developer, a mobile developer cannot choose the interactions to support via HTML and JavaScript versus those driven via traditional page templates.

The client/server communications mechanism in the age of mobile is APIs. In some cases, the same developer is charged with building the server-side and client-side interactions, but in other cases, it became necessary to have specialists building each client implementation. Further, these specialists are often external contractors, digital agencies, or system integrators.

![](https://blog.apigee.com/sites/blog/files/api%20integration2.png)

This means that the application development effort needs to have an API project at the heart of it. Application developers not only need to know how to consume an API, but have to understand how to produce one as well, with all the attendant issues such as API design, API security, and API scalability.

## APIs open enterprise applications

> RESTful design, JSON-based data, simple versioning, key-based access control

Integration architectures assume that applications are developed without considering whether to enable other applications to access their internal data and processes. Before the conventions of APIs were widely adopted, this was a prudent strategy. Trying to support application-to-application integration was an undue burden for the developer and was usually left as a project for another team.

![](https://blog.apigee.com/sites/blog/files/api%20integration3.png)

The widespread adoption of the basic conventions of API design—RESTful design, JSON-based data, simple versioning, and key-based access control—means that developers can easily build APIs into their applications and support their usage without undue distraction from their central mission of application delivery. And because application delivery already requires an API-centric architecture to enable rich HTML5/JavaScript and mobile clients, APIs will be essential elements of the project scope.

The result: when the time comes for application-to-application integration, the need for separate integration middleware is obviated.

Next time, we’ll discuss how [SOA is giving way to micro services architecture][1] as applications themselves are built from interconnected micro services wired together via APIs.



[0]: https://blog.apigee.com/detail/api_centric_architecture_all_development_is_api_development
[1]: /api/api_centric_architecture_soa_gives_way_to_micro_services/
