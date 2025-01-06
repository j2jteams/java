# Java Collections Framework: A Complete Guide

## Table of Contents
1. [Introduction to Collections Framework](#introduction)
2. [Collection Hierarchy](#hierarchy)
3. [List Interface and Implementations](#list)
4. [Set Interface and Implementations](#set)
5. [Queue Interface and Implementations](#queue)
6. [Map Interface and Implementations](#map)
7. [Utility Classes](#utilities)
8. [Complete Working Example](#example)

## Introduction to Collections Framework
The Java Collections Framework provides a unified architecture for storing and manipulating groups of objects. It provides various interfaces and their implementations to suit different needs.

## Collection Hierarchy
```
Collection (interface)
├── List (interface)
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
│       └── Stack
├── Set (interface)
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue (interface)
    ├── PriorityQueue
    └── Deque (interface)
        ├── ArrayDeque
        └── LinkedList
```

## Complete Working Example

```java
import java.util.*;

public class CollectionsDemo {
    // Student class for our examples
    static class Student implements Comparable<Student> {
        private int id;
        private String name;
        private double gpa;

        public Student(int id, String name, double gpa) {
            this.id = id;
            this.name = name;
            this.gpa = gpa;
        }

        @Override
        public String toString() {
            return "Student{id=" + id + ", name='" + name + "', gpa=" + gpa + '}';
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Student student = (Student) o;
            return id == student.id;
        }

        @Override
        public int hashCode() {
            return Objects.hash(id);
        }

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }

        public int getId() { return id; }
        public String getName() { return name; }
        public double getGpa() { return gpa; }
    }

    public static void main(String[] args) {
        // Create sample students
        Student s1 = new Student(101, "Alice", 3.8);
        Student s2 = new Student(102, "Bob", 3.5);
        Student s3 = new Student(103, "Charlie", 3.9);
        Student s4 = new Student(104, "David", 3.6);
        Student s5 = new Student(105, "Eve", 3.7);

        System.out.println("========== LIST IMPLEMENTATIONS ==========");
        
        // ArrayList Example
        System.out.println("\n--- ArrayList Example ---");
        List<Student> arrayList = new ArrayList<>();
        arrayList.add(s1);
        arrayList.add(s2);
        arrayList.add(s3);
        System.out.println("ArrayList: " + arrayList);
        System.out.println("Get student at index 1: " + arrayList.get(1));
        arrayList.remove(1);
        System.out.println("After removing index 1: " + arrayList);

        // LinkedList Example
        System.out.println("\n--- LinkedList Example ---");
        LinkedList<Student> linkedList = new LinkedList<>();
        linkedList.add(s1);
        linkedList.add(s2);
        linkedList.addFirst(s3);  // LinkedList specific method
        linkedList.addLast(s4);   // LinkedList specific method
        System.out.println("LinkedList: " + linkedList);
        System.out.println("First element: " + linkedList.getFirst());
        System.out.println("Last element: " + linkedList.getLast());

        System.out.println("\n========== SET IMPLEMENTATIONS ==========");

        // HashSet Example
        System.out.println("\n--- HashSet Example ---");
        Set<Student> hashSet = new HashSet<>();
        hashSet.add(s1);
        hashSet.add(s2);
        hashSet.add(s3);
        hashSet.add(s1);  // Duplicate - won't be added
        System.out.println("HashSet: " + hashSet);

        // LinkedHashSet Example
        System.out.println("\n--- LinkedHashSet Example ---");
        Set<Student> linkedHashSet = new LinkedHashSet<>();
        linkedHashSet.add(s3);
        linkedHashSet.add(s1);
        linkedHashSet.add(s2);
        System.out.println("LinkedHashSet (maintains insertion order): " + linkedHashSet);

        // TreeSet Example
        System.out.println("\n--- TreeSet Example ---");
        TreeSet<Student> treeSet = new TreeSet<>();
        treeSet.add(s2);
        treeSet.add(s1);
        treeSet.add(s3);
        System.out.println("TreeSet (sorted by ID): " + treeSet);
        System.out.println("First element: " + treeSet.first());
        System.out.println("Last element: " + treeSet.last());

        System.out.println("\n========== QUEUE IMPLEMENTATIONS ==========");

        // PriorityQueue Example
        System.out.println("\n--- PriorityQueue Example ---");
        // Custom comparator for GPA-based priority
        Queue<Student> priorityQueue = new PriorityQueue<>(
            Comparator.comparingDouble(Student::getGpa).reversed()
        );
        priorityQueue.offer(s1);
        priorityQueue.offer(s2);
        priorityQueue.offer(s3);
        System.out.println("PriorityQueue (ordered by GPA):");
        while (!priorityQueue.isEmpty()) {
            System.out.println("Polling: " + priorityQueue.poll());
        }

        // ArrayDeque Example
        System.out.println("\n--- ArrayDeque Example ---");
        ArrayDeque<Student> arrayDeque = new ArrayDeque<>();
        arrayDeque.addFirst(s1);
        arrayDeque.addLast(s2);
        arrayDeque.addFirst(s3);
        System.out.println("ArrayDeque: " + arrayDeque);
        System.out.println("First element: " + arrayDeque.getFirst());
        System.out.println("Last element: " + arrayDeque.getLast());

        System.out.println("\n========== MAP IMPLEMENTATIONS ==========");

        // HashMap Example
        System.out.println("\n--- HashMap Example ---");
        Map<Integer, Student> hashMap = new HashMap<>();
        hashMap.put(s1.getId(), s1);
        hashMap.put(s2.getId(), s2);
        hashMap.put(s3.getId(), s3);
        System.out.println("HashMap: " + hashMap);
        System.out.println("Value for key 101: " + hashMap.get(101));

        // LinkedHashMap Example
        System.out.println("\n--- LinkedHashMap Example ---");
        Map<Integer, Student> linkedHashMap = new LinkedHashMap<>();
        linkedHashMap.put(s3.getId(), s3);
        linkedHashMap.put(s1.getId(), s1);
        linkedHashMap.put(s2.getId(), s2);
        System.out.println("LinkedHashMap (maintains insertion order): " + linkedHashMap);

        // TreeMap Example
        System.out.println("\n--- TreeMap Example ---");
        TreeMap<Integer, Student> treeMap = new TreeMap<>();
        treeMap.put(s2.getId(), s2);
        treeMap.put(s1.getId(), s1);
        treeMap.put(s3.getId(), s3);
        System.out.println("TreeMap (sorted by key): " + treeMap);
        System.out.println("First entry: " + treeMap.firstEntry());
        System.out.println("Last entry: " + treeMap.lastEntry());

        System.out.println("\n========== UTILITY METHODS ==========");
        
        List<Student> utilList = new ArrayList<>(Arrays.asList(s1, s2, s3, s4, s5));
        
        // Sorting
        Collections.sort(utilList, Comparator.comparingDouble(Student::getGpa));
        System.out.println("\nSorted by GPA: " + utilList);

        // Binary Search
        int index = Collections.binarySearch(utilList, s3, 
            Comparator.comparingDouble(Student::getGpa));
        System.out.println("Binary search found student at index: " + index);

        // Shuffling
        Collections.shuffle(utilList);
        System.out.println("After shuffle: " + utilList);

        // Finding min/max
        Student maxGpaStudent = Collections.max(utilList, 
            Comparator.comparingDouble(Student::getGpa));
        System.out.println("Student with highest GPA: " + maxGpaStudent);
    }
}
```

## Key Points to Remember

### Lists
- ArrayList: Best for random access and frequent reads
- LinkedList: Best for frequent insertions/deletions, especially at ends
- Vector: Thread-safe version of ArrayList (legacy)

### Sets
- HashSet: Fastest performance, no order guarantee
- LinkedHashSet: Maintains insertion order
- TreeSet: Keeps elements sorted, slower performance

### Queues
- PriorityQueue: Elements processed based on priority
- ArrayDeque: More efficient than Stack and LinkedList as a queue

### Maps
- HashMap: Best general-purpose implementation
- LinkedHashMap: Maintains insertion order
- TreeMap: Keeps keys sorted

### Common Operations Complexity

#### ArrayList
- add(E): O(1)
- add(int, E): O(n)
- remove(int): O(n)
- get(int): O(1)
- contains(Object): O(n)

#### LinkedList
- add(E): O(1)
- add(int, E): O(n)
- remove(int): O(n)
- get(int): O(n)
- contains(Object): O(n)

#### HashSet
- add(E): O(1)
- remove(Object): O(1)
- contains(Object): O(1)

#### TreeSet
- add(E): O(log n)
- remove(Object): O(log n)
- contains(Object): O(log n)

### Best Practices
1. Choose the right collection type based on your needs
2. Use interface types in declarations (e.g., `List<String>` instead of `ArrayList<String>`)
3. Consider using `Collections.unmodifiableXXX()` for immutable collections
4. Implement `equals()` and `hashCode()` properly for custom objects
5. Use generics to ensure type safety
6. Consider thread safety requirements when choosing collections
7. Use utility methods from Collections class when possible
