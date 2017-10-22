---
layout: single
title: "API-Centric Architecture: Services Governance Does Not Scale"
categories: Technology API
description: In previous posts in this series ("All Development is API Development" and "SOA Gives Way to Micro Services"), we looked at how modern software is built as services, which has given rise to a shift in how applications are built and the rise of micro services architecture—a new era of application architecture where applications are built as a set of component fine-grained services, linked together via APIs.
date: 2015-04-02T06:21:28+00:00
---

> [来源][0]

In previous posts in this series ("All Development is API Development" and "SOA Gives Way to Micro Services"), we looked at how modern software is built as services, which has given rise to a shift in how applications are built and the rise of micro services architecture—a new era of application architecture where applications are built as a set of component fine-grained services, linked together via APIs.

So what does this modern application architecture mean for services governance?

With the proliferation of API services as the connective tissue between applications, front ends, backends, and even within the application architecture itself, one might think that service governance would have become paramount in importance. Actually, the reverse has happened. The majority of APIs are developed outside of most services governance processes.

## Virtualization killed the services governance star

Culture and history can explain—at least in part. Much of the early SOA implementations were done by central architecture teams who were enamored of the elegance of a unified architecture but who were removed from the realities of day-to-day application development. In addition, many of the concepts of distributed computing that architecture teams worked with were still relatively unfamiliar to most application teams.

However, we're now at a point where an entire generation of developers has come of age in the era of the internet. Services creation is a natural part of the software development process; it happens ad-hoc and is prolific. Furthermore, API design and development is every developer's job.

The emergence of virtualization (and its descendants, IaaS and PaaS) has led to the final breakage of the centralized service governance model. Easy access to server resources has allowed application developers the freedom to build API services as they see fit. Micro service architecture is the direct result of the proliferation of virtualized server instances. This has meant that putting an API into production can often be done with little or no oversight.

## Governance, for the app teams

> API治理跟标准格式的文档、安全一致性、访问控制机制直接关联

It would be easy to criticize this API proliferation as leading to an unmanageable situation. There are legitimate concerns with such practices. Many APIs built in this way will not be secured consistently across the organization. Not all of the APIs will be designed as easily reusable services, and more than a few will be used for tightly-coupled communications.

API governance has emerged as a new area of focus in these situations, separate and distinct from SOA governance. API governance concerns itself with providing standardized conventions for documentation and consistent security and access control mechanisms. It exists in support of the application teams rather than the centralized IT resources, and as a consequence is not prescriptive except in a few vital areas, such as defining standards for security mechanisms including OAuth.

In summary, with new tools and processes used by a new generation of internet-era developers, a centralized services governance process owned by a special architectural team in IT cannot maintain an iron grip on agile and decentralized API-first architectures.

Next time, we’ll look at [the influences of DevOps][1], the new software development methodology that leverages automation to enable agile development and streamlined deployment processes to accelerate the release of software to production.




[0]: https://blog.apigee.com/detail/api_centric_architecture_services_governance_does_not_scale
[1]: /api/api_centric_architecture_integration_patterns_arent_required_in_the_devops_world/
