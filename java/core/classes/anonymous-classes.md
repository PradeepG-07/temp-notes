## Anonymous Class

1. Anonymous classes in Java are nested classes without a class name and are typically declared as either subclasses of an existing class, or as implementations of some interface.
2. Anonymous Classes are defined when they are instantiated.
3. An anonymous class can access members of the enclosing class. It can also access local variables which are declared final or effectively final (since Java 8).
4. You can declare fields, methods, static initializers inside an anonymous class, but you cannot declare a constructor.
5. The same shadowing rules apply to anonymous classes as to inner classes.

### Anonymous Class Example

```java
// SuperClass.java
public class SuperClass {
  public void doIt() {
    System.out.println("SuperClass doIt()");
  }
}

// Main.java
// Example of anonymous class extending the SuperClass
SuperClass instance = new SuperClass() {
    public void doIt() {
        System.out.println("Anonymous class doIt()");
    }
};
instance.doIt(); // Anonymous class doIt()

// MyInterface.java
public interface MyInterface {
  public void doIt();
}

// Main.java
// Example of anonymous class implementing interface
MyInterface instance = new MyInterface() {
    public void doIt() {
        System.out.println("Anonymous class doIt()");
    }
};

instance.doIt();

```