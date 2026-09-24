## HashMap

1. HashMap is implemented using arrays of Nodes called as Buckets. Each Node has four attributes hash, key, value and next.
2. Default size of the array is 16 and will be doubled everytime when resize is needed.
3. `table[]`  = Buckets, `Node<K,V>` = Node for separate chaining, `TreeNode<K,V>` = Node for Red Black Tree.
4. Inserting a key-value pair into HashMap has multiple steps which are as follows:
    1. **Find Hash:**
        1. For a given key `hash` function is called, if  `key==null` , then hash is considered as 0 meaning all the null key values are stored in Bucket 0. Else `key.hashCode()` method is called to get the hashcode.
        2. After getting the hashcode, the operation is performed `hashcode ^ (hashcode >>> 16)`, this is done to avoid hash collision and achieve the evenly distribution among the buckets.
    2. **Find Bucket Index:** Bucket index is calculated using the formula `(capacity-1) & hash` .
    3. **Insert / Replace Key,Value:**
        1. If the bucket index is empty (`null`) then a new node is inserted into the bucket. Else the first node instance type is identified to know whether it is `TreeNode` or `Node` .
        2. Each is node is traversed and hash code is matched with the node. Hash, if both hashes matches then both keys are also matching then the value is updated otherwise a new node is created based on the first node instance type.
    4. **Get Value:**
        1. Step-1, 2 are done then if bucket index is empty returns null else each node is traversed by comparing `hash` and then the `key` . If both matches the value is returned.
5. **Resizing:**
    1. **Table Resizing**
        1. `table[]` is resized when the number of entries (key-value pairs) reaches the threshold. Threshold is the product of current capacity and load factor.
        2. **Threshold = (Current_Capacity) * (Load_Factor)**
        3. Where Current_Capacity is the current length of `table[]` and Load_Factor is supplied while creating `HashMap`. If it is not supplied, by default, it will be 0.75f.
        4. Let’s assume that Current_Capacity of `table[]` is 16 and Load_Factor is 0.75f, then threshold will be, `16*0.75=12`
        5. That means, `table[]` is resized when 12th element is inserted into HashMap i.e. 75% of current capacity. And whenever `table[]` is resized, it is doubled in size.
        6. Resizing `table[]` is a costly affair in `HashMap`. It is both time and space consuming as all existing key-value pairs have to be placed in new `table[]` with larger size after calculating all their bucket index again. So, it is wise to choose the initial capacity of `HashMap` by keeping number of expected entries in mind so that resizing doesn’t take place more often.
    2. **Bucket Resizing**
        1. When collision occurs primarily linked list will be used as per separate chaining principle.
        2. Using this on large data will take O(n) time in worst case. So to optimize this concept of ***Treeification*** is introduced in Java 8.
        3. **MIN_TREEIFY_CAPACITY(64):** It is the minimum capacity that an array of buckets (`table[]`) must have before a bucket is converted from linked list to red black tree. This field ensures that table[] is sufficiently big enough before a bucket is treeified. If table[] is too small then resizing table[] is more efficient than converting linked list to binary tree.
        4. If the number of nodes in a linked list reach **TREEIFY_THRESHOLD (8) after inserting current element**, then that linked list is converted to red black tree which is called as **Treeification.**
        5. If the number of nodes in a tree becomes less than or equal to the **UNTREEIFY_THRESHOLD (6),** then the tree is converted back to linked list which is called **UnTreeification.**
6. Disadvantages: Resizing overhead, Collisions Possible, No ordering, Not Thread safe
7. Reference: https://javaconceptoftheday.com/java-hashmap-interview-questions-and-answers/

## LinkedHashMap

1. Extends the HashMap and for preserving order a separate linked list is maintained with before and after pointers.
2. Whenever a node is inserted the map, the internal HashMap stores the data and handles the collision, later as final step the linked list is also updated with new node inserted at last.
3. When a special parameter called **accessOrder** is passed as true to LinkedHashMap it will behave like the LRU Cache, meaning on each `get` method call the accessed node will move to the end of the linked list.

## ConcurrentHashMap
Thread safe
For reads: No locking. to avoid race condition/visiblity problem uses volatile keyword.
For writes: 
    if bucket is empty, 
        uses CAS to add a new node. CAS(null, newNode). 
        If t1, t2 tries at same time. both tries CAS but only one succeeds the later one retries with `synchronized` because for the first node it is simple just put the node in that block.
    if bucket is not empty,
        uses synchronized keyword, because lot of operations are needed. 
        1. traversing existing nodes
        2. check if key exists
        3.  handle the collision
        4. treeify
        5. update the links accordingly
**Todo**: Time complexity table, ConcurrentHashMap, Hashtable, diff of hashtable, concurrenthashmap