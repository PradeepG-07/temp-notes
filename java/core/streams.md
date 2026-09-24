# Streams
Streams provide us a flexibility to convert imperative code to declarative code.
1. Imperative Code: We define the process of how to perform an action.
2. Declarative Code: We just say what action we have to perform.
## Hierarchy of Streams
[[stream-hierarchy.excalidraw]]
## Creating Streams
### 1. `of()` 
Creates stream from the arguments passed.
```java
Stream<Integer> stream = Stream.of(1,2,3);
IntStream intStream = IntStream.of(1,2,3);
LongStream longStream = LongStream.of(1,2,3);
DoubleStream doubleStream = DoubleStream.of(1,2,3);
```

### 2. `<collection>.stream()`
Creates a stream from a collection
```java
List<String> names = List.of("Pradeep", "Raju", "Ravi");
Stream<String> stream = names.stream();
```

### 3. `Arrays.stream(array)`
Creates a stream from a primitive or non-primitive array.
If the array is primitive array it will create either a `IntStream` or `LongStream` or `DoubleStream` otherwise `Stream<T>` , T stands for the Non-primitive type of the array.
```java
Stream<Integer> stream = Arrays.stream(new Integer[]{1,2,3});
IntStream intStream = Arrays.stream(new int[]{1,2,3});
```

### 4. Infinite streams
Infinite streams can be generated using two methods
#### i. iterate(seed, Function)
```java
Stream<Integer> stream = Stream.iterate(1, x->x+1)
        .limit(10)
        .forEach(System.out::println);
```
`iterate()` generates an infinite stream, `limit()` breaks the infiniteness and `forEach` acts a **Terminal Operation**.

#### ii. generate(Supplier)
```java
Stream<Double> stream = Stream.generate(Math::random)
        .limit(10)
        .forEach(System.out::println);
```

>**Note**: Difference between `iterate` and `generate` is the iterate method will depend on the previous value to produce the next value whereas generate does not depend on previous value.

## Working
Streams works in three phases
1. **Source**: From where the data is fetched.
2. **Intermediate Operations**: Operations that need to be performed on the stream
3. **Terminal Operations**: What to do after performing the intermediate operations

### Source
Source of the streams can be anything which is part of the **Creation** mentioned above.
### Intermediate Operations
Intermediate operations meaning what are the filters that needs to be added on the stream of data.
### Terminal Operations
Terminal operations basically are used to terminate the stream by either collecting the data, or printing it etc.

> Remember once the stream is consumed we can't consume the same stream again.

## Features
### 1. Lazy Loading
A stream will not be processed until there is a terminal operation specified.
### 2. Vertical Processing
Filters which are specified as part of the intermediate operations and terminal operations will be executed one after other for each of the element in stream.
```java
List<Integer> squaresGreaterThan10 = Stream.of(1,20,21,31,40)
                .filter(x->x>10)
                .map(x->x*x)
                .toList();
```
Consider the above example 
First the filter `x->x>10` then `x->x*x` and finally `toList` is applied on first element(1) and this continues for each of the element.

### 3. Short Circuiting
It means streams are smart enough when to terminate the processing even though there are element still left to process.
```java
Optional<Integer> firstGreaterThan10 = Stream.of(10,21,33,109)
        .filter(x -> x>10)
        .findFirst();
```
Considering the above example, when the element 21 is processed it will short circuit and stops processing the next elements as there is no use of processing them.

## Methods
### Intermediate Methods
1. **filter(Predicate)**: Used to filter the data
2. **map(Function)**: Takes an element of stream and returns a new element after processing.
3. **sorted()**: Sorts the elements, can be overloaded with a comparator 
4. **distinct()**: Make sure elements are distinct
5. **limit(long n)**: Limits the number of elements to n.
6. **skip(long n)**: Skips first n elements.
7. **peek(Consumer)**: Executes the consumer function(used for debugging purposes)
8. **flatMap**: Used to flatten nested streams into a single stream.

### Terminal Methods
1. **toList()**: Returns a unmodifiable list of elements after all the intermediate operations are performed.
2. **collect(Collector)**: Use `Collectors` class methods to collect as list or set or map.
3. **forEach(Consumer)**: Takes a consumer and executes the consumer method.
4. **min(), max()**: For Primitive Streams no comparator is needed and return value is `OptionalInt`  or `OptionalLong` or `OptionalDouble`. For Non-primitive streams comparator is needed and return value is `Optional<T>`
5. **count()**: Counts number of elements
6. **anyMatch(Predicate), allMatch(Predicate), noneMatch(Predicate)**: Returns boolean
7. **findFirst()**: Returns `OptinalInt` or `OptionalLong` or `OptionalDouble` for Primitives `Optional<T>` for Non Primitives.
>**Note**: The above methods can be either **stateful or stateless**. That means the current filter might require all the elements of previous layer to start its filter functionality. 
Example: sorted() filter starts only if all the elements are processed through previous filters.
This breaks the **Vertical Processing** feature.

## Collectors Methods
1. **toList(), toSet()**: Collects to modifiable list or set.
2. **toMap(Function keyMapper, Function valueMapper)**: Collects into a map and requires a key mapper and a value mapper. Also, it has an optional mapperFunction which is a BiFunction. Example: `duplicates.stream().collect(Collectors.toMap(num->num, num->1, (oldVal, newVal)-> oldVal+newVal));` results in frequency map of elements.
3. **joining(String delimiter)**: Joins all the values with delimiter.
4. **groupingBy(Function keyMapper)**: is a collector that groups stream elements according to a `keyMapper` function. It creates a map where each key represents a group, and the values are produced by a downstream collector. If no downstream collector is specified, `toList()` is used by default.
5. **partitioningBy(Predicate)**: Groups all the elements into two groups i.e. `true` and `false`  and collects each group elements into a downstream collector. Downstream collector is list by default and can be passed as next argument to collect result into specific collector.

### Downstream collectors
1. **counting()**: Counts number of elements in the stream. Returns a `long`.
2. **summingInt(ToIntFunction)**: Takes a lambda of argument of type T, where each stream element is passed as argument and lambda should return a `int`. All the returned values are summed up and returned as final result of type `Integer`. Similar functions exist for Double, Long as well.
3. **averagingInt(ToIntFunction)**: Similar to the `summingInt` but performs the average and returns a `Double`.
4. **mapping(Function<T,R>, Collector downstream)**: Takes a lambda of argument of type T, where each stream element is passed as argument and a downstream of collector. Main goal here is to map the current element to something and collect it in the provided collector and return it. 
5. **joining**: Joins the stream elements into a string. Other overloaded methods are `joining(String delimeter)` and `joining(String delimeter, String prefix, String suffix)`. Returns a `String`.
6. **maxBy(Comparator)**: Takes a comparator and returns a single max stream element according to the comparator passed. Returns an `Optional<T>`
7. **minBy(Comparator)**: Similar to `maxBy` but returns minimum stream element. Returns an  `Optional`.
8. **collectingAndThen(Collector downstream, Function finisher)**: After the downstream collector finishes, finisher function is applied on the collected result. [Example](https://stackoverflow.com/questions/71804304/how-to-use-collectors-collectingandthen-with-collectors-groupingby)
9. **summarizingInt(ToIntFunction mapper)**: Applies the mapper to get the integer value and returns the `IntSummaryStatistics`.
	1. Similarly `summarizingLong(ToLongFunction)` and `summarizingDouble(ToDoubleFunction)` exists to work with Long and Double types.

## Conversions
### 1. From one Primitive to another Primitive
**mapToInt(), mapToLong(), mapToDouble()** are used for converting a stream to `IntStream`, `LongStream` and `DoubleStream` respectively.
### 2. From Primitive to Non-Primitive and Vice Versa
**boxed()** is used to convert to non-primitive stream.
**mapToInt() and others** are used to convert to primitive streams.

## Parallel Streams
Parallel Streams are just like streams with one subtle difference, stream is processed by multiple threads and the thread count cannot be configured by us.
### Use Parallel streams only if 
1. The data size is too large. Approx: 1 Million
2. The pipelines are more cpu intensive
3. The pipelines don't have many stateful functions.
4. No shared mutable states
5. Source can be split well i.e. Collections which have faster random access with indices. Bcoz splitting is easier with indices.
### Working
1. Splits the source into multiple chunks using `SplitIterator`
2. Processes all chunks in parallel on multiple threads which are taken from the `ForkJoinPool`.
3. Fork and join because, based on number of chunks multiple threads are forked by main thread and finally after their task, the result is combined which is joining.

Parallel Streams internally uses a special iterator called `SplitIterator` which has two methods
1. `trySplit`: Divides source for parallel processing, returns new split iterator with half of the elements. **Cannot predict the order of elements**
    ```java
    void main(String[] args){
        List<Integer> list = List.of(1,2,3,4,5,6,7,8);
        Spliterator<Integer> sp1 = list.spliterator();
        Spliterator<Integer> sp2 = sp1.trySplit();
        
        sp1.forEachRemaining(System.out::println); // 5,6,7,8
        
        sp2.forEachRemaining(System.out::println); // 1,2,3,4
    }
    ```
2. `tryAdvance(Consumer)`: Process one element at a time, return false if all elements are exhausted.

### Differences in methods of Stream and Parallel Stream
1. **findFirst vs findAny**: Find first returns the first element maintaining the order. Find any can return any element which is faster.
2. **forEach vs forEachOrdered**: forEach does not maintain order in parallel stream where as forEachOrdered maintains.
3. **isParallel**: Tells if the stream is parallel or not.