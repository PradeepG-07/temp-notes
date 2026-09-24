## `synchronized`

A thread must acquire the monitor lock associated with an object before entering a synchronized method or block which allows only one thread to enter the critical section at any point in time.

Consider the `increment()` method from the `Count` example. If the method is declared as `synchronized`, the following happens:

1. T1 first tries to acquire the lock. Since the lock is free, T1 acquires it.
2. T1 gets context-switched and T2 starts executing. T2 tries to acquire the lock, but since the lock is already held by T1, T2 is moved to the **BLOCKED** state.
3. When T1 resumes, it continues method execution by incrementing the count. After the method execution is completed, it releases the lock.
4. This way, T1 and T2 enter the method one at a time and increment the count correctly to **20K**.

### Why is `synchronized` needed?

1. To protect shared data.
2. To make operations atomic.
3. To ensure visibility.
4. To prevent instruction reordering.

### Monitor Locks

`synchronized` uses the concept of **monitor locks**, also called **object locks**.

When we see a synchronized method, it may seem like the lock is acquired on the method itself, but that is not the case.

The lock is acquired on either:

* The class object (`Demo.class`) for static synchronized methods.
* The actual object (`new Demo()`) for instance synchronized methods.

By default, every object in Java has a lock, which is also called an **internal lock**. The JVM maintains this lock.

Consider the following example:

```java
class Demo {
    synchronized void show() {
        System.out.println("Showing");
    }

    synchronized void paste() {
        System.out.println("Pasting");
    }
}
```

Assuming all threads use the same `Demo` object:

If a thread enters any synchronized method, no other thread can enter any synchronized method of that object until the first thread finishes execution and releases the lock, because the lock is the same for all synchronized instance methods of that object.

If two different threads use two different objects, they can execute these methods concurrently.

Practice this scenario.

### Synchronized Block

Sometimes only a few statements in a method belong to the critical section. Instead of declaring the entire method as synchronized, we can use a synchronized block.

Syntax:

```java
synchronized (monitor) {
}
```

Here, `monitor` refers to the object on which we want synchronization. For now, we can use `this` as the monitor.

```java
// Statements not part of the critical section

synchronized (this) {
    // Critical section
}

// Statements not part of the critical section
```

### Synchronization of Static Methods

If a static method is declared as `synchronized`, it acquires the lock on the class object.

### How Locks Work Internally

**Mental model:**

```text
Object:
    lock: {
        ownerThread: null,
        isLocked: false,
        waitingQueue: []
    }
```

Consider the mental model of a synchronized block as:

```java
if (lock.isLocked == false) {
    lock.isLocked = true;
    lock.ownerThread = currentThread;
} else {
    lock.waitingQueue.add(currentThread);
}
```

A common doubt is that the above code also follows the **check-then-act** pattern.

Yes, it does. However, this operation is implemented atomically using **CAS (Compare-And-Set)**, so there is no problem.

### Performance Overhead

Using `synchronized` introduces overhead and can make the application slower.
