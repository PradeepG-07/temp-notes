## Optionals
Optional is introduced to handle null checks and `NullPointerException`.
Mainly optional can be used as return type of methods which can possibly return a null value.
Optional acts as a wrapper around the actual value that we want to use.
There are two types of optional
1. `Optional<T>`: For the collections
2. `OptionalInt, OptionalLong, OptionalDouble`: For the primitive types
# Creating Optional
## 1. of()
Any type of argument can be passed to this method which returns a `Optional<T>`.
**Null** is not accepted, if null is passed it throws `NullPointerException`
## 2. ofNullable()
Same as `of()` but accepts null and return *empty optional* if the argument is null.
## 3. empty()
When we want to return null from any method we can simply return a empty optional.
# Methods
1. **isPresent()**: Returns a false if the optional is empty, else returns true. 
2. **isEmpty()**: Returns a true if the optional is empty, else returns false. 
3. **get()**: Returns the actual value if optional is not empty otherwise throws `NoSuchElementException`. So it is always safe to guard the `get()` with `isPresent` method.
4. **ifPresent(Consumer)**: Takes a consumer and executes the consumer method
5. **ifPresentOrElse(Consumer,Runnable)**: Executes the consumer method if the optional is not empty otherwise executes the runnable method (takes nothing returns nothing).
6. **orElse(T ele)**: Returns the value of the optional if optional is not empty otherwise returns the ele.
7. **orElseGet(Supplier)**: Returns the value of optional if optional is not empty otherwise returns the value returned by the supplier.
8. **orElseThrow()**: Returns the value if present; otherwise, it throws a NoSuchElementException. There is also an overloaded version that accepts a Supplier of an exception, allowing you to throw a custom exception that extends Throwable when the Optional is empty.
9. **map(Function)**: if the optional is empty it returns a empty optional, otherwise wraps the return value of Function in optional and return it.
10. **flatMap(Function)**: if the optional is nested then flatmap helps to flatten it.
11. **filter(Predicate)**: If optional is empty returns empty optional, otherwise returns the same optional.
#### Different Methods of Optional for Primitives
1. **getAsInt(), getAsDouble(), getAsLong()**: These methods does the same work as `get()` and `get()` is not available for optional which handles primitives.