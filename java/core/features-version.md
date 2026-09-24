## Java 8 Features (FOLDS)
1. [Functional Interfaces and Method References](./functional-interface.md)
2. [Optionals](./optional.md)
3. [Lambda Expressions](./lambda-expressions.md)
4. [Default and static methods in interfaces](./interfaces.md)
5. [Streams](./streams.md)
6. And other changes include Data/Time Api etc.  

## Java 9 Features
1. Module System (JPMS) - Introduced modular applications with strong encapsulation and dependency management.
2. Collection Factory Methods - Added List.of(), Set.of(), and Map.of() for immutable collections.
3. Private Methods in Interfaces - Allowed code reuse inside default and static interface methods.

## Java 10 Features
1. Local Variable Type Inference (var) - Compiler automatically infers local variable types.
2. Can only be used for:
   1. Local variables 
   2. Loop variables 
   3. Variables inside code blocks
3. Example
    ```java
    var count = 10;      // int
    var price = 99.99;   // double
    var text = "Hello";  // String
    ```

## Java 11 Features (LTS)
1. String API Enhancements - Added useful methods such as isBlank(), lines(), repeat(), and strip() for easier string processing.
2. HttpClient API - Introduced a modern, non-blocking HTTP client (java.net.http.HttpClient) that supports HTTP/1.1, HTTP/2, synchronous requests, asynchronous requests using CompletableFuture, WebSockets, and built-in request/response handling.

## Java 14 Features
1. Switch Expressions - Allows switch to return values and use cleaner arrow syntax, reducing boilerplate and eliminating accidental fall-through.

```java
static{
    // Before
    String type;
    switch(day) {
        case "MON":
        case "TUE":
            type = "Weekday";
            break;
        default:
            type = "Weekend";
    }

    // After
    String type = switch(day) {
        case "MON", "TUE" -> "Weekday";
        default -> "Weekend";
    };
}
```

## Java 15 Features
1. Text Blocks - Introduced multi-line string literals using triple quotes ("""), making JSON, SQL, XML, and HTML strings much more readable.
    ```java
    // Before:
    String json =
    "{\n" +
    "  \"name\": \"Pradeep\",\n" +
    "  \"age\": 25\n" +
    "}";
    // After:
    String json = """
    {
    "name": "Pradeep",
    "age": 25
    }
    """;
    ```

## Java 16 Features
1. [Records](./records-and-enums.md)

## Java 17 Features (LTS)

1. Sealed Classes - Restrict which classes or interfaces can extend or implement a type.

    ```java
        sealed class Shape permits Circle, Rectangle { }
        
        final class Circle extends Shape { }
        
        final class Rectangle extends Shape { }
    ```
    Only Circle and Rectangle can extend Shape.

    ```text
    sealed = controlled inheritance
    
    Java must know:
    1. Who can extend me?
        1. permits - Explicitly lists the classes/interfaces allowed to directly extend or implement the sealed type.
        2. Same-file inference - If all direct subclasses are declared in the same source file, the compiler automatically treats them as permitted and the permits clause can be omitted.
    2. What does each child choose? (final / sealed / non-sealed)
    ```

2. Pattern Matching for instanceof - Combines type checking and casting into a single operation.

    ```java
    static { // Ignore this static block
        
        // Before
        if (obj instanceof String) {
            String s = (String) obj;
            System.out.println(s.length());
        }
        
        // After
        if (obj instanceof String s) {
            System.out.println(s.length());
        }
    }
    ```

## Java 21 Features (LTS)

1. Virtual Threads - Lightweight threads designed for high-concurrency applications.

2. Pattern Matching for switch - Enables type-safe pattern matching directly in switch statements.
    ```java
    static {
        // Before:
        if (obj instanceof Integer) {
            System.out.println("Integer");
        } else if (obj instanceof String) {
            System.out.println("String");
        } else {
            System.out.println("Unknown");
        }
    
        // After:
        switch (obj) {
            case Integer i -> System.out.println("Integer");
            case String s  -> System.out.println("String");
            default        -> System.out.println("Unknown");
        }
    }
    ```

3. Record Patterns - Allows deconstructing records while performing pattern matching.
    ```java
    static {
        record Person(String name, int age) { }
        Person p = new Person("Pradeep", 25);
        
        // Without Record Patterns:
        if (p instanceof Person person) {
            String name = person.name();
            int age = person.age();
        }
        
        // With Record Patterns
        if (p instanceof Person(String name, int age)) {
            System.out.println(name + " " + age);
        }
    }
    ```
   
4. Structured Concurrency - Simplifies management of related concurrent tasks by treating them as a single unit of work.

    ```java
    static {
        // Before:
        Future<User> userFuture = executor.submit(this::getUser);
        Future<Order> orderFuture = executor.submit(this::getOrders);
        
        User user = userFuture.get();
        Order order = orderFuture.get();
        
        // After:
        
        try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
        
            var userTask = scope.fork(this::getUser);
            var orderTask = scope.fork(this::getOrders);
        
            scope.join();
            scope.throwIfFailed();
        
            User user = userTask.get();
            Order order = orderTask.get();
        }
    }
    ```
5. Sequenced Collections

If one task fails, the scope can automatically cancel the remaining tasks and manage them together.