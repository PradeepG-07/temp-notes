## Problems with Multithreading

## 1. Race Condition

1. A race condition occurs when the final result of a program depends on the order in which threads execute.
2. It typically happens when multiple threads access and modify shared data without proper synchronization.
3. A common example is the `count++` operation.
4. Consider a `Counter` class with an instance variable `count`.
5. If two threads each increment `count` 10,000 times, the expected result is 20,000.
6. Due to a race condition, the actual result may be less than 20,000.
7. Practice: Create the above situation.

### Critical Section

1. A critical section is a part of a program where shared resources are accessed or modified.
2. Since multiple threads can access this section concurrently, it is prone to concurrency issues and must be protected properly.

### Shared Resource

1. A shared resource is any data or object that can be accessed by multiple threads.
2. Shared resources within a critical section are particularly vulnerable to race conditions.

---

## 2. Atomicity

### Atomic Operation

1. An atomic operation is performed as a single, indivisible unit.
2. No other thread can observe the operation in an intermediate or partially completed state.
3. Atomic operations are inherently thread-safe.

### Why `count++` is Not Atomic

1. The `count++` operation is not atomic.
2. Internally, it consists of multiple steps:
   * Read the current value.
   * Increment the value.
   * Write the updated value back.
3. A thread switch can occur between these steps.
4. Therefore, multiple threads executing `count++` can lead to race conditions.

### Making `count++` Atomic

To make increment operations thread-safe, we can use:

1. `synchronized`
2. `AtomicInteger`

---

### Some Atomic Operations in Java

Examples of atomic operations include:

1. Assignment operations on most primitive types:

   * `int x = 0`
   * `boolean flag = true`
2. Reference assignments.

---

### Some Non-Atomic Operations in Java

Examples of non-atomic operations include:

1. `x++`
2. `x--`
3. Check-then-act operations.
4. Read-modify-write operations.
5. Compound operations involving multiple steps.

---

### Note on `long` and `double`

1. Historically, the Java Language Specification did not guarantee atomic reads and writes for non-volatile `long` and `double` variables.
2. On some older JVMs and 32-bit systems, these values could be accessed in two separate 32-bit operations.
3. Modern JVM implementations generally perform atomic reads and writes for `long` and `double`.
4. Using `volatile` guarantees atomic reads and writes and also provides visibility guarantees.

---

### Relationship Between Atomicity and Race Conditions

1. Race conditions occur when multiple threads perform non-atomic operations on shared data.
2. Ensuring atomicity eliminates race conditions for that particular operation.

---

## 3. Visibility

### Definition

1. A visibility problem occurs when one thread updates a variable, but another thread does not see the updated value.
2. This happens because threads may use locally cached values instead of immediately observing updates made by other threads.

---

### Example

1. Consider a shared variable `flag = false`.
2. Thread T1 reads the value `false` and loads into its CPU cache. 
3. Thread T2 also reads the value `false` and loads into its CPU cache.
4. Later, T1 updates `flag` to `true`. The updated value is written to cache first and not immediately propagated to main memory or other CPU caches.
5. Without proper synchronization, T2 may continue reading the stale value from its cache.
6. As a result, the change made by T1 is not visible to T2.

---

### Note About `System.out.println()`

1. `System.out.println()` is internally synchronized.
2. Synchronization introduces memory visibility guarantees.
3. Therefore, using `System.out.println()` while demonstrating visibility issues can unintentionally hide the problem.
4. For this reason, it is generally avoided in visibility demonstrations.

---

### Solutions to Visibility Problems

#### 1. `volatile`

1. A variable declared as `volatile` provides visibility guarantees between threads.
2. When a thread writes to a volatile variable the JVM and CPU must ensure that:
   1. Any previous writes made by that thread are committed before the volatile write. 
   2. Other threads reading the same volatile variable cannot keep using a stale cached value. 
   3. A thread performing a volatile read must fetch a value that reflects the latest volatile write.
3. The actual implementation may involve:
   1. CPU cache coherence protocols (MESI, etc.)
   2. Cache invalidation 
   3. Memory fences/barriers 
   4. Store buffers being flushed
4. `volatile` solves visibility problems. 
5. It does **not** make compound operations such as `count++` atomic.

---

#### 2. `synchronized`

1. `synchronized` uses locks to control access to shared resources.
2. Entering and exiting a synchronized block establishes memory visibility guarantees.
3. Changes made by one thread become visible to another thread that acquires the same lock.
4. `synchronized` solves both visibility and atomicity problems.

---

## 4. Ordering

### Definition

1. Ordering refers to the sequence in which instructions are executed.
2. For performance optimization, the JVM and CPU may reorder instructions.
3. Instruction reordering is allowed as long as the behavior remains correct from a single-threaded perspective.
4. In multithreaded programs, instruction reordering can sometimes produce unexpected results.

---

### Solutions to Ordering Problems

#### 1. `synchronized`

1. `synchronized` establishes a **happens-before** relationship between threads.
2. It prevents problematic instruction reordering around synchronization boundaries.
3. It provides atomicity, visibility, and ordering guarantees.


#### 2. `volatile`

1. `volatile` also establishes a **happens-before** relationship for reads and writes of the volatile variable.
2. It prevents certain types of instruction reordering involving that variable.
3. It provides visibility and ordering guarantees.
4. It does not provide atomicity for compound operations such as `count++`.

---

### Important Note

Solving a visibility problem does **not** automatically solve a race condition.

For example, even if a variable is declared `volatile`, operations such as `count++` can still suffer from race conditions because they are not atomic.

**Atomicity prevents race conditions, while visibility ensures that updates made by one thread can be seen by other threads.**
