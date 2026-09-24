## Tomcat Internal Components
Tomcat mainly contains three components:

### Coyote
Coyote is an HTTP connector, which handles network connections. Its responsibilities are
1. Listen to a specific port(default to 8080)
2. Accept TCP connections
3. Parse HTTP requests
4. Converts raw network data into Servlet Request.
5. Send HTTP responses back to the client.

### Catalina
Catalina is the main Servlet container which executes the main java application code. Its responsibilities are 
1. Loading and managing web application
2. Creating and managing Servlets
3. Handling the Servlet life cycle (`init()`, `service()`, `destroy()`)
4. Managing HTTP sessions
5. Processing servlet requests and responses
6. Applying filters and listeners

### Jasper
Jasper is a JSP engine which will parse JSP files into servlets.

### Advantages
1. Tomcat can serve more than one application at a time and the uniqueness between the applications can be defined using `context-path`.
2. Context Path meaning the unique route prefix for each application.

## External Tomcat Server
Earlier to deploy web applications using tomcat following steps are required

1. Download an external tomcat server from [here](https://tomcat.apache.org/download-90.cgi).
2. Package the java application into WAR(Web Application Archive) format.
	1. WAR package usually contains the compiled classes, configurations, servlet classes, HTML,CSS,JavaScript files etc.
	2. While packaging the application the `jakarta-servlet-api` should be of scope `provided` in the `pom.xml` file, so that the `jakarta` classes are not packaged.
	3. Because tomcat already has these classes and will provide them at the runtime.
3. Copy the war file into `webapps` folder of tomcat.
4. Tomcat automatically generates a folder for this application.
5. Start the tomcat server and then application will be deployed at `localhost:8080/<<app-name>>`
