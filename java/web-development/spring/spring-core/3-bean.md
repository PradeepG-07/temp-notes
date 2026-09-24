## Bean
Bean is an object which is created, managed and automatically wired / injected by spring.
## Bean Creation
There are different ways to create a bean based on the type of IoC container started.
### For Annotation Based IoC Container
1. Any class for which we need spring to handle the objects can be decorated with `@Component` annotation.
2. Name of the bean will be the class name but in *camelCase*, if we want to change the name, we can add it in the annotation i.e. `@Component("bean_name")` 
3. Another way of creating beans is declaring a method in the **configuration class** that is used as constructor argument for `AnnotationConfigApplicationContext`
4. **Why this way**: Because there might be some dependencies which are coming from third party JAR and those classes cannot be annotated by us. **So we can declare those beans in the config class**.
5. **How**: Declare a method with the return type of the bean that needs to be created and decorate the method with `@Bean` annotation and return the required object from the method definition.
6. Name of the bean will the method name and can be changed by declaring it in the annotation i.e. `@Bean("bean_name")`
### For XML Based IoC Container
1. To create a bean for any class we need declare it in the XML file.
    ```xml
    <beans>
        <bean  id="beanId" 
                name="beanName1, beanName2, ..." 
                class="org.proj.Class" 
                init-method="methodName"
                destroy-method="methodName"
                scope="prototype"
        />
    </beans>
    ```
2. `id`: Bean id is used to uniquely identify the bean and should always be unique. Id is optional.
3. `name`: Name is like an alias and can be given with multiple names.
4. `class`: Class path for which bean has to be created.
5. `init-method`: Method name should be passed which will be called as part of initialisation callback.
6. `destroy-method`: Method name should be passed which will be called as part of destroying callback.
7. `scope`: Scope can be `singleton` or `prototype`. By default, scope is singleton.

<hr />

## Accessing Bean
1. Bean can be directly accessed from the application context or from the DI. 
2. It is recommended to access it from the Dependency Injection but let's see how to access from context.
3. There are some methods to access the beans from `ApplicationContext`. They are
	1. `getBean(Class)`: Returns the bean of that provided class type
	2. `getBean(String name)`: Returns the bean with that name, but it returns a `Object`, so need to typecast it.
	3. `getBean(String name, Class requiredType)`: Returns the bean with the name of that particular class type.
4. Accessing using Dependency Injection is [[4-dependency-injection]]

>**Note**: Creation and accessing of beans will done with help of Reflections in java.
