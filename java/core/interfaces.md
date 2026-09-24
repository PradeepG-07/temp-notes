## Interface

1. A **Java Interface** is a blueprint of a class that defines a set of abstract methods and constants which implementing classes must provide.
2. It is used to achieve **abstraction**, **multiple inheritance**, and **loose coupling** in Java.
3. Only public or default access modifier are allowed.
4. **Fields:** All the declared fields in interface are **constants,** meaning `public static final`  by default and accessing a variable from an interface is very similar to accessing a static variable in a class.
5. **Methods:** All the declared methods are `abstract` by default, meaning only method signature (name, parameters and exceptions) can be provided.
6. Generics are supported by Interfaces.
7. **Note: Constructors, initializer blocks** are not allowed in interface.

## Implementing Interface

1. AN interface can be implemented by a **Class, Abstract class, Nested Class, Enum, Java Dynamic Proxy.**
2. A class implementing an interface must implement all the methods of the interface, variables need not be declared.
3. A class can implement interfaces with following syntax after the class name `implements <interface_name>[,<interface_name2>,....]` , where [] = optional.
4. An interface can extend other interfaces with the following syntax after the interface name `extends <interface_name>[,<interface_name2>,....]` , where [] = optional.

## Default and Static Methods
1. When multiple classes implement the same interface from any other project, when the owner of the main project add a new method to existing interface it would crash the other project and manually this method should be overridden.
2. To solve the above problem, in **Java 8,** default methods are introduced, meaning an interface can have a default method marked as `default` with the method implementation. Simply we can say `default` methods are helpful for backward compatibility of old interfaces.
3. Interface can have **static methods,** which **must** have a method **definition.**

## Problems in Inheritance with default Methods
| Situation                                                                                                                 | Solution                                                                        |
|---------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| **A class implements two unrelated interfaces that have the same default method.**                                        | Override the method in the class and choose/customize the implementation.       |
| **An interface extends multiple interfaces that provide the same method (default/default or default/abstract conflict).** | Override/redeclare the method in the subinterface to resolve the conflict.      |
| **A class inherits a method from a superclass and also a default method from an interface.**                              | No action needed. The **class method wins automatically**.                      |
| **A class implements an interface and its subinterface, both containing the same default method.**                        | No action needed. The **more specific subinterface method wins automatically**. |


## Abstract Classes vs Interfaces
1. Use interfaces when two or more unrelated classes need to perform similar actions or follow the same contract.
2. Use abstract classes when two or more related classes share common code and behavior. Put the shared code and default implementations inside the abstract class, and allow subclasses to override the behavior when necessary. You can also combine interfaces with abstract classes to reuse common logic while still maintaining a common contract.

## Interface access modifiers
Java interfaces are meant to specify fields and methods that are publicly available in classes that implement the interfaces. Therefore, you cannot use the `private` and `protected` access modifiers on interface.

Fields and methods in interfaces are implicitly declared `public` if you leave out an access modifier, so there is no use the default access modifier either (no access modifier). but can be declared as `private`.


## Interview Questions

1. Static methods in interfaces and their purpose
    1. They are introduced, to keep utility methods related to the interface inside the interface itself.
2. Can interfaces have private methods? When were they introduced?
    1. Introduced in Java 9, To avoid duplicating code among default methods.

    ```java
    default void logInfo() {
       // duplicate code
    }
    
    default void logError() {
       // duplicate code
    }
    ```
3. Diamond problem with interfaces
    1. Occurs when two interfaces provide the same default method. Compilation fails with error **Duplicate default methods inherited.** Java forces the application to resolve the problem by overriding the method.
    2. Diamond problem does not occur with the abstract methods only
4. Difference between default method in interface and concrete method in abstract class?

    | Default Method in Interface                              | Concrete Method in Abstract class              |
    |----------------------------------------------------------|------------------------------------------------|
    | For backward compatability                               | For code reuse                                 |
    | Cannot have a state and cannot access instance variables | Can have a state and access instance variables |
5. Which method gets called if both a superclass and an interface define the same method?
    1. **Class wins over Interface:** the parent class method is called.
    2. **Why does the class win?** This preserves backward compatibility. If an interface method is later changed to a default method, existing classes should continue using their own implementation. Otherwise, behavior could change unexpectedly when the default method is introduced.
6. How is dynamic method dispatch involved with interfaces?
    1. For example, `Animal a = new Dog(); a.sound();` at compile time `Animal` and run time `Dog`. At runtime Dog's implementation executes. This is dynamic dispatch. Runtime decides the actual implementation.
7. Where are interface methods stored at runtime?
    1. Stored in method area(Metaspace). `Animal.class` contains metadata about the method and `Dog.class` contains implementation.
8. Are interface variables truly constants? Why?
    1. yes they are constants. `int x = 10'` Compiler converts it to `public static final int x=10;`
    2. Interfaces are meant to define behavior, not object state. Since interfaces cannot be instantiated and have no constructors, their fields belong to the interface itself (`static`) and are immutable constants (`final`) to maintain a consistent contract across all implementations.
9. Default access specifiers for an interface, variables and methods in it.
    1. Default access specifier is `public` for interface and other supported specifier is `default`
    2. Variables are `public static final`
    3. Methods are `public abstract`
10. Can methods of interface declared final?
    1. No, interface is used as contract that the declared abstract method must be implemented, but `final` prevents method overriding. Hence, they contradict and a compiler error rises.
11. Functional interfaces and lambda expressions
    1. A functional interface contains exactly one abstract method. Lambda expressions reduces the boilerplate code.
12. Can We Override Static Methods in Interfaces?
    1. No static methods belong to interface and does not define any contract
13. Why Does Spring Prefer Interfaces?
    1. Loose coupling, Easy swapping of implementations, Easier testing/mocking, Dependency Injection, Proxy creation (AOP)

## References

https://jenkov.com/tutorials/java/interfaces.html

https://jenkov.com/tutorials/java/interfaces-vs-abstract-classes.html