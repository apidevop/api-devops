## What is an API?

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

## JSON-RPC
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
