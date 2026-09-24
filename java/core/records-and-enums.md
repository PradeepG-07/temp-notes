## Records - From Java 14

1. A Java Record is a special type of class used to store **immutable data (meaning field value cannot be changed)** with less boilerplate code.
2. A Java record is declared with the `record` keyword and the type definition of the record is **final** meaning **subclasses cannot extend it.**
3. A Record does not have **field definitions (meaning:** no need to declare the fields explicitly) and solely **relies on the constructor parameters**.
4. **Constructors**
    1. Records can have multiple constructors but all the constructors needs to call the main constructor inorder to specify what are the fields that the explicit constructor is handling.
5. **Methods**
    1. Instance and static methods are allowed just like the class.
6. Javac compiler generates the **getters, (no setters:** because data is immutable) **hashCode, equals, toString methods** implicitly.
7. **Note:** Getters will be generated with the field name. Check below example.
- Example

    ```java
    package enums_and_records;
    public record CarRecord(String color, int tyres, String company) {
        public CarRecord(String company){
            this("black", 4, company);
        }
        public static String companyWithColor(CarRecord carRecord){
            return carRecord.color() + " " + carRecord.company();
        }
    }
    ```


## Enums

1. Introduced in Java 5, Java Enum is a special type used to define collection / set of constants.
    - Simple Example

        ```java
        public enum StatusCodes{
        	OK, // constants
        	NOT_FOUND
        }
        ```

2. A Java enum can have constructors, fields and methods(static, non-static, abstract) as well.
3. Some restrictions are the constructor should always be private, if access specifier is not specified it will default to private.
4. Constants declaration should end with `;` if there are constructors or fields or methods have to be declared.
5. All the constants should implement the abstract methods of the enum.
6. An enum can implement any interface as well.
7. Java compiler will add the following default methods
    1. **toString():** Prints the enum
    2. **valueOf():** Used to obtain an instance of enum from any string value
    3. **values():** For iterating over the enum constants
8. If a Java enum contains fields and methods, the definition of fields and methods must always come *after* the list of constants in the enum. 
- Implementations
```java
package enums_and_records;

public enum StatusCodes implements IStatusCodes{
    // Constant Declaration Start
    OK(200){
        // implemented abstract method
        void print(){
            System.out.println("OK");
        }
    },
    BAD_REQUEST(400){
        void print(){
            System.out.println("BAD_REQUEST");
        }
    },
    UNAUTHORIZED(401){
        void print(){
            System.out.println("UNAUTHORIZED");
        }
    },
    FORBIDDEN(403){
        void print(){
            System.out.println("FORBIDDEN");
        }
    },
    NOT_FOUND(404){
        void print(){
            System.out.println("NOT_FOUND");
        }
    }
    ; // mandatory
    // Constant Declaration End

    private final int statusCode;
    StatusCodes(int statusCode){
        this.statusCode = statusCode;
    }

    public int getStatusCode() {
        return statusCode;
    }

    abstract void print();

    static int  getStatusCode(StatusCodes statusCode) {
        switch (statusCode) {
            case OK: return 200;
            case BAD_REQUEST: return 400;
            case UNAUTHORIZED: return 401;
            case FORBIDDEN: return 403;
            case NOT_FOUND: return 404;
            default: return -1;
        }
    }
}

```
## EnumSet and EnumMap

1. Java contains a special **Java Set** implementation called `EnumSet` which can hold enums more efficiently than the standard Java Set implementations
    1. `EnumSet<Level> enumSet = EnumSet.of(Level.HIGH, Level.MEDIUM);`
2. Java also contains a special **Java Map** implementation which can use Java enum instances as keys.

    ```java
    EnumMap<Level, String> enumMap = new EnumMap<Level, String>(Level.class);
    enumMap.put(Level.HIGH  , "High level");
    enumMap.put(Level.MEDIUM, "Medium level");
    enumMap.put(Level.LOW   , "Low level");
    
    String levelValue = enumMap.get(Level.HIGH);
    ```