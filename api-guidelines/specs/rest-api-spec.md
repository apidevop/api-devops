# REST API Specification

## Content

- Introduction REST API Design
- Nouns are good; verbs are bad
- Plural nouns and concrete names
- Simplify associations - sweep complexity under the ‘?’
- Handling errors
- Tips for Versioning.
- Pagination and partial response
- What about responses that don’t involve resources?
- Supporting multiple formats
- What about attribute names?
- Tips for search
- Consolidate API requests in one subdomain
- Tips for handling exceptional behavior
- Authentication
- Making requests on your API
- Chatty APIs
- Complement with an SDK
- The API Façade Pattern


## Introduction

REST is an architectural style and not a strict standard, it allows for a lot of flexibly. 
Due to that flexibility and freedom of structure, there is also a big appetite for design best practices.

This document is a collection of design practices that I have made with references to inspiration from the content shared by some of the best API development teams around the world,


**Are you a Pragmatist or a RESTafarian?**

Let’s start with our overall point of view on API design. We advocate pragmatic, not
bearmatic REST. What do we mean by bearmatic?

You might have seen discussion threads on true REST - some of them can get pretty strict
and wonky. Mike Schinkel sums it up well - defining a RESTafarian as follows:

```
“A RESTifarian is a zealous proponent of the REST software architectural style as defined by
Roy T.Fielding in Chapter 5 of his PhD. dissertation at UC Irvine. 
You can find RESTifarians in the wild on the REST-discuss mailing list. 

But be careful, RESTifarians can be extremely 
meticulous when discussing the finer points of REST”

```

Our view: approach API design from the ‘outside-in’ perspective. This means we start by
asking - what are we trying to achieve with an API?

The API’s job is to make the developer as successful as possible. The orientation for APIs is
to think about design choices from the application developer’s point of view.


Why? Look at the value chain below – the application developer is the lynchpin of the
entire API strategy. The primary design principle when crafting your API should be to
maximize developer productivity and success. This is what we call pragmatic REST.

**Pragmatic REST is a design problem**

You have to get the design right, because design communicates how something will be
used. The question becomes - what is the design with optimal benefit for the app
developer?

The developer point of view is the guiding principle for all the specific tips and best
practices we’ve compiled.

## Nouns are good; verbs are bad

The number one principle in pragmatic RESTful design is: keep simple things simple.

**Keep your base URL simple and intuitive**

The base URL is the most important design affordance of your API. A simple and intuitive
base URL design makes using your API easy.

Affordance is a design property that communicates how something should be used without
requiring documentation. A door handle's design should communicate whether you pull or
push. Here's an example of a conflict between design affordance and documentation - not
an intuitive interface!


A key litmus test we use for Web API design is that there should be only **2 base URLs per
resource**. Let's model an API around a simple object or resource, a bear, and create a Web
API for it.

The first URL is for a collection; the second is for a specific element in the collection.

```
/bears /bears/

```
Boiling it down to this level will also force the verbs out of your base URLs.

**Keep verbs out of your base URLs**

Many Web APIs start by using a method-driven approach to URL design. These method-
based URLs sometimes contain verbs - sometimes at the beginning, sometimes at the end.

For any resource that you model, like our bear, you can never consider one object in
isolation. Rather, there are always related and interacting resources to account for - like
owners, veterinarians, leashes, food, squirrels, and so on.


Think about the method calls required to address all the objects in the bears’ world. The
URLs for our resource might end up looking something like this.

It's a slippery slope - soon you have a long list of URLs and no consistent pattern making it
difficult for developers to learn how to use your API.

**Use HTTP verbs to operate on the collections and elements.**

For our bear resources, we have two base URLs that use nouns as labels, and we can operate
on them with HTTP verbs. Our HTTP verbs are **POST** , **GET** , **PUT** , and **DELETE**. (We think of
them as mapping to the acronym, **CRUD** ( **C** reate- **R** ead- **U** pdate- **D** elete).)

With our two resources (/bears and /bears/1234) and the four HTTP verbs, we have a
rich set of capability that's intuitive to the developer. Here is a chart that shows what we
mean for our bears.


**Resource POST**
create

### GET

```
read
```
### PUT

```
update
```
### DELETE

```
delete
```
**/bears** Create a new
bear

```
List bears Bulk update
bears
```
```
Delete all bears
```
**/bears/1234** Error Show Bear If exists update
Bear

```
If not error
```
```
Delete Bear
```
The point is that developers probably don't need the chart to understand how the API
behaves. They can experiment with and learn the API without the documentation.

In summary:

```
Use two base URLs per resource.

Keep verbs out of your base URLs.

Use HTTP verbs to operate on the collections and elements.

```
## Plural nouns and concrete names

Let’s explore how to pick the nouns for your URLs.

Should you choose singular or plural nouns for your resource names? You'll see popular
APIs use both. Let's look at a few examples:

**Foursquare GroupOn Zappos**
```
/checkins /deals /Product

```

Given that the first thing most people probably do with a RESTful API is a GET, we think it
reads more easily and is more intuitive to use plural nouns. But above all, avoid a mixed
model in which you use singular for some resources, plural for others. Being consistent
allows developers to predict and guess the method calls as they learn to work with your
API.

**Concrete names are better than abstract**
Achieving pure abstraction is sometimes a goal of API architects. However, that abstraction
is not always meaningful for developers.

Take for example an API that accesses content in various forms - blogs, videos, news
articles, and so on.

An API that models everything at the highest level of abstraction - as /items or /assets
in our example - loses the opportunity to paint a tangible picture for developers to know
what they can do with this API. It is more compelling and useful to see the resources listed
as blogs, videos, and news articles.

The level of abstraction depends on your scenario. You also want to expose a manageable
number of resources. Aim for concrete naming and to keep the number of resources
between 12 and 24.

In summary, an intuitive API uses plural rather than singular nouns, and concrete rather
than abstract names.


## Type Formatting

### Boolean

A boolean is represented with **true** or **false**.

### Dates and Times

Use [**ISO 8601**](http://en.wikipedia.org/wiki/ISO_8601) format for passing in and out dates and times. Use [**UTC**](https://en.wikipedia.org/wiki/UTC) as timezone.

|Date|2015-07-02|
|---|---|
|Combined date and time in [**UTC**](https://en.wikipedia.org/wiki/UTC)|2015-07-02T14:47:47Z|


### Currency

Use [**3-character ISO-4217**](http://en.wikipedia.org/wiki/ISO_4217#Active_codes) codes for specifying currencies.

##### Samples
	 EUR - Euro  
	 CHF - Swiss franc  
	 USD - US Dollar  

### Country

Use country codes defined by the [**ISO 3166-1-alpha-2**](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) code standard.

##### Samples
	 AD - Andorra  
	 DE - Germany  
	 FR - France  
	 US - United States of America  

### Amount

Use the format [0-9]+(\.[0-9]+)? to represent an amount like money.
Separate amount and currency in different fields.

##### Samples
	     12.23
	   3789.1
	  56782
	    -12.02
	   +231.98

### UUID

A UUID or GUID is represented in the Format  '**dddddddd-dddd-dddd-dddd-dddddddddddd**' where each `d` represents `[A-Fa-f0-9]`.

##### Samples
	 56e94f2b-25ac-4c58-9828-f63b66220999
	 2ABAD998-48F6-4847-9EB2-2A6B5328C51D
	 

##HTTP Status Codes
The following table lists the HTTP response status codes for the `GET` (retrieve), `POST` (create), `PUT` (modify), and `DELETE` operations.

<div><table><colgroup><col><col><col><col></colgroup><thead><tr><th>Response Code</th><th>HTTP Operation</th><th>Response Body Contents</th><th>Description</th></tr></thead><tbody><tr><td>200</td><td>GET, PUT, DELETE</td><td>Resource</td><td>No error, operation successful.</td></tr><tr><td>201 Created</td><td>POST</td><td>Resource that was created</td><td>Successful creation of a resource. The Location HTTP Header can provide a link to the newly created resource.</td></tr><tr><td>204 No Content</td><td>POST, PUT, DELETE</td><td>N/A</td><td>The request was processed successfully, but no response body is needed.</td></tr><tr><td>304 Not Modified</td><td>conditional GET</td><td>N/A</td><td>Resource has not been modified. <span style="line-height: 1.4285;">Used when HTTP caching headers are in play.</span></td></tr><tr><td>400 Bad Request</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>Malformed syntax or a bad query.</td></tr><tr><td>401 Unauthorized</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>Action requires user authentication.</td></tr><tr><td>403 Forbidden</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>

When authentication succeeded but authenticated user doesn't have access to the resource.

</td></tr><tr><td>404 Not Found</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>Resource not found.</td></tr><tr><td>405 Not Allowed</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>Method not allowed on resource.</td></tr><tr><td>406 Not Acceptable</td><td>GET</td><td>Error Message</td><td>Requested representation not available for the resource.</td></tr><tr><td>408 Request Timeout</td><td>GET, POST</td><td>Error Message</td><td>Request has timed out.</td></tr><tr><td>409 Resource Conflict</td><td>PUT, PUT, DELETE</td><td>Error Message</td><td>State of the resource doesn't permit request.</td></tr><tr><td>410 Gone</td><td>GET, PUT</td><td>Error Message</td><td>

Indicates that the resource at this end point is no longer available. Useful as a blanket response for old API versions

</td></tr><tr><td>411 Length Required</td><td>POST, PUT</td><td>Error Message</td><td>The server needs to know the size of the entity body and it should be specified in the Content Length header.</td></tr><tr><td>412 Precondition failed</td><td>GET</td><td>Error Message</td><td>Operation not completed because preconditions were not met.</td></tr><tr><td>415 Unsupported Type</td><td>POST, PUT</td><td>Error Message</td><td>Representation not supported for the resource.</td></tr><tr><td>422 Unprocessable Entity</td><td>POST, PUT</td><td>Error Message</td><td>

Used for validation errors

</td></tr><tr><td>500 Server Error</td><td>GET, POST, PUT</td><td>Error Message</td><td>Internal server error.</td></tr><tr><td>501 Not Implemented</td><td>POST, PUT, DELETE</td><td>Error Message</td><td>Requested HTTP operation not supported.</td></tr></tbody></table></div>

## URI Components
### Hostname

Make no assumption about the hostname.
Assume that each API will have a different hostname. In particular do not assume a specific IP address. Services can be dynamically deployed or moved.
DNS was invented to deal with that.

###Service namespace

In any URI, the first noun (which may be singular or plural, depending on the situation) should be considered a “Service namespace”. Service namespaces should reflect the customer's perspective on the (business) service boundary.

Do not use acronyms. Use small caps and underscore `_` for `<space>` (governs all of the url and messages).

##### URI Template

	/{namespace}/

##### Example

	/user_management/

### Version

The URI should include `/vN` with the major version (`N`) as a prefix. No major.minor syntax. URL-based versioning is utilized for it's simplicity of use for API consumers, versus the more complex header-based approach.[^1]

[^1] See the following [brief overview of versioning methods](https://www.3scale.net/2016/06/api-versioning-methods-a-brief-reference/)

Version is a single number and only to be incremented on ‘backward’ compatibility breaking. Adding fields or deprecating fileds are **NOT** breaking changes. API Clients need to be able to **ignore** additional elements. API Providers need to be able to use meaningful defaults for new fields not expected by existing clients.  

Please stick to the following rule:

> In general do **NOT** increment the version of your API. Until you must. And it is really rare that you must. Only increment on breaking changes. Extending the API is not a breaking change.

##### URI Template

	/{namespace}/v{version}/

##### Example

	/user_management/v1/

### Resource References

The URI references for resources should consistently use the same path components to refer to resources. Sub-namespace or sub-folders should be avoided, to maintain path consistency. This allows consumer developers to have a predictable experience in case they are building URIs in code.

##### URI Template

	/{namespace}/{version}/{resource}/{resource-id}/{sub-resource}/{sub-resource-id}


### baseurl

In the following sections of the guide the term **baseurl** is often used to indicate an absolute URL.
The **baseurl** is a replacement for the host, namespace and version.

##### URI Template

	baseurl = {scheme}://{host}/{namespace}/{version}

### Valid and invalid url

Your rest endpoints normally end with a noun like

    {host}/user_management/v1/users

There is no trailing slash. You CAN support a trailing slash. But it is not common. Do NOT support urls like `{host}/user_management/v1/users/?queryparams`. This is not valid syntax!

## Resources

The fundamental concept in any RESTful API is the **resource**. One of the first steps in developing a RESTful web service is designing the resource model.
The resource model identifies and classifies all the resources the client uses to interact with the Server.

**Design your API with the API consumer in mind!**

Each resource MUST make sense from the perspective of the API consumer.
Beware of simply copy your internal model. Do not leak irrelevant implementation details out to your API.

A resource is an object with a type, associated data, relationships to other resources, and a set of methods that operate on it. HTTP verbs are used to manipulate the resources.

Resources can be grouped into **collections**. Each collection is homogeneous so that it contains only one type of resource, and unordered. Resources can also exist outside any collection. In this case, we refer to these resources as singleton resources. Collections are themselves resources as well.

Collections can exist globally at the top level of an API, but can also be contained inside a single resource. In the latter case, we refer to these collections as sub-collections. Sub-collections are usually used to express some kind of “contained in” relationship. We go into more detail on this in [Relationships and Sub-Resources](../relationships-and-sub-resources/relationships-and-sub-resources.md).

The diagram below illustrates the key concepts in a RESTful API.

![Rest Resource Model](restresourcemodel.png)

We call information that describes available resources types, their behavior, and their relationships the resource model of an API. The resource model can be viewed as the RESTful mapping of the application data model.

Find more details in the chapters:

- [Collection Resources](../collection-resources/collection-resources.md)
- [Filtering, sorting, field selection and paging](../filtering-sorting-field-selection-and-paging/filtering-sorting-field-selection-and-paging.md)

	 
## Request headers Parameters
| Parameter name   | Required      | Description  |
| :------------- | :-------------: | :-----|
| Authorization | Yes   | Needed only when authorization is necessary |
| Accept-Encoding| Yes  | HTTP compression is mandatory |
| Api-Key | Yes     | API consumer unique application key  |
| Accept | No     | Required for content and version negotiation |
| Accept-Language | No     | Used for region and language selection, using ISO region and language Code,|
| OIDC-ClientId | Yes     | Client id (Audience), that identifies consuming client org Id relying party. Required only when using org Id REF access tokens |
| OIDC-Issuer | Yes     | REF token issuer, required only when using org Id REF access tokens|
| Backend-OperationId | No     | Id used for tracking operation execution end to end for fault tracing purposes|

## Response headers Parameters
| Parameter name   | Required      | Description  |
| ------------- |:-------------:| -----:|
| Backend-Region | No			| Returns which region that handled the request |

## Simplify associations - sweep complexity under the ‘?’

In this section, we explore API design considerations when handling associations between
resources and parameters like states and attributes.

**Associations**
Resources almost always have relationships to other resources. What's a simple way to
express these relationships in a Web API?

Let's look again at the API we modeled in nouns are good, verbs are bad - the API that
interacts with our **bears** resource. Remember, we had two base URLs: /bears and
bears/1234.

We're using HTTP verbs to operate on the resources and collections. Our bears belong to
owners. To get all the bears belonging to a specific owner, or to create a new bear for that
owner, do a GET or a POST:

```
GET /owners/5678/bears

```

```
POST /owners/5678/bears

```
Now, the relationships can be complex. Owners have relationships with veterinarians, who
have relationships with bears, who have relationships with food, and so on. It's not
uncommon to see people string these together making a URL 5 or 6 levels deep. Remember
that once you have the primary key for one level, you usually don't need to include the
levels above because you've already got your specific object. In other words, you shouldn't
need too many cases where a URL is deeper than what we have above
/resource/identifier/resource.

**Sweep complexity behind the ‘?’**
Most APIs have intricacies beyond the base level of a resource. Complexities can include
many states that can be updated, changed, queried, as well as the attributes associated with
a resource.

Make it simple for developers to use the base URL by putting optional states and attributes
behind the HTTP question mark. To get all red bears running in the park:

```
GET /bears?color=red&state=running&location=park

```
In summary, keep your API intuitive by simplifying the associations between resources,
and sweeping parameters and other complexities under the rug of the HTTP question
mark.


## Hypermedia and REST
There are four different Levels in the REST API Evolution. They are described in the Richardson Maturity Model. You can find more information in [this](http://martinfowler.com/articles/richardsonMaturityModel.html) article from Martin Fowler.

Hypermedia represents the links to your API. The main characteristics of Hypermedia are:

- Decouple the Server and the Client
- Links are to help developers know how to use the API
	- The developer interacts with the first URL
	- The Navigation is controlled by the server
- API becomes self-describing
- Links become the application state
- HATEOAS

> (**HATEOAS**) Hypermedia as the Engine of Application State. The principle of addressability just says that every 
> resource should have its own URL. If something is important to your application, it should have a unique name, a 
> URL, so that you and your users can refer to it unambiguously.

The URI Templates for HATEOAS are covered in RFC 6570 and the Link Header in RFC 5988.

At its core, HATEAOAS provides a way to interact with the REST API entirely through hyperlinks. With each call that you make to the API, we’ll return an array of links that allow you to request more information about a call and to further interact with the API. You no longer need to hard code the logic necessary to use the REST API.

HATEOAS links are contextual, so you only get the information that is relative to a specific request.

Details about Hypermedia formats are described in [Hypermedia Formats](../response-format/response-format.md).

[On Choosing a Hypermedia Format](http://sookocheff.com/post/api/on-choosing-a-hypermedia-format/) provides a good overview and guide of Hypermedia Formats.

### Relative vs Absolute

It is strongly recommended that the URLs generated by the API should be absolute URLs.

The reason is mostly for ease of use by the client, so that it never has to find out the correct base URI for a resource, which is needed to interpret a relative URL. The [URL RFC](http://tools.ietf.org/html/rfc3986#section-5.1) specifies the algorithm for determining a base URL, which is rather complex. One of the options for finding the base URL is to use the URL of the resource that was requested. Since a resource may appear under multiple URLs (for example, as part of a collection, or stand-alone), it would be a significant overhead to a client to remember where he retrieved a representation from. By using absolute URLs, this problem doesn’t present itself.

#### On determining which URI to return

In [Hypermedia responses](../response-format/response-format.md), we require the use of absolute URLs (as stated in the above section). If your API lives behind an [API Management](../api-management/api-management.md), the actual URI of the (backend) API will be different from the URI which is the one to call, i.e. the one of the API management gateway.

As a rule, the API Management gateway MUST pass an additional header with the call to the backend API containing the API Gateway base URL for the API at hand. This header must be named `Forwarded`, as described in [RFC 7239](https://tools.ietf.org/html/rfc7239). The two parameters `host` and `proto` are pre-defined in the RFC, whereas we need additional ones, which is explicitly allowed per Section 5.5 of the RFC:

* `host`: Must contain the host name of the API Gateway; this is the host an API client uses to actually talk to the API.
* `proto`: The protocol/schema to use. This MUST be `https` for all of our APIs (see [security and authentication](../security-and-authentication/security-and-authentication.md))
* Extension `port`: OPTIONAL - The port of the API Gateway. Usually this can be assumed to be `443` if `proto` equals `https`; use if deviating
* Extension `prefix`: The base path of the API on the API Gateway, e.g. `/someapi/v1`. 

**Example**: A request goes to `https://api.contenthub.haufe.io/ingest/v1/bulk/63874638746834`, which is forwarded to the ingest service in the backend. The API Gateway must pass on this information in the following way:

```
Forwarded: proto=https;host=api.contenthub.haufe.io;port=443;prefix=/ingest/v1
```

If present, this header MUST be used to assemble hypermedia links in responses, so that an API consumer is directed to the right place.

If this header is not present, the API backend SHOULD use its own absolute base URI as a fallback.

## Hypermedia Response Format
> Use existing standard formats like JSON, ATOM, Collection-JSON, HAL (both JSON and XML) etc. over home made formats.

The list here represents only a subset of possible formats. Refer to [On Choosing a Hypermedia Format](http://sookocheff.com/post/api/on-choosing-a-hypermedia-format/) for a more detailed overview of Hypermedia Formats.

Regardless of your choice all formats need to support link relation elements.

* Use **IANA** defined link relation attribute values by default - you can find the list of supported values [here](http://www.iana.org/assignments/link-relations/link-relations.xhtml).
* if new link relation attributes are required, register them with IANA (alternative or fallback is the canonicalized list maintained by Haufe CTO Office). Please contact the CTO Office for the latter.

A response SHOULD return related and valid link elements, and MUST return a link element to **self**.

An API client should not hardcode any resource URL or make assumptions about an existing URL. Instead URL’s should be derived from link elements via the appropriate link relation Attribute.

[Here](https://en.wikipedia.org/wiki/Internet_media_type) you can find a list of Content types.

> Our prefered way for hypermedia support is HAL. Other formats like Atom are also a valid choice. It is strongly recommended that the URLs generated by the API should be absolute URLs.


### HAL – Hypermedia Application Language

It has custom content and profile media type, the description of the data.

The content types are:

    application/hal+json
    application/hal+xml

More about it in their official [website](http://stateless.co/hal_specification.html).

This is the desciption from the Website http://stateless.co/hal_specification.html.

HAL provides a set of conventions for expressing hyperlinks in either JSON or XML.

**The rest of a HAL document is just plain old JSON or XML with optional embedded resources**

Instead of using ad-hoc structures - or spending valuable time designing your own format - you can adopt HAL's conventions and focus on building and documenting the data and transitions that make up your API.

HAL is a little bit like HTML for machines, in that it is generic and designed to drive many different types of application via hyperlinks. The difference is that HTML has features for helping 'human actors' move through a web application to achieve their goals, whereas HAL is intended for helping 'automated actors' move through a web API to achieve their goals.

Having said that, **HAL is actually very human-friendly too**. Its conventions make the documentation for an API discoverable from the API messages themselves.  This makes it possible for developers to jump straight into a HAL-based API and explore its capabilities, without the cognitive overhead of having to map some out-of-band documentation onto their journey.

### ATOM
	Content-Type: application/Atom+XML

Mike Amundsen described the format [here.](http://amundsen.com/hypermedia/atom/) The next paragraph is a copy from his page.

> Atom is an XML-based document format that describes lists of related information known as "feeds". Feeds are composed of a number of items, known as "entries", each with an extensible set of attached metadata. For example, each entry has a title. The primary use case that Atom addresses is the syndication of Web content such as weblogs and news headlines to Web sites as well as directly to user agents. </span>[<sup>[1]</sup>](http://amundsen.com/hypermedia/atom/#ref-atom "The Atom Syndication Format")

> The Atom Publishing Protocol is an application-level protocol for publishing and editing Web Resources using HTTP [RFC2616] and XML 1.0\. The protocol supports the creation of Web Resources and provides facilities for: 1) Collections: Sets of Resources, which can be retrieved in whole or in part; 2) Services: Discovery and description of Collections; and 3) Editing: Creating, editing, and deleting Resources. </span>[<sup>[2]</sup>](http://amundsen.com/hypermedia/atom/#ref-atompub "The Atom Publishing Protocol")

### Collection+JSON

	Content-Type: application/vnd.collection+json

Mike Amundsen described the format [here](http://amundsen.com/media-types/collection/format/) in a clear and lucid way. The next paragraph is a copy from his page.

> The [Collection+JSON](http://amundsen.com/media-types/collection/) hypermedia type is designed to support full read/write capability for simple lists (contacts, tasks, blog entries, etc.). The standard application semantics supported by this media type include Create, Read, Update, and Delete (CRUD) along w/ support for predefined queries including [query templates](http://amundsen.com/media-types/collection/format/#query-templates) (similar to HTML "GET" forms). Write operations are defined using a [template](http://amundsen.com/media-types/collection/format/#objects-template) object supplied by the server as part of the response representation.

> Each [item](http://amundsen.com/media-types/collection/format/#arrays-items) in a [Collection+JSON](http://amundsen.com/media-types/collection/)[collection](http://amundsen.com/media-types/collection/format/#objects-collection) has an assigned URI (via the [href](http://amundsen.com/media-types/collection/format/#properties-href) property) and an optional array of one or more [data](http://amundsen.com/media-types/collection/format/#arrays-data) elements along with an optional array of one or more [link](http://amundsen.com/media-types/collection/format/#arrays-links) elements. Both arrays support a [name](http://amundsen.com/media-types/collection/format/#properties-name) property for each object in the collection in order to decorate these elements with domain-specific semantic information (e.g. `"data" : [{"name" : "first-name", ...},...]`).

> The [Collection+JSON](http://amundsen.com/media-types/collection/) hypermedia type has a limited set of predefined [link relation](http://amundsen.com/media-types/collection/format/#link-relations) values and supports [additional values](http://amundsen.com/media-types/collection/format/#rels-other) applied by implementors in order to better describe the application domain to which the media type is applied.

> The following sections describe the process of reading and writing data using the [Collection+JSON](http://amundsen.com/media-types/collection/) hypermedia type as well as the way to parse and execute [Query Templates](http://amundsen.com/media-types/collection/format/#query-templates). Additional examples can be found in the [Examples](http://amundsen.com/media-types/collection/examples/) section of this documentation.

### JSON

[Here](https://en.wikipedia.org/wiki/JSON) you can find a more detailed explanation.

	Content-Type: application/json

The following example shows a possible JSON representation describing a person.

	{
	  "firstName": "John",
	  "lastName": "Smith",
	  "isAlive": true,
	  "age": 25,
	  "address":
    {
	    "streetAddress": "21 2nd Street",
	    "city": "New York",
	    "state": "NY",
	    "postalCode": "10021-3100"
	  },
	  "phoneNumbers":
    [
	    {
	      "type": "home",
	      "number": "212 555-1234"
	    },
	    {
	      "type": "office",
	      "number": "646 555-4567"
	    }
	  ],
	  "children": [],
	  "spouse": null
	}

### XML

JSON has outrun XML as response format. However it is a good idea to support both.

	Content-Type: application/xml

The following example shows a possible XML representation describing a person.

	<person>
	  <firstName>John</firstName>
	  <lastName>Smith</lastName>
	  <age>25</age>
	  <address>
	    <streetAddress>21 2nd Street</streetAddress>
	    <city>New York</city>
	    <state>NY</state>
	    <postalCode>10021</postalCode>
	  </address>
	  <phoneNumbers>
	    <phoneNumber>
	      <type>home</type>
	      <number>212 555-1234</number>
	    </phoneNumber>
	    <phoneNumber>
	      <type>fax</type>
	      <number>646 555-4567</number>
	    </phoneNumber>
	  </phoneNumbers>
	  <gender>
	    <type>male</type>
	  </gender>
	</person>

### Hypermedia Link base URLs

For a discussion of the `Forwarded` header and its usage, see [Hypermedia and REST](../hypermedia-and-rest/hypermedia-and-rest.md).

## Filtering, sorting, field selection and paging
### Filtering
Use a unique query parameter for all fields or a query language for filtering.

	GET /cars?color=red Returns a list of red cars
	GET /cars?type=%2Avan%2A Returns a list of cars whose type contains the 'van' word (Minivan, Cargo Van...). %2A is the encoded form of the * wildcard. 
	GET /cars?seats<=2 Returns a list of cars with a maximum of 2 seast

### Time selection

`start_time` or `{property_name}_from`, `end_time` or `{property_name}_to` query parameters should be provided if time selection is needed.

### Sorting

Allow ascending and descending sorting over multiple fields.

	GET /cars?sort=-manufactorer,+model


This returns a list of cars sorted by descending manufacturers and ascending models.

The sort order for each sort field **MUST** be specified with one of the following prefixes:

- Plus (U+002B PLUS SIGN, "+") to request an ascending sort order.
- Minus (U+002D HYPHEN-MINUS, "-") to request a descending sort order.

### Field selection

Mobile clients display just a few attributes in a list. They don’t need all attributes of a resource. Give the API consumer the ability to choose returned fields. This will also reduce the network traffic and speed up the usage of the API.

	GET /cars?fields=manufacturer,model,id,color


### Paging

There are two popular ways how to support paging.

#### limit and offset style

Use **limit** and **offset**. It is flexible for the user and common in API implementations (Facebook). The default should be limit=**20** and offset=**0**.

	GET /products?limit=25&offset=50


##### Response Fields
	{
	   "limit": 25,
	   "offset": 50,
	   "total_count": 1634
	}

|Name|Type|Description|
|---|---|---|
|offset|Integer|The offset used in the execution of the query.|
|limit|Integer|The limit used in the execution of the query.|
|total_count|Integer|The total number of records available.|

#### Page style

Pages of results should be referred to consistently by the query parameters `page` and `page_size`, where `page_size` refers to the amount of results per request, and `page` refers to the requested page. Additionally, responses should include `total_count` and `total_pages` whenever possible, where `total_count` indicates the total records in the requested collection, and `total_pages` is the number of pages (interpolated from `total_count`/`page_size`).

The default should be `page=1` and `page_size=20`.

	GET /products?page=2&page_size=30


##### Response Fields

	{
	   "page": 2,
	   "page_size": 30,
	   "total_count": 1634,
	   "total_pages": 55
	}

|Name|Type|Description|
|---|---|---|
|page|Integer|The page used in the execution of the query.|
|page_size|Integer|The page_size used in the execution of the query.|
|total_count|Integer|The total number of records available.|
|total_pages|Integer|The total number of pages available|

### Hypermedia links

Hypermedia links are high value in navigating paged resource collections, as `page`/`page_size` query parameters can be maintained while navigating pages of results.

Links should be provided with `rels` of `next`, `previous`, `first`, `last` wherever appropriate.


## Handling errors

Many software developers, including myself, don't always like to think about exceptions
and error handling but it is a very important piece of the puzzle for any software developer,
and especially for API designers.

**Why is good error design especially important for API designers?**

From the perspective of the developer consuming your Web API, everything at the other
side of that interface is a black box. Errors therefore become a key tool providing context
and visibility into how to use an API.

First, developers learn to write code through errors. The "test-first" concepts of the
extreme programming model and the more recent "test driven development" models
represent a body of best practices that have evolved because this is such an important and
natural way for developers to work.

Secondly, in addition to when they're developing their applications, developers depend on
well-designed errors at the critical times when they are troubleshooting and resolving
issues after the applications they've built using your API are in the hands of their users.

**How to think about errors in a pragmatic way with REST?**

Let's take a look at how three top APIs approach it.

**Facebook**

HTTP Status Code: 200

```
{"type" : "OauthException", "message":"(#803) 
Some of the aliases you requested do not exist: foo.bar"}

```
**Twilio**

HTTP Status Code: 401

```
{"status" : "401", "message":"Authenticate","code": 20003, "more
info": "http://www.twilio.com/docs/errors/20003"}

```
**SimpleGeo**

HTTP Status Code: 401

```
{"code" : 401, 
"message": "Authentication Required"}
```
**Facebook**
No matter what happens on a Facebook request, you get back the 200-status code -
everything is OK. Many error messages also push down into the HTTP response. Here they
also throw an #803 error but with no information about what #803 is or how to react to it.

**Twilio**
Twilio does a great job aligning errors with HTTP status codes. Like Facebook, they provide
a more granular error message but with a link that takes you to the documentation.
Community commenting and discussion on the documentation helps to build a body of
information and adds context for developers experiencing these errors.

**SimpleGeo**
SimpleGeo provides error codes but with no additional value in the payload.

**A couple of best practices**

**Use HTTP status codes**
Use HTTP status codes and try to map them cleanly to relevant standard-based codes.

There are over 70 HTTP status codes. However, most developers don't have all 70
memorized. So if you choose status codes that are not very common you will force
application developers away from building their apps and over to Wikipedia to figure out
what you're trying to tell them.

Therefore, most API providers use a small subset. For example, the Google GData API uses
only 10 status codes; Netflix uses 9, and Digg, only 8.

**Google GData**
```
200 201 304 400 401 403 404 409 410 500

```
**Netflix**
```
200 201 304 400 401 403 404 412 500

```
**Digg**
```
200 400 401 403 404 410 500 503

```
**How many status codes should you use for your API?**

When you boil it down, there are really only 3 outcomes in the interaction between an app
and an API:

```
- Everything worked - success
- The application did something wrong – client error
- The API did something wrong – server error

```

Start by using the following 3 codes. If you need more, add them. But you shouldn't need to
go beyond 8.

```
- 200 - OK
- 400 - Bad Request
- 500 - Internal Server Error

```
If you're not comfortable reducing all your error conditions to these 3, try picking among
these additional 5:

```
- 201 - Created
- 304 - Not Modified
- 404 – Not Found
- 401 - Unauthorized
- 403 - Forbidden
```

(Check out this good Wikipedia entry for all HTTP Status codes.)

It is important that the code that is returned can be consumed and acted upon by the
application's business logic - for example, in an if-then-else, or a case statement.

**Make messages returned in the payload as verbose as possible.**

**Code for code**

200 – OK
401 – Unauthorized

**Message for people**

{"developerMessage" : "Verbose, plain language description of
the problem for the app developer with hints about how to fix
it.", "userMessage":"Pass this message on to the app user if
needed.", "errorCode" : 12345, "more info":
"http://dev.teachbearrest.com/errors/12345"}

In summary, be verbose and use plain language descriptions. Add as many hints as your
API team can think of about what's causing an error.

We highly recommend you add a link in your description to more information, like Twilio
does.


## Tips for versioning.
Versioning is one of the most important considerations when designing your Web API.

**Never release an API without a version and make the version mandatory.**

Let's see how three top API providers handle versioning.

Twilio /2010- 04 -01/Accounts/

salesforce.com /services/data/v20.0/sobjects/Account

Facebook ?v=1.

**Twilio uses a timestamp in the URL (note the European format).**

At compilation time, the developer includes the timestamp of the application when the
code was compiled. That timestamp goes in all the HTTP requests.

When a request arrives, Twilio does a look up. Based on the timestamp they identify the
API that was valid when this code was created and route accordingly.

It's a very clever and interesting approach, although we think it is a bit complex. For
example, it can be confusing to understand whether the timestamp is the compilation time
or the timestamp when the API was released.

**Salesforce.com uses v20.0, placed somewhere in the middle of the URL.**

We like the use of the **v.** notation. However, we don't like using the **.0** because it implies
that the interface might be changing more frequently than it should. The logic behind an
interface can change rapidly but the interface itself shouldn't change frequently.

**Facebook also uses the v. notation but makes the version an optional parameter.**

This is problematic because as soon as Facebook forced the API up to the next version, all
the apps that didn't include the version number broke and had to be pulled back and
version number added.


**How to think about version numbers in a pragmatic way with REST?**

Never release an API without a version. Make the version mandatory.

Specify the version with a **'v'** prefix. Move it all the way to the left in the URL so that it has
the highest scope (e.g. /v1/bears).

Use a simple ordinal number. Don't use the dot notation like v1.2 because it implies a
granularity of versioning that doesn't work well with APIs--it's an interface not an
implementation. Stick with v1, v2, and so on.

**How many versions should you maintain?** Maintain at least one version back.

**For how long should you maintain a version?** Give developers at least one cycle to react
before obsoleting a version.

Sometimes that's 6 months; sometimes it’s 2 years. It depends on your developers'
development platform, application type, and application users. For example, mobile apps
take longer to rev’ than Web apps.

**Should version and format be in URLs or headers?**

There is a strong school of thought about putting format and version in the header.

Sometimes people are forced to put the version in the header because they have multiple
inter-dependent APIs. That is often a symptom of a bigger problem, namely, they are
usually exposing their internal mess instead of creating one, usable API facade on top.

That’s not to say that putting the version in the header is a symptom of a problematic API
design. It's not!

In fact, using headers is more correct for many reasons: it leverages existing HTTP
standards, it's intellectually consistent with Fielding's vision, it solves some hard real-
world problems related to inter-dependent APIs, and more.

However, we think the reason most of the popular APIs do not use it is because it's less fun
to hack in a browser.

Simple rules we follow:

If it changes the logic you write to handle the response, put it in the URL so you can see it
easily.

If it doesn't change the logic for each response, like OAuth information, put it in the header.


These for example, all represent the same resource:

```
bears/
Content-Type: application/json
```
```
bears/
Content-Type: application/xml
```
```
bears/
Content-Type: application/png
```

The code we would write to handle the responses would be very different.

There's no question the header is more correct and it is still a very strong API design.


## Pagination and partial response

Partial response allows you to give developers just the information they need.

Take for example a request for a tweet on the Twitter API. You'll get much more than a
typical twitter app often needs - including the name of person, the text of the tweet, a
timestamp, how often the message was re-tweeted, and a lot of metadata.

Let's look at how several leading APIs handle giving developers just what they need in
responses, including Google who pioneered the idea of **partial response**.

**LinkedIn**

```
/people:(id,first-name,last-name,industry)

```

This request on a person returns the ID, first name, last name, and the industry.

LinkedIn does partial selection using this terse :(...) syntax which isn't self-evident.
Plus it's difficult for a developer to reverse engineer the meaning using a search engine.

**Facebook**

```
/joe.smith/friends?fields=id,name,picture

```

**Google**

```
?fields=title,media:group(media:thumbnail)

```

Google and Facebook have a similar approach, which works well.

They each have an optional parameter called fields after which you put the names of fields
you want to be returned.

As you see in this example, you can also put sub-objects in responses to pull in other
information from additional resources.

**Add optional fields in a comma-delimited list**

The Google approach works extremely well.

Here's how to get just the information we need from our bears API using this approach:

```
/bears?fields=name,color,location

```
It's simple to read; a developer can select just the information an app needs at a given time;
it cuts down on bandwidth issues, which is important for mobile apps.


The partial selection syntax can also be used to include associated resources cutting down
on the number of requests needed to get the required information.

**Make it easy for developers to paginate objects in a database**

It's almost always a bad idea to return every resource in a database.

Let's look at how Facebook, Twitter, and LinkedIn handle pagination. Facebook uses **offset**
and **limit**. Twitter uses page and **rpp** (records per page). LinkedIn uses **start** and **count**

Semantically, Facebook and LinkedIn do the same thing. That is, the LinkedIn start & count
is used in the same way as the Facebook offset & limit.

To get records 50 through 75 from each system, you would use:

```
- Facebook - **offset 50** and **limit 25**
- Twitter - **page 3** and **rpp 25** (records per page)
- LinkedIn - **start 50** and **count 25**

```
**Use limit and offset**

We recommend limit and offset. It is more common, well understood in leading databases,
and easy for developers.

```
/bears?limit=25&offset=

```
**Metadata**

We also suggest including metadata with each response that is paginated that indicated to
the developer the total number of records available.

**What about defaults?**

My loose rule of thumb for default pagination is **limit=10** with **offset=0.**
(limit=10&offset=0)

The pagination defaults are of course dependent on your data size. If your resources are
large, you probably want to limit it to fewer than 10; if resources are small, it can make
sense to choose a larger limit.


In summary:

Support **partial response** by adding optional fields in a **comma delimited list.**

Use **limit and offset** to make it easy for developers to paginate objects.


## What about responses that don’t involve resources?

API calls that send a response that's not a resource **_per se_** are not uncommon depending on
the domain. We've seen it in financial services, Telco, and the automotive domain to some
extent.

Actions like the following are your clue that you might not be dealing with a "resource"
response.

```
Calculate
Translate
Convert

```
For example, you want to make a simple algorithmic calculation like how much tax
someone should pay, or do a natural language translation (one language in request;
another in response), or convert one currency to another. None involve resources returned
from a database.

In these cases:

**Use verbs not nouns**

For example, an API to convert 100 euros to Chinese Yen:

```
/convert?from=EUR&to=CNY&amount=

```

**Make it clear in your API documentation that these “non-resource” scenarios are
different.**

Simply separate out a section of documentation that makes it clear that you use verbs in
cases like this – where some action is taken to generate or calculate the response, rather
than returning a resource directly.


## Supporting multiple formats

We recommend that you support more than one format - that you push things out in one
format and accept as many formats as necessary. You can usually automate the mapping
from format to format.

Here's what the syntax looks like for a few key APIs.

**Google Data**

```
?alt=json

```
**Foursquare**

```
/venue.json

```
**Digg***

Accept: application/json
?type=json

* The type argument, if present, overrides the Accept header.

Digg allows you to specify in two ways: in a pure RESTful way in the Accept header or in
the type parameter in the URL. This can be confusing - at the very least you need to
document what to do if there are conflicts.

We recommend the Foursquare approach.

To get the JSON format from a collection or specific element:

```
bears.json
```

```
/bears/1234.json
```
Developers and even casual users of any file system are familiar to this dot notation. It also
requires just one additional character (the period) to get the point across.

**What about default formats?**

In my opinion, JSON is winning out as the default format. JSON is the closest thing we have
to universal language. Even if the back end is built in Ruby on Rails, PHP, Java, Python etc.,
most projects probably touch JavaScript for the front-end. It also has the advantage of being
terse - less verbose than XML.


## What about attribute names?

In the previous section, we talked about formats - supporting multiple formats and working
with JSON as the default.

This time, let's talk about what happens when a response comes back.

**You have an object with data attributes on it. How should you name the attributes?**

Here are API responses from a few leading APIs:

**Twitter**

```
"created_at": "Thu Nov 03 05:19;38 +0000 2011"
```
**Bing**

```
"DateTime": "2011-10-29T09:35:00Z"
```
**Foursquare**

```
"createdAt": 1320296464

```

They each use a different code convention. Although the Twitter approach is familiar to me
as a Ruby on Rails developer, we think that Foursquare has the best approach.

How does the API response get back in the code? You parse the response (JSON parser);
what comes back populates the Object. It looks like this

var myObject = JSON.parse(response);

If you chose the Twitter or Bing approach, your code looks like this. Its not JavaScript
convention and looks weird - looks like the name of another object or class in the system,
which is not correct.

timing = myObject.created_at;

timing - myObject.DateTime;

**Recommendations**

- Use JSON as default
- Follow JavaScript conventions for naming attributes
    - Use medial capitalization (aka CamelCase)
    - Use uppercase or lowercase depending on type of object


This results in code that looks like the following, allowing the JavaScript developer to write
it in a way that makes sense for JavaScript.

"createdAt": 1320296464

timing = myObject.createdAt;


## Relationships and Sub-Resources
A resource is the fundamental unit in RESTful API design. Resources model objects from the application data model.
Resources almost always have relationships to other resources.

Our Goal is to provide self describing APIs where links help the API consumer how to use the API.

### Hypermedia Support for relationships

A response

```
- MUST return a link element to **self**
- MUST return links to **sub-resource**
- SHOULD return links to **related objects**

```
##### Example Request

	```
	GET /invoices/INV-567A89HG1
	
    ```

##### Example Response
HAL response Format
```
	{
	  "_links":
		{
	    "self": { "href": "{baseurl}/invoices/INV-567A89HG1"},
	    "customer": { "href": "{baseurl}/customer/CUST-12ATCVWV"},
	    "collection/lineitems": { "href": "{baseurl}/invoices/INV-567A89HG1/lineitems"},
	    "collection/payments": { "href": "{baseurl}/invoices/INV-567A89HG1/payments"},
	  },
	  "id": "INV-567A89HG1",
	  "number" : "627726",
	  "total": 20.30,
	  "date" : "2015-06-09 12:00:00",
	  "customer_id": "CUST-12ATCVWV",
	  "customer_name": "Max Mustermann"
	}
```
### Modeling Relationships

Relationships are often modeled by a **sub-resource**.
Use the following pattern for sub-resources.

	```
	GET  /{resource}/{resource-id}/{sub-resource}
	GET  /{resource}/{resource-id}/{sub-resource}/{sub-resource-id}
	POST /{resource}/{resource-id}/{sub-resource}
    
    ```
> :information_source: Limit the depth of your URIs to ony one sub-resource to keep the API clean and simple.

### When to use sub-resources

#### Existantially dependent

In case of a 1:N relationship, where the child object is existentially dependent on the parent object use a sub-resource. “Existentially dependent” means that a child object cannot exist without its parent.
An example of such a relationship are lineitems of an invoice.

	```
	GET  /invoices/INV-567A89HG1/lineitems
	POST /invoices/INV-567A89HG1/lineitems
    
    ```
#### Belongs to relationship

If you have a "Belongs to" relationship use a sub-resource.

Invoices belong to a customer. To get all invoices to a specific customer, or to create a new invoice for that customer, do a GET or a POST:
    ```
	GET  /customers/CUST-12ATCVWV/invoices
	POST /customers/CUST-12ATCVWV/invoices
    
    ```
Other samples are  "A dog belongs to an owner",  "a shirt belongs to a person".

#### N:M relationship

In case of a N:M relationship use a sub-resource.
The sub-resource can be defined initially to support the lookup direction that you most often need to follow.
If you need to query both sides of the relationship often, then two sub-resources can be defined, one on each linked object.

A good example is a car pool. There are N drivers and M cars.

	```
	GET /cars/CAR-3767FSHS/drivers
	GET /drivers/DRI-ZU99983/cars
   
    ```

### Using a query parameter instead of sub-resources

Another valid option is to use query parameters instead of sub-resources in case of the Belongs to and N:M relationship.

	```
	GET  /invoices?customer_id=CUST-12ATCVWV
	POST /invoices

	GET /drivers?car_id=CAR-3767FSHS
	GET /cars?driver_id=DRI-ZU99983
    
    ```
It is important to be consistent how to design your API.

## Tips for search

While a simple search could be modeled as a resourceful API (for example,
bears/?q=red), a more complex search across multiple resources requires a different
design.

This will sound familiar if you've read the topic about using verbs not nouns when results
don't return a resource from the database - rather the result is some action or calculation.

If you want to do a global search across resources, we suggest you follow the Google model:

**Global search**

```
/search?q=fluffy+fur
```
Here, search is the verb; **?q** represents the query.

**Scoped search**

To add scope to your search, you can prepend with the scope of the search. For example,
search in bears owned by resource ID 5678

```
/owners/5678/bears?q=fluffy+fur
```
Notice that we’ve dropped the explicit search in the URL and rely on the parameter ‘q’ to
indicate the scoped query. (Big thanks to the contributors on the API Craft Google group for
helping refine this approach.)

**Formatted results**

For search or for any of the action oriented (non-resource) responses, you can prepend
with the format as follows:

```
/search.xml?q=fluffy+fur

```
## Consolidate API requests in one subdomain

We’ve talked about things that come after the top-level domain. This time, let's explore
stuff on the other side of the URL.

Here's how Facebook, Foursquare, and Twitter handle this:

Facebook provides two APIs. They started with api.facebook.com, then modified it to orient
around the social graph graph.facebook.com.

graph.facebook.com
api.facebook.com

Foursquare has one API.

api.foursquare.com

Twitter has three APIs; two of them focused on search and streaming.

```
stream.twitter.com
api.twitter.com
search.twitter.com

```
It's easy to understand how Facebook and Twitter ended up with more than one API. It has
a lot to do with timing and acquisition, and it's easy to reconfigure a CName entry in your
DNS to point requests to different clusters.

But if we're making design decisions about what's in the best interest of app developer, we
recommend following Foursquare's lead:

**Consolidate all API requests under one API subdomain.**

It's cleaner, easier and more intuitive for developers who you want to build cool apps using
your API.

Facebook, Foursquare, and Twitter also all have dedicated developer portals.

developers.facebook.com
developers.foursquare.com
dev.twitter.com

*******How to organize all of this?********
Your API gateway should be the top-level domain. 

For example, api.teachbearrest.com

In keeping with the spirit of REST, your developer portal should follow this pattern:

developers.yourtopleveldomain. 

For example,
developers.teachbearrest.com

**Do Web redirects**

Then optionally, if you can sense from requests coming in from the browser where the
developer really needs to go, you can redirect.

Say a developer types api.teachbearrest.com in the browser but there's no other
information for the GET request, you can probably safely redirect to your developer portal
and help get the developer where they really need to be.

```
api developers (if from browser)

dev  developers

developer  developers
```

## Tips for handling exceptional behavior

So far, we've dealt with baseline, standard behaviors.

Here we’ll explore some of the **exceptions** that can happen - when clients of Web APIs
can't handle all the things we've discussed. For example, sometimes clients intercept HTTP
error codes, or support limited HTTP methods.

What are ways to handle these situations and work within the limitations of a specific
client?

**When a client intercepts HTTP error codes**

One common thing in some versions of Adobe Flash - if you send an HTTP response that is
anything other than HTTP 200 OK, the Flash container intercepts that response and puts
the error code in front of the end user of the app.

Therefore, the app developer doesn't have an opportunity to intercept the error code. App
developers need the API to support this in some way.

Twitter does an excellent job of handling this.

They have an optional parameter suppress_response_codes. If
suppress_response_codes is set to true, the HTTP response is always 200.

/public_timelines.json?
suppress_response_codes=true
HTTP status code: 200 {"error":"Could not authenticate you."}

Notice that this parameter is a big verbose response code. (They could have used
something like src, but they opted to be verbose.)

This is important because when you look at the URL, you need to see that the response
codes are being suppressed as it has huge implications about how apps are going to
respond to it.

**Overall recommendations:**

```
1 - Use suppress_response_codes = true

2 - The HTTP code is no longer just for the code
```
The rules from our previous Handling Errors section change. In this context, the HTTP code
is no longer just for the code - the program - it's now to be ignored. Client apps are never
going to be checking the HTTP status code, as it is always the same.


3 - Push any response code that we would have put in the HTTP response down into the
response message

In my example below, the response code is 401. You can see it in the response message.
Also include additional error codes and verbose information in that message.

Always return OK
/bears?suppress_response_codes = true

Code for ignoring
200 - OK

Message for people & code
```
{response_code" : 401, "message" : "Verbose, plain language
description of the problem with hints about how to fix it."
"more_info" : "http://dev.tecachbearrest.com/errors/12345",
"code" : 12345}
```
**When a client supports limited HTTP methods**

It is common to see support for GET and POST and not PUT and DELETE.

To maintain the integrity of the four HTTP methods, we suggest you use the following
methodology commonly used by Ruby on Rails developers:

**Make the method an optional parameter in the URL.**

Then the HTTP verb is always a GET but the developer can express rich HTTP verbs and
still maintain a RESTful clean API.

Create

```
/bears?method=post
```
Read
```
/bears

```
Update
```
/bears/1234?method=put&location=park

```
Delete

```
/bears/1234?method=delete
```

**WARNING:** _It can be dangerous to provide post or delete capabilities using a GET method
because if the URL is in a Web page then a Web crawler like the Googlebot can create or
destroy lots of content inadvertently. Be sure you understand the implications of supporting
this approach for your applications' context._


## Authentication

There are many schools of thought. My colleagues at Apigee and I d on't always agree on
how to handle authentication - but overall here's my take.

Let's look at these three top services. See how each of these services handles things
differently:

**PayPal**
Permissions Service API

**Facebook**
OAuth 2.0

**Twitter**
OAuth 1.0a

Note that PayPal's proprietary three-legged permissions API was in place long before
OAuth was conceived.

What should you do?

Use the latest and greatest OAuth - OAuth 2.0 (as of this writing). It means that Web or
mobile apps that expose APIs don’t have to share passwords. It allows the API provider to
revoke tokens for an individual user, for an entire app, without requiring the user to
change their original password. This is critical if a mobile device is compromised or if a
rogue app is discovered.

Above all, OAuth 2.0 will mean improved security and better end-user and consumer
experiences with Web and mobile apps.

Don't do something *like* OAuth, but different. It will be frustrating for app developers if
they can't use an OAuth library in their language because of your variation.


## Making requests on your API

Lets take a look at what some API requests and responses look like for our bears API.

**Create a brown bear named Al**
```
POST /bears
name=Al&furColor=brown
```
Response
200 OK

```
{
"bear":{
"id": "1234",
"name": "Al",
"furColor": "brown"
}
}
```
**Rename Al to Rover - Update**

```
PUT /bears/1234
name=Rover

Response
200 OK
```
```
{
"bear":{
"id":"1234",
"name": "Rover",
"furColor": "brown"

}
}
```

**Tell me about a particular bear**

```
GET /bears/1234
```
```
Response
200 OK
```

```
{
"bear":{
"id":"1234",
"name": "Rover",
"furColor": "brown"
}
}
```
**Tell me about all the bears**

```
GET /bears

Response
200 OK
```

```
{
"bears":
[{"bear":{
"id":"1233",
"name": "Fido",
"furColor": "white"}},
{"bear":{
"id":"1234",
"name": "Rover",
"furColor": "brown"}}]

"_metadata":

[{"totalCount":327,"limit":25,"offset":100}]
}
```
**Delete Rover :-(**

```
DELETE /bears/1234

Response
200 OK
```

## Correlation Id ##

It is extremely hard to analyse incorrect and faulty behaviour in a distributed system environment. Systems are calling other systems either synchronous or ansynchronous to fulfill a request. Each system logs to its own logging target. Manually analyzing the way of a request is a nightmare. To set up automated failure analytics is errorprone and brittle.
The [Correlation Pattern](http://www.enterpriseintegrationpatterns.com/patterns/messaging/CorrelationIdentifier.html), which depends on the use of Correlation ID is a well documented Enterprise Integration Pattern. Its target is to correlate requests over multiple systems and responses to another.

A Correlation ID is a unique identifier value that is attached to requests and messages that allow reference to a particular transaction or event chain. Attaching a Correlation ID to a request is arbitrary. You don’t have to use one. But if you are designing a distributed system that incorporates message queues and asynchronous processing, you will do well to include a Correlation ID in your messages. And even in synchronous chains it helps you to follow the call squence over different systems.

The Correlation Id is build by the client or as early as possible in the request chain. If a request contains no Correlation Id header it is up to each system to create and add it to the header.
It is the responsibility of each part/service/system in the chain to extract the Correlation Id from the request and use it in log entries. Calls to subsequent subsystems must contain the Correlation Id.

Currently there is no standard that fixes the field name for the Correlation Id. X-Headers are deprecated, see [https://tools.ietf.org/html/rfc6648](https://tools.ietf.org/html/rfc6648) and [https://tools.ietf.org/html/rfc7231#section-8.3.1](https://tools.ietf.org/html/rfc7231#section-8.3.1).

We chose the name **Correlation-Id**.
It is recommended to use a **UUID** as Correlation Id. That allows our systems to validate the Correlation Id with a regular expression and therefore prevent against log forging attacks.

```
	{   
	   "Correlation-Id": <UUID>
	}
```
> :information_source: [Kong plugins](https://getkong.org/plugins/correlation-id/)

## Create Correlation Id with Wicked ##

Haufes [API Management Solution Wicked](http://wicked.haufe.io/) supports the creation of Correlation Id.
Read more [at](http://wickedhaufeio.readthedocs.io/en/stable/configuring-kong-plugins/).

## Collection Resources
A list of all of the given resources, including any related metadata. Array of resources should be in the `_embedded` field. Fields like `total_items` and `total_pages` help provide context to paged results. Consistent naming of collection resource fields allow API clients to create generic handling for using the provided data across various resource collections.

The GET verb should not affect the system, and should not change response on subsequent requests (unless the underlying data changes), i.e. it should be idempotent. Exceptions to 'changing the response' are typically instrumentation/logging-related.

The list of data is presumed to be filtered based on the provided security context of the API client, this should not be a list of all resources in the domain.

Providing a summarized, or minimized version of the data representation can reduce the bandwidth footprint, in cases where individual resources contain a large object.

### Resource naming

Collection resource names should be plural nouns, e.g. `/users`. This helps visually disambiguate collections from singletons.
Please have a look at [REST principles](../rest-principles/rest-principles.md) for naming Guidelines.

### Get List of resources

##### URI Template

	GET /{namespace}/{version}/{resource}


##### Example Request

	GET /user_management/v1/users


##### Example Response

HAL response format

	{
	  "_links":
		{
	    "self": { "href": "{baseurl}/users"},
	    "first": { "href": "{baseurl}/users?page=1"},
	    "last": { "href": "{baseurl}/users?page=11"},
	    "next": { "href": "{baseurl}/users?page=2"},
	    "find": { "href": "{baseurl}/users{?id}", "templated": true}
	  },
	  "page": 1,
	  "page_size": 20,
	  "total_pages" : 11,
	  "total_count": 217,
	  "_embedded":
		{
	    "users":
			[
	      {
	        "_links": {
	          "self": { "href": "{baseurl}/users/A14DA7707FE2458DAE37C2CF81E8F9B1" }
	        },
	        "id" : "A14DA7707FE2458DAE37C2CF81E8F9B1",
	        "name" : "Mustermann"
	      },
	      {
	        "_links": {
	          "self": { "href": "{baseurl}/users/BE14A7269802498F992813885546D058" }
	        },
	        "id" : "BE14A7269802498F992813885546D058",
	        "name" : "VIPUser"      
	      }
	    ]
	  }
	}



#### HTTP Status

If the collection is empty (0 items in response), `404 Not Found` is not appropriate. The corresponding array should just be empty, and collection metadata fields provided (e.g. `"total_count": 0`). Invalid query parameter values can result in 400 Bad Request. Otherwise `200 OK` is utilized for a successful response.

### Read Single Resource

A single resource, typically derived from the parent collection of resources (often more detailed than the collection resource items).

Executing GET should never affect the system, and should not change response on subsequent requests, i.e. it should be idempotent.

> All identifiers for sensible resources (customers, individuals) should be non-sequential, and preferrably non-numeric.

In scenarios where this data might be used as a subordinate to other data, immutable string identifiers should be utilized for easier readability and debugging (i.e. "NAME_OF_VALUE" vs 1421321).

##### URI Template

	GET /{namespace}/{version}/{resource}/{resource-id}


##### Example Request

	GET /user_management/v1/users/BE14A7269802498F992813885546D058


##### Example Response

	{
	  "_links":
		{
	    "self": { "href": "{baseurl}/users/BE14A7269802498F992813885546D058" },
	  }
	  "id": "BE14A7269802498F992813885546D058",
	  "name": "Mustermann"
	}


##### HTTP Status

If the provided resource identifier is not found, responds `404 Not Found` HTTP status. Otherwise, `200 OK` HTTP status should be utilized when data is found.

### Update Single Resource

Updates a single resource. The shape of the PUT request should maintain parity with the GET response for the selected resource. Fields in the request body can be optional or ignored during deserialization, such as `create_time` or other system-calculated values.

##### URI Template

	PUT /{namespace}/{version}/{resource}/{resource-id}


##### Example Request

	PUT /user_management/v1/users/BE14A7269802498F992813885546D058
	{
	  "id": "BE14A7269802498F992813885546D058",
	  "name": "Changed Name"
	}


##### HTTP Status

Any failed request validation responds `400 Bad Request` HTTP status. If clients attempt to modify read-only fields, this is also a `400 Bad Request`.

If there are business rules (more than data type/length/etc), it is best to provide a specific error code & message (in addition to the 400) for that validation.

For situations which require interaction with APIs or processes outside of the current request, the `422 Unprocessable Entity` status code is appropriate.

After successful update, PUT operations should respond with `204 No Content` status, with no response body.

### Update Partial Single Resource

##### Support of partial updates is optional.

PATCH updates a part of a single resource. Unlike PUT, which requires parity with GET, PATCH merely changes the fields provided, and leaves the rest of the resource unaffected.

HTTP PATCH method has been been formalized in [RFC 7386: JSON Merge Patch](https://tools.ietf.org/html/rfc7386).

Response should be `204 No Content` and no response body. Because PATCH is often called frequently in interactive form UX design, returning the entire response could be irresponsible from a bandwidth perspective, especially in mobile scenarios.

System generated values should be commonly changed by an update,

##### URI Template

	PATCH /{namespace}/{version}/{resource}/{resource-id}


##### Example Request

	PATCH /user_management/v1/users/BE14A7269802498F992813885546D058
	{
	  "name" : "newname"
	}


##### Example Response

	204 No Content

##### HTTP Status

Status/response for PATCH is the same as PUT.

### Delete Single Resource

Deletes a single resource. In order to enable retries (typically patchy connectivity), DELETE is treated as idempotent, so it should always respond with a `204 No Content` HTTP status.
`404 Not Found` HTTP status should **not** be utilized here, as on a second retry a client might mistakenly think the resource never existed at all. GET can be utilized to verify the resources exists prior to DELETE.

##### URI Template

	DELETE /{namespace}/{version}/{resource}/{resource-id}

##### Example Request

	DELETE /user_management/v1/users/BE14A7269802498F992813885546D058

##### Example Response

	204 No Content

### Create New Resource

Creates a new resource in the collection. Request body may be somewhat different than GET/PUT response/request (typically fewer fields as the server will generate some values).

In most cases, the API server produces an identifier for the resource. In cases where identifier is supplied by the API consumer, use [Create New Resource - Consumer Supplied Identifier](#create-new-resource---consumer-supplied-identifier) below.

Once the POST has successfully completed, a new resource will be created. The identifier for this resource should be added to the resource collection URI.

Hypermedia links provide an easy way to get the URL of the newly created resource, using the rel: self, in addition to other links for operations allowed for the new resource. Addionally a location header in the response can point to the newly created resource.

##### URI Template

	POST /{namespace}/{version}/{resource}

##### Example Request

Note that server-generated values are not provided in the request.

	POST /user_management/v1/users

	{
	    "name": "MyName123",
	}

##### Example Response

	201 Created
	Location: http://api.haufe-lexware.com/user_management/v1/users/E75E30C0607446219C6EA31735C691B9

	{
	  "_links":
		{
	    "self": { "href": "{baseurl}/users/E75E30C0607446219C6EA31735C691B9" },
	  }
	  "id": "E75E30C0607446219C6EA31735C691B9",
	  "name": "MyName123"
	}

### Create New Resource - Consumer Supplied Identifier

When an API consumer defines the resource identifier, the PUT verb should be utilized, as the operation is idempotent, even during creation.

The same interaction as  [Create New Resource](#create-new-resource) is used here. 201 + response body on resource creation, and 204 + no response body when an existing resource is updated.


There are some reasons why you have to consider to use Caching

- improve Speed
- reduce latency
- reduce server load
- reduce network traffic

Goal of caching is to never generate the same response twice.

> Tip: For security reasons, do not allow sensitive data to be cached.
Use server side mechanisms like setting `Cache-Control` to `no-store` in the response header.
Additionally you should use HTTPS.


#### Expires HTTP Header

The `Expires` HTTP header is a basic means of controlling caches - it tells all caches how long the associated representation is fresh for.
After that time, caches will always check back with the origin server to see, if a document is changed.
`Expires` headers are supported by practically every cache.
Expires headers are especially good for making static images (like navigation bars and buttons) cacheable. Because they don't change much, you can set extremely long expiry time on them, making your site appear much more responsive to your users.

    GET /user_management/v1/users/BE14A7269802498F992813885546D058

    HTTP/1.1 200 OK
    ...
    Expires: Fri, 30 Oct 2015 14:19:41 GMT
    Last-Modified: Mon, 29 Jun 2015 02:28:12 GMT
    Content-Type: text/json; charset=utf-8
    Content-Length: ...
    { "id": "BE14A7269802498F992813885546D058", "name": "Mustermann" }

You can find more infos in:
[https://www.mnot.net/cache_docs/#CACHE-CONTROL](https://www.mnot.net/cache_docs/#EXPIRES)

#### Cache-Control Header

Use `Cache-Control` header that indicates, whether the data in the body of the response can be safely cached by the client or an intermediate server.
Use `max-age` to indicate after how many seconds the response should be considered out-of-date.
The following example shows an HTTP GET request and the corresponding response that includes a Cache-Control header:

    GET /user_management/v1/users/BE14A7269802498F992813885546D058

    HTTP/1.1 200 OK
    ...
    Cache-Control: max-age=600, private
    Content-Type: text/json; charset=utf-8
    Content-Length: ...
    { "id": "BE14A7269802498F992813885546D058", "name": "Mustermann" }

You can find a detailed explanation of the different `Cache-Control` opportunities in:
[https://www.mnot.net/cache_docs/#CACHE-CONTROL](https://www.mnot.net/cache_docs/#CACHE-CONTROL)

#### ETag

You can use an `ETag` (Entity Tag) in the response header for entity data.
An ETag is an opaque string indicating the version of a resource - each time a resource changes, the Etag is also modified.
This ETag should be cached as part of the data by the client application.
The ETag is primarily useful to save bandwidth.

    GET /user_management/v1/users/BE14A7269802498F992813885546D058

    HTTP/1.1 200 OK
    ...
    Cache-Control: max-age=600, private
    Content-Type: text/json; charset=utf-8
    ETag: "2147483648"
    Content-Length: ...
    { "id": "BE14A7269802498F992813885546D058", "name": "Mustermann" }

The client constructs a GET request containing the `ETag` for the currently cached version of the resource referenced
in an `If-None-Match` HTTP header:

    GET /user_management/v1/users/BE14A7269802498F992813885546D058
    If-None-Match: "2147483648"

##### ETag matches

Return an HTTP response with an empty message body and a status code of `304 Not Modified`.

##### ETag does not match

The data has changed and the web API should return an HTTP response with the new data in the message body and a status code of `200 OK`.

### Further reading

Find below a list of great articles on the topic **Caching**

[https://www.mnot.net/cache_docs/](https://www.mnot.net/cache_docs/)  
[http://restcookbook.com/Basics/caching/](http://restcookbook.com/Basics/caching/)  
[http://odino.org/rest-better-http-cache/](http://odino.org/rest-better-http-cache/)  
[https://www.subbu.org/blog/2005/01/http-Caching](https://www.subbu.org/blog/2005/01/http-Caching)

Resources for implementation
[https://github.com/mspnp/azure-guidance/blob/master/API-Implementation.md#considerations-for-optimizing-client-side-data-access](https://github.com/mspnp/azure-guidance/blob/master/API-Implementation.md#considerations-for-optimizing-client-side-data-access)


> :bulb: Please give us Feedback which Caching strategy you chose and your experieneces.


## Chatty APIs

Let’s think about how app developers use that API you're designing and dealing with chatty
APIs.

**Imagine how developers will use your API**
When designing your API and resources, try to imagine how developers will use it to say
construct a user interface, an iPhone app, or many other apps.

Some API designs become very chatty - meaning just to build a simple UI or app, you have
dozens or hundreds of API calls back to the server.

The API team can sometimes decide not to deal with creating a nice, resource-oriented
RESTful API, and just fall back to a mode where they create the 3 or 4 Java-style getter and
setter methods they know they need to power a particular user interface.

We don't recommend this. You can design a RESTful API and still mitigate the chattiness.

**Be complete and RESTful and provide shortcuts**

First design your API and its resources according to pragmatic RESTful design principles
and then provide shortcuts.

What kind of shortcut? Say you know that 80% of all your apps are going to need some sort
of composite response, then build the kind of request that gives them what they need.

Just don't do the latter instead of the former. First design using good pragmatic RESTful
principles!

**Take advantage of the partial response syntax**

The partial response syntax discussed in a previous section can help.

To avoid creating one-off base URLs, you can use the partial response syntax to drill down
to dependent and associated resources.

In the case of our bears API, the bears have association with owners, who in turn have
associations with veterinarians, and so on. Keep nesting the partial response syntax using
dot notation to get back just the information you need.

/owners/5678?fields=name,bears.name


## Complement with an SDK

It’s a common question for API providers - do you need to complement your API with code
libraries and software development kits (SDKs)?

If your API follows good design practices, is self consistent, standards-based, and well
documented, developers may be able to get rolling without a client SDK. Well-documented
code samples are also a critical resource.

On the other hand, what about the scenario in which building a UI requires a lot of domain
knowledge? This can be a challenging problem for developers even when building UI and
apps on top of APIs with pretty simple domains – think about the Twitter API with it’s
primary object of 140 characters of text.

You shouldn't change your API to try to overcome the domain knowledge hurdle. Instead,
you can c omplement your API with code libraries and a software development kit (SDK).

In this way, you don't overburden your API design. Often, a lot of what's needed is on the
client side and you can push that burden to an SDK.

The SDK can provide the platform-specific code, which developers use in their apps to
invoke API operations - meaning you keep your API clean.

Other reasons you might consider complementing your API with an SDK include the
following:

**Speed adoption on a specific platform.** ( For example Objective C SDK for iPhone.)
Many experienced developers are just starting off with objective C+ so an SDK might be
helpful.

**Simplify integration effort required to work with your API** - If key use cases are
complex or need to be complemented by standard on-client processing.

**An SDK can help reduce bad or inefficient code** that might slow down service for
everyone.

**As a developer resource** - good SDKs are a forcing function to create good source code
examples and documentation. Yahoo! and Paypal are good examples:

Yahoo! [http://developer.yahoo.com/social/sdk/](http://developer.yahoo.com/social/sdk/)

Paypal https://cms.paypal.com/us/cgi-bin/?cmd=_render-
content&content_ID=developer/library_download_sdks

**To market your API to a specific community** - you upload the SDK to a samples or plug-
in page on a platform’s existing developer community.


## The API Facade Pattern

At this point, you may be asking -

What should we be thinking from an architectural perspective?

How do we follow all these best practice guidelines, expose my internal services and systems in
a way that’s useful to app developers, and still iterate and maintain my API?_

Back-end systems of record are often too complex to expose directly to application
developers. They are stable (have been hardened over time) and dependable (they are
running key aspects of you business), but they are often based on legacy technologies and
not always easy to expose to Web standards like HTTP. These systems can also have
complex interdependencies and they change slowly meaning that they can’t move as
quickly as the needs of mobile app developers and keep up with changing formats.

In fact, the problem is not creating an API for just one big system but creating an API for an
array of complementary systems that all need to be used to make an API valuable to a
developer.

It’s useful to talk through a few anti-patterns that we’ve seen. Let’s look at why we believe
they don’t work well.

**The Build Up Approach**

In the build-up approach, a developer exposes the core objects of a big system and puts an
XML parsing layer on top.

XML
This approach has merit in that it can get you to market with version 1 quickly. Also, your
API team members ( your internal developers) already understand the details of the
system.

Unfortunately, those details of an internal system at the object level are fine grained and
can be confusing to external developers. You’re also exposing details of internal
architecture, which is rarely a good idea. This approach can be inflexible because you have
1:1 mapping to how a system works and how it is exposed to API. In short, building up
from the systems of record to the API can be overly complicated.

**The Standards Committee Approach**

Often the internal systems are owned and managed by different people and departments
with different views about how things should work. Designing an API by a standards
committee often involves creating a standards document, which defines the schema and
URLs and such. All the stakeholders build toward that common goal.

The benefits of this approach include getting to version 1 quickly. You can also create a
sense of unification across an organization and a comprehensive strategy, which can be
significant accomplishments when you have a large organization with a number of
stakeholders and contributors.

A drawback of the standards committee pattern is that it can be slow. Even if you get the
document created quickly, getting everybody to implement against it can be slow and can
lack adherence. This approach can also lead to a mediocre design as a result of too many
compromises.

**The Copy Cat Approach**

We sometimes see this pattern when an organization is late to market – for example, when
their close competitor has already delivered a solution. Again, this approach can get you to
version 1 quickly and you may have a built-in adoption curve if the app developers who
will use your API are already familiar with your competitor’s API.

However, you can end up with an undifferentiated product that is considered an inferior
offering in the market of APIs. You might have missed exposing your own key value and
differentiation by just copying someone else’s API design.

**Solution – The API façade pattern**

The best solution starts with thinking about the fundamentals of product management.
Your product (your API) needs to be credible, relevant, and differentiated. Your product
manager is a key member of your API team

Once your product manager has decided what the big picture is like, it’s up to the
architects.

We recommend you implement an **API façade pattern**. 
This pattern gives you a buffer or
virtual layer between the interface on top and the API implementation on the bottom. You
essentially create a façade – a comprehensive view of what the API should be and
importantly from the perspective of the app developer and end user of the apps they
create.

The developer and the app that consume the API are on top. The API façade isolates the
developer and the application and the API. Making a clean design in the facade allows you
to decompose one really hard problem into a few simpler problems.

“Use the façade pattern when you want to provide a simple interface to a complex subsystem.
Subsystems often get more complex as they evolve.”
Design Patterns – Elements of Reusable Object-Oriented Software_
(Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides)

## Implementing an API façade pattern involves three basic steps.

```1 - Design the ideal API – design the URLs, request parameters and responses, payloads,
headers, query parameters, and so on. The API design should be self-consistent.

2 - I mplement the design with data stubs. This allows application developers to use your
API and give you feedback even before your API is connected to internal systems.

3 - Mediate or integrate between the façade and the systems.
```

## API Facade
Using the three-step approach you’ve decomposed one big problem to three smaller
problems. If you try to solve the one big problem, you’ll be starting in code, and trying to
build up from your business logic (systems of record) to a clean API interface. You would
be exposing objects or tables or RSS feeds from each silo, mapping each to XML in the right
format before exposing to the app. It is a machine–to-machine orientation focused around
an app and is difficult to get this right.

Taking the façade pattern approach helps shift the thinking from a silo approach in a
number of important ways. First, you can get buy in around each of the three separate
steps and have people more clearly understand how you’re taking a pragmatic approach to
the design. Secondly, the orientation shifts from the app to the app developer. The goal
becomes to ensure that the app developer can use your API because the design is self-
consistent and intuitive.

Because of where it is in the architecture, t he façade becomes an interesting gateway. You
can now have the façade implement the handling of common patterns (for pagination,
queries, ordering, sorting, etc.), authentication, authorization, versioning, and so on,
uniformly across the API. (This is a big topic and a full discussion is beyond the scope of
this article.)

Other benefits for the API team include being more easily able to adapt to different use
cases regardless of whether they are internal developer, partner, or open scenarios. The
API team will be able to keep pace with the changing needs of developers, including the
ever-changing protocols and languages. It is also easier to extend an API by building out
more capability from your enterprise or plugging in additional existing systems.

## References 

Representational State Transfer (REST), Roy Thomas Fielding, 2000

RESTful API Design Webinar, 2nd edition, Brian Mulloy, 2011

Apigee API Tech & Best Practices Blog.