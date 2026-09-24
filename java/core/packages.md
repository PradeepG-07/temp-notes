## Packages

1. A directory which contains Java source files which are related to each other.
2. A package can have multiple packages recursively one inside others.
3. Accessing classes from packages can be done with help of a **fully qualified name.**
    1. Let's say a Java source file is in **src/com/pradeep/services/Auth.java**
    2. Fully Qualified name will be **com.pradeep.services.Auth**.
4. To use package, we have to do two steps
    1. Add all the source files into the package i.e. folder
    2. Each of the source file should start with declaring its package name
    3. Example: **src/com/pradeep/services/Auth.java** should have the first line of the file as **package com.pradeep.services;**
5. Importing classes
    1. If both classes are in same package, any class can be **directly used in any other class.**
    2. If both classes are from different packages we have import the class from any of the following approach below
        1. import java.util.ArrayList ⇒ Imports only ArrayList class
        2. import java.util.* ⇒ Imports all the classes available in java.util package
        3. java.util.ArrayList ar = new java.util.ArrayList();