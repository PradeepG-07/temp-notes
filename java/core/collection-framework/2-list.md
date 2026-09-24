## ArrayList

1. ArrayList is a concrete class which implements the List Interface and internally it uses dynamic array to store the data.
2. When arraylist is created with default constructor the dynamic array is initialised with empty object array. Default value is `null`.
3. When a element is added into the arraylist, the size of the array is changed to default capacity of 10 and the element is added into the list at first index and remaining indices will be having null.
4. When the size of DEFAULT_CAPACITY (10) exceeds from here a formula `newCapacity = oldCapacity + oldCapacity >> 1` will be created and existing data will be copied to the new dynamic array and the new element is added. This is **1.5x** growth.
5. On removing elements the dynamic array will not shrink as frequent reallocations, copy elements from existing array to new array make the program slower and garbage collection overhead also rises.
6. If a element is removed the value will be replaced with `null` at the given index.
7. addAll, removeAll, add all these uses native method `System.arraycopy` to add the data.

| Advantages                         | Disadvantages                                   |
|------------------------------------|-------------------------------------------------|
| 1. Fast random Access              | 1. Insertion and deletion is slow in the middle |
| 2. Cache friendly                  | 2. Resize overhead and not thread safe          |
| 3. Suitable for read heavy systems |                                                 |

## LinkedList

1. LinkedList is a concrete class which also implements the List interface, and internally it is implemented as a **Doubly LinkedList** with help of `Node` class which has data, prev, next pointers.
2. The `Node` class is the Nested Static class of `LinkedList` class.
3. Insertion at ends will be faster and removal is faster if the node reference is already available.

| Advantages               | Disadvantages                                   |
|--------------------------|-------------------------------------------------|
| 1. Faster access at ends | 1. Insertion and deletion is slow in the middle |
| 2. No resizing overhead  | 2. More memory overhead and not thread safe     |

## When to use `ArrayList` and `LinkedList`?

Use `ArrayList` because it provides O(1) random access, better cache locality, and generally better real-world performance. I'd consider `LinkedList` when the application performs frequent insertions and deletions at the beginning or middle of the list and does not require frequent indexed access. The most significant difference is that `ArrayList` supports O(1) index-based access, whereas `LinkedList` requires O(n) traversal to reach an element by index.
