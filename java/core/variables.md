## Variables

1. A Java variable is a piece of memory that can contain a **data value** or a **reference** based on the data type of the variable.

    ```java
    datatype variableName; // template
    int x; // declaration
    x = 10; // assignment
    int y = 10; // declaration + assignment
    ```

2. Naming Conventions
    1. Enforced Conventions:
        1. Names are case sensitive.
        2. Name must start with a letter or $ or _.
        3. Name cannot be a keyword.
        4. Name should only contain numbers, letters, $, _
    2. Developer Conventions:
        1. Names should be in camel case if multiple words exists. Ex: x = 10, carCount = 10
        2. Static Final fields should be all captials separated by _. Ex: MAX_SIZE = 10
3. During compilation, information about variables (names, types, scope, etc.) is maintained in the compiler’s symbol table.

## Variable Scope

1. Scope of the variable will be either class scope, method scope, loop scope, bracket scope.
2. **Variable Shadowing:** It happens when a variable is declared with same name but with smaller inner scope compared to the variable in the outer scope.

```java
public class Main{
    private int x = 120; // Class scope: cannot be outside the class
    public static void main(String args[]){
        int x = 10; // variable shadowing
        
        int z = 10; // Method scope: available only in this method
        
        for(int i=0;i<10;i++) System.out.println(i); // Loop Scope: i is accessible only inside loop 
        System.out.println(i); // error
        
        {
            int val = 10; // Bracket Scope: Only accessible inside the brackets
        }
        System.out.println(val); // error
    }
    System.out.println(z); // error 
}
```