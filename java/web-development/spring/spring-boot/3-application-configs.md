## External Configs
Values except object reference type of other class can be injected into a bean from external sources.
The external sources are as follows
1. `application.properties`
2. `application.yaml`
3. Environment Variables
4. Command Line arguments
5. System Properties

## What are `application.properties` and `application.yml`?
1. Both files serve the same purpose: they store externalized configuration for a Spring Boot application.

2. Spring Boot automatically reads them at startup and makes the values available through:
    1. @Value
    2. @ConfigurationProperties
    3. The Spring `Environment`

### @Value
- `@Value` can be used on class variables or constructor parameters or method parameters etc.

    **Example:** 
    ```java
    class TestBean{
        // Syntax: @Value("${varName:defaultValue}")
        @Value("${gateway.retry-count:3}")
        private int retryCount;
        private String type;
        public TestBean(@Value("${gateway.type:Razorpay}") String type){
            this.type = type;
        }
    }
    ```

**Injecting in this way is not feasible** because there can be many attributes / variables or a typo can be there etc.

>So we have another annotation called `@ConfigurationProperties("prefix-name")`.

### @ConfigurationProperties
- This annotation is used on a class and the attributes of the class can be used as the actual properties. 
- Whenever the properties are needed we can inject this properties class as a dependency.

    **GateWayProperties.java**
    ```java
    @ConfigurationProperties("gateway")
    @Component
    class GateWayProperties{
        private int retryCount;
        private String type;
    }
    ```

    **application.yaml**
    ```yml
    gateway:
        retry-count: 3
        type: razorpay
    ```

Here spring boot automatically converts the canonical form of `application.yaml` to camel case and injects into the properties class.

> To enable external configuration support without using spring-boot we have to use the `@PropertySource("classpath:app.properties")` annotation to register the configuration file.

## Differences between `application.properties` and `application.yml`
| applicaton.properties                                             | application.yml                   |
|-------------------------------------------------------------------|-----------------------------------|
| Uses `key=value` pairs.                                           | Uses `key:value` pairs            |
| Nested properties are represented using dots (.).                 | Uses indentation instead of dots. |

### Working with lists
`application.properties`
```text
server[0] = server1
server[1] = server2
server[2] = server3
```
`application.yml`
```yml
server:
  - server1
  - server2
  - server3
```
### What if both file exist ?
If both files exist in the application. Spring Boot will read both files. If both define the same property, the one in application.properties is used.

## Environment Variables
Application properties can be injected via environment variables either application specific or the operating system level environment variables.

This type of configuration is mostly seen in docker and kubernetes deployments

```bash
SPRING_PROFILES_ACTIVE=prod
```
## Command line
We can use the following commands to specify the properties in runtime through command line
```bash
java -jar app.jar --spring.profiles.active=dev
```
```bash
mvn spring-boot:run -Dspring-boot.run.profiles=dev
```