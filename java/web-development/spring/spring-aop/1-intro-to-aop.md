## Aspect Oriented Programming (AOP)
AOP (Aspect-Oriented Programming) was introduced to solve a problem called cross-cutting concerns.

## What is the problem?
Imagine we have 100 service methods:
```text
createStudent()
updateStudent()
deleteStudent()
createCourse()
updateCourse()
...
```
Now suppose we want to:
- Log method execution
- Measure execution time
- Check permissions
- Start transactions
- Audit actions
- Cache the method result

**Without AOP, we end up writing the same code everywhere, which violates DRY principle.**

## What AOP does?
AOP lets us write that common logic once and apply it to many methods.

AOP can be compared to filters, interceptors.
- Filter works at the Servlet/Web Container level performing global common logic.
- Interceptor works at the Spring MVC level performing common logic across the controllers.
- AOP works at the Spring Bean/Method level performing common logic across beans/methods.

## What is a concern?
A concern is simply a responsibility or functionality that a piece of application needs to handle. There can be multiple concerns in an application.

Let's consider a method `createStudent`. Some of the concerns are listed below:
1. Business Logic: Validate and create student record.
2. Logging: Log who created student.
3. Security: Check whether user has permission to create.
4. Transactions: Commit or rollback changes 
5. Monitoring: Measure execution time

Here the **core concern** of the `createStudent` method is to **perform the business logic**, but there are other concerns as well to perform.

And the **remaining concerns** logging, security, transactions, monitoring can be **needed in some other places** in the application.

## Cross Cutting Concerns
> Functionality that appears in many places or horizontally spans across the application are called Cross Cutting Concerns.

### Problems 
When cross-cutting concerns are present and AOP has not been implemented, the application typically faces two major issues:
- **Scattering**: The same logic is duplicated and spread across multiple classes or methods. 
- **Tangling**: A single method becomes tightly coupled with multiple unrelated services or responsibilities, making the code harder to maintain.

### Solutions
- We can try using a **helper class**, but we need to call the helper methods which also violates DRY, might make typos and makes code hard to read.
- We can try **inheritance**, this can reduce the scattering but ends up in the same issue of calling unncessary methods in the business logic.
- Use **wrapper classes** which is nothing but implementing decorator pattern.
    ```mermaid
    flowchart LR
        A[StudentController]
        B["
        StudentServiceInterface
        create(){}
        "]
        C["
        StudentServiceImpl
        create(){
        // create student record
        }
        "] -->|is a| B
        D["
        StudentServiceDecorator
        create(){
            //cross cutting concerns
            // call studentService create
        }
        "] --> |is a| B
        D --> |has a| C
        A --> |has a| B
        A --> |concrete implementation| D
    ```
- It looks like we have solved the problem, but there are more:
  - Whenever a service is created, a wrapper should be created.
  - All the wrappers should be associated with proper decorators.
  - Main consumer should receive the top decorator etc.
  - All these can be error-prone.
- So we use AOP instead of these solutions.
  - AOP answers the problem with three questions (WHAT, WHERE, WHEN)
  - Example:
  - **WHAT**: Logging
  - **WHERE**: All methods in all services
  - **WHEN**: Method start and method end