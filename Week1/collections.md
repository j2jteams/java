 **Java Collections Framework hierarchy** 

```
                    java.util.Collection
                           |
    ------------------------------------------------
    |                        |                     |
   List                     Set                  Queue
    |                        |                     |
---------------------------------        ------------------------
|            |              |          |         |             |
ArrayList LinkedList  Vector        HashSet   LinkedHashSet  TreeSet
                     (Stack)                        |
                                               NavigableSet
                                                    |
                                                TreeSet

                    java.util.Map
                           |
          ------------------------------------
          |                 |                |
        HashMap       LinkedHashMap       TreeMap
          |
      WeakHashMap
```

**Explanation**:

### Key Points:
1. **Collection Interface**:
   - Represents a group of objects; extended by `List`, `Set`, and `Queue`.
   
2. **List**:
   - Ordered collections allowing duplicates.
   - Common implementations: `ArrayList`, `LinkedList`, `Vector` (legacy).
   - `Stack` extends `Vector`.

3. **Set**:
   - Does not allow duplicate elements.
   - Common implementations: `HashSet`, `TreeSet`, and `LinkedHashSet`.
   - `TreeSet` implements `NavigableSet` for sorted sets.

4. **Queue**:
   - Supports element ordering for processing.
   - Common implementations: `LinkedList`, `PriorityQueue`, and `ArrayDeque`.

5. **Map**:
   - Not part of `Collection` but closely related.
   - Stores key-value pairs.
   - Common implementations: `HashMap`, `LinkedHashMap`, and `TreeMap`.
   - `TreeMap` implements `NavigableMap` for sorted key-value pairs.
