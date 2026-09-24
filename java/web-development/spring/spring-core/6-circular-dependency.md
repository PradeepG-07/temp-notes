## Circular Dependency

Suppose there are two beans `A` and `B`.
Let's assume that `A` requires `B` as a dependency to perform an action and `B` also requires `A` as a dependency to perform an action.

Consider we are using **Constructor Injection** meaning object of A cannot be created without B and vice versa.
In this case a circular dependency occurs and spring fails to create bean.

<hr />

### But how ?
- First spring tries to create A and will get to know that in order to create A i need B, so it stops creation of A and checks if B already exists in the container, in this case B doesn't exist so spring tries to create B.
- When B is being created it will get to know that in order to create B it requires A, so it stops creation of B and checks if A already exists in the container, yes it exists and it is in creation state, so spring throws `BeanCurrentlyInCreationException`.

<hr />

> This kind of code is not encouraged and should be architectured in different way, but there are two ways to resolve this.

### 1. Field/Setter Injection
Remove the constructor injection and use field/ setter injection.

#### How is it resolved ?
Generally spring follows the following steps to create beans `A` and `B`
1. Create object of `A`.
2. Inject the dependencies of `A`.
3. Create object of `B`.
4. Inject the dependencies of `B`.

So when the constructor injection is not used, to create object of A, object of B is not mandatory so spring creates object of A and starts injecting dependencies and get to know it requires B and checks if B exists in the container, B doesn't exist so spring will create B object and start injecting B dependencies and get to know that it requires A, and checks if A, exist in the IoC container, yes it does and hence it injects the partial object(object is created but not fully initialized) of A into B and continues, after B is created completely then spring resumes the injection of A. 

In spring boot, this solution is discouraged and will not work from 2.6 version and needs to be explicitly enabled by setting the property `spring.main.allow-circular-reference` to `true`. 

### 2. `@Lazy` Annotation
- We can make one of the bean lazy and make the constructor parameter also to be lazy.
- Let's assume we have class B declared with `@Lazy` and the constructor A accepts a `@Lazy` B parameter object.
- In this case, bean A is created first, with the proxy of B and when any method or property of proxy is called then spring will go and create actual bean of B and inject here in A replacing the proxy B. 