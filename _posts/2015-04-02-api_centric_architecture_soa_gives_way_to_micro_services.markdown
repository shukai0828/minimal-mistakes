---
layout: single
title: "API-Centric Architecture: SOA Gives Way to Micro Services"
categories: Technology API
description: In a previous post we looked at how modern software is built as services, which has given rise to a shift in how applications are built. Rather than using web frameworks that invoke services and produce web pages, applications today are built by consuming and producing APIs.
date: 2015-04-02T06:03:24+00:00
---

> [来源][0]

In a previous post we looked at how modern software is built as services, which has given rise to a shift in how applications are built. Rather than using web frameworks that invoke services and produce web pages, applications today are built by consuming and producing APIs.

We saw how applications have embraced APIs on the front end for connecting to rich clients; on the backend for integrating with internal systems; and on the sides for enabling other applications to access their internal data and processes.

This time we’ll take a look at the next phase of application architecture—micro services architecture, or MSA. This is the concept of exploding an application into a set of component fine-grained services, linked via APIs.

The promise of SOA has become reality in micro services architecture. The motivations haven't necessarily been about generic service reuse as much as about best practices for building scalable and reliable applications using IaaS and PaaS.

## Resilient and scalable cloud deployment for componentized applications

Modern Java applications are heavily componentized. Using frameworks like Spring or Java CDI, applications are assembled from a set of service components wired together at runtime. The advantages are:

- complicated applications can be broken down into components that can be developed by disparate teams

- applications can leverage and reuse existing components

- components are interconnected without fragile, complex dependencies or tightly-coupled linkages

While the benefits of the software development principles behind these practices are inarguable, the implementation of this type of architecture has generally occurred in ways that are antithetical to the deployment of resilient and scalable cloud deployments. The problem has been that enterprises have embraced application server deployments that have generally sought to run as much of a componentized application as possible within increasingly large server instances.

Heavyweight application server architectures, as exemplified by the typical WebSphere or WebLogic deployment, are expensive to scale elastically. The idea of firing up new server instances on-demand ,based on fine-grained assessments of load and traffic, was not a central consideration in the design of such servers. The loading up of all components in a single JVM process within an application server means that the entire application (and its container application server) is either “up” or “down”—the latter condition being a catastrophic application failure.

Further, not all components in the application are under equal load, yet they all must be scaled together. It's not easy to allocate additional computing resources to a single component, nor is it easy to protect the other components from one that is consuming large amounts of CPU or memory resources.

There is an assortment of mechanisms for remote invocation of components that obviates the need to run them all locally, but these mechanisms typically attempt to hide the fact that the invocation is remote from the application developer. The intent is to make it easy to invoke code that runs locally or remotely, but the result is at best “magic,” and at worst a system that is impossible for a developer to debug. As a result, most developers distrust such mechanisms and simply opt for provisioning very large server instances where they can run as much as possible inside the safety of the JVM.

However, because of the limitations of this approach, most highly scalable and available cloud applications tend to move to a networked component model where applications are decomposed into micro services, which can be deployed and scaled independently.

## Decomposed services and polyglot enterprise development

> REST/JSON API已经成为现代软件架构的基础

Rather than leveraging the more complicated traditional enterprise mechanisms (whether the legacy RPC approaches of CORBA and RMI or the cumbersome web services protocols such as SOAP), many developers are finding that the same lightweight API services that have proven to be resilient, scalable, and agile for front-end, back-end, and application-to-application scenarios can also be leveraged for application assembly. This is the essence of the micro services architecture.

![](https://blog.apigee.com/sites/blog/files/MicroServicesArch.png)

In recent years, enterprise development has moved to become “polyglot,” as PHP, Ruby-on-Rails, Python, and node.js all find a home in the enterprise toolbox. Because of this, many Java-only or JVM-only architectural practices have become less compelling for many organizations. This is one of the reasons why the simple REST/JSON API, which can be built or consumed from all popular languages and frameworks, has become the foundation for modern architectures. It is also the reason why the new “API generation” of developers doesn’t see a connection between these practices and classic SOA.

Next time, we’ll discuss [the implications API-centric architecture has upon services governance][1].




[0]: https://blog.apigee.com/detail/api_centric_architecture_soa_gives_way_to_micro_services
[1]: /api/api_centric_architecture_services_governance_does_not_scale/
