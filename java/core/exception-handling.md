## Basic try/catch/finally Handling

1. Exceptions are used in a program to signal that some error or exceptional situation has occurred, and that it doesn't make sense to continue the program flow until the exception has been handled.
2. Exception are propagated up the call stack, from the method that initially throws it, until a method in the call stack catches it or the program crashes.
3. If a method needs to throw exception it should declare the exceptions in the method signature and use `throw` keyword in the function definition.
4. Execution stops right after the `throw` statement is executed.
5. Any builtin exception or custom exception can be thrown if it is declared in the signature.
6. If a method declares that it throws an exception A, then it is also legal to throw subclasses of A.
7. If a method calls any other method which can throw a checked exception, the caller should handle the exception or pass the exception upto the call stack.
8. Wrap the set of instructions which can throw exception in a try block and add single/multiple catch blocks to handle the exception if thrown, add finally block as a cleanup process.
9. `try` should always be followed by `catch` or `finally` or both.

## Try with Resources

1. Java try-with-resources is an exception handling mechanism that can automatically close resources like a java `InputStream` or a JDBC Connection, irrespective of the exception is thrown or not. Added in **Java 7.**
2. finally or catch are not mandatory with try-with-resource block.
3. Example

    ```java
    private static void printFile() throws IOException {
        try(FileInputStream input = new FileInputStream("file.txt")) {
            int data = input.read();
            while(data != -1){
                System.out.print((char) data);
                data = input.read();
            }
        }
    }
    ```

4. When the `try` block finishes, the `FileInputStream` will be closed automatically. This is possible because `FileInputStream` implements the Java interface `AutoCloseable`.
5. Any class that implements the `Autocloseable` interface can be used in try-with-resources.
6. From **Java 9,** syntax has been changed, variables can be declared and assigned outside, and can be tagged to try block. In this case the tagged variable should be **final or effectively final**.

    ```java
    private static void printFile() throws IOException {
        FileInputStream input = new FileInputStream("file.txt");
        try(input) {
            int data = input.read();
            // do something
        }
    }
    ```

7. **Multiple input streams** can be declared in try block and the **closing order** will be from **last to first.**
8. When exception is thrown in try-with-resource block the resources still tried to be closed and the exception is propagated up the call stack.
9. If there are multiple exceptions thrown while closing the resources, the first exception which is raised while closing will be propagated up the call stack and remaining exceptions will be suppressed, all the suppressed exceptions can be retrieved by `e.getSuppressed();` method which return `Throwable` array.
10. Exception inside try-with-resource > First exception thrown by resource.
11. Additionally, a catch block can be added to try-with-resource block, this catch block works just like try-catch, but before the catch block is entered the try-with-resources will attempt to close all the opened resources.
12. Additionally, a `finally` block can be added at the last, which works just like the try-catch-finally block. If any is exception thrown from the finally block, all the existing exceptions will be ignored.

## Multiple catch blocks

1. Before Java 7

    ```java
    try {
    
        // execute code that may throw 1 of the 3 exceptions below.
    
    } catch(SQLException e) {
        logger.log(e);
    
    } catch(IOException e) {
        logger.log(e);
    
    } catch(Exception e) {
        logger.severe(e);
    }
    ```

2. From Java 7

    ```java
    try {
    
        // execute code that may throw 1 of the 3 exceptions below.
    
    } catch(SQLException | IOException e) {
        logger.log(e);
    
    } catch(Exception e) {
        logger.severe(e);
    }
    ```


## Exception Hierarchies

1. A hierarchy is created when an exception class is extended by the other classes.
2. For a single try block there can be multiple catch blocks where different type of exceptions can be handled, if needed hierarchy wise as well.
3. When exceptions are being declared in the function signature it is allowed to specify both super class and subclass exceptions if they can be thrown by that function. Even though if subclasses are not specified, specifying parent class also works, but it is good to specify for better understanding of other developers.
4. Even the function signature does not declare the subclass exception, the caller of this function can still be able to handle the subclass exception separately.

## Types of Exceptions

There are two types of exceptions namely **checked exceptions, unchecked exceptions** these are also called as **Compile Time Exceptions, Runtime Exceptions.**

### 1. Checked Exception

- Checked exception is an exception that is checked by the compiler at compile time. The programmer must either handle it using `try-catch` or declare it using the `throws` keyword.
- These exceptions usually represent recoverable conditions such as:
    - File handling errors
    - Database connection issues
    - Network failures
- Examples: `IOException`

### 2. UnChecked Exception

- Unchecked exception is an exception that is not checked by the compiler at compile time. These exceptions mainly occur due to programmer logical mistakes during the execution of the program.
- Unchecked exceptions are basically subclasses of `RuntimeException`
- Examples: `ArithemeticException` `ArrayIndexOutOfBoundsException` `NullPointerException`

**Note:** For a project, choosing unchecked exceptions or checked exceptions is an argument and mainly decided by application owner.

Because using checked exception reduces the code readability by adding multiple try catch blocks and exception method declarations.

If unchecked exceptions are used developers might forget handling the unchecked exception as compiler does not warn about the handling of runtime exceptions.

## Exception Wrapping

1. It is wrapping an exception with another new exception and throwing that newly created exception.
2. Exception wrapping is to prevent the code up the call stack to know and handle all the exceptions thrown in the system.
3. Let's say there is a DAO, and currently it throws the SQLException which is handled by the service layer. But in future it may implement some other interfaces for DB and can throw RemoteException or FileNotFoundException which needs to be handled by the Service layer again.
4. So to avoid this we better throw a single DAOException and wrap the raised exception with DAOException.
5. All the classes which extend **Exception** class will take cause as second parameter. Also by default the cause is also logged by JVM. To get the cause we can use `getCause`  method explicitly.
6. Example

    ```java
     try{
         dao.readPerson();
      } catch (SQLException sqlException) {
         throw new MyException("error text", sqlException);
      }
    ```


## Pluggable Exception Handlers

1. Pluggable exception handlers can be used to plug a type of exception handler based on the use case.
2. There might be a situation where all the exceptions thrown needs to collected or add some more information to the exception and rethrow it or just ignore the exception silently.
3. In these cases we can create different type of exception handlers and plug them as objects via DI and handle accordingly.

## On What Level Exceptions should be Logged?

1. It is good to log exceptions at the top level meaning up of the call stack because if we log the exceptions at bottom or mid-level we may have to write the logging statement everywhere and any change in logging framework will take a lot of time.
2. In the bottom or mid-levels we might not have enough context to log the exception meaning the user details, what is the root of the exception path, exact meaningful cause for the exception.
3. Hence, it is preferred to log exceptions at the top level and if any enrichments can be done in the bottom and mid-levels meaning any additional information can be added to the exception before throwing it to the upper level of call stack.

**Note:** Throw the exceptions only when the data which caused exception cannot be corrected meaning if something can be addressed to avoid exception such as user submitting invalid form and can resubmit with correct data. In these cases try to validate the data without exceptions and send back the errors requesting the form resubmission.

Logging for a multithreaded environment with stack tree is [here](https://jenkov.com/tutorials/exception-handling-strategies/execution-context.html).