Spring container matches the type of bean requested with available beans and injects the bean as  a dependency.

If the type of requested bean is not found in the container then spring throws `NoSuchBeanDefinitionException` 
### Bean Conflict
1. When two or more beans available in the spring container for the requested bean type then spring gets confused which one to inject and throws an `NoUniqueBeanDefinitionException`.
2. This conflict can be resolved with the help of following annotations
	1. `@Primary`: When there is conflict, spring injects a bean which is declared with this annotation.
	2. `@Qualifier("beanName")`: With this annotation we can specify which exact bean is to be injected.