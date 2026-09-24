## Dependency Injection
## Annotation Based
In Annotation Based IoC container, dependencies can be injected in three ways
### Constructor Injection
1. When a class is declared with a single constructor then spring automatically injects the dependencies through this constructor.
2. When multiple constructors are available, spring injects in the constructor which is marked with annotation `@AutoWired`.
3. For each parameter of the `@AutoWired` constructor, there should be a unique bean present in the IoC container otherwise it throws the `UnsatisfiedDependencyException`
4. If there are any parameters in the constructor for which the IoC container has no bean then we can follow either way of the below:
	1. we can use `@Value` and *application.properties* to tell spring about the value of parameter.
	2. we can declare a custom bean in the *configuration* class.

### Setter Injection
Setter injection is simple, we can declare a setter for a property of the class and add `@AutoWired` on the setter.

### Field Injection
We can declare a property in a class and add `@AutoWired` on that property, spring will inject that particular dependency.

<hr /> 

## XML Based
In XML Based IoC container, dependencies can be injected in two ways
### Constructor Injection
1. To achieve constructor injection we have a tag called `<constructor-arg />`
	```xml
	<bean id="beanId" class="beanClassPath">
	   <constructor-arg value="someValue" 
	   ref="beanid" index="0" name="attributeName"/>
	</bean>
	```
2. `ref`: Here we can pass the bean id of another bean to inject.
3. `value`: A general value for other parameters of constructor can be passed.
4. `index`: Index defines for which parameter the `value/ref` belongs to.
5. `name`: Can give constructor parameter name to inject the `value/ref`.
6. Multiple parameters values can be passed by declaring multiple `constructor-arg` tags.
7. Similarly, a list or set or map can also be passed.
    ```xml
    <constructor-arg name="attributeName">
        <list>
            <value>Bangalore</value>
            <value>Hyderabad</value>
        </list>
    </constructor-arg>
    <constructor-arg name="attributeName">
        <set>
            <value>Bangalore</value>
            <value>Hyderabad</value>
        </set>
    </constructor-arg>
    <constructor-arg name="attributeName">
        <map>
            <entry key="key-1" value="value-1"></entry>
            <entry key="key-2" value="value-2"></entry>
        </map>
    </constructor-arg>
    ```

### Setter Injection
1. To achieve setter injection we have a tag called `<property />`.
	```xml
	<property name="attributeName" ref="com.project.ClassName" />
	```
2. `name`: Here we have to pass attribute name and for that attribute setter must exist.
3. `ref`: Bean id has to be passed for which bean needs to be injected.

### Field Injection
1. Field Injection is not possible because the fields which can be private cannot be accessed from the `.xml` file.