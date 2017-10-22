---
layout: single
title: "Architectural Components"
categories: Technology API
description: To architect an effective API Management infrastructure, it is necessary to adopt and deploy some new technology components. In this lesson, we look into specific requirements, features and benefits associated with the components of an effective API Management architecture.
date: 2015-04-01T05:52:43+00:00
---

> [来源][1]

*To architect an effective API Management infrastructure, it is necessary to adopt and deploy some new technology components. In this lesson, we look into specific requirements, features and benefits associated with the components of an effective API Management architecture.*

----

In Lesson 101: API Management Basics, we give an overview of two integrated components necessary for a full-featured API Management solution: an API Gateway and an API Portal. Below, we provide a more detailed examination of these technologies.

## API Gateway

An API Gateway is a networking component (either hardware or virtual) that delivers an effective way to implement the kind of layered API architecture described in API Design Lesson 201: Architectural Layers.

The API Management functionality required of a Gateway will vary depending on the specific architectural layers it supports but is likely to include features for:

- Security – Protecting exposed backend systems against attack and hijack
- Performance – Maximizing API and client app efficiency and minimizing downtime
- Translation – Converting backend systems into API and app-friendly formats
- Orchestration – Composing new APIs from multiple backend resources
- Logging – Recording message-based events for analysis and auditing

An API Gateway supports layered API architecture by providing a central point to which these kinds of API Management functionality can be abstracted, away from the interface implementation as such.

Abstracting key API functionality out to the Gateway removes the need to build this functionality into each new API, making the processes of API design, implementation and management considerably simpler and more consistent.

A key advantage of this approach is “loose coupling” between exposed resources and client applications.  Each API call must pass through every architectural layer encapsulated by the Gateway before reaching the interface, so resources and apps do not interact directly.

Aside from its security benefits, loose coupling simplifies the entire process of API design, implementation and management by providing a place where messages can be translated between backend, API and app formats and protocols.

Again, centralization is the key – legacy backend systems do not need to be updated and APIs do not need to be designed with every potential client platform in mind. The Gateway provides a central point through which all traffic is translated to the required protocol or format.

Centralization creates various other benefits for architects managing API programs, including:

- Providing a place for applying a consistent set of API Management policies
- Minimizing the amount of code and infrastructural components to be supported

![API-Gateway](http://www.apiacademy.co/sites/default/files/API-Gateway-v5.png)

## API Portal

As discussed in API Design Lesson 102: The Developer Experience, one of the keys to a successful API program is making sure client application developers can quickly and easily leverage APIs to create apps that offer something of real value.
While an API Gateway will support most of the architectural functionality required for composing, implementing and managing APIs, it cannot entirely satisfy the requirement to engage and enable client app developers.

API publishers also need ways to engage, onboard, educate and manage developers – whether these developers are inside or outside the API-owning organization itself. This will generally mean delivering registration services, documentation, analytics and other resources.

The best way to make these resources available to developers is via a purpose-built Web site – usually referred to as a “developer portal” or “API Portal”. A full-featured portal will offer a range of functionality for developers and API owners, including:

- Discovery – Making it simple for developers to find and learn about APIs
- Onboarding – Allowing developers to sign-up for owner-denied API usage plans
- Education – Providing developers with the information they need to make use of APIs
- Examples – Illustrating functionality with sample applications and code fragments
- Community – Enabling developers to share best practices via forums
- Analytics – Delivering insight into API and app usage and performance 
- An API Portal may be built entirely in-house or based on one of several available white-label portal solutions. Building a portal in-house allows complete control over site functionality as well as look-and-feel. However, it can also lead to a great deal of development overhead.

Luckily, the API Portal market is now mature enough to include providing solutions that offer a broad range of customization options. Furthermore, a white-label portal solution can often be delivered fully integrated with an API Gateway.

![API-Portal](http://www.apiacademy.co/sites/default/files/API-Portal-v5.png)

These components are powerful individually but are especially useful when they are integrated to work together. For example, this integrated infrastructure empowers developers to self-register on the Portal and immediately begin sending requests to the Gateway.

Together a Gateway and Portal significantly simplify the process of managing APIs and developers in order to minimize integration costs, maintain the secure functioning of backend systems and facilitate the creation of truly valuable client applications.


[下一章][2]


[1]: http://www.apiacademy.co/lessons/api-management/architectural-components
[2]: /api/choosing-solution/
