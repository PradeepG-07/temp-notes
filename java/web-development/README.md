## Servlets
1. [Introduction](servlets/1-evolution.md)
2. [Servlets and Servlet Container](servlets/2-servlet-and-servlet-container.md)
3. [Tomcat Detailed](servlets/3-more-about-tomcat.md)
4. [Servlet Detailed](servlets/4-more-about-servlet.md)
5. [Filters](servlets/5-filters-in-servlets.md)
6. [Drawbacks](servlets/drawbacks.md)

## JDBC
1. [Introduction](jdbc/1-intro.md)

## Hibernate
1. [Introduction](hibernate/1-intro.md)
2. [Hibernate Before JPA](hibernate/2-internals-before-jpa.md)

## Build Tools
1. [Maven](build-tools/1-maven.md)
2. [Gradle](build-tools/2-gradle.md)

## Spring Framework

### Spring Core
1. [Introduction](spring/spring-core/1-basics.md)
2. [Spring IOC](spring/spring-core/2-spring-ioc.md)
3. [About Bean](spring/spring-core/3-bean.md)
4. [Dependency Injection (DI)](spring/spring-core/4-dependency-injection.md)
5. [Conflicts and Exceptions in DI](spring/spring-core/5-di-conflicts-exceptions.md)
6. [Circular Dependencies](spring/spring-core/6-circular-dependency.md)
7. [Bean scopes](spring/spring-core/7-bean-scopes.md)
8. [Bean LifeCycle](spring/spring-core/8-bean-lifecycle.md)

### Spring MVC
Prerequisites: **Servlets**
1. [Introduction](spring/spring-mvc/1-intro.md)
2. [Application Flow](spring/spring-mvc/2-detailed-application-flow.md)
3. [Create a Web Application](spring/spring-mvc/3-create-web-mvc-application.md)
4. [Advices](spring/spring-mvc/4-global-cross-cutting-logic-with-advices.md)
5. [Global Exception Handling With Advices](spring/spring-mvc/5-global-exception-handling.md)

### Spring Boot
Always remember Spring Boot just autoconfigures the application.

Prerequisites: **Spring Core**, **Spring MVC**
1. [Introduction](spring/spring-boot/1-intro.md)
2. [Enable Auto Configuration](spring/spring-boot/1.1-enable-auto-configuration.md)
3. [Runner classes](spring/spring-boot/1.2-runners.md)
4. [DTO and Validations](spring/spring-boot/2-dto-and-validations.md)
5. [Application Configurations](spring/spring-boot/3-application-configs.md)
6. [Profiles](spring/spring-boot/4-profiling.md)
7. [Filters](spring/spring-boot/5-filters.md)
8. [Interceptors](spring/spring-boot/6-interceptors.md)

### Spring AOP
1. [Introduction](spring/spring-aop/1-intro-to-aop.md)
2. [Implementing AOP](spring/spring-aop/2-implementing-aop.md)

### Spring JDBC
Prerequisites: **JDBC**
1. [Introduction and Implementation](spring/spring-jdbc/1-intro.md)

### Spring Data JPA
Prerequisites: **JDBC**, **Spring JDBC**, **Hibernate**
1. [JPA and Changes in Hibernate After JPA](spring/spring-data-jpa/1-hibernate-internals-after-jpa.md)
2. [Hibernate with Spring Boot](spring/spring-data-jpa/2-hibernate-with-spring-boot.md)
3. [JPA Relationships](spring/spring-data-jpa/3-jpa-relationships.md)
4. [Cascading, Lazy Loading, N+1 Problem, Entity Graphs](spring/spring-data-jpa/4-cascading-lazy-loading-n+1-entity-graph.md)
5. [JPA Repositories](./spring/spring-data-jpa/5-jpa-repositories.md) TODO

### Spring Security
