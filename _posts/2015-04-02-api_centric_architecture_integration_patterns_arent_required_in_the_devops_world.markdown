---
layout: single
title: "API-Centric Architecture: Integration Patterns Aren't Required in the DevOps World"
categories: Technology API
description: ntegration technologies are foundational components of many IT architectures. Orchestration and transformation of complex back-end legacy systems are necessary functions in many settings.
date: 2015-04-02T06:33:47+00:00
---

> [来源][0]

In previous posts in this series, we’ve looked at how modern applications are built, the rise of micro services architecture, and what this new application architecture means for services governance.

Integration technologies are foundational components of many IT architectures. Orchestration and transformation of complex back-end legacy systems are necessary functions in many settings. In addition, there is ample evidence that SaaS services, particularly first-generation services such as Salesforce.com, cannot avoid complex integration processes in order to connect to existing enterprise systems.

New API development happens in a DevOps model, leveraging IaaS and PaaS, on either public or private clouds. The new API use cases (especially those driven by mobile), as well as new API-centric or micro-service based development efforts, typically have little need for integration server technologies in order to be implemented.

DevOps (a portmanteau of development and operations) is a software development method that leverages automation to enable agile development and streamlined deployment processes to accelerate the release of software to production.
Speed and agility force the issue

Although integration vendors have long touted the ease of configuration of their products as key benefits of using their offerings, the reality of their usage is that they are often deployed in “set and forget” scenarios. Integrations are built and then left alone until some form of breakage requires them to be updated. As a consequence, most integration servers are attended to infrequently and the few administrators within IT who are knowledgeable about their capabilities use them. For most enterprise developers, the integration servers are a black box, and while application developers may be clients of the services they expose, they seldom, if ever, get involved in the configuration of the integrations exposed.

Many vendors have delivered their solutions in the form of appliances—as either virtualized appliances or physical boxes running in the datacenter. This delivery vehicle, together with the fact that many of the users of these technologies preferred them in appliance form, demonstrates that their role is not within the mainstream development processes.

The limitations of these approaches are manifest. Not only have these products been impossible (in the case of a physical appliance) or at least difficult to deploy in public or private cloud environments, but their usage is proving to be of questionable value, especially within the increasingly common practice of DevOps.

Initially very popular within internet companies that needed to break down the traditional walls between development and IT operations in order to deliver cloud-based applications at scale, DevOps has also become a fixture within enterprise IT. Enterprises are embracing the same cloud techniques as internet companies and DevOps has proven an efficient way to reduce costs by sharing responsibilities across development and operations teams.
APIs: Ops at the speed of Dev

The DevOps model only works when, as much as possible, the resources within the application tier, including APIs and the systems they connect, can be managed by the same common set of DevOps automation tools (such as Chef, Puppet, Ansible, or Salt, as well as version control services such as Git).

To make this automation possible, developers look for ways that application configuration can drive all aspects of the deployment process, and integration servers, which frequently bring manually configured dependencies to the application delivery process, are unwelcome sources of breakage. Developers prefer coding to a set of APIs and taking responsibility for adapting their applications to those APIs rather than introducing black boxes into the dependency chain.

Consider that the principal use cases where API development happens today occur in the application teams who are building the front-end technologies most likely to be aligned with DevOps. Because most web technologies are updated and need to be deployed at a faster cadence than would be possible in traditional developer-to-operations handoffs, deploying web technologies was one of the first places where DevOps processes became necessary.

It becomes apparent that integration models rooted in appliance-heritage products have no place in the automation-centric DevOps processes. There is no need for a separate “integration” product owned by a special ops group in IT in order to satisfy the new API use cases. And as mobile is inherently API-centric, this further reinforces the idea that these use cases should not be addressed by heavyweight integration products.

Next time, we’ll look at how data is leveraged in the enterprise and how [APIs make agile data possible][1].

For a comprehensive look at how APIs have become a foundational technology for developing robust and scalable enterprise applications, check out our new eBook [APIs are Different than Integration][2].




[0]: https://blog.apigee.com/detail/api_centric_architecture_integration_patterns_arent_required_in_the_devops_world
[1]: https://blog.apigee.com/detail/api_centric_architecture_apis_make_agile_data_possible
[2]: https://pages.apigee.com/ebook-apis-are-different-than-integration-reg.html?utm_medium=blog&utm_campaign=eBook
