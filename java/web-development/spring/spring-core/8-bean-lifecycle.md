## Bean Life Cycle
Beans are created in the following steps for a **singleton** bean:

1. **ApplicationContext** (IOC Container) will be started.
2. Read configurations from configuration file/class
3. Read Bean Definitions
4. Instantiate Objects
	1. Objects are initialized one by one according to the bean definitions available.
	2. There is no exact order for creating objects, whatever the file is scanned and contains the `@Component` first will be the one whose object is created first.
5. Dependencies are injected
	1. This step is combined in the 4th step itself when there is a constructor injection happened.
6. Aware Interfaces are called
	1. Aware interfaces such as `BeanNameAware`, `ApplicationContextAware` contains a single method in each of them and will be called by spring.
	2. To know about the `beanName`, `context` related to that bean we can implement the above interfaces and override the `setBeanName` and `setApplicationContext` methods.
	3. Here the naming convention starts with `set` because spring is calling these call back methods which means spring is trying to set the bean name for us once the beans are created and  injection is done.
	4. Mainly these callbacks are used for logging. Specially `ApplicationContextAware` helps in debugging when both XML and Annotation based IoC containers are used.
7. Initialisation Call Backs
	1. These call backs are called by spring to help us perform some initialization logic after the bean is constructed.
	2. Some of the operations that can be performed over here is validating cache, pre-filling any data, or loading any heavy files etc.
	3. There are 3 ways we can create these methods. They are
		1. **InitializationBean**: This is an interface with an abstract method `afterPropertiesSet`which is called when this interface is implemented by the bean class.
		2. **initMethod**: Whenever a bean is created using the `@Bean` annotation in the configuration class, we can create an initialization method in the bean class and pass the name of it to the `@Bean` annotation. Ex: `@Bean(initMethod = "initMethodName")`.
		3. **@PostConstruct**: We can create a method in a bean and decorate it with the annotation. This annotation is part of the `jakarta` package.
	4. **Why we need this step ?**: We can do these things in the constructor itself but doing it here gives us more advantages as the bean dependencies are initialized completely and all the properties are set.
8. Bean is ready to use
9.  Destroying Call Backs
	1. These call backs are called by spring to help us perform some cleanup logic before the bean is destroyed.
	2. Some of the operations that can be performed over here is invalidating cache, clearing filling any data, or closing any opened heavy files etc.
	3. There are 3 ways we can create these methods. They are
		1. **DisposableBean**: This is an interface with an abstract method `destroy`which is called when this interface is implemented by the bean class.
		2. **destroyMethod**: Whenever a bean is created using the `@Bean` annotation in the configuration class, we can create a pre destroy method in the bean class and pass the name of it to the `@Bean` annotation. Ex: `@Bean(destoryMethod = "destroyMethodName")`.
		3. **@PreDestroy**: We can create a method in a bean and decorate it with the annotation. This annotation is part of the `jakarta` package.
10. Bean is destroyed.

<hr />

For the **prototype bean** till the step-8 all the steps are same.  And the object is handover to the application itself.
The destruction of the object is to be handled by the application code. Mostly this will be handled by the garbage collector itself.