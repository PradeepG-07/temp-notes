## Gradle

1. Gradle is modern build automation and project management tool which supports multiple languages unlike maven.
2. `gradle init` is used to initialize the java application with some other parameters asked in the command line.
3. `gradle tasks` is used to list all the tasks that Gradle can perform on the application such as executing tests, packaging application etc.
4. `gradle run` is used to run the java application.
5. Project structure of a java application by Gradle

    ```text
    project-name/
    │
    ├── src/
    │   ├── main/
    │   │   ├── java/              -> Application source code
    │   │   ├── resources/         -> Configuration files
    │   │
    │   ├── test/
    │   │   ├── java/              -> Unit test code
    │   │   ├── resources/         -> Test resources
    │
    ├── build/                     -> Generated build files
    │
    ├── gradle/
    │   └── wrapper/
    │       ├── gradle-wrapper.jar
    │       └── gradle-wrapper.properties
    │
    ├── build.gradle               -> Main build configuration
    ├── settings.gradle            -> Project/module settings
    ├── gradlew                    -> Linux/Mac Gradle wrapper script
    ├── gradlew.bat                -> Windows Gradle wrapper script
    ```

6. **Note:** When an application is packaged using Gradle, the application can be executed in the environments where Gradle is unavailable via the Gradle wrappers(`gradlew` or `gradlew.bat`) which are included in the part of packaging the application by Gradle.
7. **Gradle Wrapper:** It is set of scripts and small jar files that allow us to run Gradle without manually installing Gradle on to the system.
8. `build.gradle` is same like `pom.xml` which is the main configuration file for package naming, test framework selection, repositories, dependencies and their versions, java package version etc.
9. `build.gradle` can be written in groovy or kotlin, which is chosen at the time of project initialization.