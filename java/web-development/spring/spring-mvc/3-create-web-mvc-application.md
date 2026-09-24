## Introduction
We need some dependencies to create a spring mvc application. They are `spring-web-mvc`, `embed-tomcat` and `jackson-core`.

Spring Web MVC contains annotations such as `@Controller`, `@RestController`, `@RequestMapping`, `@GetMapping` etc to support for web development.

Embed tomcat is used to deploy the application automatically instead of manual copy pasting files

Jackson core is used for serialization and deserialization for `@RequestBody`.

## Creating a Web Application
### Steps
1. Project structure will be similar to that of the MVC architecture
2. Configuration class needs to have the `@EnableWebMvc` annotation to activate the other annotations such as `@RequestBody` etc.
3. **Configure the Tomcat server, IoC container, dispatcher servlet**
4. Start the tomcat server.
5. [Github](https://github.com/PradeepG-07/practice-repo/tree/main/java/spring/mvc/student-crud).

### 3.1 Configure Tomcat Server
Configuring embedding tomcat can be done using the following steps:
1. Create a tomcat object and set the port number
2. Create a connector to handle different protocols such as HTTP, HTTPS, AJP etc.
3. Multiple connectors can also be added.
4. Create a context path, base document (folder URL where all the applications present. ex: webapps) and configure with tomcat object. 

### 3.2 Configure IoC Container
Configure IoC container for web mvc which is `AnnotationWebConfigApplicationContext`and add the configuration class to the IoC Container.

### 3.3 Configure Dispatcher Servlet
- Create a dispatcher servlet and pass the IoC Container to the dispatcher servlet.
- **Why?** Because `HandlerMapping` inside the dispatcher servlet requires IoC Container to create mappings between HTTP request data(url, method, etc.) and the handler methods.
