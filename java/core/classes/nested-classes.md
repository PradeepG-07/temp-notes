
## Nested Class
We can declare classes inside a class and a method as well.

1. Outer class is called the Enclosing class.
2. Nested classes are treated like members of the enclosing class hence all the access modifiers can be used to define scope of the nested class.
3. Nested classes inside the enclosing class can be of two types
    1. Static Nested Class
    2. Non-Static Nested Class
4. **Static Nested Class:** Static Nested class can access the members of enclosing class directly with variable name (if not shadowed) or indirectly by calling Enclosing class.
5. **Non-Static Nested Class:** Nested classes can access members from the enclosing class directly with variable name or indirectly by calling Enclosing class this.

### Nested Class Example

```java
package nested_classes;
public class NestedClassExample {
    public static int staticVariable;
    public int variable;
    public String shadow = "Parent";
    public static String staticShadow = "Parent";

    // Static Nested class
    static class StaticNestedClass{
        String shadow = "nested";
        static String staticShadow = "nested";
        void print(){
            System.out.println(staticVariable);
            System.out.println(shadow);
            System.out.println(staticShadow);
            System.out.println(NestedClassExample.staticShadow);
        }
    }
    // Non-static Nested class
    class NestedClass{
        String shadow = "nested";
        void print(){
            System.out.println(variable);
            System.out.println(shadow);
            System.out.println(NestedClassExample.this.shadow);
        }
    }
    public static void main(String args[]){
	    // Static Nested Class
	    NestedClassExample.StaticNestedClass staticNestedClass = 
	    new NestedClassExample.StaticNestedClass();
	    
	    // Non-static nested class
	    NestedClassExample nestedExample = new NestedClassExample();
	    NestedClassExample.NestedClass nestedClass = 
	    nestedExample.new NestedClass();
    }
}

```

## Local Class

1. When a class is defined inside a method definition then that class is called as Local Class.
2. Local classes can be accessed only inside the defined method or block.
3. The local variables which are **final or effectually final** (meaning variable value is not changed throughout the method or the block where the nested class is declared) of the method and the fields of the enclosing class can be accessed.
4. Local classes cannot be declared as static.
5. Same concept of variable shadowing of Nested classes is applicable here as well.

### Local Class Example

```java
package nested_classes;
public class LocalClassExample {
    private String name = "OuterClass";
    void print(){
        int methodVal = 10;
        class InnerClass {
            void print(){
                String name = "Inner Class";
                System.out.println(name); // Shadowed Variable
                System.out.println(methodVal); // Accessible as methodVal is final or effectively final
                System.out.println(LocalClassExample.this.name); // Access outer class variable
            }
        }
        // Local Classes cannot be declared static
        // Invalid => Even the enclosing method is static also
        // static class AnotherInnerClass{
        //
        //}
    }
}

```