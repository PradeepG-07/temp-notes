## What is a Servlet Container
Servlet Container is a java server which helps to run java web applications. There are multiple servlet containers available. Some of them are **Apache Tomcat, eclipse jetty**, etc.

Servlet container manages all the servlet objects and maps them accordingly to each request. This is the same analogy used for the Spring IoC Container as well.
### Apache Tomcat
Tomcat is a java servlet container and a long-running process which listens on a port number continuously and maps incoming HTTP traffic to one of the handlers to fulfill the request.

---

## Servlet
The handlers that are executed by tomcat for each request is called **Servlet**.

Servlets are generally classes whose methods containing business logic are actually executed to handle the incoming request.

## Basic Application Flow
1. Tomcat listening on some port number(default: 8080) identifies that there is a new request.
2. Then tomcat reads the request, identifies request method, end point, headers, body and creates a `HttpServletRequest` object and an empty `HttpServletResponse` object.
3. Based on the endpoint tomcat identifies the servlet mapped to the end point and passes these two objects to the respective servlet.
4. Servlet will get the required information from the `HttpServletRequest` and write the response that needs to be sent to the client into the `HttpServletResponse`.
5. As java follows pass by reference, when servlet writes the data into the response object it is available with the tomcat as well  as it has the reference to response object.
6. After the servlet execution is completed, tomcat returns back the response from `HttpServletResponse` to the client.

## TODO
create servlet
	using annotation based (explore more)
	HttpServletRequest, HttpServletResponse
	how to handle JSON body 
	using XML config
	dependency scope provided? how many dependency scopes exist like this
	lifecycle methods (init, service, destroy) 
		what happens inside HttpServlet class `service` method
tomcat
	how tomcat actually works ? (in depth later)
	XML file to configure port, how to deploy servlet on tomcat, Catalina
	tomcat lifecycle
	eager or lazy 
	idea of central servlet / dispatcher servlet

