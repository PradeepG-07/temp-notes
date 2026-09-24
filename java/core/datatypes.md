## Data Types

1. Data types enforces the type of data that a variable can store and how much memory a variable takes.
2. Classified into two types
    1. Primitives
    2. Non Primitives / References
3. From Java 10, `var` keyword supports local type inference by recognizing type from the assigned value.

### Primitive Data Types

1. boolean, byte, short, char, int, long, float, double are part of the primitives.

    | boolean | A binary value of either `true` or `false`           |
    |---------|------------------------------------------------------|
    | byte    | 8 bit signed value, values from -2^7 to (2^7) - 1    |
    | short   | 16 bit signed value, values from -(2^15) to (2^15)-1 |
    | char    | 16 bit Unicode character from 0 to 2^16              |
    | int     | 32 bit signed value, values from -2^31 to (2^31) - 1 |
    | long    | 64 bit signed value, values from -2^63 to (2^63) - 1 |
    | float   | 32 bit floating point value                          |
    | double  | 64 bit floating point value                          |
2. float, double follows the IEEE 754 format. For float: 1 sign bit, 8 exponent bits, 23 fraction bits, For double: 1 sign bit, 11 exponent bits, 52 fraction bits

### Non-Primitive / References

1. Non-primitive types store references to objects.
2. Examples include:
    1. Boolean, Byte, Short, Character, Integer, Long, Float, Double, String, Classes, Arrays, Interfaces, Enums.
3. Every primitive type has a corresponding wrapper class.
4. These are immutable and can be cached by compiler. Default cache range of Integer: `-128` to `127`.

    ```java
    Integer x = 10;
    x = x + 1; // creates a new object
    
    Integer a = 10;
    Integer b = 10;
    System.out.println(a == b); // true due to caching
    a = 200;
    b = 200;
    System.out.println(a == b) // false
    ```

5. Wrapper classes are commonly used in collections because Java collections can store only objects.

### Auto Boxing and Unboxing

1. **Before Java 5**, in order to convert primitive values to non primitives we have to create an object of that non-primitive class manually.

    ```java
     // Primitive -> Wrapper
     int x = 10; 
     Integer y = new Integer(x);
     
     // Wrapper -> Primitive
     Integer z = 10; 
     int a = z.intValue();
    ```

2. **From Java 10,** these conversions will be handled by the `javac` compiler itself.
3. So the newer syntax would be Integer

    ```java
    int x = 10;  
    Integer z = x; // auto boxing
    int k = z; // unboxing
    
    // Javac Compiler automatically converts the above code to below one
    int x = 10;  
    Integer z = Integer.valueOf(x); // auto boxing
    int k = x.intValue(); // unboxing
    ```

4. **Auto Boxing**: Automatic conversion of a primitive type into its corresponding wrapper object by the compiler.
5. **Unboxing**: Automatic conversion of a wrapper object  into its corresponding primitive type by the compiler.
6. Wrapper classes can be null, so unboxing might throw `NullPointerException`
