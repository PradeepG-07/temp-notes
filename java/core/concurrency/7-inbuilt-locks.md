## Limitations of `synchronized`

### 1. No Control Over Lock Acquisition

When a lock is already acquired by another thread, the current thread simply waits(BLOCKED state) until the lock becomes available.

It does not provide any control such as:

* Trying to acquire the lock with wait timeout.
* Executing alternative logic if the lock cannot be acquired.

### 2. No Timeout Support

`synchronized` does not provide a way for a thread to wait for specific amount of time trying to acquire a lock.

If the lock is held by another thread, the current thread immediately goes to BLOCKED state until the lock becomes available.

### 3. No Fairness Guarantee

Consider threads T1, T2, T3, and T4 waiting to acquire a lock.

There can be situations where a thread that arrived first does not acquire the lock first. As a result, a thread may wait indefinitely even though it has been waiting longer than others.

This situation is called **starvation**.

---

## Lock Interface

To provide more control over locking, Java introduced the **Lock** interface.

`Lock` is an interface whose contract includes methods such as `lock()` and `unlock()`.

Some important implementations are:

1. ReentrantLock
2. ReadWriteLock
3. StampedLock
4. Semaphore

Earlier, in synchronized methods and blocks, lock acquisition and release were handled internally by the JVM.

With the Lock API, we manually acquire and release locks.

Just as a synchronized block wraps a critical section, `lock()` and `unlock()` wrap the critical section when using the Lock API.

It is always recommended to use a `try-finally` block to ensure that the lock is released properly.

---

## ReentrantLock

```java
class Example {
    Lock lock = new ReentrantLock();

    void f1() {
        lock.lock();
        try {
            // ...
        } finally {
            lock.unlock();
        }
    }
}
```

### Features

#### 1. `tryLock()`

Returns `true` if the lock is acquired successfully; otherwise, returns `false`.

This gives us the flexibility to execute alternative logic when the lock cannot be acquired.

Example:

```java
if (lock.tryLock()) {
    // Critical section

    // Unlock
} else {
    // Execute alternative logic
}
```

#### 2. Reentrancy

The same thread can acquire the lock multiple times.

However, the lock must be released the same number of times it was acquired.

### Common Methods

1. `lock()`
2. `unlock()`
3. `tryLock()`
4. `tryLock(timeout, TimeUnit)`
    Example:
    ```java 
    lock.tryLock(2, TimeUnit.SECONDS);
    ```
5. `isLocked()`
6. `isHeldByCurrentThread()`
7. `getHoldCount()` – Returns the number of times the lock has been acquired by the current thread.
8. `isFair()` – Returns a boolean value indicating whether the lock uses a fairness policy.

> Enabling fairness on Java locks generally adds some overhead compared to the default unfair mode.

### Creating a Fair Lock

```java
Lock lock = new ReentrantLock(true);
```

---

## ReadWriteLock

Reading by multiple threads is generally a non-destructive operation.

However:

* Multiple writes at the same time can cause data corruption.
* Reading and writing simultaneously can cause visibility or consistency issues.

To solve this, we can allow:

* Multiple threads to read simultaneously.
* Only one thread to write at a time.

This can be implemented using **ReadWriteLock**.

It provides:

* A shared lock for readers.
* An exclusive lock for writers.

### Interface Methods

`ReadWriteLock` provides:

* `readLock()`
* `writeLock()`

Both methods return a `Lock`.

`ReadWriteLock` is an interface and is implemented by `ReentrantReadWriteLock`.

### Characteristics

1. A shared lock can be acquired by multiple readers simultaneously.
2. An exclusive lock can be acquired by only one writer at a time.
3. Shared and exclusive locks cannot be held simultaneously.

It is suitable for implementing the **Reader-Writer Problem**.

### Writer Starvation

Starvation can occur when readers continuously acquire the read lock.

As more readers keep entering, writers may wait indefinitely.

This situation is called **Writer Starvation**.

### Lock Downgrading

Lock downgrading means a thread that currently holds a write lock acquires a read lock before releasing the write lock, thereby transitioning from exclusive access to shared access without allowing another writer to modify the data in between.

#### Why is it called "downgrading"?
Because you're moving from a stronger lock to a weaker lock. But still retain access, but with fewer privileges.

Example:

```java
writeLock.lock();
try {
    value = 100;

    readLock.lock();
} finally {
    writeLock.unlock();
}
```
> Most ReadWriteLock implementations do not safely support upgrading because it can easily lead to deadlocks. However, downgrading is supported and is a common pattern.

---

## StampedLock

`StampedLock` is a modern alternative to `ReadWriteLock`.

### Important Methods

1. `writeLock()`
2. `readLock()`
3. `tryOptimisticRead()`

It works using **stamps** (`long` values).

### Basic Usage

```java
StampedLock lock = new StampedLock();

long stamp = lock.readLock();

try {
    // ...
} finally {
    lock.unlockRead(stamp);
}
```

```java
long stamp = lock.writeLock();

try {
    // ...
} finally {
    lock.unlockWrite(stamp);
}
```

---

## Locking Techniques

### 1. Pessimistic Locking

Locks are used to prevent concurrent access.

Because threads may need to wait, this approach can be slower.

### 2. Optimistic Locking

No lock is acquired initially.

Instead, we assume conflicts are rare and verify later whether the data was modified.

---

### How `tryOptimisticRead()` Works

1. Obtain a stamp.
2. Read the data without acquiring a lock.
3. Validate whether the data was modified.
4. If validation fails, use a fallback mechanism (typically acquiring a read lock and reading again).

Example:

```java
long stamp = lock.tryOptimisticRead();

int currentVal = val;

if (!lock.validate(stamp)) {
    stamp = lock.readLock();

    try {
        currentVal = val;
    } finally {
        lock.unlockRead(stamp);
    }
}

System.out.println(currentVal);
return currentVal;
```

### Reentrancy

`StampedLock` is **not reentrant**.

If the same thread attempts to acquire the lock multiple times, it can result in a deadlock.

---

## Semaphore

A Semaphore is used to control how many threads are allowed to access a resource or critical section at the same time.

### Example

```java
Semaphore s1 = new Semaphore(3);
```

This allows up to **3 threads** to enter the critical section at the same time.

### Methods

1. `acquire()`: Takes one permit.
2. `release()`: Releases one permit.

---

### Types of Semaphore

#### 1. Binary Semaphore

Works similarly to a lock by allowing only one permit.

#### 2. Counting Semaphore

Allows multiple permits.

---

### Difference Between Lock and Semaphore

#### Lock

* Has an owner thread.
* The thread that acquires the lock is responsible for releasing it.

#### Semaphore

* Does not have an owner thread.
* Any thread with a valid permit can release it.

---

### Examples

* API rate limiting
* Limiting parallel task execution

---

## Condition

`Condition` is an interface that provides functionality similar to `wait()`, `notify()`, and `notifyAll()`.

### Methods

1. `await()` – Similar to `wait()`
2. `signal()` – Similar to `notify()`
3. `signalAll()` – Similar to `notifyAll()`

---

### Why Was `Condition` Introduced?

`Condition` provides the flexibility to create multiple waiting queues for the same lock.

Consider the producer-consumer problem.

Using `wait()` and `notify()`, both producers and consumers wait on the same monitor queue. Because of this, using `notify()` can wake up the wrong type of thread.

With `Condition`, we can create separate waiting queues:

* One condition for producers.
* One condition for consumers.

A producer can signal consumers, and a consumer can signal producers.

This provides finer control over thread coordination.

**Practice:** Implement the producer-consumer problem using `Condition`.

### Example

```java id="4g8n7l"
Lock lock = new ReentrantLock();

Condition condition = lock.newCondition();

condition.await();
condition.signal();
condition.signalAll();
```

### Note

Spurious wakeups can still occur when using `Condition`.

Therefore, conditions should also be checked inside a `while` loop rather than an `if` statement.
