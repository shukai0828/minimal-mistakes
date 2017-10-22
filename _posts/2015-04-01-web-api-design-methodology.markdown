---
layout: single
title: "A Web API Design Methodology"
categories: Technology API
description: Designing Web APIs is more than just URLs, HTTP status codes, headers, and payloads.本文描述了一个分7步完成的API设计方法论。
date: 2015-04-01T10:16:15+00:00
---

> [来源][0]

Designing Web APIs is more than just URLs, HTTP status codes, headers, and payloads. The process of design -- what is essentially a "look and feel" for your API -- is very important and is well-worth the effort. This article briefly outlines a methodology that results in an API design that takes advantage of both HTTP and the Web. And it can work for more than just HTTP. If, at some point you need to implement the same service over WebSockets, XMPP, MQTT, etc. most of the features of the resulting API design will work the same. That can make supporting multiple protocols in the future easier to implement and maintain.

## Good Design Goes Beyond URLs, Status Codes, Headers, and Payloads

Typically, Web API design guidance focuses on the the common features such as URL design, proper use of HTTP features such as status codes, methods, headers, and the design of payloads that hold serialized objects or object graphs. These are valuable implementation details, but not much in the way of API design. And it is the design of the API -- the way the essential features of the service are expressed and described -- that can make an important contribution to the success and usability of your Web API.

A good design process or methodology defines a consistent, repeatable set of steps to employ when working to expose a server-side service component as an accessible, usable Web API. That means that a clear methodology can be shared with developers, designers, and software architects in order to help coordinate activities throughout the implementation cycle. An established methodology can also be refined over time as each team discovers ways to improve and streamline the process without adversely affecting implementation details. In fact, implementation details can change (e.g. which platform, OS, frameworks, and UI style to employ) independently of the design process when these two are cleanly separated and defined.

## A Seven Step API Design Methodology

What follows is a brief overview of the design methodology covered in the book "RESTful Web APIs" by Richardson and Amundsen. There is not enough room here to go into depth for each step in the process but this article can give the big picture. Also, the reader can use this overview as a guide for developing a unique Web API design process that fits the local skills and goals of your group.

*NOTE: Yes, seven seems like quite a few. In reality there are only five design steps and two additional items in the list including implementing and publishing. These last two round out the process to provide and end-to-end experience.*

You should plan to reiterate through these steps as needed. You may get through Step 2 (Draw State Diagrams) and realize there is more work to be done in Step 1 (List All the Parts). When you get around to writing the code (Step 6), you may discover a number of things missed in Step 5 (Create a Semantic Profile), etc. The key is to use the process to expose as many details as possible and be willing to go back a step or two in order to capture the items you missed along the way. Iteration is the key to building a more complete picture of your service and clarifying how it can be exposed to client applications.

### Step 1 : List All the Parts

> 站在调用方（client）的立场，罗列调用方想要获取或传入的所有数据项

The first step is to list all the pieces of data a client application might want to get out of the service or put into it. We'll call these the semantic descriptors. Semantic because they deal with the meaning of data in the application and descriptors since they describe what is happening in the application itself. Note that the point of view here is that of the client, not the service. It's important to design the API as something the client will be using.

For example, in a simple To-Do List app, you might find the following semantic descriptors:

- id : the unique identifier for each record in the system
- title : the title of each to-do item
- dateDue : the date the to-do item is due for completion
- complete : a yes/no flag indicating whether the to-do item has been completed

In a full-featured application, there could be many more semantic descriptors to cover things like categories of to-do items (work, family, gardening, etc.), user information (for a multi-tenant implementation), and so on. We'll keep this one simple in order to focus on the process itself.

### Step 2 : Draw State Diagrams

The next step is to draw out state diagrams for the proposed API. Each box in the diagram represents a possible representation -- a document that includes one or more of the semantic descriptors identified in Step 1. You can use arrows to indicate transitions from one box to the next -- from one state to the next. These transitions are initiated by protocol requests.

Don't worry about indicating which protocol method is used in each transition yet. Just indicate whether the transition is safe (e.g. HTTP GET), unsafe/non-idempotent (e.g. HTTP.POST), or unsafe/idempotent (PUT).

NOTE: Idempotent actions are repeatable without unexpected side-effects. For example HTTP PUT is idempotent because the specification says servers should use the state values pass from the client to replace any existing values for the target resource. However, HTTP POST is non-idempotent since the HTTP spec states that POSTed values should be used to append to an existing resource collection, not replace.

In this case, a client application for our simple To-Do service might need to access the list of available items, be able to filter that list, view a single item, and mark an item complete. Many of these actions use state values to pass data between the client and server. For example the add-item action allows the client to pass the state values title and dueDate. Here is a diagram that illustrates those actions.

![](http://www.infoq.com/resource/articles/web-api-design-methodology/en/resources/figure1.png)

The actions shown in the diagram (and listed below) are also semantic descriptors -- they describe the semantic actions for this service.

- read-list
- filter-list
- read-item
- create-item
- mark-complete

You might find, as you work through the diagram, that you missed actions or data items the client will actually want or need. That's an opportunity to go back to step 1 to add new descriptors and/or improve on the diagram in step 2.

Once you have reiterated through these two steps you should have a good idea of all the data points and actions the client will need to interact with your service.

### Step 3 : Reconcile Magic Strings

The next step is to reconcile all the "magic strings" in your service interface. The "magic strings" are all the descriptor names -- they have no intrinsic meaning, they just represent actions or data elements clients will access when communicating with your service. Reconciling these descriptor names means adopting well-known public names from sources like:

- [Schema.org](http://schema.org/)
- [microformats.org](http://microformats.org/)
- [Dublin Core](http://dublincore.org/documents/dces/)
- [IANA Link Relation Values](http://www.iana.org/assignments/link-relations/link-relations.xhtml)

These are all repositories of well-defined, shared names. When you use names from these sources for your service interface, it is likely that developers have seen them before and understand what they mean. This can improve the usability of your API.

*NOTE: While it is a good idea to use shared names for descriptors on your service interface, you don't need to use them for your internal implementation (e.g. the data field names in a database). The service itself can map the public interface names to the internal storage names without any problem.*

For the sample To-Do service, I was able to find acceptable existing names for all but one of my semantic descriptors - create-item. For this case, I resorted to creating a unique URI based on rules from the Web Linking RFC5988. There are trade-offs when selecting well-known names for your interface descriptors. They are rarely a perfect match to your internal data storage elements and that's OK.

Here are my results:

- id -> [identifier from Dublin Core](http://purl.org/dc/elements/1.1/identifier)
- title - [name from Schema.org](https://schema.org/name)
- dueDate -> [scheduledTime from Schema.org](https://schema.org/scheduledTime)
- complete -> [status from Schema.org](https://schema.org/status)
- read-list -> [collection from IANA Link Relation Values](http://www.iana.org/assignments/link-relations/link-relations.xhtml)
- filter-list -> [search from IANA Link Relation Values](http://www.iana.org/assignments/link-relations/link-relations.xhtml)
- read-item -> [item from IANA Link Relation Values](http://www.iana.org/assignments/link-relations/link-relations.xhtml)
- create-item -> http://mamund.com/rels/create-item using RFC5988
- mark-complete - [edit from IANA Link Relation Values](http://www.iana.org/assignments/link-relations/link-relations.xhtml)

So, based on my name-reconciliation, here is my updated state diagram:

![](http://www.infoq.com/resource/articles/web-api-design-methodology/en/resources/figure2.jpg)

### Step 4 : Select a Media Type

The next step in the design process for your API is to select a media type to use when passing messages back and forth between client and server. One of the hallmarks of the Web is that data is passed as standardized documents over a uniform interface. It is important to select a media type that supports both the data descriptors (e.g. "identifier", "status", etc.) as well as the action descriptors (e.g. "search", "edit", etc.). There are quite a few formats available.

Some of top hypermedia formats as I write this are (in no special order):

- HyperText Markup Language ([HTML](http://www.w3.org/TR/html5/))
- Hypertext Application Language ([HAL](http://stateless.co/hal_specification.html))
- Collection+JSON ([Cj](http://amundsen.com/media-types/collection/))
- [Siren](https://github.com/kevinswiber/siren/blob/master/README.md)
- [JSON-API](http://jsonapi.org/)
- Uniform Basis for Exchanging Representations ([UBER](http://g.mamund.com/uber))

It is also important to select a media type that will work well with your target protocol. Most developers prefer the HTTP protocol for service interfaces. However, WebSockets, XMPP, MQTT, and CoAP are also used -- especially for high-speed, short-message, peer-to-peer implementations.

For this example, I'll use HTML as the message format and HTTP as the protocol. HTML has all the data descriptor support that's needed (`<UL>` for lists, `<LI>` for items, and `<SPAN>` for data elements). It also has adequate support for action descriptors (`<A>` for safe links, `<FORM method="get">` for safe transitions and `<FORM method="post">` for unsafe transitions.

*NOTE: The state diagram currently shows the "edit" action as idempotent (e.g. HTTP PUT) and HTML still does not have native support for PUT. For this example, I'll use an added field to help make HTML's POST-only support idempotent.*

Ok, now I can "try out" the interface by creating some sample representations based on the state diagram. For our example, we have only two representations to render: the "To-Do List" and the "To-Do Item" representations:

**Figure 1 : The To-Do List Collection Representation in HTML**

~~~ html
<html>
  <head>
    <!-- for test display only -->
    <title>To Do List</title>
    <style>
      .name, .scheduledTime, .status, .item {display:block}
    </style>
  </head>
  <body>
    <!-- for test display only -->
    <h1>To-Do List</h1>
    <!-- to-do list collection -->
    <ul>
      <li>
        <a href="/list/1" rel="item" class="item">
          <span class="identifier">1</span>
        </a>
        <span class="name">First item in the list</span>
        <span class="scheduledTime">2014-12-01</span>
        <span class="status">pending</span>
      </li>
      <li>
        <a href="/list/2" rel="item" class="item">
          <span class="identifier">2</span>
        </a>
        <span class="name">Second item in the list</span>
        <span class="scheduledTime">2014-12-01</span>
        <span class="status">pending</span>
      </li>
      <li>
        <a href="/list/3" rel="item" class="item">
          <span class="identifier">3</span>
        </a>
        <span class="name">Third item in the list</span>
        <span class="scheduledTime">2014-12-01</span>
        <span class="status">complete</span>
      </li>
    </ul>
    <!-- search transition -->
    <form method="get" action="/list/" class="search">
      <legend>Search</legend>
      <input name="name" class="identifier" />
      <input type="submit" value="Name Search" />
    </form>
    <!-- create-item transition -->
    <form method="post" action="/list/" class=">
      <legend>Create Item</legend>
      <input name="name" class="name" />
      <input name="scheduledTime" class="scheduledTime" />
      <input type="submit" value="Create Item" />
    </form>
  </body>
</html>
~~~

**Figure 2: The To-Do List Item Representation in HTML**

~~~ html
<html>
  <head>
    <!-- for test display only -->
    <title>To Do List</title>
    <style>
      .name, .scheduledTime, .status, .item, .collection {display:block}
    </style>
  </head>
  <body>
    <!-- for test display only -->
    <h1>To-Do Item</h1>
    <a href="/list/" rel="collection" class="collection">Back to List</a>
    <!-- to-do list collection -->
    <ul>
      <li>
        <a href="/list/1" rel="item" class="item">
          <span class="identifier">1</span>
        </a>
        <span class="name">First item in the list</span>
        <span class="scheduledTime">2014-12-01</span>
        <span class="status">pending</span>
      </li>
    </ul>
    <!-- edit transition -->
    <form method="post" action="/list/1" class="edit">
      <legend>Update Status</legend>
      <input type="hidden" name="etag" value="q1w2e3r4t5y6" class="etag" />
      <input type="text" name="status" value="pending" class="status" />
      <input type="submit" value="Update" />
    </form>
  </body>
</html>
~~~

Remember, as you work through the representation samples of your state diagram, you may find things you missed in earlier steps (missing descriptors, changes in action descriptors such as idempotency, etc.). That's fine. Now is the time to work these all out -- before you commit this design to code.

Once you're satisfied the representations are complete, there is an additional step you need to do before starting to write code -- creating the Semantic Profile.

### Step 5 : Create a Semantic Profile

A Semantic Profile is a document that lists all the descriptors in your design and includes details about each one that will help developers when building both client and server implementations. The profile is an implementation guide, not an implementation description. This is an important distinction.

#### Service Description Formats

Service description document formats have been around for quite a while and are handy when you want to generate code for, or document, an existing implementation of a service. There are quite a few formats around.

The top contenders as I write the article are:

- Web Service Definition Language ([WSDL](http://www.w3.org/TR/wsdl))
- Atom Service Description ([AtomSvc](https://tools.ietf.org/html/rfc5023#section-8))
- Web Application Description Language ([WADL](http://www.w3.org/Submission/wadl/))
- [Blueprint](https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md)
- [Swagger](https://github.com/wordnik/swagger-spec/blob/master/README.md)
- RESTful Application Modeling Language ([RAML](http://raml.org/spec.html))

#### Profile Formats

There are only a few profile formats at the moment. The ones I recommend are:

Application-Level Semantic Profiles (ALPS)
JSON-LD + Hydra
Both are relatively new. The JSON-LD specification reached W3C Recommendation status early in 2014. Hydra is still an Unofficial Draft (as of this writing) and has an active community of developers. ALPS is still in early draft stage with the IETF.

Since the idea of a profile document is to describe the real-life aspects of a problem space (not just a single implementation within that space), the format is quite different than typical description formats:

**Figure 3 : The To-List Semantic Profile in ALPS**

~~~ html
<html>
<alps version="1.0">
  <doc>
    ALPS profile for InfoQ article on "API Design Methodology"
  </doc>
  <!-- data descriptors -->
  <descriptor id="identifier" type="semantic" ref=" />
  <descriptor id="name" type="semantic" ref=" />
  <descriptor id="scheduledTime" type="semantic" ref=" />
  <descriptor id="status" type="semantic" ref=" />
  <!-- action descriptors -->
  <descriptor id="collection" type="safe" ref=" />
  <descriptor id="item" type="safe" ref=">
    <descriptor href="#identifier" />
  </descriptor>
  <descriptor id="search" type="safe" ref=">
    <descriptor href="#name" />
  </descriptor>
  <descriptor id="create-item" type="unsafe" ref=">
    <descriptor href="#name" />
    <descriptor href="scheduledTime" />
  </descriptor>
  <descriptor id="edit" type="idempotent" ref=">
    <descriptor href="#identifier" />
    <descriptor href="#status" />
  </descriptor>
</alps>
~~~

You'll notice that this document looks like a basic vocabulary of all the possible data values and actions in the To-Do service interface -- and that's the idea. Services who agree to abide by this profile can make their own decisions about protocol, message format, and even URLs. Clients who agree to accept this profile will be built to recognize and, if appropriate, activate the descriptors shown in this document.

This is also a great format for generating human-readable documentation, analysing similar profiles, tracking which profiles are most-commonly used, even generating state diagrams. But that's a subject for another article.

Now that you have the complete list of descriptors with reconciled names, the annotated state chart, and a semantic profile document, you're ready to start coding a sample server and client.

### Step 6 : Write Some Code

At this point, you should be able to turn over your design documents (state charts and semantic profile) to developers of both the server and client apps in order to start a specific implementation.

The HTTP server should implement the state diagram created in Step 2 and requests from the client should trigger the proper state transitions in the service. Each representation sent from the service should be in the format selected in Step 3 and should include a link to the profile created in Step 4. Responses should include the appropriate hypermedia controls that implement the actions shown in the state chart and described in the profile document. Client and server developers can build their implementations relatively independently at this point and use test runs to validate compliance with the state diagram and profile.

Once you have stable running code, there is one more step in our list: Publishing.

### Step 7 : Publish Your API

Web APIs should publish at least one URL that is promised to always respond to clients -- even in the far future. I call this the "billboard URL" -- the one everyone knows. It is also a good idea to publish the profile document so that new implementations of the service can link to it in responses. You can also publish human-readable documentation, tutorials, etc. to help developers understand and use your service.

Once that is done, you should have a well-designed, stable, accessible service up and running, ready to use.

## In Conclusion

This article covered a set of steps for designing APIs for the Web. The focus was on getting the data and action descriptions correct and documenting them in a machine-readable way in order to make it easy for human developers to implement clients and servers for this design even if they are not in direct contact with each other.

The steps are:

1. **List all the Parts**
Gather all the data elements clients will need in order to interact with the service
1. **Draw State Diagrams**
Document all the actions (state transitions) that will be available for the service
3. **Reconcile Magic Strings**
Clean up your public interface to match (as best possible) well-known names
1. **Select a Media Type**
Review message formats to find the one that most closely aligns the service transitions with the target protocol.
1. **Create a Semantic Profile**
Write up a profile document that defines all the descriptors used in the service
1. **Write Some Code**
Share the profile document and the state diagram to client and server developers and start writing code to test compliance and adjust the profile/diagrams as needed.
1. **Publish Your API**
Publish your "billboard URL" and profile document so that others can use them to create new services and/or client applications.

It is likely that you'll need to reiterate through some steps along the way as you discover missing elements and make trade-off decisions with your design. The sooner that happens in the process, the better. It's also possible that you will be able to use this API design at some point in the future to create implementations using new formats and protocols that may be requested by developers at some point.

Finally, this methodology is just one possible way to create a dependable, repeatable, consistent process for designing your Web APIs. As you work through this example, you may find it works better for you to insert additional steps, collapse some, and -- of course -- the message format and protocol decisions may vary from one case to the next.

Hopefully this gives you some ideas on how you can create an optimal API design methodology for your organization and/or team.




[0]: http://www.infoq.com/articles/web-api-design-methodology
