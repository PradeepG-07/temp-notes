# Access Modifiers

1. Access modifiers control the visibility of the classes, field, methods, constructors.
2. There are four type of access modifiers. They are **private, default, protected, public**

## private modifier

1. `private` modifier can be used on fields, methods and constructors.
2. How private modifier affects each part of a class is as follows:
    1. **Fields:** It cannot be accessed outside the class
    2. **Methods:** Cannot be accessed outside the class
    3. **Constructors:** Cannot be accessed outside but can be accessed from inside for creating an object. Mostly used in singleton pattern.
    4. **Class:** Cannot be used because if we make it private no other class cannot access this class, and there is no use of writing the class itself.

## default modifier

1. `default` modifier can be used on class, fields, methods and constructors.
2. How default modifier affects each part of a class is as follows:
    1. **Fields:** It cannot be accessed by any class, outside the source class package
    2. **Methods:** Cannot be accessed by any class, outside the source class package
    3. **Constructors:** Cannot be accessed by any class, outside the source class package
    4. **Class:** Cannot be accessed by any class outside the source class package

## protected modifier

1. `protected` modifier can be used on fields, methods and constructors.
2. How protected modifier affects each part of a class is as follows:
    1. **Fields:** It cannot be accessed by any class, outside the source class package except the child classes.
    2. **Methods:** Cannot be accessed by any class, outside the source class package except the child classes.
    3. **Constructors:** Cannot be accessed by any class, outside the source class package except the child classes.
    4. **Class:** Cannot be used because if we make it protected also, it will still work as default.

## public modifier

1. `public` modifier can be used on class, fields, methods and constructors.
2. All the parts which are marked as public can be accessed from anywhere in the application.
3. But the public modifier of fields, methods and constructors bound to the class access modifier.
4. Example:

    ```java
    package a;
    class A{
    	public A(){}
    	public int a(){ System.out.println("a");
    }
    
    package b;
    class B{
    	public int a(){
    		A a = new A(); // error as class has default modifier 
    		a.a(); 
    	}
    }
    ```
## **Access Modifiers and Inheritance**

When overriding a method, a subclass cannot reduce the accessibility of the parent method.

The overridden method must have the same or broader access modifier.

For example, a `protected` method can become `public`, but a `public` method cannot become `protected` or `private`.