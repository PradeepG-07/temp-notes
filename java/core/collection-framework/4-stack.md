## Vector

1. Vector is concrete legacy synchronized ArrayList. All the methods of Vector are synchronized and it is thread safe.
2. Performance is slower as there is lock overhead.
3. When the size of DEFAULT_CAPACITY (10) exceeds the new array is created with double the capacity of the existing array or the incrementCapacity can be passed via constructor at the time of creation.

## Stack

1. Stack is extending legacy vector so it is also slow and it follows LIFO principle.
2. All the methods of Stack i.e. **push, pop, peek** are also synchornized.
3. Preferred usage is using `Deque`
4. `Deque<Integer> stack = new ArrayDeque<>();`
