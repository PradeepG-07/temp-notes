# Java Memory Management Model

When a Java program is executed using `java ClassName`, internally a jvm instance is created and memory is allocated for this java process.

![Java Memory Management Model](https://deen3evddmddt.cloudfront.net/uploads/content-images/architecture-of-java-virtual-machine%20(2).webp)
JVM divides the allocated memory into several parts. They are

**JVM Runtime Data Areas:**
1. Heap Memory
2. Method Area
3. Java Stack
4. Program Counter (PC) Register
5. Native Method Stack

**Additional JVM Area:**
1. Code Cache (stores JIT-compiled native code)

### Why is it necessary to divide the memory ?
A program can contain kind of variables such as *short-lived, long-lived or which can stay till program ends*. 
If all these variables are stored in the same memory it would be difficult to perform garbage collection. Because GC need to check all the items whether they are eligible for garbage collection which is a overhead.
So in order to avoid this, memory is divided into several parts.

## 1. Heap Memory
Heap memory size will be larger compared to others, it stores *objects, non-primitive values, instance variables*.
Example: Objects, Strings, Arrays, etc.
Heap memory is again divided into several parts and explained clearly [here](5-garbage-collection.md).

## 2. Method Area
Method area is to store the metadata of the program which includes the following
1. Class structure: Class name, package, Super class, interfaces
2. Field metadata: Field names, types, modifiers (static, final, etc.)
3. Method metadata: Method names, return types, parameters, Byte code of methods,
4. Exception tables
5. Runtime Constant Pool (per class)
6. Symbolic references (to methods, fields, classes)
7. Literals (like "hello", numbers) as references
8. Annotations metadata
9. `ClassLoader`-related data
10. Internal JVM structures for reflection & linking
    1. Method area is specification of JVM saying that there should be some memory which stores the metadata of the program. 
    2. Before Java 8, Hot spot compiler consider the method area as `PermGen` which is part of Heap memory. 
    3. From Java 8, the implementation of method area is changed, and **it is called as MetaSpace**, and it is not part of heap anymore.
    
## 3. Stack Memory
1. Stack memory follows LIFO principle.
2. It stores *local variables, object references, parameters of method, return address* in each frame.
3. Each method call pushes a new frame and each return will pop a frame from the stack.
4. If too many frames are pushed it results in `StackOverFlowError`.

## 4. Program Counter
In Java the PC Register contains the address (bytecode index) of the instruction currently being executed by a thread.
When a method is called, a new stack frame is pushed onto the stack, storing the return address (the next instruction to execute after the method returns). When the method completes, its stack frame is popped, and the PC register is updated with the return address to resume execution.
[PC example](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&layers=1&nav=1&title=Java%20Memory%20Management&page-id=VMfw85cEeY_ZlDrgVa6p&dark=1#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D17HGxatNJ07rkeM3uvzlnEd8Ct_O7ECgq%26export%3Ddownload)

## 5. Native Method Stack
The Native Method Stack is used for the execution of native methods written in languages like C or C++ and invoked through JNI.

## 6. Code Cache
When the HotSpot JIT compiler compiles bytecode into machine code, the generated machine code is stored in the JVM's Code Cache and executed directly by the CPU. This is done for the hot methods (meaning called many times).