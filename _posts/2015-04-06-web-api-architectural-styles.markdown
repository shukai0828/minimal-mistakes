---
layout: single
title: "Web API Architectural Styles"
categories: Technology API
description: Generally speaking, the term “Web API” describes any software interface that is exposed over the Web using the HTTP protocol in order to facilitate application development (but not limited to any particular message format, pattern or implementation). In this lesson, we provide a detailed overview of the main Web API design styles.
date: 2015-04-01T01:59:06+00:00
---


> [来源][1]
>
> 笔记：
> 1. 使用混合了多种风格的API在某些场合下会比较有意义，但是这种方式带来了API的复杂度，维护起来就比较困难
> 2. Web API的风格
>  a. Tunneling Style
>  b. URI Style
>  c. Hypermedia Style
>  d. Event-Driven Style

*Generally speaking, the term “Web API” describes any software interface that is exposed over the Web using the HTTP protocol in order to facilitate application development (but not limited to any particular message format, pattern or implementation). In this lesson, we provide a detailed overview of the main Web API design styles.*

----

From a design perspective, it is helpful to categorize the different types of Web API that exist based on the properties they exhibit. Understanding how APIs can be categorized will help you identify a style that will fit the goals of your API program and the requirements of your developers as well as the specific strengths and constraints of your organization.

In some cases, it may make sense to use a mixture of different API styles as part of a larger set of business processes or to support multiple user bases. However, taking such a heterogeneous approach to API design will make your API seem more complex to application developers and will make the interface more challenging to maintain.

![Web-API-Styles](http://www.apiacademy.co/sites/default/files/Web-API-Styles-v5.png)

## Tunneling Style

The most well-known implementation of the Tunneling API style is the SOAP messaging standard. SOAP defines an RPC-like interface for application integration and utilizes a standard called WSDL to describe the interface. Client applications can generate proxy code based on a WSDL document and make calls as if the remote component is local.

A Tunneling API normally:

- Exposes an RPC-like interface and provides an interface descriptor for binding
- Uses an XML-centric message format
- Uses HTTP as a transport protocol for a higher application-level protocol

One of the great advantages of this style is that it is transport-agnostic. Applications are free to deliver the same SOAP message over HTTP, JMS or raw TCP/IP connections, greatly increasing flexibility. Additionally, there is a well-defined set of protocol standards in the WS-* specifications able to support a wide array of interaction behaviors and message properties.

However, the use of a transport-agonistic protocol is inefficient when it is known that network interactions will only ever take place over HTTP. In addition, SOAP and the WS-* specification set may be unfamiliar to mobile and Web application developers, resulting in decreased usage and increased development costs in these communities.

## URI Style

The URI style, arguably the most familiar to application developers, exhibits these properties:

- An object- or resource-centric API is exposed
- URIs and query parameters are used to identify and filter objects
- CRUD (create, read, update, delete) operations are mapped to HTTP methods

URI APIs provide an intuitive, simple way for application developers to invoke requests. Well-designed APIs of this style employ “hackable” URI designs, which also act as a form of self-documentation. The reliance on the HTTP protocol can also act as an advantage for client application developers who are familiar with the protocol.

The challenge in designing a URI-style API is that it is difficult to map a complex set of application interactions to the simplified set of four HTTP operations acting upon a resource. This challenge can lead to URI designs that become increasingly complex, resulting in a steeper learning curve for developers. 

Another challenge is that this object-based interaction style may require applications to make multiple requests in cases where many collections of data resources are required to achieve a task. In other words, a URI-style API may encourage “chattiness”, which can be detrimental to perceived application responsiveness for end users.

## Hypermedia Style

This is similar to the URI style but it utilizes hypermedia to create interactions focused on tasks rather than on objects. Essentially, these are browser-based interaction for machines. In much the same way that you use links to navigate the Web and forms to provide input, a Hypermedia API provides links to navigate a workflow and template input to request information.

A Hypermedia API:

- Exposes a task-based interface, often incorporating a workflow or state machine
- Uses media to describe link semantics, template-based input and message structures
- Provides its own URIs

A key benefit of this style is that it favors long-running services. When designed correctly, a Hypermedia API can evolve over many years and continue to support applications that have been developed during its infancy. However, there is a lack of mature tooling and support for this type of API. Consequently, some developers see hypermedia APIs as excessively complex.

## Event-Driven Style

API interactions based on event-driven architectures have gained in popularity recently. A popular example of the event-driven style is the WebSocket protocol standard that has been incorporated into the HTML 5 specification. WebSockets provide a useful way of transmitting data with low overhead, in both directions between a client and server.

A Web API designed using the Event-Driven style will typically exhibit both of the following key properties:

- The client and/or the server listen for new events
- Events are transmitted via asynchronous messages, as they occur

While some Event-Based APIs utilize the HTTP protocol, there is a growing body of network-based protocols like WebSocket that favour low overhead, asynchronous communication. This style of API can be very effective for applications that need to frequently update UI widgets or for bi-directional, message-intensive applications such as multiplayer video games.

However, WebSocket APIs require additional infrastructure considerations beyond the traditional HTTP-based API components and are not well-suited to request-reply interactions. Designers should also be conscious that a WebSocket conversation is only initiated in HTTP – once the handshake is complete, the protocol shifts to a TCP/IP WebSocket-based protocol.

While this style is primarily associated with mobile apps, API designers who need to support the multitude of network-connected devices within the Internet of Things (IoT) also have an interest in interactions that reduce overhead and facilitate real-time, event-based communication.





[下一章][2]


[1]: http://www.apiacademy.co/lessons/api-design/developer-experience
[2]: /api/architectural-layers/
