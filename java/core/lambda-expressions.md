## Lambda Expressions
1. Introduced in Java 8, A Java lambda expression is thus a function which can be created without belonging to any class.
2. Java lambda expressions can only be used with the single method interface (functional interface).
3. Interfaces with default and static methods are not considered until the interface has only one abstract method.
4. Coming to difference between the lambda expressions and anonymous classes is that anonymous classes can have its own instance variables. A lambda expression cannot have such fields. A lambda expression is thus said to be stateless.
5. In anonymous classes `this` refers to anonymous class object, in lambda `this` refers to enclosing object.

### Lambda Declaration

    ```java
    // No Param Usage
    NoParamInterface greet1 = () -> System.out.println("Hello");
    
    // Single Param Usage
    MySingleParamInterface greet2 = (String msg) -> {
        System.out.println(msg);
    };
    MySingleParamInterface greet3 = (String msg) -> System.out.println(msg);
    MySingleParamInterface greet4 = msg -> System.out.println(msg);
    
    // Multi Param Usage
    MultipleParamInterface greet5 = (msg, name) -> {
        System.out.println("Hello "+name+" "+msg);
    };
    MultipleParamInterface greet6 = (msg, name) -> System.out.println("Hello "+name+" "+msg);
    ```

### Variable captures
1. **Local variable capture:** The variables which are final or effectively final can be used in the lambda function.
2. **Instance variable capture:** Instance variables can be captured in lambda with help of `this` .
3. **Static variable capture:** Static variables can be used directly just like other variables.
4. **Note:** Instance and static variables need not be final or effectively final.

### Method references
1. In the case where all your lambda expression does is to call another method with the parameters passed to the lambda,
2. The Java lambda implementation provides a shorter way to express the method call which is method reference.
3. This is a concept where we directly provide the method definition as lambda expression
4. The following types of methods:
    1. Static method
    2. Instance method on parameter objects
    3. Instance method
    4. Constructor
5. Method reference will be done with help of `::` operator.

```java
package methodreferences;

interface Operation<T, R> {
    R perform(T value);
}

class Calculator {

    // Static Method
    public static Integer square(Integer num) {
        return num * num;
    }

    // Instance Method
    public String toLower(String text) {
        return text.toLowerCase();
    }
}

class Student {
    private String name;

    // Constructor
    public Student(String name) {
        this.name = name;
    }

    public void display() {
        System.out.println("Student Name: " + name);
    }
}

public class MethodReferenceDemo {

    public static void main(String[] args) {

        /*
         * 1. Reference to a Static Method
         * Syntax:
         * ClassName::staticMethod
         */
        Operation<Integer, Integer> staticRef = Calculator::square;

        System.out.println("Square: " + staticRef.perform(5));

        /*
         * 2. Reference to an Instance Method
         * of a Particular Object
         * Syntax:
         * objectReference::instanceMethod
         */
        Calculator calculator = new Calculator();

        Operation<String, String> instanceRef = calculator::toLower;

        System.out.println("Lowercase: " + instanceRef.perform("HELLO"));

        /*
         * 3. Reference to an Instance Method
         * of an Arbitrary Object of a Particular Type
         * Syntax:
         * ClassName::instanceMethod
         */
        Operation<String, String> arbitraryRef = String::trim;

        System.out.println("Trimmed: '" +
                arbitraryRef.perform("   Java   ") + "'");

        /*
         * 4. Reference to a Constructor
         * Syntax:
         * ClassName::new
         */
        Operation<String, Student> constructorRef = Student::new;

        Student student = constructorRef.perform("Pradeep");

        student.display();
    }
}
```