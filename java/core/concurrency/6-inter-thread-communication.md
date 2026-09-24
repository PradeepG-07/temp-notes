## Producer-Consumer Problem

Consider a problem with one box where a producer produces a value into the box and a consumer consumes the value from the box.

Because of multithreading, there might be situations where:

1. A consumer could read a `null` value.
2. A producer could overwrite a new value before the consumer has consumed the old value.

### Does the `synchronized` Keyword Solve This Problem?

No. `synchronized` ensures that only one thread can enter a method at a time, but it cannot control the order in which threads enter.

Here, The producer should wait when the box already contains an item, and the consumer should wait when the box is empty.

### Busy Waiting Approach

We can solve this using infinite loops.

For the producer:

```java
while (item != null) {
    // do nothing
}
```

For the consumer:

```java
while (item == null) {
    // do nothing
}
```

However, this wastes CPU time.

Consider that the producer takes 2 seconds to produce a value. Until then, the consumer thread keeps consuming CPU time by running in the infinite loop.

This is called **busy waiting**. It means the consumer thread is busy (executing on the CPU) while waiting for the value to be produced.

This approach can introduce race conditions because checking the condition and acting on it are not performed atomically. A context switch between these operations can lead to incorrect behavior.

**Practice:** Implement the producer-consumer problem using busy waiting.

### Using `synchronized` with Busy Waiting

To avoid race conditions, we can add `synchronized`.

However, if we do that and the consumer thread starts when there is no item, it will continue running in the infinite loop while holding the lock, preventing other threads from acquiring it and causing a deadlock.

**Practice:** Create this deadlock scenario.

## Inter-Thread Communication

Instead, what if the threads could communicate with each other?

For example:

* When the producer produces a value, it tells the consumer to consume it.
* After consuming the value, the consumer tells the producer to produce the next one.

To solve this, we introduce **inter-thread communication** for coordination between threads.

A thread communication problem mainly has three components:

1. **Shared Resource** – here, it is the box.
2. **Condition** – whether the box contains an item or not.
3. **Waiting** – the consumer should wait if no item exists, and the producer should wait if an item already exists.


### Methods

### `wait()`

A thread that executes `wait()` pauses its execution, releases the monitor that the thread currently owns, and moves to the **WAITING** state. It stays there until another thread wakes it up.

### `notify()`

`notify()` Wakes up one waiting thread that is waiting on the same monitor and moves it from the **WAITING** state to the **BLOCKED** state. The JVM does not guarantee which waiting thread will be selected.

Steps:

1. One random thread is selected from the waiting queue.
2. That thread is moved to the **BLOCKED** state.
3. It competes for the lock.
4. Once the lock is acquired, the thread moves to the **RUNNABLE** state.

### `notifyAll()`

1. All threads in the waiting queue are moved to the **BLOCKED** state.
2. All awakened threads become eligible to compete for the monitor lock, but only one thread can acquire it at a time.
3. Only one thread gets the lock at a time.

### Why Use `notifyAll()` Instead of `notify()`?

Consider the producer-consumer case with threads **P1, P2, C1, and C2**.

Currently, **P1, P2, and C1** are in the waiting state.

Suppose **C2** checks whether an item exists. Assume it does exist. Then C2 consumes the value, calls `notify()`, and completes its execution.

Be careful here:

* If either **P1** or **P2** wakes up, there is no problem.
* However, if **C1** wakes up and acquires the lock, it checks the condition, sees that there is no item, it will keep looping while holding the lock. As a result, the producer cannot acquire the lock to produce an item, and the program makes no progress.
* This is usually called:
  * thread starvation of progress 
  * missed notification problem 
  * permanent waiting

### Important Rule

`wait()`, `notify()`, and `notifyAll()` should be called only inside synchronized methods or synchronized blocks. Otherwise, an `IllegalMonitorStateException` is thrown.

---

## Spurious Wakeup

When a thread remains in the waiting state for a long time, the CPU may wake it up even though no thread has called `notify()` or `notifyAll()`.

According to our program logic, this should not happen because no notification was sent.

Because of this possibility, whenever a condition is being checked, we use a **while loop** instead of an **if statement**.

With a `while` loop, a spuriously awakened thread checks the condition again and goes back to waiting if the condition is still not satisfied.

If an `if` statement is used, the spuriously awakened thread will not recheck the condition and may proceed to perform work that it should not perform.

A guarded block is a loop that repeatedly checks a condition and calls `wait()` until the condition becomes true.
