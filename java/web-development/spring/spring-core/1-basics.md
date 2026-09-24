## Normal JAR vs Spring Boot Executable JAR
A normal library JAR does not contain all of its dependency JARs inside it.

**Example:** 

There is a reusable library created by you and when its jar is created, then it contains all the *compiled classes* whereas the dependencies are managed through `pom.xml`
Spring Boot application is commonly packaged as ***Executable JAR*** meaning the jar contains the ***application class files , all its dependencies*** so that the application can run as one executable JAR. 

>Executable JAR is also known as **fat JAR**.
## IoC Principle
1. IoC means the control of object creation moves from the class itself to an external system.
2. **Dependency Injection** is one way to achieve *Inversion of control*.
3. Spring takes this idea and automates it through the ***IoC Container***
## Spring IoC Container
1. It helps in creating, managing and wiring of the objects.
2. In Spring, this IoC Container is called as `ApplicationContext` which is a interface.
3. In order to manage the objects spring IoC container needs to know what are the objects needs to be handled by it.
4. This can be done in two ways either using *Annotation Based Configuration* or *XML Based Configuration*. 
5. With help of the configuration spring know what are the objects needs to be taken care by it.
### Beans
1. Objects managed by the Spring IoC Container are called as **Beans**.
2. Creation of bean differs based on the type of IoC Container chosen.
### Annotation Based Configuration
To bring up the IoC Container which supports Annotation based configuration we will use a class `AnnotationConfigApplicationContext` and pass our configuration class in the constructor.
### XML Based Configuration
To bring up the IoC Container which supports XML based configuration we will use `ClassPathXmlApplicationContext` and pass the XML file path in the constructor.

> **Note**: Now a days XML Based configuration is not used, it can be found in legacy applications.
Annotation Based configuration is preferred and is also used by Spring Boot.