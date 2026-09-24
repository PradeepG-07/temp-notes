### `Thread.sleep(milliseconds)`

1. The current thread executing this statement sleeps for the specified duration.
2. The thread state changes from **RUNNABLE -> TIMED_WAITING -> RUNNABLE**.
3. `sleep()` does not release any locks held by the thread.
4. If the thread is interrupted while sleeping, `sleep()` throws `InterruptedException`.

---

### `targetThread.join()`

1. The current thread waits until the `targetThread` completes its execution.
2. The current thread moves from **RUNNABLE → WAITING**.
3. The `targetThread` continues execution and eventually moves from **RUNNABLE → TERMINATED**.
4. Once the `targetThread` terminates, the waiting thread moves from **WAITING → RUNNABLE** and resumes execution.


### `targetThread.join(milliseconds)`

1. Similar to `join()`, but the current thread waits only for the specified amount of time.
2. The current thread moves to the **TIMED_WAITING** state.
3. If the `targetThread` completes before the timeout expires, the current thread resumes immediately.
4. Otherwise, the current thread resumes execution after the specified timeout.

---

### `Thread.yield()`

1. The currently executing thread indicates that it is willing to give up its current CPU time.
2. The scheduler may choose another runnable thread of the same priority for execution.
3. `yield()` is only a suggestion to the scheduler and may be ignored by the operating system.
4. The thread which called `yield()` remains in the **RUNNABLE** state.

---

### `thread.interrupt()`

1. Sends an interruption request to a thread.
2. It does not immediately stop the thread; it only sets the thread's interrupt status (flag) to `true`.
3. A thread can respond gracefully by periodically checking its interrupt status using `isInterrupted()`.
4. Common use cases include:

    * Stopping a thread running in a loop.
    * Cancelling a long-running task.
    * Shutting down tasks managed by a thread pool.

---

### `thread.isInterrupted()`

1. Returns the interrupt status (`true` or `false`) of the specified thread.
2. Does not reset the interrupt flag.

### `Thread.interrupted()`

1. Returns the interrupt status of the **current thread**.
2. Resets the interrupt flag to `false` after checking it.
3. It is a static method.


### Interrupting a Waiting Thread

1. If a thread is blocked in `sleep()`, `join()`, or `wait()`, it is in the **TIMED_WAITING** or **WAITING** state.
2. Calling `interrupt()` on such a thread causes the blocked method to throw `InterruptedException`.
3. The interrupt flag is cleared when the exception is thrown.

---

### `thread.isAlive()`

1. Checks whether a thread is alive.
2. A thread is considered alive if it has been started and has not yet terminated.

---

### `Thread.currentThread()`

1. Returns a reference to the currently executing thread.

---

### `thread.getPriority()`

1. Java provides three predefined priority constants:

    * `MIN_PRIORITY = 1`
    * `NORM_PRIORITY = 5`
    * `MAX_PRIORITY = 10`
2. Thread priority is a scheduling hint provided to the JVM and operating system.
3. Higher-priority threads may be given preference for execution.
4. The actual behavior is platform-dependent:

    * Some systems respect thread priorities.
    * Some partially respect them.
    * Others may largely ignore them.

---

## Daemon Threads

1. Daemon threads are background threads that perform supporting tasks for the application.
2. They typically handle services that run behind the scenes.
3. When all user threads terminate, the JVM automatically stops any remaining daemon threads.
4. A common example is the Garbage Collector (GC) thread.

---

## User Threads

1. User threads perform the main work of an application.
2. The JVM continues running as long as at least one user thread is alive.
3. The application terminates only when all user threads have completed execution.
