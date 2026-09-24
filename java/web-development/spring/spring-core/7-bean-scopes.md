## Bean Scope
There are mainly two types of bean scopes

### 1. Singleton
- Singleton meaning there exists a single bean in the IoC container per each bean definition.
- By default, the bean scope is singleton.
- How many times the bean is requested either by calling `getBean` or through dependency injection the same bean is injected.
- Scope of a bean can be declared by using `@Scope("scopeName")` annotation.
- In this case the `scopeName` is "singleton" hence the annotation is `@Scope("singleton")`.
- Singleton scope is used for the beans which are stateless. 
- **Example**: `OrderService`, `PaymentService`, etc.

### 2. Prototype
- When a bean of *Prototype* scope is requested every time a new bean is injected/returned.
- As there might be many number of objects created spring will not handle entire object life cycle.
- A bean can be declared as prototype by using `@Scope("prototype")`.
- Prototype scope is used for the beans which are stateful. 
- **Example**: `User`, `Order`, etc.

### 3. Request
### 4. Session
### 5. Application

<hr />

## Bean initialisation
There are two types of bean initialization

### 1. Eager Initialisation
It means beans are created and stored  in the IoC container at the startup of the IoC container.
By default, the **singleton** beans are eagerly initialized.

### 2. Lazy Initialisation
It means beans are created when it is requested by `getBean` or via dependency injection.
By default, the **prototype** beans are lazily initialized.
We can make **singleton** bean initialization to lazy by adding `@Lazy` annotation.

#### @Lazy
- When `@Lazy` is used at the class level, then its bean is created only when someone requests for it via either `getBean` or dependency injection.
- When `@Lazy` is used at a variable level, then spring does not inject/create the bean.
- Instead, injects a **proxy of that type** and actually creates that bean, injects it when any method or variable is accessed of the proxy is called.

`spring.main.lazy-initialization=true` makes all the beans to be lazy by default globally.
When all bean are declared lazy globally, then we can use `@Lazy(false)` which will make the class to be eagerly initialized.

### Why does spring promote eager initialization ?
Spring follows the `fail-fast` approach here, lets say we have some issues related to wiring like multiple beans exists without primary etc.

If we use eager initialization we will get to know all kinds of issues, bugs etc. at the startup itself and does not cause any issues while the application is running.