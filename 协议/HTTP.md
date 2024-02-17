# 1. HTTP Header Fields



## 1.1 General Format

In HTTP version 1.x, header fields are transmitted after the request line (in case of a request HTTP message) or the response line (in case of a response 

HTTP message), which is the first line of a message.

Header fields are **colon-separated key-value pairs** in **clear-text** string format, terminated by a carriage return (CR) and line feed (LF) character sequence. 

The end of the header section is indicated by an empty field line, resulting in the transmission of two consecutive CR-LF pairs.



## 1.2 Fields Value

A few fields can contain **comments** (i.e. in User-Agent, Server, Via fields), which can be ignored by software.

Many field values may contain a **quality (q) key-value pair** separated by equals sign, specifying a weight to use in **content negotiation**.

For example, a browser may indicate that it accepts information in German or English, with German as preferred by setting the q value for de higher than 

that of en, as follows:

```
Accept-Language: de; q=1.0, en; q=0.5
```



## 1.3 Size limits

The standard imposes **no limits** to the size of each header field name or value, or to the number of fields. However, most servers, clients, and proxy 

software impose some limits for practical and security reasons. 

For example, the Apache 2.3 server by default limits the size of each field to 8,190 bytes, and there can be at most 100 header fields in a single request



# 2. Request Message

> Request messages are sent by a client to a target server



## 2.1 Syntax

A client sends *request messages* to the server, which consist of:

1. **request line**

   > a **request line**, consisting of the case-sensitive **request method, a space, the requested URL, another space, the protocol version**,
   >
   > **a carriage return, and a line feed**, e.g.：`GET /images/logo.png HTTP/1.1`
   >
   > In the HTTP/1.1 protocol, all header fields except `Host: hostname` are optional.
   >
   > 
   >
   > method+space+URL+space+protocol version

2. **request header**

   > zero or more request header fields(at least 1 or more headers in case of HTTP/1.1)
   >
   > each consisting of the **case-insensitive field name, a colon, optional leading whitespace, the field value, an optional trailing whitespace **
   >
   > **and ending with a carriage return and a line feed**, e.g.:
   >
   > ```
   > Host: www.example.com
   > Accept-Language: en
   > ```
   >
   > 
   >
   > field name+`:`+[white space]+value+[white space]

3. **an empty line**

   > consisting of a carriage return and a line feed

4. **message body**



> A request line containing only the path name is accepted by servers to maintain compatibility with HTTP clients before the HTTP/1.0 specification in RFC 1945



## 2.2 Methods

HTTP defines methods (sometimes referred to as *verbs*, but nowhere in the specification does it mention *verb*) to indicate the desired action to be 

performed on the identified resource.

> The HTTP/1.0 specification defined the GET, HEAD, and POST methods, and the HTTP/1.1 specification added five new methods: PUT, DELETE, 
>
> CONNECT, OPTIONS, and TRACE.
>
> There is no limit to the number of methods that can be defined, which allows for future methods to be specified without breaking existing 
>
> infrastructure. For example, WebDAV defined seven new methods and RFC 5789 specified the PATCH method.



1. **GET**

   > The GET method requests that the target resource **transfer** a representation of its state.
   >
   > GET requests should only retrieve data and should have no other effect. 

2. **HEAD**

   > The HEAD method requests that the target resource **transfer** a representation of its state, as for a GET request, **but without the **
   >
   > **representation data enclosed in the response body. **
   >
   > This is useful for retrieving the representation metadata in the response header, without having to transfer the entire representation.
   >
   > Uses include checking whether a page is available through the status code and quickly finding the size of a file (Content-Length).
   
3. **POST**

   > The POST method requests that the target resource **process** the representation enclosed in the request according to the semantics of the target 
   >
   > resource

4. **PUT**

   > The PUT method requests that the target resource **create or update** its state with the state defined by the representation enclosed in the 
   >
   > request.
   >
   > A distinction from POST is that **the client specifies the target location on the server**.

5. **DELETE**

   > The DELETE method requests that the target resource **delete** its state.

6. **CONNECT**

   > The CONNECT method requests that **the intermediary establish a TCP/IP tunnel to the origin server identified by the request target.** 
   >
   > It is often used to secure connections through one or more HTTP proxies with TLS.

7. **OPTIONS**

   > The OPTIONS method requests that **the target resource transfer the HTTP methods that it supports.** 
   >
   > This can be used to check the functionality of a web server by requesting '*' instead of a specific resource.

8. **TRACE**

   > The TRACE method requests that **the target resource transfer the received request in the response body. **
   >
   > That way a client can see what (if any) changes or additions have been made by intermediaries.

9. **PATCH**

   > The PATCH method requests that **the target resource modify its state according to the partial update defined in the representation **
   >
   > **enclosed in the request. **
   >
   > This can save bandwidth by updating a part of a file or document without having to transfer it entirely.



## 2.3 Standard header fields

> Request header fields allow the client to pass additional information beyond the request line, acting as request modifiers (similarly to the 
>
> parameters of a procedure). 
>
> They give information about the client, about the target resource, or about the expected handling of the request.



|                             Name                             |                         Description                          |                           Example                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|                             A-IM                             |      Acceptable instance-manipulations for the request.      |                         `A-IM: feed`                         |
|                            Accept                            |    Media type(s) that is/are acceptable for the response.    |                     `Accept: text/html`                      |
|                        Accept-Charset                        |             Character sets that are acceptable.              |                   `Accept-Charset: utf-8`                    |
|                       Accept-Datetime                        |                 Acceptable version in time.                  |       `Accept-Datetime: Thu, 31 May 2007 20:35:00 GMT`       |
|                       Accept-Encoding                        |                List of acceptable encodings.                 |               `Accept-Encoding: gzip, deflate`               |
|                       Accept-Language                        |       List of acceptable human languages for response.       |                   `Accept-Language: en-US`                   |
| Access-Control-Request-Method, Access-Control-Request-Headers | Initiates a request for cross-origin resource sharing with `Origin` . |             `Access-Control-Request-Method: GET`             |
|                        Authorization                         |     Authentication credentials for HTTP authentication.      |     `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`      |
|                        Cache-Control                         | Used to specify directives that *must* be obeyed by all caching mechanisms along the request-response chain. |                  `Cache-Control: no-cache`                   |
|                          Connection                          | Control options for the current connection and list of hop-by-hop request fields.Must not be used with HTTP/2. |        `Connection: keep-alive``Connection: Upgrade`         |
|                       Content-Encoding                       |            The type of encoding used on the data.            |                   `Content-Encoding: gzip`                   |
|                        Content-Length                        |   The length of the request body in octets (8-bit bytes).    |                    `Content-Length: 348`                     |
|                         Content-Type                         | The [Media type](https://en.wikipedia.org/wiki/Media_type) of the body of the request (used with POST and PUT requests). |      `Content-Type: application/x-www-form-urlencoded`       |
|                            Cookie                            | An HTTP cookie previously sent by the server with `Set-Cookie` . |               `Cookie: $Version=1; Skin=new;`                |
|                             Date                             |   The date and time at which the message was originated .    |            `Date: Tue, 15 Nov 1994 08:12:31 GMT`             |
|                            Expect                            | Indicates that particular server behaviors are required by the client. |                    `Expect: 100-continue`                    |
|                          Forwarded                           | Disclose original information of a client connecting to a web server through an HTTP proxy. | `Forwarded: for=192.0.2.60;proto=http;by=203.0.113.43` `Forwarded: for=192.0.2.43, for=198.51.100.17` |
|                             From                             |      The email address of the user making the request.       |                   `From: user@example.com`                   |
|                             Host                             | The domain name of the server (for virtual hosting), and the TCP port number on which the server is listening. The port number may be omitted if the port is the standard port for the service requested. Mandatory since HTTP/1.1. If the request is generated directly in HTTP/2, it should not be used. |                `Host: en.wikipedia.org:8080`                 |
|                           If-Match                           | Only perform the action if the client supplied entity matches the same entity on the server. This is mainly for methods like PUT to only update a resource if it has not been modified since the user last updated it. |        `If-Match: "737060cd8c284d8af7ad3082f209582d"`        |
|                      If-Modified-Since                       | Allows a *304 Not Modified* to be returned if content is unchanged. |      `If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT`      |
|                        If-None-Match                         | Allows a *304 Not Modified* to be returned if content is unchanged. |     `If-None-Match: "737060cd8c284d8af7ad3082f209582d"`      |
|                           If-Range                           | If the entity is unchanged, send me the part(s) that I am missing; otherwise, send me the entire new entity. |        `If-Range: "737060cd8c284d8af7ad3082f209582d"`        |
|                     If-Unmodified-Since                      | Only send the response if the entity has not been modified since a specific time. |     `If-Unmodified-Since: Sat, 29 Oct 1994 19:43:31 GMT`     |
|                         Max-Forwards                         | Limit the number of times the message can be forwarded through proxies or gateways. |                      `Max-Forwards: 10`                      |
|                            Origin                            | Initiates a request for cross-origin resource sharing (asks server for Access-Control-* response fields). |       `Origin: http://www.example-social-network.com`        |
|                            Pragma                            | Implementation-specific fields that may have various effects anywhere along the request-response chain. |                      `Pragma: no-cache`                      |
|                            Prefer                            | Allows client to request that certain behaviors be employed by a server while processing a request. |               `Prefer: return=representation`                |
|                     Proxy-Authorization                      |     Authorization credentials for connecting to a proxy.     |  `Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`   |
|                            Range                             |  Request only part of an entity. Bytes are numbered from 0.  |                    `Range: bytes=500-999`                    |
|                           Referer                            | This is the address of the previous web page from which a link to the currently requested page was followed. |      `Referer: http://en.wikipedia.org/wiki/Main_Page`       |
|                              TE                              | The transfer encodings the user agent is willing to accept: the same values as for the response header field Transfer-Encoding can be used, plus the "trailers" value (related to the "chunked" transfer method) to notify the server it expects to receive additional fields in the trailer after the last, zero-sized, chunk.Only `trailers` is supported in HTTP/2. |                   `TE: trailers, deflate`                    |
|                           Trailer                            | The Trailer general field value indicates that the given set of header fields is present in the trailer of a message encoded with chunked transfer coding. |                   `Trailer: Max-Forwards`                    |
|                      Transfer-Encoding                       | The form of encoding used to safely transfer the entity to the user. Currently defined methods are: chunked, compress, deflate, gzip, identity.Must not be used with HTTP/2. |                 `Transfer-Encoding: chunked`                 |
|                          User-Agent                          |           The user agent string of the user agent.           | `User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:12.0) Gecko/20100101 Firefox/12.0` |
|                           Upgrade                            | Ask the server to upgrade to another protocol.Must not be used in HTTP/2. |    `Upgrade: h2c, HTTPS/1.3, IRC/6.9, RTA/x11, websocket`    |
|                             Via                              | Informs the server of proxies through which the request was sent. |        `Via: 1.0 fred, 1.1 example.com (Apache/1.1)`         |



# 3. Response Message

> A response message is sent by a server to a client as a reply to its former request message.



## 3.1 Syntax

A server sends *response messages* to the client, which consist of:

1. **status line**

   >a status line, consisting of the **protocol version**, **a space**, the response **status code**, another **space**, a possibly empty **reason phrase**, a carriage 
   >
   >return and a line feed, e.g.:
   >
   >```
   >HTTP/1.1 200 OK
   >```

2. **response header**

3. **empty line**

4. **an optional message body**.



## 3.2 Status code

In HTTP/1.0 and since, **the first line** of the HTTP response is called the **status line** and includes a **numeric status code** (such as "404") and a **textual** 

**reason phrase** (such as "Not Found"). 

The response status code is a three-digit integer code representing the result of the server's attempt to understand and satisfy the client's corresponding 

request.The way the client handles the response depends primarily on the status code, and secondarily on the other response header fields. 

Clients may not understand all registered status codes but they must understand their class (given by the first digit of the status code) and treat an 

unrecognized status code as being equivalent to the **x00 status code** of that class.



The first digit of the status code defines its class:

- **`1XX` (informational)**

  The request was received, continuing process.

- **`2XX` (successful)**

  The request was successfully received, understood, and accepted.

- **`3XX` (redirection)**

  Further action needs to be taken in order to complete the request.

- **`4XX` (client error)**

  The request contains bad syntax or cannot be fulfilled.

- **`5XX` (server error)**

  The server failed to fulfill an apparently valid request.





## 3.3 Standard header fields

|                          Field name                          |                         Description                          |                           Example                            |       Status        |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :-----------------: |
|                          Accept-CH                           |                  Requests HTTP Client Hints                  |                  `Accept-CH: UA, Platform`                   |    Experimental     |
| Access-Control-Allow-Origin, Access-Control-Allow-Credentials, Access-Control-Expose-Headers, Access-Control-Max-Age, Access-Control-Allow-Methods, Access-Control-Allow-Headers | Specifying which web sites can participate in cross-origin resource sharing |               `Access-Control-Allow-Origin: *`               | Permanent: standard |
|                         Accept-Patch                         | Specifies which patch document formats this server supports  |          `Accept-Patch: text/example;charset=utf-8`          |      Permanent      |
|                        Accept-Ranges                         | What partial content range types this server supports via byte serving |                    `Accept-Ranges: bytes`                    |      Permanent      |
|                             Age                              |   The age the object has been in a proxy cache in seconds    |                          `Age: 12`                           |      Permanent      |
|                            Allow                             | Valid methods for a specified resource. To be used for a *405 Method not allowed* |                      `Allow: GET, HEAD`                      |      Permanent      |
|                           Alt-Svc                            | A server uses "Alt-Svc" header (meaning Alternative Services) to indicate that its resources can also be accessed at a different network location (host or port) or using a different protoco. When using HTTP/2, servers should instead send an ALTSVC frame. |    `Alt-Svc: http/1.1="http2.example.com:8001"; ma=7200`     |      Permanent      |
|                        Cache-Control                         | Tells all caching mechanisms from server to client whether they may cache this object. It is measured in seconds |                `Cache-Control: max-age=3600`                 |      Permanent      |
|                          Connection                          | Control options for the current connection and list of hop-by-hop response fields. Must not be used with HTTP/2. |                     `Connection: close`                      |      Permanent      |
|                     Content-Disposition                      | An opportunity to raise a "File Download" dialogue box for a known MIME type with binary format or suggest a filename for dynamic content. Quotes are necessary with special characters. |   `Content-Disposition: attachment; filename="fname.ext"`    |      Permanent      |
|                       Content-Encoding                       |            The type of encoding used on the data.            |                   `Content-Encoding: gzip`                   |      Permanent      |
|                       Content-Language                       | The natural language or languages of the intended audience for the enclosed content |                    `Content-Language: da`                    |      Permanent      |
|                        Content-Length                        |    The length of the response body in octets(8-bit bytes)    |                    `Content-Length: 348`                     |      Permanent      |
|                       Content-Location                       |         An alternate location for the returned data          |                `Content-Location: /index.htm`                |      Permanent      |
|                        Content-Range                         |  Where in a full body message this partial message belongs   |           `Content-Range: bytes 21010-47021/47022`           |      Permanent      |
|                         Content-Type                         |                The MIME type of this content                 |           `Content-Type: text/html; charset=utf-8`           |      Permanent      |
|                             Date                             |         The date and time that the message was sent          |            `Date: Tue, 15 Nov 1994 08:12:31 GMT`             |      Permanent      |
|                          Delta-Base                          |   Specifies the delta-encoding entity tag of the response.   |                     `Delta-Base: "abc"`                      |      Permanent      |
|                             ETag                             | An identifier for a specific version of a resource, often a message digest |          `ETag: "737060cd8c284d8af7ad3082f209582d"`          |      Permanent      |
|                           Expires                            | Gives the date/time after which the response is considered stale |           `Expires: Thu, 01 Dec 1994 16:00:00 GMT`           | Permanent: standard |
|                              IM                              |       Instance-manipulations applied to the response.        |                          `IM: feed`                          |      Permanent      |
|                        Last-Modified                         |       The last modified date for the requested object        |        `Last-Modified: Tue, 15 Nov 1994 12:45:26 GMT`        |      Permanent      |
|                             Link                             | Used to express a typed relationship with another resource, where the relation type is defined by RFC 5988 |               `Link: </feed>; rel="alternate"`               |      Permanent      |
|                           Location                           | Used in **redirection**, or when a new resource has been created. | Example 1: `Location: http://www.w3.org/pub/WWW/People.html`Example 2: `Location: /pub/WWW/People.html` |      Permanent      |
|                             P3P                              | This field is supposed to set [P3P](https://en.wikipedia.org/wiki/P3P) policy, in the form of `P3P:CP="your_compact_policy"`. However, P3P did not take off,[[53\]](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#cite_note-54) most browsers have never fully implemented it, a lot of websites set this field with fake policy text, that was enough to fool browsers the existence of P3P policy and grant permissions for [third party cookies](https://en.wikipedia.org/wiki/Third_party_cookie). | `P3P: CP="This is not a P3P policy! See https://en.wikipedia.org/wiki/Special:CentralAutoLogin/P3P for more info."` |      Permanent      |
|                            Pragma                            | Implementation-specific fields that may have various effects anywhere along the request-response chain. |                      `Pragma: no-cache`                      |      Permanent      |
|                      Preference-Applied                      | Indicates which Prefer tokens were honored by the server and applied to the processing of the request. |         `Preference-Applied: return=representation`          |      Permanent      |
|                      Proxy-Authenticate                      |         Request authentication to access the proxy.          |                 `Proxy-Authenticate: Basic`                  |      Permanent      |
|                       Public-Key-Pins                        | HTTP Public Key Pinning, announces hash of website's authentic TLS certificate | `Public-Key-Pins: max-age=2592000; pin-sha256="E9CZ9INDbd+2eRQozYqqbQ2yXLVKB9+xcprMF+44U1g=";` |      Permanent      |
|                         Retry-After                          | If an entity is temporarily unavailable, this instructs the client to try again later. Value could be a specified period of time (in seconds) or a HTTP-date. | Example 1: `Retry-After: 120`Example 2: `Retry-After: Fri, 07 Nov 2014 23:59:59 GMT` |      Permanent      |
|                            Server                            |                    A name for the server                     |                `Server: Apache/2.4.1 (Unix)`                 |      Permanent      |
|                          Set-Cookie                          |                        An HTTP cookie                        |    `Set-Cookie: UserID=JohnDoe; Max-Age=3600; Version=1`     | Permanent: standard |
|                  Strict-Transport-Security                   | A HSTS Policy informing the HTTP client how long to cache the HTTPS only policy and whether this applies to subdomains. | `Strict-Transport-Security: max-age=16070400; includeSubDomains` | Permanent: standard |
|                           Trailer                            | The Trailer general field value indicates that the given set of header fields is present in the trailer of a message encoded with chunked transfer coding. |                   `Trailer: Max-Forwards`                    |      Permanent      |
|                      Transfer-Encoding                       | The form of encoding used to safely transfer the entity to the user. Currently defined methods are: chunked, compress, deflate, gzip, identity.Must not be used with HTTP/2. |                 `Transfer-Encoding: chunked`                 |      Permanent      |
|                              Tk                              | Tracking Status header, value suggested to be sent in response to a DNT(do-not-track), possible values:`"!" — under construction "?" — dynamic "G" — gateway to multiple parties "N" — not tracking "T" — tracking "C" — tracking with consent "P" — tracking only if consented "D" — disregarding DNT "U" — updated ` |                           `Tk: ?`                            |      Permanent      |
|                           Upgrade                            | Ask the client to upgrade to another protocol.Must not be used in HTTP/2 |    `Upgrade: h2c, HTTPS/1.3, IRC/6.9, RTA/x11, websocket`    |      Permanent      |
|                             Vary                             | Tells downstream proxies how to match future request headers to decide whether the cached response can be used rather than requesting a fresh one from the origin server. |    Example 1: `Vary: *`Example 2: `Vary: Accept-Language`    |      Permanent      |
|                             Via                              | Informs the client of proxies through which the response was sent. |        `Via: 1.0 fred, 1.1 example.com (Apache/1.1)`         |      Permanent      |
|                       WWW-Authenticate                       | Indicates the authentication scheme that should be used to access the requested entity. |                  `WWW-Authenticate: Basic`                   |      Permanent      |



# 4. Content-Type

This header field indicates the **media type** of the message content, consisting of a *type* and *subtype*, for example

```
Content-Type: text/plain
```



## 4.1 Media Type

A media type (formerly known as a **MIME type**) is a two-part identifier for file formats and format contents transmitted on the Internet.

Their purpose is somewhat similar to file extensions in that they identify the intended data format.



### Naming

A media type consists of a *type* and a *subtype*, which is further structured into a *tree*. A media type can optionally define a **suffix** and **parameters**:

```
mime-type = type "/" [tree "."] subtype ["+" suffix]* [";" parameter];
```



### Types
The "type" part defines the broad use of the media type.

As of November 1996, the registered types were: **application, audio, image, message, multipart, text and video**.

By December 2020, the registered types included the **foregoing, plus font, example, and model**.

An unofficial top-level type in common use is **chemical**.



### Subtypes

A subtype typically consists of a **media format**, but it may or must also contain other content, such as a **tree prefix, producer, product or suffix**, according to the different rules in registration trees.



#### Tree prefix

For the efficiency and flexibility of the media type registration process, different structures of subtypes can be registered in registration trees that are distinguished by the use of tree prefixes. Currently the following trees are created: **standard** (no prefix), **vendor** (`vnd.` prefix), **personal or vanity** (`prs.` prefix), unregistered (`x.` prefix).

1. **Standard tree**

   > The standards tree does not use any tree prefix. Examples are `text/javascript`, `image/png`

2. **Vendor tree**

   > The vendor tree includes media types associated with publicly available products. It uses the `vnd.` tree prefix. 
   >
   > Examples are: `application/vnd.ms-excel`, `application/vnd.oasis.opendocument.text`.

3. **Personal or vanity tree**

   > The personal or vanity tree includes media types associated with non publicly available products or experimental media types.
   >
   > It uses the `prs.` tree prefix. Examples are `audio/prs.sid`, `image/prs.btif`.

4. **Unregistered tree**

   > The unregistered tree includes media types intended exclusively for use in private environments and only with the active agreement of the parties exchanging them. 
   >
   > It uses the `x.` tree prefix. Examples are `application/x.foo`, `video/x.bar`. Media types in this tree cannot be registered.
   >
   > 
   >
   > Media types that have been widely deployed (with a subtype prefixed with `x-` or `X-`) without being registered, should be, if possible, re-registered with a proper prefixed subtype. 
   >
   > If this is not possible, the media type can, after an approval by both the media types reviewer and the IESG, be registered in the standards tree with its unprefixed subtype.
   >
   > `application/x-www-form-urlencoded` is an example of a widely deployed type that ended up registered with the `x-` prefix.



#### Suffix

Suffix is an augmentation to the media type definition to additionally specify the underlying structure of that media type, allowing for generic processing based on that structure and independent of the exact type's particular semantics. 

The `+xml` suffix has been defined since January 2001 (RFC 3023), and was formally included in the initial contents of the Structured Syntax Suffix Registry along with `+json`, `+ber`, `+der`, `+fastinfoset`, `+wbxml`, and `+zip` in January 2013 (RFC 6839). 

Subsequent additions include `+gzip`, `+cbor`, `+json-seq`, and `+cbor-seq`.



## 4.2 Multipart messages

The MIME multipart message contains a boundary in the header field `Content-Type:`; this boundary, which must not occur in any of the parts, is placed between the parts, and at the beginning and end of the body of the message, as follows:

```http
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary=frontier

This is a message with multiple parts in MIME format.
--frontier
Content-Type: text/plain

This is the body of the message.
--frontier
Content-Type: application/octet-stream
Content-Transfer-Encoding: base64

PGh0bWw+CiAgPGhlYWQ+CiAgPC9oZWFkPgogIDxib2R5PgogICAgPHA+VGhpcyBpcyB0aGUg
Ym9keSBvZiB0aGUgbWVzc2FnZS48L3A+CiAgPC9ib2R5Pgo8L2h0bWw+Cg==
--frontier--
```

Each part consists of its own content header (zero or more `Content-` header fields) and a body.



Notes:

- Before the first boundary is an area that is ignored by MIME-compliant clients. This area is generally used to put a message to users of old non-MIME clients.
- It is up to the sending mail client to choose a boundary string that doesn't clash with the body text. Typically this is done by inserting a long random string.
- The last boundary must have two **hyphens** at the end.



### Multipart subtypes

1. mixed

2. digest

3. alternative

4. related

5. report

6. signed

7. encryped

8. **form-data**

   > The MIME type *multipart/form-data* is used to express values submitted through a form. 
   >
   > Originally defined as part of HTML 4.0, it is most commonly used for submitting files with HTTP. 
   >
   > It is specified in RFC 7578, superseding RFC 2388.

9. x-mixed-replace

10. byterange
