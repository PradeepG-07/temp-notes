## Implementing AOP

1. To use Aspect-Oriented Programming (AOP) in a Spring Boot application, add the `spring-boot-starter-aop` dependency to your project.
2. For spring-boot 4.0, use `spring-boot-starter-aspectj`
3. Classes that contain cross-cutting concerns (such as logging, security, or auditing) are called **Aspects**. 
4. A class is declared as an aspect by annotating it with `@Aspect`.
5. When implementing AOP, three key questions must be answered:
    - **What** should be executed?
    - **Where** should it be executed?
    - **When** should it be executed?
6. **What** is defined by the logic written inside the methods of the Aspect class.
7. **When** is determined by advice annotations such as `@Before`, `@After`, and `@Around`.
8. **Where** is specified using a pointcut expression that identifies the target methods to which the advice should be applied.

## Advice

**Advice** is a method defined inside an Aspect class and annotated with an AOP annotation such as `@Before`, `@After`, or `@Around`. The advice contains the code that should execute when a matching join point is reached.

In simple terms, advice defines **what action should be performed and when it should be performed**.

## Pointcut

A **Pointcut** is an expression that identifies the join points where an advice should be applied. It specifies the target methods or execution points that match the criteria defined in the expression.

In simple terms, a pointcut defines **where an advice should be executed**.

## Advice Annotations

Methods inside an Aspect class are annotated with advice annotations to specify **when** the advice should run relative to the execution of the target method.

### 1. `@Before`

- An Advice, annotated with `@Before` executes immediately before the target method is invoked.
- It is commonly used for tasks such as logging, validation, authentication, and auditing.
- **Example use case:** Log method details before the actual business logic begins execution.

### JoinPoint
`JoinPoint` is a special class which represents the current intercepted method execution.

It allows the advice to inspect contextual information such as method signature, arguments of the target method, proxy object, target object, etc.

| Method                   | Information Returned                          |
|--------------------------|-----------------------------------------------|
| getSignature()           | Target method signature information           |
| getArgs()                | Arguments passed to target method             |
| getTarget()              | Underlying target object                      |
| getThis()                | The Proxy object currently handling the call  |

### Some Questions
**Q: Can `@Before` prevent the target method execution?** \
**A:** Yes. By throwing a `RuntimeException`.

**Q: Can `@Before` stop modify arguments passed to the target method?** \
**A:** No, but the arguments can be read. To modify use `@Around`.

### 2. `@AfterReturning`

- An Advice, annotated with `@AfterReturning` executes immediately after the **target method returns something and there is no exception raised**.
- `@AfterReturning(value = "pointcut-expression", returning="variablename")` is used to access the return value. 
- Advice should have a parameter of type target method return type and variable name declared in the `returning` param of `@AfterReturning`. 

### Some Questions
**Q: Can `@AfterReturning` replace the returned result?** \
**A:** No, Even though the advice returns any value it is ignored. Better to declare the advice as `void`. If the returned value is a mutable object technically it is possible to modify the result, but not recommended.

### 3. `@AfterThrowing`

- An Advice, annotated with `@AfterThrowing` executes immediately **after the target method throws an exception**.
- `@AfterThrowing(value = "pointcut-expression", throwing="variablename")` is used to access the exception thrown from the target method.
- Advice should have a parameter of type which can handle the exception type and variable name declared in the `throwing` param of `@AfterThrowing`.
- **Example use case:** Error logging, alert creation, failed records audit, etc.

### Some Questions
**Q: Can `@AfterThrowing` handle the exception thrown by target method?** \
**A:** No. As we are not executing the method from inside the advice, so we can't add try catch and handle the exception.

### 4. `@After`

- An Advice, annotated with `@After` executes immediately **after the method execution is completed irrespective of exception thrown**.
- **Example use case:** Removing a `ThreadLocal`value, Releasing any resource or locks, etc.

### 5. `@Around`

- An Advice, annotated with `@Around` executes **before and after the target method execution**.
- It provides full control 
  1. Before the method invocation: execute custom logic before the method runs, modify the arguments passed
  2. After the method invocation: execute custom logic after the method runs, modify the returned value 
  3. Handle exceptions or rethrow the exceptions
  4. Prevent method execution or invoke the method multiple times
- Advice receives an argument called `ProceedingJoinPoint` which extends the `JoinPoint`. It gives info + control over the currently intercepted target method invocation.  
- This advice always returns an `Object` to satisfy any type of caller requirement type.

### `ProceedingJoinPoint`
- It represents the currently intercepted target method invocation and provides the ability to continue the execution.
- It provides the controllable methods `proceed()` and `proceed(Object[] args)` to control the execution.
- `proceed()` means continue with the remaining advices and eventually execute the target method.

> In simple way, **`@Around` advice is just like a wrapper method to the target method**. We have to control everything from passing args, exception handling, returning results.

## Mental Model of Advices
```java
beforeAdviceLogic(); // -->@Before
static { // Ignore this static block
    try{
        String result = targetMethod(); 
        afterReturningAdviceLogic(result); // -->@AfterReturning

    } catch(Exception e){
        afterThrowingAdviceLogic(e); // -->@AfterThrowing
        throw e;
    } finally{
        afterAdviceLogic(); // -->@After
    }
}
```
>Carefully, observe the order of the statements as well, it shows what is done after what.
**Example:** If exception is thrown, `@AfterThrowing` is executed then the exception is propagated up.

## Internal Flow of Execution When AOP is implemented