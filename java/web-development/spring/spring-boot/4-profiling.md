## Profiling
In Spring Boot, profiles allow to use different configurations for different environments such as development, testing, staging, and production without changing code.

## Profile-specific Configuration
Imagine application needs different database URLs for different environments. Instead of changing the configuration file everytime we can use the following way.

**Syntax:** `application-{profileName}.properties` or `application-{profileName}.yml`

**Default configuration**

```text
# application.properties

spring.application.name=EmployeeApp
```
**Development profile**
```text
# application-dev.properties

server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/devdb
logging.level.root=DEBUG
```

**Production profile**
```text
# application-prod.properties

server.port=8080
spring.datasource.url=jdbc:mysql://prod-server:3306/proddb
logging.level.root=ERROR
```
### Activating a profile
1. Add this property `spring.profiles.active=dev` in `application.properties` file.
   1. Spring Boot loads: `application.properties` first then `application-dev.properties`. The profile-specific values override the defaults.
2. Also, can be done using **command line**, **vm options**, **environment variables**.

## Profile-specific Beans
- Profile can also be used to create beans that exist only in certain environments. 
- It can be a configuration class or any other bean (like service, repository etc.)
    ```java
    // AppConfig.java - Load this config when active profile is dev
    @Configuration
    @Profile("dev")
    public class DevConfig {
    
        @Bean
        public DataSource dataSource() {
            // development datasource
        }
    }
    // ------------------------------------- //
  
    // DevEmailService.java - - Create this bean when active profile is dev
    @Service
    // @Profile("dev")
    @Profile({"dev", "default", "qa"})
    public class DevEmailService {
    }
    ```
> Multiple profiles can be active simultaneously like `spring.profiles.active=prod,dev`. If any property exists in both prod and dev profiles, the last profile dev will override the existing ones.