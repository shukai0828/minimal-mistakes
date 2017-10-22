---
layout: single
title: "Private APIs vs. Open APIs"
modified:
categories: Technology API
description: One of the key considerations that should guide both your API business strategy and your interface architecture is the distinction between open and private APIs. An interface is defined as open or private depending on whether it targets external or in-house developers. In this lesson, we explain the distinction in detail and explore ways it may impact your API program.
date: 2015-03-31T09:57:02+00:00
---

> [来源][1]

*One of the key considerations that should guide both your API business strategy and your interface architecture is the distinction between open and private APIs. An interface is defined as open or private depending on whether it targets external or in-house developers. In this lesson, we explain the distinction in detail and explore ways it may impact your API program.*

## Private APIs

A private API is an interface that opens parts of an organization’s backend data and application functionality for use by developers working within (or contractors working for) that organization. The new applications these devs create may be distributed publicly but the interface itself is unavailable to anyone not working directly for the API publisher.

Private APIs can significantly reduce the development time and resources needed to integrate internal IT systems, build new systems that maximize productivity and create customer-facing apps that extend market reach and add value to existing offerings. Rather than creating siloed applications from scratch, devs can draw from a common pool of internal software assets.

Essentially then, the goal of a private API program is to enable internal developers who are building new applications that leverage existing systems. Therefore, the needs and preferences of these devs should drive the decisions made by business managers and interface developers who are implementing the program.

There are other considerations that need to be kept in mind – for example, how to ensure that the program meets both the organization’s immediate project goals and its future connectivity requirements. Crucially, it is vital to handle the ongoing management of any API program, to ensure the security and performance of backend systems is maintained over the long term.

Managing a private API program may sound easy: interfaces are only exposed to internal developers, reducing security risks; API designers have direct access to these developers, making it easier to create dev-friendly interfaces. However, it is important to remember that exposing software interface always creates a range of security and management challenges.

For example, in many cases client apps will communicate with APIs via the public Internet or mobile networks – even if apps are only for use by internal employees. There are also challenges associated with integrating systems that use different protocols and standards – particularly as legacy systems are often unsuitable for use on mobile devices or the Web.

![Private-APIs](http://www.apiacademy.co/sites/default/files/Private-APIs-v5.png)

## Open APIs

An open APIs is an interface that has been designed to be easily accessible by the wider population of Web and mobile developers. This means an open API may be used both by developers inside the organization that published the API or by any developers outside that organization who wish to register for access to the interface.

An open API publisher is usually seeking to leverage the ever-growing community of free-agent app developers. This will allow the organization to stimulate development of innovative apps that add value to its core business, without investing directly in development efforts – it simultaneously increases the production of new ideas and decreases dev costs.

An open API may be used by internal developers but it is fair to say – in most cases – the success of an open API program will depend on its ability to attract external developers and help them create truly valuable new apps that people actually want to use. Open API publishers need to engage developers and they need to make sure these developers are successful.

Therefore, for business managers and interface designers alike, the key goal should be to increase both the quantity and quality of API usage. This will mean targeting a specific dev audience, delivering an interface and accompanying documentation designed to meet that audience’s preferences and conducting targeted outreach/education activities.

It is also worth noting that opening an interface to external developers can significantly add to the management and security challenges associated with publishing APIs. For example, with many third-party client apps active in the field, it can be very challenging to ensure that interface updates do not break application functionality.

Increased security risks represent another major challenge associated with exposing software interfaces publicly. Not only does publishing an open API theoretically mean that any developer can access exposed backend systems, it also risks bringing the existence of that exposure to the attention of hackers who might never have noticed a private API.

Publishing open APIs can additionally make it harder for organizations to control the experience end users have with their information assets. Open API publishers cannot assume client apps built on their APIs will offer a good user experience. Furthermore, they cannot fully ensure that client apps maintain the look and feel of their corporate branding.

![Open-APIs](http://www.apiacademy.co/sites/default/files/Open-APIs-v5.png)

## Partner APIs

A partner API is a hybrid form of the open and private interface models. This kind of API is usually implemented to support applications built by developers within an organization that has an existing relationship with the API publisher. This means that business managers and interface designers will have knowledge of and direct access to client application developers.

While this will certainly ease the process of designing and implementing an API, it must be noted that publishing a partner APIs comes with its own set of challenges. Many of these challenges arise from the fact that – although managers and designers will have some access to client developers, they lack direct influence or authority over these devs.




[下一章][2]


[1]: http://www.apiacademy.co/lessons/api-strategy/private-apis-vs-open-apis
[2]: /api/api-design-basics/
