## Spring IoC Container
There are two types of configuration that can be provided to spring for starting the IoC Container.
1. Annotation Based Configuration
2. XML Based Configuration

> First we need `spring-context` dependency which gives us all annotations and features of `ApplicationContext` such as Component Scanning, support for Annotation based configuration, Bean Creation and Dependency Injection.

### Annotation Based Configuration
1. For annotation based configuration we will use `AnnotationConfigApplicationContext` concrete class and pass a *configuration class* as a parameter to its constructor.
2. In this configuration if we want beans for a class we need to declare the class with `@Component` annotation. 
3. The **configuration class** will tell spring which package needs to be scanned to create beans and also provides additional configuration of declaring custom beans.
	1. Config class for application context should  be declared with two annotations 
	2. `@Configuration` which tells spring that this class is a configuration class
	3. `@ComponentScan`which tells the spring from where to start the scanning for classes decorated with `@Component`
	4. Another variation of component scan is `@ComponentScan(basePackage)`.
    ```java
    @Configuration  
    // @ComponentScan  or 
    @ComponentScan(basePackage = "com.pradeep.classes")  
    public class AppConfig {  
        // ....
    }
    ```
### XML Based Configuration
1. First we need `spring-context` dependency which gives us all features of `ApplicationContext` such as Component Scanning, Bean Creation and DI.
2. For XML based configuration we will use `ClassPathXmlApplicationContext` concrete class and pass a *configuration file name* as a parameter to its constructor.
3. In the config file we need to add schema for the XML which can be found [here](https://docs.spring.io/spring-framework/docs/4.2.x/spring-framework-reference/html/xsd-configuration.html)
4. In this configuration if we want beans for a class we need to declare a `<bean>` tag with the `id` and `class`.
    ```xml
   <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
            <!-- bean definitions here -->
    </beans>
    ```
5. We can add beans in two or more XML files and import in a parent file and use it. \
    `AppConfig.xml`
    ```xml
   <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans" 
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
            
        <import resource="beans.xml" />
        <import resource="beans1.xml" />
   
    </beans>
   ```
    `beans.xml`
    ```xml
   <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
            <!-- bean definitions here -->
    </beans>
    ```
   `beans1.xml`
    ```xml
   <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
            <!-- bean definitions here -->
    </beans>
    ```