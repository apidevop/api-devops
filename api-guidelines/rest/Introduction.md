##What is an API?

In the simplest of terms, API is the acronym for Application Programming
Interface, which is a software intermediary that allows two applications to
talk to each other. In fact, each time you check the weather on your phone,
use the Facebook app or send an instant message, you are using an API.

Every time you use one of these applications, the application on your phone
is connecting to the Internet and sending data to a server. The server then
retrieves that data, interprets it, performs the necessary actions and sends
it back to your phone. The application then interprets that data and
presents you with the information you wanted in a human, readable format.

What an API really does, however, is provide a layer of security. Because
you are making succinct and explicit calls, your phone’s data is never fully
exposed to the server, and likewise the server is never fully exposed to your
phone. Instead, each communicates with small packets of data, sharing
only that which is necessary—kind of like you ordering food from a drive-




## Many Types of APIs

There are many types of APIs. For example, you may have heard of Java
APIs, or interfaces within classes that let objects talk to each other in the
Java programming language. Along with program-centric APIs, there are
also Web APIs such as the Simple Object Access Protocol (SOAP), Remote
Procedure Call (RPC), and perhaps the most popular—at least in name—
Representational State Transfer (REST).

While the function of an API may be fairly straightforward and simple, the
process of choosing which type to build, understanding why that type of
API is best for your application, and then designing it to work effectively has
proven to be far more difficult.

One of the greatest challenges of building an API is building one that will
last. This is especially true for Web APIs, where you are creating both a
contract between you and your users and a programming contract between
your server and the client.



## Web APIs

A Web API, otherwise known as a Web Service, provides an interface for
Web applications, or applications that need to connect to each other via the
Internet to communicate. To date, there are over 13,000 public APIs that
can be used to do everything from checking traffic and weather, to
updating your social media status and sending Tweets, to even making
payments.

In addition to the 13,000 public APIs, there are hundreds of thousands
more private Web APIs, or APIs that are not available for consumption by
the general public, that are used by companies to extend their capabilities
and services across a broad scope of use-cases, including multiple devices.
One of the most common forms of a private cross-device Web APIs would
be an API written for a mobile application that lets the company transmit
data to the app on your phone.

Since 2005 , the use of Web APIs has exploded exponentially, and multiple
Web formats and standards have been created as technology has
advanced:


## SOAP
  SOAP was designed back in 1998 by Dave Winer, Don Box, Bob Atkinson
  and Mohsen Al-Ghosein for Microsoft Corporation. It was designed to offer
  a new protocol and messaging framework for the communication of
  applications over the Web. While SOAP can be used across different
  protocols, it requires a SOAP client to build and receive the different
  requests, and relies heavily on the Web Service Definition Language (WSDL)
  and XML:
  
```
  <?xml version="1.0"?>
  <soap:Envelope
  xmlns:soap="http://www.w3.org/2001/12/soap-­‐envelope"
  soap:encodingStyle="http://www.w3.org/2001/12/soap-­‐
  encoding">
  <soap:Body xmlns:m="http://www.example.com/weather">
  <m:GetWeather>
  <m:ZipCode> 94108 </m:ZipCode>
  </m:GetWeather>
  </soap:Body>
  </soap:Envelope>
```

Early on, SOAP did not have the strongest support in all languages, and it
often became a tedious task for developers to integrate SOAP using the
Web Service Definition Language. However, SOAP calls can retain state,
something that REST is not designed to do.

## XML-RPC
On the other hand, Remote Procedure Calls, or RPC APIs, are much
quicker and easier to implement than SOAP. XML-RPC was the basis for
SOAP, although many continued to use it in its most generic form, making
simple calls over HTTP with the data formatted as XML.

However, like SOAP, RPC calls are tightly coupled and require the user to
not only know the procedure name, but often the order of parameters as
well. This means that developers would have to spend extensive amounts
of time going through documentation to utilize an XML-RPC API, and
keeping documentation in sync with the API was of utmost importance, as
otherwise a developer’s attempts at integrating it would be futile.

##JSON-RPC
Introduced in 2002, the JavaScript Object Notation was developed by State
Software, Inc. and made most famous by Douglas Crawford. The format
was originally designed to take advantage of JavaScript’s ability to act as a
messaging system between the client and the browser (think AJAX).

JSON was then developed to provide a simple, concise format that could
also capture state and data types, allowing for quick deserialization.

Yahoo started taking advantage of JSON in 2005, quickly followed by
Google in 2006. Since then JSON has enjoyed rapid adoption and wide
language support, becoming the format of choice for most developers.

You can see the simplicity that JSON brought to data formatting as
compared to the SOAP/ XML format above:

```
{“zipCode”  
  :    
  “ 94108 ”}
```

However, while JSON presented a marked improvement over XML, the
downsides of an RPC API still exist with JSON-RPC APIs, including tightly
coupled URIs. Just the same, JSON-APIs have been widely adopted and
used by companies such as MailChimp, although they are often mislabeled
as “RESTful.”

## REST
Now the most popular choice for API development, REST or RESTful APIs
were designed to take advantage of existing protocols. While REST can be
used over nearly any protocol, it typically takes advantage of HTTP when
used for Web APIs. This means that developers do not need to install
libraries or additional software in order to take advantage of a REST API.

As defined by Dr. Roy Fielding in his 2000 Doctorate Dissertation, REST
also provides an incredible layer of flexibility. Since data is not tied to
methods and resources, REST has the ability to handle multiple types of
calls, return different data formats and even change structurally with the
correct implementation of hypermedia.

As you can see in this chart, each type of API offers different strengths and
weaknesses. REST, however, provides a substantial amount of freedom
and flexibility, letting you build an API that meets your needs while also
meeting the needs of very diverse customers.

Unlike SOAP, REST is not constrained to XML, but instead can return XML,
JSON, YAML or any other format depending on what the client requests.
And unlike RPC, users aren’t required to know procedure names or specific
parameters in a specific order.

But you also lose the ability to maintain state in REST, such as within
sessions, and it can be more difficult for newer developers to use. It’s also
important to understand what makes a REST API RESTful, and why these
constraints exist before building your API. After all, if you do not understand
why something is designed in the manner it is, you are more likely to
disregard certain elements and veer off course, often times hindering your
efforts without even realizing it.

## Understanding REST

Surprisingly, while most APIs claim to be RESTful, they fall short of the
requirements and constraints asserted by Dr. Roy Fielding. One of the most
commonly missed constraints of REST is the utilization of hypermedia as
the engine of application state, or HATEOAS, but we’ll talk about that in
another chapter.

There are six key constraints to REST that you should be aware of when
deciding whether or not this is the right API type for you.


Client-Server
The client-server constraint operates on the concept that the client and the
server should be separate from each other and allowed to evolve
individually. In other words, I should be able to make changes to my mobile
application without impacting either the data structure or the database
design on the server. At the same time, I should be able to modify the
database or make changes to my server application without impacting the
mobile client. This creates a separation of concerns, letting each
application grow and scale independently of the other and allowing your
organization to grow quickly and efficiently.

## Stateless
REST APIs are stateless, meaning that calls can be made independently of
one another, and each call contains all of the data necessary to complete
itself successfully. A REST API should not rely on data being stored on the
server or sessions to determine what to do with a call, but rather solely rely
on the data that is provided in that call itself.

This can be confusing, especially when you hear about using hypermedia
as the state of the application (Wait—I thought REST was stateless?)..Don’t
worry, we’ll talk about this later, but the important takeaway here is that
sessions or identifying information is not being stored on the server when
making calls. Instead, each call has the necessary data, such as the API
key, access token, user ID, etc. This also helps increase the API’s reliability

by having all of the data necessary to make the call, instead of relying on a
series of calls with server state to create an object, which may result in
partial fails. Instead, in order to reduce memory requirements and keep
your application as scalable as possible, a RESTful API requires that any
state is stored on the client—not on the server.

Cache
Because a stateless API can increase request overhead by handling large
loads of incoming and outbound calls, a REST API should be designed to
encourage the storage of cacheable data. This means that when data is
cacheable, the response should indicate that the data can be stored up to a
certain time (expires-at), or in cases where data needs to be real-time, that
the response should not be cached by the client.

By enabling this critical constraint, you will not only greatly reduce the
number of interactions with your API, reducing internal server usage, but
also provide your API users with the tools necessary to provide the fastest
and most efficient apps possible.


Keep in mind that caching is done on the client side. While you may be able
to cache some data within your architecture to perform overall performance,
the intent is to instruct the client on how it should proceed and whether or
not the client can store the data temporarily.

Uniform Interface
The key to the decoupling client from server is having a uniform interface
that allows independent evolution of the application without having the
application’s services, or models and actions, tightly coupled to the API
layer itself. The uniform interface lets the client talk to the server in a single
language, independent of the architectural backend of either. This interface
should provide an unchanging, standardized means of communicating
between the client and the server, such as using HTTP with URI resources,
CRUD (Create, Read, Update, Delete) and JSON.

However, to provide a truly uniform interface, Fielding identified four
additional interface constraints: identifying resources, manipulation through
representations, self-describing messages and HATEOAS (hypermedia as
the engine of application state).



## Layered System
As the name implies, a layered system is a system comprised of layers,
with each layer having a specific functionality and responsibility. If we think
of a Model View Controller framework, each layer has its own
responsibilities, with the models comprising how the data should be formed,
the controller focusing on the incoming actions and the view focusing on
the output. Each layer is separate but also interacts with the other.

In REST, the same principle holds true, with different layers of the
architecture working together to build a hierarchy that helps create a more
scalable and modular application. For example, a layered system allows for
load balancing and routing (preferably through the use of an API
Management Proxy tool which we’ll talk about in Chapter 11). A layered
system also lets you encapsulate legacy systems and move less commonly
accessed functionality to a shared intermediary while also shielding more
modern and commonly used components from them. A layered system also
gives you the freedom to move systems in and out of your architecture as
technologies and services evolve, increasing flexibility and longevity as long
as you keep the different modules as loosely coupled as possible.



There are substantial security benefits of having a layered system since it
allows you to stop attacks at the proxy layer, or within other layers,
preventing them from getting to your actual server architecture. By utilizing
a layered system with a proxy, or creating a single point of access, you are
able to keep critical and more vulnerable aspects of your architecture
behind a firewall, preventing direct interaction with them by the client.

Keep in mind that security is not based on single “stop all” solution, but
rather on having multiple layers with the understanding that certain security
checks may fail or be bypassed. As such, the more security you are able to
implement into your system, the more likely you are to prevent damaging
attacks.

## Code on Demand
Perhaps the least known of the six constraints, and the only optional
constraint, Code on Demand allows for code or applets to be transmitted
via the API for use within the application. In essence, it creates a smart
application that is no longer solely dependent on its own code structure.

However, perhaps because it’s ahead of its time, Code on Demand has
struggled for adoption as Web APIs are consumed across multiple
languages and the transmission of code raises security questions and
concerns. (For example, the directory would have to be writeable, and the
firewall would have to let what may normally be restricted content through.)

Just the same, I believe that while it is currently the least adopted constraint
because of its optional status, we will soon see more implementations of
Code on Demand as its benefit becomes more apparent.

Together, these constraints make up the theory of Representational State
Transfer, or REST. As you look back through these you can see how each
successive constraint builds on top of the previous, eventually creating a
rather complex—but powerful and flexible—application program interface.

But most importantly, these constraints make up a design that operates
similarly to how we access pages in our browsers on the World Wide Web.
It creates an API that is not dictated by its architecture, but by the
representations that it returns, and an API that—while architecturally
stateless—relies on the representation to dictate the application’s state.

Please keep in mind that this is nothing more than a quick overview of the
constraints of REST as defined by Dr. Fielding. For more information on the
different constraints, you can read his full dissertation online at
http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm.

Architectural Diagrams provided courtesy of Roy Fielding, from
Architectural Styles and the Design of Network-based Software
Architectures. Doctoral dissertation, University of California, Irvine, 2000.
