## `net` package

There is a java package called `java.net` which already provides `ServerSocket` and `Socket` classes which provide functionalities like listening to a port and mapping the request to the function call.

As we are dealing with web servers mainly the client-server architecture, we need to handle with specific request and response format according to the HTTP protocol.

But `java.net` package is generic package which can handle only the raw TCP requests and does not know about the HTTP protocol, request & response formats.

If we want to use the `java.net` package we need to handle all the following responsibilities:

1. Read the raw HTTP request
2. Parse the URL, end point, request method, query parameters, headers, body, etc
3. Create an HTTP response
4. Add a status code, body, headers, cookies
5. Send response back to the client
6. Manage multiple requests by implementing multi threading
7. Manage the threads
8. Keep the server up continuously

So there is a need to automatically handle all these responsibilities and make the developer focus only on business logic.

>To solve this, servlet and servlet containers came into the picture.


