## Class

1. A class is a blueprint or template used to create objects with common properties and behaviors.
2. Class usually contains **fields, methods, constructors and nested classes.** No destructors are available as the garbage collection is automatic in java.
3. **Field**: It is a variable defined in a class scope which can be a primitive or non-primitive.
    1. **Declaration**: `[<access_specifier>] [static] [final] type var_name [=initial_value];` where [] are optional.
    2. <access_specifier>: Defines scope of the field. Available access specifiers are **public, private, default, protected.**
    3. static: It is a keyword which makes the field to be class field not an object field.
    4. final: This makes the field a constant restricting the value change of that field.
    5. type: This will be an either primitive or non-primitive type
    6. var_name: This will be the field name / identifier through which we can access the value.
    7. initial_value: The field will be initialized the given initial_value.
4. **Methods:** Methods are block of java instructions which does some operation and may or may not return the result.
    1. Methods can be used to break down code into smaller, more comprehensible and reusable segments of code, rather than writing your program as one, big method.
    2. **Declaration:** `[<access_specifier] [static / final] return_type fn_name(params) [throws Exception]{}`
    3. **static/final**: If static is used, the method will become a class method and cannot be accessed from an object. If final is used we cannot override the method in subclasses.
    4. **Note:** Both static and final cannot be used at once because static methods are already hidden, meaning at compile time itself we know which method to execute based on the type of the variable we are storing.
    5. **return_type:** Return type tells which type of data will be returned when the method is executed.
    6. **params:** Params are the variables local to the function which can be used to pass the data from the caller. Declaration: fn(int x), fn(final int x, int y)
    7. Example

        ```java
        public class Main{
        	 // instance/object method
        	 public int addTwo(int num){
        		 return num+2;
        	 }
        	 // class method
        	 public static int addThree(int num){
        		 return num+3;
        	 }
        	 // final method -> cannot be overridden by subclasses
        	 public final int addFour(int num){
        		 return num+4;
        	 }
        	 // Cannot change the value of the `num` parameter
        	 public int addFive(final int num){
        		 num+=5;// throws an error
        		 return num+5;
        	 }
        	 // Throws execution
        	 public int divide(int divisor, int dividend) throws Exception {
        		 if(divisor == 0) throw Exception("Divisor can't be zero");
        		 return dividend/divisor;
        	 }
        	 public static void main(String args[]){
        		 Main m = new Main();
        		 System.out.println(m.addTwo(2)); // 4
        		 System.out.println(Main.addThree(2)); // 5
        	 }
        }
        ```

5. **Constructors**: These are mainly used to initialize values of an object of a class.
    1. Constructor is a special method without any return type which will be called on object instantiation. There are two types of constructors
        1. Default Constructor
        2. Parameterized Constructor
    2. **Default Constructor:** By default javac compiler adds the default constructor into bytecode, if no constructor is specified.
    3. **Parameterized Constructor:** Parameterized constructor will be mainly used to initialize the object with some specific values that can be passed as arguments to the constructor.
    4. Having multiple constructors with different parameters often called as **Constructor Overloading.**
    5. Declaration:

        ```java
        public class Main{
        	int val;
        	// Default constructor
        	public Main(){
        	}
        	// Parameterized constructor
        	public Main(int val){
        		val = val; // => Bug, variable shadowing happened and Main.val is 
        		// never initialized
        	}
        	// Parameterized constructor
        	public Main(int val){
        		this.val = val;
        	}
        }
        ```

    6. **Note:** When a parameterized constructor is created, we should manually add the default constructor as well.
    7. **Calling Constructors**
        1. The `this` keyword followed by parentheses and parameters means that another constructor in the same Java class.
        2. When `super` keyword is followed by parentheses like it is here, it refers to a constructor in the superclass.
            1. All Constructors of the subclass should call at least one of the super class constructor.
            2. If Default constructor exists in Super Class, then automatically default constructor will be called, else we need to call the specific constructor of the parent.
            3. Parent constructor call should be the first line in the constructor.