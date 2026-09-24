## Why Spring Boot ?
1. In Spring framework, the subprojects like spring core, spring mvc and others come with a lot of configurations to be made before we write actual business logic.
2. So Spring Boot is introduced to handle these configurations automatically so that developers can focus on business logic.

## Spring Boot
1. Spring boot is not specifically made for spring mvc only but to automate configurations for all other subprojects as well.
2. Spring boot application mainly need a dependency called `spring-boot-starter`.
3. Spring boot application can be easily created with [spring initializer](https://start.spring.io/).

### What is inside @SpringBootApplication ?
Spring boot application annotation mainly has three annotations
#### 1. @SpringBootConfiguration
- This annotation is from spring boot and tells spring boot that this is the main configuration file.
- Other configuration files can be added again just like in spring core with help of `@Configuration` annotation.
#### 2. @EnableAutoConfiguration
- In a simpler way, we can say that this annotation will look at the project(dependencies) and create any beans which are important.
#### 3. @ComponentScan
- This tells that start scanning for beans in the current package of the file annotated with `@SpringBootApplication`.
- Overrides which can be done on `@SpringBootApplication` annotation
```java
@SpringBootApplication(
    scanBasePackages = "com.project"
)
class Main{
    static void main(String[] args){
        SpringApplication.run(Main.class, args); // Returns ConfigurableApplicationContext
    }
}
```
