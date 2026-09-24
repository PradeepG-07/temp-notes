# References

## Strong Reference

The default reference type in Java. As long as at least one strong reference points to an object, the object is not eligible for garbage collection.

```java
public static void main(String[] args) {

    Person person = new Person("Pradeep");

    System.gc();

    System.out.println(person.name);
}
```
### Explanation:
- `person` is a strong reference to the Person object. 
- Calling `System.gc()` does not remove the object because it is still strongly reachable. 
- The object remains in memory until all strong references to it are removed. 
- This is the reference type used for almost all objects in normal Java applications.

## Soft Reference
A soft reference allows an object to remain in memory until the JVM needs memory. The garbage collector may remove the object when memory pressure occurs.
```java
public static void main(String[] args) {

    SoftReference<Image> imageRef =
            new SoftReference<>(new Image());

    Image image = imageRef.get();

    if (image != null) {
        System.out.println("Image still available");
    } else {
        System.out.println("Image was removed by GC");
    }
}
```

### Explanation
- The Image object is wrapped inside a SoftReference. 
- `imageRef.get()` returns the actual object if it has not been collected. 
- The JVM tries to keep soft-referenced objects alive as long as memory is available. 
- When memory becomes low, the garbage collector may reclaim the object. 
- Soft references are commonly used for memory-sensitive caches.

## Weak Reference
A weak reference does not prevent garbage collection. If an object is only weakly reachable, it becomes eligible for collection during the next garbage collection cycle.
```java
public static void main(String[] args) {

    User user = new User();

    WeakReference<User> weakRef =
            new WeakReference<>(user);

    user = null;

    System.gc();

    User value = weakRef.get();

    if (value == null) {
        System.out.println("Object collected");
    } else {
        System.out.println("Object still alive");
    }
}
```
### Explanation
- `user` initially holds a strong reference to the object. 
- `weakRef` holds a weak reference to the same object. 
- After `user = null`, the strong reference is removed. 
- The object is now reachable only through a weak reference. 
- During the next GC cycle, the object may be collected. 
- `weakRef.get()` returns null if the object has already been reclaimed. 
- Weak references are commonly used in `WeakHashMap` and metadata caches.

## Phantom Reference
A phantom reference is used to receive notification that an object is ready to be reclaimed by the garbage collector. The referenced object cannot be accessed through a phantom reference.

```java
public static void main(String[] args) {

    ReferenceQueue<Resource> queue =
            new ReferenceQueue<>();

    Resource resource = new Resource();

    PhantomReference<Resource> phantomRef =
            new PhantomReference<>(resource, queue);

    resource = null;

    System.gc();

    System.out.println(phantomRef.get());
}
```
### Explanation
- A `ReferenceQueue` is created to receive cleanup notifications. 
- The Resource object is wrapped inside a PhantomReference. 
- After `resource = null`, no strong references remain. 
- Calling `phantomRef.get()` always returns `null`. 
- The phantom reference is not used to access the object. 
- When the object becomes eligible for reclamation, the JVM places the phantom reference into the associated ReferenceQueue. 
- Phantom references are commonly used for resource cleanup and advanced memory-management scenarios.

TODO: put something on queue to do something when notification arrives.