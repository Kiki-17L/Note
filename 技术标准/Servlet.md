# 1. Overview



## 1.1 What is a Servlet?

> A servlet is a Jakarta technology-based web component, managed by a container, that generates dynamic content. 



## 1.2 What is a Servlet Container?

The servlet container is **a part of a web server or application server** that provides the network services over which requests and responses are sent, decodes MIME-based requests, and formats MIME-based responses.

A servlet container also contains and manages servlets through their lifecycle.

A servlet container can be built into a host web server, or installed as an **add-on component** to a web server via that server’s native extension API. 

Servlet containers can also be built into or possibly installed into web-enabled application servers.



## 1.3 An example

The following is **a typical sequence of events:**
1. A client (e.g., a web browser) accesses a web server and **makes an HTTP request**.

2. The request is received by the web server and **handed off to the servlet container**. 

   > The servlet container can be running in the same process as the host web server, in a different process on the same host, or on a different host from the web server for which it processes requests.

3. The servlet container **determines which servlet to invoke** based on the configuration of its servlets, and calls it with **objects representing the request and response**.

4. The servlet **uses the request object to find out** who the remote user is, what HTTP POST parameters may have been sent as part of this request, and other relevant data. 

   The servlet performs whatever logic it was programmed with, and **generates data to send back to the client**.

   It sends this data back to the client **via the response object**.

5. Once the servlet has finished processing the request, the servlet container **ensures** that the response is properly flushed, and **returns control back to the host web server**.



## 1.4 Servlets vs Other Technologies

In functionality, servlets provide a higher level abstraction than Common Gateway Interface (CGI) programs but a lower level of abstraction than that provided by web frameworks such as Jakarta Server Faces.



Servlets have the following advantages over other server extension mechanisms:

1. They are generally much **faster** than CGI scripts because a different process model is used.
2. They **use a standard API** that is supported by many web servers.
3. They have all the advantages of the Java programming language, including **ease of development and platform independence**.
4. They can access the **large set of APIs** available for the Java platform.



# 2. The Servlet Interface

The Servlet interface is the central abstraction of the Jakarta Servlet API.

All servlets implement this interface either directly, or more commonly, **by extending a class that implements the interface**. 

The two classes in the Jakarta Servlet API that implement the Servlet interface are `GenericServlet` and `HttpServlet`. 

For most purposes, Developers will extend `HttpServlet` to implement their servlets.



## 2.1 Request Handling Methods

The basic Servlet interface defines a **service method** for handling client requests. 

This method is called for each request that the servlet container routes to an instance of a servlet.



### 2.1.1 HTTP Specific Request Handling Methods

The HttpServlet abstract subclass adds additional methods beyond the basic Servlet interface that are automatically called by the service method in the HttpServlet class to aid in processing HTTP-based requests. These methods are:

1. **doGet** for handling HTTP GET requests
2. **doPost** for handling HTTP POST requests
3. **doPut** for handling HTTP PUT requests
4. **doDelete** for handling HTTP DELETE requests
5. **doHead** for handling HTTP HEAD requests
6. **doOptions** for handling HTTP OPTIONS requests
7. **doTrace** for handling HTTP TRACE requests



### 2.1.2 HEAD Method

The `doHead` method in HttpServlet, **by default**, directly calls the `doGet` method and relies on the container to implement HEAD behaviour of the HTTP
protocol.
However, if the "jakarta.servlet.http.legacyDoHead" `ServletConfig` init parameter is set to "TRUE" then the `doGet` method is called with the 

`ServletResponse` wrapped to provide only the headers produced by the `doGet` method.

The legacy mode is **deprecated** as it may not be accurate in producing the same heads as GET method would have returned and may be removed in a future release.



### 2.1.3 Additional Methods



### 2.1.4 Conditional GET Support

The **HttpServlet** class defines the **getLastModified** method to support conditional GET operations. 

 A conditional **GET** operation requests a resource be sent only if it has been modified since a specified time.



## 2.2 Number of Instances

The **servlet declaration** which is either via the **annotation** or part of the **deployment descriptor** of the web application containing the servlet controls 

how the servlet container provides instances of the servlet.



For a servlet **not hosted in a distributed environment (the default)**, the servlet container must use **only one instance per servlet declaration**.



In the case where a servlet was deployed as part of an application **marked in the deployment descriptor as distributable**, a container may have only 

**one instance per servlet declaration per Java Virtual Machine**.



## 2.3 Servlet Life Cycle

This life cycle is expressed in the API by the **init, service, and destroy** methods of the jakarta.servlet.Servlet interface that all servlets must implement 

directly or indirectly through the `GenericServlet` or `HttpServlet` abstract classes.



### 2.3.1 Loading and Instantiation

The loading and instantiation can occur **when the container is started**, or **delayed until the container determines the servlet is needed** to service a request.

The servlet container loads the servlet class using **normal Java class loading facilities**.

The loading may be from **a local file system, a remote file system, or other network services**.

After loading the Servlet class, the container **instantiates** it for use.



### 2.3.2 Initialization

Initialization is provided so that a servlet can **read persistent configuration data, initialize costly resources (such as JDBC™ API-based connections), and perform other one-time activities**. 

The container initializes the servlet instance by calling the `init` method of the Servlet interface with a unique (per servlet declaration) object implementing the `ServletConfig` interface.

> This configuration object allows the servlet to access name-value initialization parameters from the web application’s configuration information.
>
> The configuration object also gives the servlet access to an object (implementing the `ServletContext` interface) that describes the servlet’s runtime environment.



#### 2.3.2.1 Error Conditions on Initialization

During initialization, the servlet instance can throw an `UnavailableException` or a `ServletException`.

In this case, the servlet must not be placed into active service and **must be released** by the servlet container.

The `destroy` method is not called as it is considered unsuccessful initialization.



A new instance may be instantiated and initialized by the container after a failed initialization.

The exception to this rule is when an `UnavailableException` indicates a minimum time of unavailability, and the container must wait for the period to pass before creating and initializing a new servlet instance.



#### 2.3.2.2 Tool Considerations

The triggering of static initialization methods when a tool loads and introspects a web application is to be distinguished from the calling of the `init` method.

Developers should not assume a servlet is in an active container runtime until the `init` method of the Servlet interface is called.

For example, a servlet should not try to establish connections to databases or Jakarta Enterprise Beans containers **when only static (class) initialization methods have been invoked**.



### 2.3.3 Request Handling

Requests are represented by request objects of type `ServletRequest`.

The servlet fills out responses to requests by calling methods of a provided object of type `ServletResponse`.

These objects are passed as parameters to the `service` method of the `Servlet` interface.

In the case of an **HTTP request**, the objects provided by the container are of types `HttpServletRequest` and `HttpServletResponse`.

> Note that a servlet instance placed into service by a servlet container may handle **no requests** during its lifetime.



#### 2.3.3.1 Multithreading Issues

A servlet container may send **concurrent** requests through the `service` method of the servlet.

To handle the requests, the Application Developer must make adequate provisions for concurrent processing with **multiple threads** in the `service` method.



#### 2.3.3.2 Exceptions During Request Handling

A servlet may throw either a `ServletException` or an `UnavailableException` during the service of a request. 



1. **ServletException**

   A `ServletException` signals that some error occurred during the processing of the request and that the container should take appropriate measures to clean up the request.

2. **UnavailableException**

   An `UnavailableException` signals that the servlet is unable to handle requests either temporarily or permanently.

   

   If a **permanent** unavailability is indicated by the `UnavailableException`, the servlet container must remove the servlet from service, call its `destroy` method, and release the servlet instance.

   Any requests refused by the container by that cause must be returned with a **SC_NOT_FOUND (404)** response.

   

   If **temporary** unavailability is indicated by the `UnavailableException`, the container may choose to not route any requests through the servlet during the time period of the temporary unavailability.

   Any requests refused by the container during this period must be returned with a **SC_SERVICE_UNAVAILABLE (503)** response status along with a `Retry-After` header indicating when the unavailability will terminate.



#### 2.3.3.3 Asynchronous Processing

The asynchronous processing of requests is introduced to **allow the thread to return** to the container and perform other tasks.

When asynchronous processing begins on the request, **another thread or callback** may either **generate the response and call complete or dispatch the request** so that it may run in the context of the container using the `AsyncContext.dispatch` method.



A typical sequence of events for asynchronous processing is:

1. The request is received and passed via normal filters for authentication etc. to the servlet.
2. The servlet processes the request parameters and/or content to determine the nature of the
request.
3. The servlet issues requests for resources or data, for example, sends a remote web service request or joins a queue waiting for a JDBC connection.
4. The servlet **returns** without generating a response.
5. After some time, the requested resource becomes available, the thread handling that event continues processing **either in the same thread or by dispatching** to a resource in the container using the `AsyncContext`



The `@WebServlet` and `@WebFilter` annotations have an attribute `asyncSupported` that is a boolean with a default value of **false**.

When `asyncSupported` is set to **true** the application can start asynchronous processing in a separate thread by calling `startAsync`, passing it a reference to the request and response objects, and then exit from the container on the original thread.

This means that the response will traverse (in reverse order) the same filters (or filter chain) that were traversed on the way in.

The response isn’t committed till `complete` (see below) is called on the `AsyncContext`. 



1. Dispatching from a servlet that has `asyncSupported=true` to one where `asyncSupported` is set to false is allowed
2. Dispatching from a **synchronous** servlet to an **asynchronous** servlet would be **illegal**.



In addition to the annotation attributes, the following methods / classes are provided:



**ServletRequest**

```java
public AsyncContext startAsync(ServletRequest req, ServletResponse res)
```
> This method puts the request into asynchronous mode and initializes its `AsyncContext` with the given request and response objects and the time out returned by `getAsyncTimeout`. 
>
> A call to this method ensures that the response isn’t committed when the application exits out of the `service` method. It is committed when
> `AsyncContext.complete` is called on the returned `AsyncContext` or the `AsyncContext` times out and there are no listeners associated to handle the time out.
>
> The `AsyncContext` could be used to write to the response **from the async thread**. It can also be used to just notify that the response is not closed and committed.
>
> It is **illegal** to call `startAsync` if the request is within the scope of a servlet or filter that does not support asynchronous operations, or if the response has been committed and closed, or is called again during the same dispatch.



```java
public AsyncContext startAsync()
```

> This method is provided as a convenience that uses the original request and response objects for the async processing. 
>
> Please note users of this method **SHOULD** flush the response if they are wrapped before calling this method if you wish, to ensure that any data written to the wrapped response isn’t lost.



```java
public AsyncContext getAsyncContext()
```

> Returns the `AsyncContext` that was created or reinitialized by the invocation of `startAsync`.
>
> It is illegal to call `getAsyncContext` if the request has not been put in asynchronous mode.



```java
public boolean isAsyncSupported()
```

>Returns **true** if the request supports async processing, and **false** otherwise.
>
>Async support will be disabled as soon as the request has passed a filter or servlet that does not support async processing



```java
public boolean isAsyncStarted()
```

> Returns **true** if async processing has started on this request, and **false** otherwise.
>
> If this request has been dispatched using one of the `AsyncContext.dispatch` methods since it was put in asynchronous mode, or a call to `AsynContext.complete` is made, this method returns **false**



```java
public DispatcherType getDispatcherType()
```







**AsyncContext**

> This class represents the execution context for the asynchronous operation that was started on the `ServletRequest`. 
>
> An `AsyncContext` is created and initialized by a call to `ServletRequest.startAsync` as described above. 



```java
public ServletRequest getRequest()
```
> Returns the request that was used to initialize the `AsyncContext` by calling one of the `startAsync` methods.
>
> Calling `getRequest` when `complete` or any of the dispatch methods has been previously called in the asynchronous cycle will result in an `IllegalStateException`.



```java
public ServletResponse getResponse()
```
> Returns the response that was used to initialize the `AsyncContext` by calling one of the `startAsync` methods.
>
> Calling `getResponse` when `complete` or any of the dispatch methods has been previously called in the asynchronous cycle will result in an `IllegalStateException`.



```java
public void setTimeout(long timeoutMilliseconds)
```
> Sets the time out for the asynchronous processing in milliseconds. 
>
> A call to this method overrides the time out set by the container.
>
> If the time out is not specified via the call to `setTimeout`, 30000 is used as the **default**.
>
> A value of 0 or less indicates that the asynchronous operation will never time out. 



```java
public long getTimeout()
```
> Gets the time out, in milliseconds, associated with the `AsyncContext`.
>
> This method returns the container’s default time out, or the time out value set via the most recent invocation of `setTimeout` method.



```java
public void addListener(AsyncListener listener, ServletRequest req, ServletResponse res)
```
> Registers the given listener for notifications of `onTimeout`, `onError`, `onComplete` or `onStartAsync`.



```java
public <T extends AsyncListener> createListener(Class<T> clazz)
```
> Instantiates the given `AsyncListener` class. 
>
> The returned `AsyncListener` instance may be further customized before it is registered with the `AsyncContext` via a call to one of the `addListener` methods specified below.



```java
public void addListener(AsyncListener)
```
> Registers the given listener for notifications of `onTimeout`, `onError`, `onComplete` or `onStartAsync`.



```java
public void dispatch(String path)
```
> Dispatches the request and response that were used to initialize the `AsyncContext` to the resource with the given path.
>
> The given path is interpreted as relative to `the ServletContext` that initialized the `AsyncContext`.



```java
public void dispatch()
```

> Provided as a convenience to dispatch the request and response used to initialize the `AsyncContext` as follows.
>
> If the `AsyncContext` was initialized via the `startAsync(ServletRequest, ServletResponse)` and the request passed is an instance of `HttpServletRequest`, then the dispatch is to the URI returned by `HttpServletRequest.getRequestURI()`.
>
> Otherwise the dispatch is to the URI of the request when it was last dispatched by the container.



```java
public void dispatch(ServletContext context, String path)
```

> Dispatches the request and response used to initialize the `AsyncContext` to the resource with the given path in the given `ServletContext`.



```java
public boolean hasOriginalRequestAndResponse()
```

> This method checks if the `AsyncContext` was initialized with the original request and response objects by calling `ServletRequest.startAsync()` or if it was initialized by calling `ServletRequest.startAsync(ServletRequest, ServletResponse)` and neither the `ServletRequest` nor the
> `ServletResponse` argument carried any application provided wrappers, in which case it returns **true**.



```java
public void start(Runnable r)
```

> This method causes the container to dispatch a thread, possibly from a managed thread pool, to run the specified `Runnable`.
>
> The container may propagate appropriate contextual information to the `Runnable`.



```java
public void complete()
```

> If `request.startAsync` is called then this method MUST be called to complete the async processing and commit and close the response.
>
> The `complete` method can be invoked by the container if the request is dispatched to a servlet that does not support async processing, or the target servlet called by `AsyncContext.dispatch` does not do a subsequent call to `startAsync`. 



**ServletRequestWrapper**

```java
public boolean isWrapperFor(ServletRequest req)
```

> Checks recursively if this wrapper wraps the given `ServletRequest` and returns true if it does, else it returns false.



**ServletResponseWrapper**

```java
public boolean isWrapperFor(ServletResponse res)
```

> Checks recursively if this wrapper wraps the given `ServletResponse` and returns true if it does, else it returns false.



**AsyncListener**

```java
public void onComplete(AsyncEvent event)
```

> Is used to notify the listener of completion of the asynchronous operation started on the `ServletRequest`.



```java
public void onTimeout(AsyncEvent event)
```

> Is used to notify the listener of a time out of the asynchronous operation started on the `ServletRequest`.



#### 2.3.3.4 Thread Safety

Other than the `startAsync` and `complete` methods, implementations of the request and response objects are not guaranteed to be thread safe.

This means that they should either only be used within the scope of the request handling thread or the application must ensure that access to the request and response objects are thread safe

Be aware that other than the `startAsync`, and `complete` methods, the request and response objects are not thread safe.

If those objects were accessed in the multiple threads, **the access should be synchronized or be done through a wrapper** to add the thread safety, 



#### 2.3.3.5 Update Processing

The servlet container provides an HTTP upgrade mechanism.

However the servlet container itself does not have knowledge about the upgraded protocol.

The protocol processing is encapsulated in the`HttpUpgradeHandler`.

Data reading or writing between the servlet container and the `HttpUpgradeHandler` is in **byte streams**.



When an upgrade request is received, the servlet can invoke the `HttpServletRequest.upgrade` method, which starts the upgrade process.

This method instantiates the given `HttpUpgradeHandler` class.

The returned `HttpUpgradeHandler` instance may be further customized.

The application prepares and sends an appropriate response to the client.

After exiting the `service` method of the servlet, the servlet container completes the processing of all filters and marks the connection to be handled by the `HttpUpgradeHandler`. 

It then calls the `HttpUpgradeHandler`'s `init` method, passing a `WebConnection` to allow the protocol handler access to the data streams.



### 2.3.4 End of Service

The servlet container is **not required** to keep a servlet loaded for any particular period of time. 

A servlet instance may be kept active in a servlet container for a period of milliseconds, for the lifetime of the servlet container (which could be a number of days, months, or years), or any amount of time in between.



When the servlet container determines that a servlet should be removed from service, it calls the `destroy` method of the `Servlet` interface to allow the servlet to release any resources it is using and save any persistent state.

For example, the container may do this when it wants to **conserve memory resources**, or **when it is being shut down.**



**Before** the servlet container calls the `destroy` method, it must allow any threads that are currently running in the service method of the servlet to complete execution, or exceed a server-defined time limit.



**Once** the `destroy` method is called on a servlet instance, the container may not route other requests to that instance of the servlet. If the container needs to enable the servlet again, it must do so with a new instance of the servlet’s class.



**After** the `destroy` method completes, the servlet container must release the servlet instance so that it is eligible for garbage collection.



# 3. The Request

> The request object encapsulates all information from the client request.
>
> In the HTTP protocol, this information is transmitted from the client to the server in the HTTP headers and the message body of the request.



