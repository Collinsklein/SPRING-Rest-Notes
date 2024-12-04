# SPRING-Rest-Notes

Chapter 1: Introduction to Spring REST
Overview of REST: REST (Representational State Transfer) is a software architectural style that provides guidelines for creating scalable web services.

Focuses on resources, represented by URLs.
Utilizes standard HTTP methods (GET, POST, PUT, DELETE) for interaction.
Stateless communication between client and server.
Spring REST Basics:

Built on top of Spring Framework.
Offers tools and annotations for building RESTful services efficiently.
Emphasizes simplicity and ease of integration with other Spring projects.
Core Concepts:

Controllers: Use @RestController to define REST endpoints.
RequestMapping: Maps HTTP requests to handler methods using @RequestMapping and its derived annotations (@GetMapping, @PostMapping).
Chapter 2: Setting Up a Spring REST Project
Project Structure:

Standard Maven/Gradle project layout.
Important directories: src/main/java for source code, src/main/resources for configurations.
Dependencies:

Essential dependencies include Spring Web, Spring Boot Starter, and optionally, testing libraries like JUnit or Mockito.
Dependency management tools (Maven or Gradle) are used to handle these.
Configuration:

Spring Boot simplifies setup through application.properties or application.yml.
Details on port configuration, database connections, and other essential properties.
Chapter 3: Building Your First REST Endpoint
Step-by-Step Implementation:

Create a @RestController class.
Define methods annotated with @GetMapping, @PostMapping, etc., to handle specific HTTP requests.
Response Handling:

Use ResponseEntity for fine-grained control over HTTP status and response body.
Return objects are automatically converted to JSON (using Jackson library).
Error Handling:

Spring provides default error responses.
Custom exception handling can be implemented using @ControllerAdvice and @ExceptionHandler.
Testing:

Importance of testing endpoints using tools like Postman or via unit tests using Spring’s MockMvc.



Introduction to REST
KEY Concepts:
•	REST fundamentals
•	REST resources and their representations
•	HTTP methods and status codes
•	Richardson’s maturity model

What is REST?
REST stands for representational State Transfer and is an architectural style for designing distributed network applications. 
Roy Fielding coined the term REST in his PhD dissertation1 and proposed the 
following six constraints or principles as its basis:
•	Client-Server—Concerns should be separated between clients and servers. This 
enables client and server components to evolve independently and in turn allows 
the system to scale.
•	Stateless—The communication between client and server should be stateless. The 
server need not remember the state of the client. Instead, clients must include all of 
the necessary information in the request so that server can understand and process it.
•	Layered System—Multiple hierarchical layers such as gateways, firewalls, and proxies 
can exist between client and server. Layers can be added, modified, reordered, or 
removed transparently to improve scalability.
•	Cache—Responses from the server must be declared as cacheable or noncacheable. 
This would allow the client or its intermediary components to cache responses and 
reuse them for later requests. This reduces the load on the server and helps improve 
the performance.
Chapter 1
 ■ Introduction to rest
•Uniform Interface— All interactions between client, server, and intermediary 
components are based on the uniformity of their interfaces. This simplifies 
the overall architecture as components can evolve independently as long as 
they implement the agreed-on contract. The uniform interface constraint is 
further broken down into four sub constraints—resource identification, resource 
representations, self-descriptive messages, and hypermedia as the engine of 
application state or HATEOAS. 
•	Code on demand—Clients can extend their functionality by downloading and 
executing code on demand. Examples include JavaScript scripts, Java applets, 
Silverlight, and so on. This is an optional constraint.
-Applications that adhere to these constraints are considered to be RESTful. As you might have noticed, these constraints don’t dictate the actual technology to be used for developing applications. Instead, adherence to these guidelines and best practices would make an application scalable, visible, portable, reliable, and able to perform better. In theory, it is possible for a RESTful application to be built using any networking infrastructure or transport protocol. In practice, RESTful applications leverage features and capabilities of the Web and use HTTP as the transport protocol.
The Uniform Interface constraint is a key feature that distinguishes REST applications from other network-based applications. Uniform Interface in a REST application is achieved through abstractions such as resources, representations, URIs, and HTTP methods. In the next sections, we will look at these important 
REST abstractions.
Understanding Resources
“The key abstraction of information in REST is a resource."
—Roy Fielding
Fundamental to REST is the concept of resource. A resource is anything that can be accessed or manipulated. Examples of resources include “videos,” “blog entries,” “user profiles,” “images,” and even tangible things such as persons or devices. Resources are typically related to other resources. 
For example, in an e-commerce application, a customer can place an order for any number of products. In this scenario, the product resources are related to the corresponding order resource. It is also possible for a resource to be grouped into collections. Using the same e-commerce example, “orders” is a collection of individual “order” resources.
Identifying Resources
Before we can interact and use a resource, we must be able to identify it. The Web provides the Uniform Resource Identifier, or URI, for uniquely identifying resources. The syntax of a URI is:
scheme:scheme-specific-part
The scheme and the scheme-specific part are separated using a semicolon. Examples of a scheme 
include http or ftp or mailto and are used to define the semantics and interpretation of the rest of the URI. 
Take the example of the URI—http://www.apress.com/9781484208427. The http portion of the example is the scheme; it indicates that a HTTP scheme should be used for interpreting the rest of the URI. The HTTP scheme, defined as part of RFC 7230,2 indicates that the resource identified by our example URI is located on 
a machine with host name apress.com.
Table 1-1 shows examples of URIs and the different resources they represent.
Even though a URI uniquely identifies a resource, it is possible for a resource to have more than one URI. 
For example, Facebook can be accessed using URIs https://www.facebook.com and https://www.fb.com. 
The term URI aliases is used to refer to such URIs that identify the same resources. URI aliases provide 
flexibility and added convenience such as having to type fewer characters to get to the resource.
URI Templates
When working with REST and a REST API, there will be times where you need to represent the structure 
of a URI rather than the URI itself. For example, in a blog application, the URI http://blog.example.
com/2014/posts would retrieve all the blog posts created in the year 2014. Similarly, the URIs http://blog.
URI Templates
When working with REST and a REST API, there will be times where you need to represent the structure 
of a URI rather than the URI itself. For example, in a blog application, the URI http://blog.example.
com/2014/posts would retrieve all the blog posts created in the year 2014. Similarly, the URIs http://blog.
example.com/2013/posts, http://blog.example.com/2012/posts, and so forth would return blog posts 
corresponding to the years 2013, 2012, and so on. In this scenario, it would be convenient for a consuming 
client to know the URI structure http://blog.example.com/year/posts that describes the range of URIs 
rather than individual URIs.
URI templates, defined in RFC 6570 (http://tools.ietf.org/html/rfc6570), provide a standardized 
mechanism for describing URI structure. The standardized URI template for this scenario could be:
http://blog.example.com/{year}/posts
The curly braces {} indicate that the year portion of the template is a variable, often referred to as a path 
variable. Consuming clients can take this URI template as input, substitute the year variable with the right 
value, and retrieve the corresponding year’s blog posts. On the server side, URL templates allow the server 
code to parse and retrieve the values of the variables or selected portions of URI easily.
Table 1-1. URI and resource description
URI Resource Description
http://blog.example.com/posts Represents a collection of blog post resources
http://blog.example.com/posts/1 Represents a blog post resource with identifier “1”; such resources 
are called singleton resources
http://blog.example.com/
posts/1/comments
Represents a collection of comments associated with the blog 
entry identified by “1”; collections such as these that reside under a 
resource are referred to as subcollections
http://blog.example.com/
posts/1/comments/245
Represents the comment resource identified by “245”

RepresentationRESTful resources are abstract entities. The data and metadata that make a RESTful resource needs to be serialized into a representation before it gets sent to a client. This representation can be viewed as a snapshot 
of a resource’s state at a given point in time. Consider a database table in an ecommerce application that stores information about all the available products. When an online shopper uses their browser to buy a product and requests its details, the application would provide the product details as a Web page in HTML. 
Now, when a developer writing a native mobile application requests product details, the ecommerce application might return those details in XML or JSON format. In both scenarios, the clients didn’t interact with the actual resource—the database record-holding product details. Instead, they dealt with its representation.
 ■Note  reSt components interact with a resource by transferring its representations back and forth. they never directly interact with the resource.
As noted in this product example, the same resource can have several representations. These 
representations can range from text-based HTML, XML, and JSON formats to binary formats such as PDFs, 
JPEGs, and MP4s. It is possible for the client to request a particular representation and this process is termed 
as content negotiation. Here are the two possible content negotiation strategies:
•	Postfixing the URI with the desired representation—In this strategy, a client 
requesting product details in JSON format would use the URI http://www.example.
com/products/143.json. A different client might use the URI http://www.example.
com/products/143.xml to get product details in XML format.
•	Using the Accept header—Clients can populate the HTTP Accept header with 
the desired representation and send it along with the request. The application 
handling the resource would use the Accept header value to serialize the requested 
representation. The RFC 26163 provides a detailed set of rules for specifying one or 
more formats and their priorities.
 ■Note  JSon has become the de facto standard for reSt services. all of the examples in this book use 
JSon as the data format for requests and responses.
HTTP MethodsThe “Uniform Interface” constraint restricts the interactions between client and server through a handful of 
standardized operations or verbs. On the Web, the HTTP standard4 provides eight HTTP methods that allow 
clients to interact and manipulate resources. Some of the commonly used methods are GET, POST, PUT, and 
DELETE. Before we delve deep in to HTTP methods, let’s review their two important characteristics—safety 
and idempotency.

Safety
A HTTP method is said to be safe if it doesn’t cause any changes to the server state. Consider methods such 
as GET or HEAD, which are used to retrieve information/resources from the server. These requests are 
typically implemented as read-only operations without causing any changes to the server’s state and, hence, 
considered safe.
Safe methods are used to retrieve resources. However, safety doesn’t mean that the method must return 
the same value every time. For example, a GET request to retrieve Google stock might result in a different 
value for each call. But as long as it didn’t alter any state, it is still considered safe.
In real-world implementations, there may still be side effects with a safe operation. Consider the 
implementation in which each request for stock prices gets logged in a database. From a purist perspective 
we are changing the state of the entire system. However, from a practical standpoint, because these side 
effects were the sole responsibility of the server implementation, the operation is still considered to be safe.
Idempotency
An operation is considered to be idempotent if it produces the same server state whether we apply it once 
or any number of times. HTTP methods such as GET, HEAD (which are also safe), PUT, and DELETE are 
considered to be idempotent, guaranteeing that clients can repeat a request and expect the same effect as 
making the request once. The second and subsequent requests leave the resource state in exactly the same 
state as the first request did.
Consider the scenario in which you are deleting an order in an ecommerce application. On successful 
completion of the request, the order no longer exists on the server. Hence, any future requests to delete that 
order would still result in the same server state. By contrast, consider the scenario in which you are creating 
an order using a POST request. On successful completion of the request, a new order gets created. If you 
were to re“POST” the same request, the server simply honors the request and creates a new order. Because a 
repeated POST request can result in unforeseen side effects, POST is not considered to be idempotent.
GET
The GET method is used to retrieve a resource’s representation. For example, a GET on the URI http://
blog.example.com/posts/1 returns the representation of the blog post identified by 1. By contrast, a GET on 
the URI http://blog.example.com/posts retrieves a collection of blog posts. Because GET requests don’t 
modify server state, they are considered to be safe and idempotent.
HEAD
On occasions, a client would like to check if a particular resource exists and doesn’t really care about the 
actual representation. In another scenario, the client would like to know if a newer version of the resource is 
available before it downloads it. In both cases, a GET request could be “heavyweight” in terms of bandwidth 
and resources. Instead, a HEAD method is more appropriate.
The HEAD method allows a client to only retrieve the metadata associated with a resource. No resource 
representation gets sent to the client. This metadata represented as HTTP headers will be identical to 
the information sent in response to a GET request. The client uses this metadata to determine resource 
accessibility and recent modifications. Here is a hypothetical HEAD request and the response
DELETE
The DELETE method, as the name suggests, requests a resource to be deleted. On receiving the request, a 
server deletes the resource. For resources that might take a long time to delete, the server typically sends a 
confirmation that it has received the request and will work on it. Depending on the service implementation, 
the resource may or may not be physically deleted.
On successful deletion, future GET requests on that resource would yield a “Resource Not Found” error 
via HTTP status code 404. We will be covering status codes in just a minute.
In this example, the client requests a post identified by 1 to be deleted. On completion, the server could 
return a status code 200 (OK) or 204 (No Content), indicating that the request was successfully processed
PUT
The PUT method allows a client to modify a resource state. A client modifies the state of a resource and 
sends the updated representation to the server using a PUT method. On receiving the request, the server 
replaces the resource’s state with the new state.
In this example, we are sending a PUT request to update a post identified by 1. The request contains 
an updated blog post’s body along with all of the other fields that make up the blog post. The server, 
on successful processing, would return a status code 200, indicating that the request was processed 
successfully.

POST
The POST method is used to create resources. Typically, it is used to create resources under subcollections—
resource collections that exist under a parent resource. For example, the POST method can be used to create 
a new blog entry in a blogging application. Here, “posts” is a subcollection of blog post resources that reside 
under a blog parent resource
PATCH
As we discussed earlier, the HTTP specification requires the client to send the entire resource representation 
as part of a PUT request. The PATCH method proposed as part of RFC 5789 (http://tools.ietf.org/html/
rfc5789) is used to perform partial resource updates. It is neither safe nor idempotent. Here is an example 
that uses PATCH method to update a blog post title.
CRUD and http Verbs
data-driven applications typically use the term Crud to indicate four basic persistence functions—
Create, read, update, and delete. Some developers building reSt applications have mistakenly 
associated the four popular http verbs Get, poSt, put, and deLete with Crud semantics. the typical 
association often seen is:
Create -> POST
Update -> PUT
Read -> GET
Delete -> DELETE
HTTP Status Codes
The HTTP Status codes allow a server to communicate the results of processing a client’s request. These 
status codes are grouped into the following categories:
•	Informational Codes—Status codes indicating that the server has received the 
request but hasn’t completed processing it. These intermediate response codes are 
in the 100 series.
•	Success Codes—Status codes indicating that the request has been successfully 
received and processed. These codes are in the 200 series.
•	Redirection Codes—Status codes indicating that the request has been processed, but 
the client must perform an additional action to complete the request. These actions 
typically involve redirecting to a different location to get the resource. These codes 
are in the 300 series.
•	Client Error Codes—Status codes indicating that there was an error or a problem 
with client’s request. These codes are in the 400 series.
•	Server Error Codes—Status codes indicating that there was an error on the server 
while processing the client’s request. These codes are in the 500 series
Status Code and Description
100 (Continue) Indicates that the server has received the first part of the request and the rest 
of the request should be sent.
200 (OK) Indicates that all went well with the request.
201 (Created) Indicates that request was completed and a new resource got created.
202 (Accepted) Indicates that request has been accepted but is still being processed.
204 (No Content) Indicates that the server has completed the request and has no entity body to 
send to the client.
301 (Moved Permanently) Indicates that the requested resource has been moved to a new location and 
a new URI needs to be used to access the resource.
400 (Bad Request) Indicates that the request is malformed and the server is not able to 
understand the request.
401 (Unauthorized) Indicates that the client needs to authenticate before accessing the resource. 
If the request already contains client’s credentials, then a 401 indicates 
invalid credentials (e.g., bad password).
403 (Forbidden) Indicates that the server understood the request but is refusing to fulfill it. 
This could be because the resource is being accessed from a blacklisted IP 
address or outside the approved time window.
404 (Not Found) Indicates that the resource at the requested URI doesn’t exist.
406 (Not Acceptable) Indicates that the server is capable of processing the request; however, the 
generated response may not be acceptable to the client. This happens when 
the client becomes too picky with its accept headers.
500 (Internal Server Error) Indicates that there was an error on the server while processing the request 
and that the request can’t be completed.
503 (Service Unavailable) Indicates that the request can’t be completed, as the server is overloaded or 
going through scheduled maintenance.
Richardson’s Maturity Model
The Richardson’s Maturity Model (RMM), developed by Leonard Richardson, classifies REST-based Web 
services on how well they adhere to REST principles
Level Zero
This is the most rudimentary maturity level for a service. Services in this level use HTTP as the transport 
mechanism and perform remote procedure calls on a single URI. Typically, POST or GET HTTP methods are 
employed for service calls. SOAP- and XML-RPC-based Web services fall under this level.
Level One
The next level adheres to the REST principles more closely and introduces multiple URIs, one per resource. 
Complex functionality of a large service endpoint is broken down into multiple resources. However, services in this layer use one HTTP verb, typically POST, to perform all of the operations.
Level Two
Services in this level leverage HTTP protocol and make the right use of HTTP verbs and status codes available in the protocol. Web services implementing CRUD operations are good examples of Level 2 services.
Level Three
This is the most mature level for a service and is built around the notion of Hypermedia as the Engine of Application State, or HATEOAS. Services in this level allow discoverability by providing responses that contain links to other related resources and controls that tell the client what to do next.
2 - HTTP Verbs
1 - Resources
0 - RMI/XML-RPC
3 – HATEOAS
Building a RESTful API
Designing and implementing a beautiful RESTful API is no less than an art. It takes time, effort, and several 
iterations. A well-designed RESTful API allows your end users to consume the API easily and makes its 
adoption easier. At a high level, here are the steps involved in building a RESTful API:
 1. Identify Resources—Central to REST are resources. We start modeling different 
resources that are of interest to our consumers. Often, these resources can be the 
application’s domain or entities. However, a one-to-one mapping is not always 
required.
 2. Identify Endpoints—The next step is to design URIs that map resources to 
endpoints. In Chapter 4, we will look at best practices for designing and naming 
endpoints.
 3. Identify Actions—Identify the HTTP methods that can be used to perform 
operations on the resources.
 4. Identify Responses—Identify the supported resource representation for the 
request and response along with the right status codes to be returned.
In the rest of the book, we will look at best practices for designing a RESTful API and implementing it using Spring technologies
