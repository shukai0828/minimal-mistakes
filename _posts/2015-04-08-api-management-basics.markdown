---
layout: single
title: "API Management Basics"
modified:
categories: Technology API
description: In API Design Lesson 202"Architectural Layers", we explain how key API functionality can be abstracted away from the implementation itself and handled via centralized server architecture. Here, we explain the technology components needed for this architecture and how they create the basis for an ongoing process of API Management.
date: 2015-04-01T05:28:37+00:00
---

> [来源][1]

*In API Design Lesson 202: Architectural Layers, we explain how key API functionality can be abstracted away from the implementation itself and handled via centralized server architecture. Here, we explain the technology components needed for this architecture and how they create the basis for an ongoing process of API Management.*

----

When launching an API program, it is necessary to ensure your APIs are secure, user-friendly, scalable, testable and reliable. Rather than building these qualities into the design of each API, it is more efficient to handle them in a single, centralized API architecture. This will make it possible to apply consistent API Management policies to the interfaces, on an ongoing basis.

This is the essence of API Management: Creating a centralized API architecture that makes the process of composing, securing and managing high-performance interfaces significantly simpler and more consistent. However, building this kind of infrastructure usually requires the adoption of a specialized API Management solution.

A range of enterprise-level API Management solutions are now available, the best of which deliver functionality essential for the layered architectural style discussed in Lesson 202. This is likely to include features for:

- Composition and orchestration (converting backend services to developer-friendly APIs)
- Security (protecting exposed information assets against attack and misuse)
- Identity (enabling client apps to provide secure, seamless access to backend services)
- Performance and lifecycle (ensuring the availability of APIs for client apps and users)
- Developer engagement (onboarding, educating and managing app devs)

The core component of a full-featured API Management solution is an API Gateway. This is a networking appliance (either hardware or virtual) that acts as a proxy so that APIs do not have to directly interact with client applications. The Gateway represents a central point where all the abstracted API functionality is located and managed via a set of governance policies.

While Gateways cover most of the technical features needed for layered API architecture, additional functionality is required to make the APIs themselves user-friendly for developers. This will normally involve integrating a Web-based API Portal into the Gateway, through which devs can register for APIs, access educational resources and monitor app/API performance.

Using an API Gateway and integrated API Portal, an organization can ensure that its API remains secure, performs efficiently, does not impact the performance of backend systems, can be updated without breaking client apps and – perhaps most importantly of all – that developers can quickly, easily leverage the interface to build truly valuable client applications.

More detailed information on these key API Management components is provided in Lesson 102: Architectural Components.

![Combined-API-Architecture](http://www.apiacademy.co/sites/default/files/Combined-API-Architecture-v7.png)



[下一章][2]


[1]: http://www.apiacademy.co/lessons/api-management/api-management-basics
[2]: /api/architectural-components/
