## Architecture of Heap
In order to understand the garbage collection, first lets understand the architecture of heap memory.

![Heap Memory Model](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ1pYbARE5h9I8-gfby3BRcDNGOk7YCj3peYToNTj78jV2fRQYx-de-sduv&s=10)
Heap Memory consists of three parts namely,

1. Young Generation
2. Old Generation
3. Permanent Generation / Metaspace

### Young Generation

1. Young Generation is divided into three spaces
    1. Eden space
    2. S0 - Survivor Space - 0
    3. S1 - Survivor Space - 1
2. **Eden Space** is the space where all the new objects will be placed.
3. **S0 and S1** are used as containers to increase the ages of the existing objects with help of swapping. Explained clearly below.

### Old Generation

Objects from S1 or S0 are promoted to older generation after the age of object crosses a standard limit of 15.

The exact threshold is controlled by JVM option: **XX:MaxTenuringThreshold**

The JVM may promote objects earlier than the threshold if:

1. Survivor spaces are too small
2. There’s memory pressure

**Note:** Age is the number of GC cycles took place.

### Metaspace

1. Introduced in Java8, Earlier it is PermGen which is part of heap, stores all the metadata of the class.
2. It includes:
    1. Class structure: Class name, package, Superclass, interfaces
    2. Field metadata: Field names, types, modifiers (static, final, etc.)
    3. Method metadata: Method names, return types, parameters, Bytecode of methods,
    4. Exception tables
    5. Runtime Constant Pool (per class)
    6. Symbolic references (to methods, fields, classes)
    7. Literals (like "hello", numbers) as references
    8. Annotations metadata
    9. ClassLoader-related data
    10. Internal JVM structures for reflection & linking

## Garbage Collection

1. JVM automatically frees memory by removing objects that are no longer reachable from any live references is garbage collection.
2. The JVM triggers GC when it decides memory needs cleanup, mainly in these situations:
    1. **Young Generation (minor GC):** Happens when the Eden space gets full.
    2. **Old Generation (major/full GC):** Happens when the old generation fills up or after multiple minor GCs.
    3. **Explicit request (not guaranteed):** Calling `System.gc()` *suggests* a GC, but the JVM may ignore it.
    4. **Memory pressure:** When the JVM can’t allocate memory for new objects.
3. Simple Flow
    1. **Object creation:** New objects go into the Young Generation (Eden).
    2. **Mark phase:** JVM finds which objects are still reachable.
    3. **Sweep phase:** Removes unreachable objects.
    4. **Compact (optional):** Rearranges memory to avoid fragmentation.
    5. **Promotion:** Objects that survive multiple GCs move to Old Generation.

### How GC actually identifies unused objects

1. Start from GC Roots
2. Traverse all references (like a graph)
3. Mark every reachable object as “alive”
4. Anything not marked = unreachable = garbage

### Types of Garbage Collectors

1. Serial GC
    1. Single-threaded GC that pauses all application threads (Stop-The-World).
    2. Best for small apps or low-resource environments.
2. Parallel GC (Throughput Collector)
    1. Uses multiple threads for GC but still pauses the application.
    2. Optimized for high throughput, not low latency.
3. CMS (Concurrent Mark Sweep) GC - **Deprecated (Java 9+) and removed in later versions**
    1. Performs most GC work using multiple threads to perform the collection it doesn’t freeze all the threads but utilizes some of them to do its job..
    2. Slower than Serial, Parallel Gc, as application does not stop and can produce more garbage. Calling `System.gc()` will lead to concurrent mode failure
4. G1 (Garbage First) GC
    1. Splits heap into regions and collects garbage in the most filled regions first. This approach is called Garbage-First.
    2. Performs compaction by copying objects from one or several memory regions into a single region. Designed for low pause time + large heaps (default in modern JVMs).
5. ZGC (Z Garbage Collector) - **Experimental in Java11, Production in Java15**
    1. Ultra low-latency GC with pause times typically under 10ms.
    2. Scales well for very large heaps (multi-GB to TB).
6. Shenandoah GC - **Not very important**
    1. Concurrent GC aiming for very low pause times, independent of heap size.
    2. Similar goal as ZGC but different implementation.
7. Epsilon GC
    1. No-op GC (does not collect garbage at all).
    2. **Used for testing**/performance benchmarking only.

## References

1. [Concept And Coding By Shrayansh - Youtube](https://www.youtube.com/watch?v=vz6vSZRuS2M&list=PL6W8uoQQ2c63f469AyV78np0rbxRFppkx&index=10)
2. [Bella Soft - Blog](https://bell-sw.com/announcements/2022/09/07/garbage-collection-in-java/)