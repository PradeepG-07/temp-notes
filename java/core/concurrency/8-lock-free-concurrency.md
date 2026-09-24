# Lock-Free Concurrency

Java provides several features to support lock-free concurrency.

## 1. Atomic Variables

Java provides atomic variables such as `AtomicInteger`, `AtomicLong`, `AtomicBoolean`, etc.

Atomic classes help prevent race conditions on a single variable, even when multiple threads execute concurrently or in parallel. They use **CAS (Compare-And-Set)** operations to perform updates atomically without using traditional locks.

### Common Methods

1. `get()`
2. `set()`
3. `incrementAndGet()` → equivalent to `++x`
4. `getAndIncrement()` → equivalent to `x++`
5. `addAndGet(val)` → equivalent to `x = x + val`
6. `getAndAdd(val)`
7. `compareAndSet(expectedValue, newValue)`

### Practice

Implement a counter using atomic variables.

---

## AtomicReference

`AtomicReference` provides atomic operations on object references.

It is useful when multiple threads need to update a shared reference safely without using locks.

### Example

Seat Booking System

### Syntax

```java
AtomicReference<String> ref =
        new AtomicReference<>("EMPTY");

ref.compareAndSet("EMPTY", name);
```

Even if multiple threads execute `compareAndSet()` concurrently, each thread can attempt the operation simultaneously. However, if multiple threads are competing to change the same value, only the thread whose CAS condition matches the current value will succeed. Other threads will fail and must retry if required.

### How CAS Works

CAS is implemented using atomic CPU instructions provided by the underlying hardware.

The basic steps are:

1. Read the current value.
2. Compare it with the expected value.
3. If they match, update the value atomically.
4. Otherwise, fail the operation.

The entire compare-and-update operation is performed as a single atomic hardware operation.

### Hardware Support

Modern processors provide special atomic instructions such as:

* `CMPXCHG` on x86 processors
* `LDXR/STXR` on ARM processors

The JVM uses these hardware primitives to implement atomic classes.

Atomicity is guaranteed through a combination of:

* CPU atomic instructions
* Cache coherence protocols (such as MESI)
* Memory ordering guarantees provided by the hardware and JVM

Because of these mechanisms, all processors observe updates in a consistent manner without requiring traditional locks.

### Practice

Implement a Like Counter using `AtomicReference`.

---

## AtomicReferenceArray

`AtomicReferenceArray` provides atomic operations on array elements.

It can be viewed as an array version of `AtomicReference`, where each element supports atomic operations independently.

### Syntax

```java
AtomicReferenceArray<String> arr =
        new AtomicReferenceArray<>(5);
```

### Common Methods

* `set()`
* `get()`
* `compareAndSet()`
* `getAndSet()`
* and others

### Practice

Implement a seat-booking system with multiple seats.

---

## Compare-And-Swap (CAS)

CAS stands for **Compare-And-Swap**.

It is a hardware-supported atomic operation that:

1. Compares the current value with an expected value.
2. If they match, replaces the current value with a new value.
3. Returns whether the operation succeeded or failed.

A CAS operation itself does not automatically retry when it fails.

For example:

```java
boolean success =
        ref.compareAndSet(expectedValue, newValue);
```

This performs only a single CAS attempt.

If retry behavior is required, it is typically implemented in a loop:

```java
while (true) {
    int current = counter.get();

    if (counter.compareAndSet(current, current + 1)) {
        break;
    }
}
```

This retry loop is what makes many lock-free algorithms work.

---

## Limitations of Atomic Variables

Atomic variables solve race conditions only for operations performed on the atomic variable itself.

For example:

```java
AtomicInteger count = new AtomicInteger(0);
```

Operations such as:

```java
count.incrementAndGet();
```

are atomic.

However, multi-step operations may still have race conditions:

```java
if (count.get() < 10) {
    count.incrementAndGet();
}
```

The above code is **not atomic as a whole** because another thread may modify `count` between the `get()` and `incrementAndGet()` calls.

Therefore, atomic classes do not automatically make entire business operations thread-safe.

---

# ABA Problem

Although CAS is powerful, it cannot detect whether a value changed and then changed back to its original value.

This is known as the **ABA Problem**.

### Example

```java
AtomicInteger ref = new AtomicInteger(100);

// T1
int value = ref.get(); // Reads A (100)

// Context switch

// T2
ref.compareAndSet(100, 300); // A -> B
ref.compareAndSet(300, 100); // B -> A

// T1 resumes
boolean success = ref.compareAndSet(value, 200);

// success = true
// T1 assumes the value never changed,
// but it actually changed A -> B -> A
```

### Explanation

1. T1 reads the value `100`.
2. T1 gets paused.
3. T2 changes the value from `100 → 300 → 100`.
4. T1 resumes and performs CAS.
5. Since the current value is again `100`, CAS succeeds.

From T1's perspective, it appears that the value never changed, even though it actually changed twice.

This situation is called the **ABA Problem**.

---

# Solution: AtomicStampedReference

The ABA problem can be solved by associating a version number (called a stamp) with the value.

Java provides `AtomicStampedReference` for this purpose.

### Example

```java
AtomicStampedReference<Integer> ref =
        new AtomicStampedReference<>(100, 1);

// T1
int stamp = ref.getStamp();
int value = ref.getReference(); // Reads A (100)

// Context switch

// T2
ref.compareAndSet(100, 300, 1, 2); // A -> B
ref.compareAndSet(300, 100, 2, 3); // B -> A

// T1 resumes
boolean success =
        ref.compareAndSet(value, 200, stamp, stamp + 1);

// success = false
// Value is still 100, but the stamp changed from 1 to 3
// ABA is detected
```

### Explanation

1. T1 reads value `100` and stamp `1`.
2. T1 gets paused.
3. T2 changes the value from `100 → 300 → 100`.
4. During these updates, the stamp changes from `1 → 2 → 3`.
5. T1 resumes and attempts CAS using the expected pair `(100, 1)`.
6. The current pair is `(100, 3)`.
7. Since the stamp does not match, CAS fails.

Although the value returned to its original state, the version number reveals that modifications occurred in between. This allows the ABA problem to be detected.

---

## Summary

* Atomic classes provide lock-free, thread-safe operations on individual variables.
* They rely on hardware-supported CAS operations rather than traditional locks.
* Multiple threads can execute CAS simultaneously, but only matching CAS operations succeed.
* Atomic classes protect only the atomic variable itself, not arbitrary multi-step business logic.
* CAS cannot detect the ABA problem.
* `AtomicStampedReference` solves the ABA problem by associating a version number (stamp) with the value.
* Lock-free algorithms are often implemented using CAS inside retry loops.
