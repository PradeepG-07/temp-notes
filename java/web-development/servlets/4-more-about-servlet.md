## Creating a Servlet
1. A servlet can be created by extending the `HttpServlet` class and overriding the abstract methods.
2. Mapping of the servlet can be done by any of the below methods in the Mapping of Servlets section.
3. In web MVC there will be only class based endpoints which can support all the HTTP methods for that particular end point. 
4. We can override `doGet`, `doPost`, ... methods to handle respective HTTP method. These methods will receive `HttpServletRequest` and `HttpServletResponse` as parameters.

## Mapping of Servlets
There are two options on how to map servlets to a particular end point.
### 1. Annotation Based
We can use a annotation called `@WebServlet("route-path")`
### 2. XML Based
Create  a `web.xml` file in the resources folder and add mapping in this file.
```xml
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
     version="5.0">
    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.example.HelloServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

## Life Cycle Methods 
Servlets have 3 methods as part of their life cycle. These methods are invoked by tomcat

### 1. init()
- Init method is called by tomcat when the servlet is created for the first time.
- Tomcat uses lazy initialisation so servlet will be created only if at least one request is sent to that particular route.
### 2. service()
- Basically we don't override the `service()` method of `HttpServlet` parent class.
- Because it manages the responsibility of calling a particular method such as `doGet`, `doPost` etc. based on the request method.
### 3. destroy()
- Destroy method will be called by the tomcat when the tomcat gets shutdown.