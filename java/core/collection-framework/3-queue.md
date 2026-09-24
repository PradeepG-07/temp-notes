## Deque
This interface extends both **Queue** and **SequencedCollection** and provides the `pollFirst`, `pollLast`, `peekFirst`, `peekLast`, `offerFirst`, `offerLast`, `removeFirst`, `removeLast`, `removeFirstOccurrence`, `removeLastOccurrence`.

## ArrayDeque

1. Deque is Double ended queue meaning insertion and deletion can happen at both the ends. Can be used as Stack or Queue.
2. Internally implemented using dynamic array with head and tail variables managing the operations.
3. When the size of DEFAULT_CAPACITY (17) exceeds
    1. if the existing `array size < 64` the `jump = oldCapacity + 2`
    2. else `jump = oldCapacity >> 1`
    3. `newCapacity = oldCapacity + jump`

## PriorityQueue

1. `PriorityQueue` is a concrete class which implements `AbstractQueue` which internally extends the `AbstractCollection` which internally implements `Collection`.
2. Internally implemented using **binary heap** with dynamic object array supporting custom comparator.
