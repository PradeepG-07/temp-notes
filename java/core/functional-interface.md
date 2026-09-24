# Functional Interface
1. Functional Interface is an interface which is declared with `@FunctionalInterface` annotation and exactly has one abstract method.
2. Declaring the interface with `@FunctionalInterface` is optional.
3. Now with `@FunctionalInterface` introduced which restricts the interface to have only one abstract method gives support for the ***Lambda functions and method references***. 
4. Main reason to introduce this  is to provide a type for lambda expressions and reduce the boilerplate of creating implementation classes or anonymous inner classes.

Some of the default and useful function interfaces are:
## Function
1. Function takes an argument of T, process it and returns back a result of R.
2. Abstract method is `apply`.
3. Multiple variations of the Function are below
### UnaryOperator
Single argument of type T and return result of same type T.
**ToIntFunction**
Single argument of type T and return result of `int`. Similarly we have `ToLongFunction`, `ToDoubleFunction`. Abstract methods are `applyAsInt`, `applyAsLong`, `applyAsDouble`.
## BiFunction
Two arguments of type T, V and return result of type R .
**BinaryOperator**
Two arguments of type T and return result of same type T.
## Predicate
1. Predicate takes an argument of T, process it and returns back a result of `boolean`.
2. Abstract method is `test`.
3. Multiple variations of the Predicate are below
### BooleanPredicate
One `boolean` argument and return type is `boolean`.
### DoublePredicate
One `double` argument and return type is `boolean`.
### LongPredicate
One `long` argument and return type is `boolean`.
## Consumer
1. Consumer takes an argument of type T and does not return any result.
2. Abstract method is `accept`.
3. Multiple variations of Consumer are:
### BooleanConsumer
Takes argument of type `boolean` and return `void`.
### DoubleConsumer
Takes argument of type `double` and return `void`.
### LongConsumer
Takes argument of type `long` and return `void`.
## Supplier
1. Supplier takes 0 arguments and returns a value of type T.
2. Abstract method is `get`.
3. Multiple variations of supplier are
### BooleanSupplier
Takes no argument and returns a value of type `boolean`.
 Abstract method is `getAsBoolean`.
### IntSupplier
Takes no argument and returns a value of type `int`.
 Abstract method is `getAsInt`.
### DoubleSupplier
Takes no argument and returns a value of type `double`.
Abstract method is `getAsDouble`.
### LongSupplier
Takes no argument and returns a value of type `long`.
Abstract method is `getAsLong`.