## Maven

1. It is a powerful build automation and project management tool  used in Java projects.
2. It helps developers standardize project structure, manage dependencies, automate builds, run tests, and package applications.
3. Dependencies are external libraries required by the project.
4. Maven helps in:
    1. Managing project structure
    2. Managing dependencies
    3. Compiling source code
    4. Running unit tests
    5. Packaging applications (`.jar`, `.war`)
    6. Installing artifacts locally
    7. Deploying artifacts to remote repositories
    8. Managing plugins
    9. Maintaining build consistency across teams
5. Maven enforces a standard directory structure.

    ```
    project-name/
    │
    ├── src/
    │   ├── main/
    │   │   ├── java/        -> Application source code
    │   │   ├── resources/   -> Configuration files, properties files
    │   │
    │   ├── test/
    │   │   ├── java/        -> Test source code
    │   │   ├── resources/   -> Test resources
    │
    ├── target/              -> Generated compiled files and packaged output
    │
    ├── pom.xml              -> Maven configuration file
    ```


### POM

`pom.xml` is the main configuration file of a Maven project. pom stands for Project Object Model.

1. Using `pom.xml`, we can configure:
    1. Project metadata
    2. Dependencies
    3. Plugins
    4. Build configurations
    5. Java version
    6. Packaging type
    7. Profiles
    8. Repositories
2. Example:

    ```xml
    <project>
    	<modelVersion>4.0.0</modelVersion> => POM Version
    	
    	<groupId>com.example</groupId> => Organization or company name
    	<artifactId>demo-app</artifactId> => Project name
    	<version>1.0</version> => Version of the project
    	<packaging>jar</packaging> => Type of output (jar, war)
    	
    	<dependencies>
    		<dependency>
    			<groupId>org.springframework</groupId>
    			<artifactId>spring-core</artifactId>
    			<version>6.0.0</version>
    		</dependency>
    	</dependencies>
    	
    	<build>
        <plugins>
        </plugins>
    	</build>
    </project>
    ```

3. Every Maven project inherits configurations from Maven’s **Super POM**.
4. The final combined configuration of all the pom files which is used by maven to build the project is called `Effective POM`
5. It contains inherited plugins, default configurations, repositories, lifecycle bindings.
6. Command to view it: `mvn help:effective-pom`

### Maven Plugins

1. Most functionality in Maven is provided through **plugins**.
2. Plugins are reusable components that perform specific tasks.
3. A plugin contains goals, configurations, executable logic.
4. Example:
    1. `maven-surefire-plugin` runs Junit,TestNG tests. Command: `mvn test`
    2. `spring-boot-maven-plugin`
        1. creates executable JAR,
        2. embeds Tomcat server,
        3. simplifies running Spring Boot apps.
        4. Command: `mvn spring-boot:run`

### Maven Life Cycle

1. Maven works using **lifecycles**, where each lifecycle contains multiple **phases/goals**.
2. Three important life cycles are `clean` ⇒ Removes old build files `default` ⇒ Builds and packages application and has multiple phases in it, `site` ⇒ Generates Project Documentation.

#### Default Lifecycle

| Phase      | Purpose                                 |
|------------|-----------------------------------------|
| `validate` | Validates project structure             |
| `compile`  | Compiles source code                    |
| `test`     | Runs unit tests                         |
| `package`  | Creates `.jar` or `.war`                |
| `verify`   | Runs additional checks                  |
| `install`  | Installs artifact into local repository |
| `deploy`   | Deploys artifact to remote repository   |

When a phase is executed, all previous phases are executed automatically.

### Maven Repositories

deep

1. Maven downloads dependencies from repositories.
2. **Local Repository:** Dependencies downloaded by maven are stored locally in the `~/m2/repository` folder which avoids downloading the dependencies repeatedly.
3. **Central Repository:** The default public repository maintained by Maven. Contains millions of Java libraries. Maven downloads dependencies from central repository when not found in local repository.
4. **Remote Repository:** Organizations maintain private repositories to store internal libraries. Ex: Nexus, JFrog, Artifactory etc.
5. Maven automatically downloads dependencies, resolves transitive dependencies, maintains versions and adds them to classpath.
6. **Transitive Dependency Management:** A dependency might contain other related dependencies, maven installs related dependencies as well which is called **Transitive Dependency Management.**

### Creating a maven Project

1. Maven project can be created using command line or IDE.
2. Maven contains predefined templates called **Archetypes**. To view all the archetypes use the following command: `mvn archetype:generate`
3. Generates a project of archetype webapp, for servlet projects

    ```bash
    mvn archetype:generate \
    -DgroupId=com.example \
    -DartifactId=mywebapp \
    -DarchetypeArtifactId=maven-archetype-webapp \
    -DinteractiveMode=false
    ```

4. Creates src/main/webapp, WEB-INF/, web.xml

**Note:** When an application is packaged using maven, wherever the application needs to be executed, maven must be installed.