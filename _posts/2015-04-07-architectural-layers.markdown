---
layout: single
title: "Architectural Layers"
modified:
categories: Technology API
description: No matter what API design style you choose, there are certain key qualities you will want your interface to have. Designing API architecture able to encompass all these qualities can be challenging. In this lesson, we outline a layered architectural style that simplifies the process of implementing a full-functioned Web API design.
date: 2015-04-01T03:28:07+00:00
---

> [来源][1]

*No matter what API design style you choose, there are certain key qualities you will want your interface to have. Designing API architecture able to encompass all these qualities can be challenging. In this lesson, we outline a layered architectural style that simplifies the process of implementing a full-functioned Web API design.*

----

For a Web API to function effectively, it must meet a range of functional and non-functional requirements. These requirements, which are essentially software qualities the interface should display, will vary depending on the context but are likely to include:

- Security – Protected against attack and misuse
- Usability – Easy for developers to effectively leverage
- Scalability – Able to handle rapid spikes in traffic
- Testability – Designed to help devs experiment with functionality
- Reliability – Robust enough to minimize downtime

![API-Software-Qualities](http://www.apiacademy.co/sites/default/files/API-Software-Qualities-v5.png)

Meeting all these requirements can be a complex process but it can be simplified by adopting a layered architectural style. In this scenario, the API server architecture is divided into a number of layers, through which each request must travel before reaching the API implementation itself. Each layer allows the interface to meet a key functional or non-functional requirement.

This simplifies the process of interface design because each key software quality is abstracted away from the API implementation and handled in a centralized API server architecture, which can be used across multiple interfaces. Depending on the specific requirements, a well-designed API server architecture might contain the following layers:

- Security
- Caching
- Representation
- Orchestration

![Layered-Architecture](http://www.apiacademy.co/sites/default/files/Layered-Architecture-v5.png)

## The Security Layer

Establishing Web API security is challenging and requires a special focus. Creating a specialized layer to enforce security is a good way of abstracting the intricacies of security away from the design of the API itself.

Security enforcement works best at the edges and access control components should be the first to act upon all API requests. The security layer should function as a gatekeeper for the API architecture.

## The Caching Layer

A server-based caching implementation should be capable of delivering content decorated with caching instructions. It should also be able to act upon caching validation metadata from client applications.

Caching can be implemented in many locations within any API architecture. Positioning at least one caching layer close to the edge will help reduce resource utilization when cached versions of a response can be delivered.

## The Representation Layer

A representation layer provides a consumer-centric view of the data or service that an API is exposing. Implementing a representation layer makes it possible to isolate the interface from the data and services that support it.

This will go a long way towards improving the usability of the API by making it possible to focus on improving the interface without impacting the implementation of the underlying module that serves the data.

This layer also allows an architect to build new interfaces for existing legacy services that may not be suitable for new consumers. For example, a common challenge faced by enterprise architects is how to adapt a SOAP-based Web service for mobile client applications.

## The Orchestration Layer

While many API requests may be served by a single backend component, it is often the case that data will need to be aggregated or composed from multiple backend sources. This is where the orchestration layer comes in.

An orchestration layer can abstract this composition work in a way that makes it easy to create new APIs by selectively joining together existing data elements. An orchestration layer may also call external APIs to provide enriched responses to client applications.

## Implementing a Layered API Server Architecture

In practice, implementing this layered architectural style is likely to require new infrastructural components, as well as some new security and management practices. These additional requirements are explored in the API Management section of this site.



[下一章][2]


[1]: http://www.apiacademy.co/lessons/api-design/architectural-layers
[2]: /api/api-management-basics/
