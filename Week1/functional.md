## Functional Programming Principles in Java

### Embracing Functional Programming in Java

Functional programming is a programming paradigm that treats computation as the evaluation of mathematical functions, avoiding changing state and mutable data. It emphasizes key principles such as immutability, higher-order functions, and declarative code. With the introduction of Java 8, Java added support for functional programming concepts, allowing developers to blend object-oriented and functional styles seamlessly.

This post will explore core functional programming principles and how to implement them in Java with practical examples.

### 1. Immutability: Avoiding Side Effects

Immutability ensures that objects, once created, cannot be modified. In traditional programming, mutable state can lead to unpredictable behavior, especially in concurrent applications. Functional programming encourages creating new instances instead of modifying existing ones, resulting in safer and more predictable code.

#### Example: Immutable Class

```java
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;

public class ImmutabilityExample {

    // Pre-Java 8 immutable class
    public static final class ImmutablePersonPreJava8 {
        private final String name;
        private final int age;
        private final Map<String, String> attributes;

        public ImmutablePersonPreJava8(String name, int age, Map<String, String> attributes) {
            this.name = name;
            this.age = age;
            this.attributes = Collections.unmodifiableMap(new HashMap<>(attributes));
        }

        public String getName() { return name; }
        public int getAge() { return age; }
        public Map<String, String> getAttributes() { return attributes; }

        @Override
        public String toString() {
            return "ImmutablePersonPreJava8{name='" + name + "', age=" + age + ", attributes=" + attributes + '}';
        }
    }

    // Java 16+ immutable record
    public record ImmutablePersonJava16(String name, int age, Map<String, String> attributes) {
        public ImmutablePersonJava16 {
            attributes = Map.copyOf(attributes);
        }

        public ImmutablePersonJava16 withAge(int newAge) {
            return new ImmutablePersonJava16(this.name, newAge, this.attributes);
        }
    }

    public static void main(String[] args) {
        // Pre-Java 8 approach
        Map<String, String> attrsOld = new HashMap<>();
        attrsOld.put("eyeColor", "blue");
        attrsOld.put("height", "180cm");

        ImmutablePersonPreJava8 personOld = new ImmutablePersonPreJava8("John", 30, attrsOld);

        System.out.println("Pre-Java 8 Person: " + personOld);

        try {
            personOld.getAttributes().put("weight", "75kg");
        } catch (UnsupportedOperationException e) {
            System.out.println("Cannot modify pre-Java 8 attributes: " + e.getMessage());
        }

        // Java 16+ approach
        Map<String, String> attrsNew = Map.of("eyeColor", "brown", "height", "175cm");
        ImmutablePersonJava16 personNew = new ImmutablePersonJava16("Jane", 25, attrsNew);

        System.out.println("Java 16+ Person: " + personNew);

        try {
            personNew.attributes().put("weight", "65kg");
        } catch (UnsupportedOperationException e) {
            System.out.println("Cannot modify Java 16+ attributes: " + e.getMessage());
        }

        // Demonstrating the 'with' method in Java 16+ approach
        ImmutablePersonJava16 olderPersonNew = personNew.withAge(26);
        System.out.println("Older Java 16+ Person: " + olderPersonNew);
    }
}
```
1. The pre-Java 8 approach using a final class with private final fields and defensive copying.
2. The Java 16+ approach using a record, which provides built-in immutability.
3. Both approaches prevent modification of the attributes map.
4. The Java 16+ version demonstrates the use of a 'with' method to create a new instance with a modified value.

### 2. Higher-Order Functions: Passing Behavior

Higher-order functions accept other functions as arguments or return them. In traditional Java, this was achieved using anonymous classes, which often required verbose syntax. With the introduction of lambdas and method references, Java makes it easier to pass behavior, improving code readability and maintainability.

#### Example: Custom Sorting with Lambdas

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Predicate;

public class HigherOrderFunctionExample {

    // Pre-Java 8 approach
    interface OldPredicate {
        boolean test(int value);
    }

    static List<Integer> oldFilterList(List<Integer> list, OldPredicate predicate) {
        List<Integer> result = new ArrayList<>();
        for (Integer num : list) {
            if (predicate.test(num)) {
                result.add(num);
            }
        }
        return result;
    }

    // Java 8+ approach
    static List<Integer> newFilterList(List<Integer> list, Predicate<Integer> predicate) {
        List<Integer> result = new ArrayList<>();
        for (Integer num : list) {
            if (predicate.test(num)) {
                result.add(num);
            }
        }
        return result;
    }

    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        System.out.println("Pre-Java 8 approach:");
        List<Integer> oldEvenNumbers = oldFilterList(numbers, new OldPredicate() {
            @Override
            public boolean test(int value) {
                return value % 2 == 0;
            }
        });
        System.out.println("Even numbers: " + oldEvenNumbers);

        System.out.println("\nJava 8+ approach:");
        List<Integer> newEvenNumbers = newFilterList(numbers, n -> n % 2 == 0);
        System.out.println("Even numbers: " + newEvenNumbers);

        // Additional example using Java 8+ approach
        List<Integer> greaterThanFive = newFilterList(numbers, n -> n > 5);
        System.out.println("Numbers greater than 5: " + greaterThanFive);
    }
}
```
1. Pre-Java 8 approach using a custom interface and anonymous class.
2. Java 8+ approach using the Predicate functional interface and lambda expressions.
3. Both approaches implement a higher-order function filterList that takes a list and a behavior (predicate) as parameters.
4. The Java 8+ version is more concise and flexible, allowing easy creation of different predicates using lambda expressions14.

### 3. Declarative Programming: What, Not How

Declarative programming focuses on describing *what* to do instead of *how* to do it. Traditional approaches often require detailed step-by-step instructions, while declarative approaches (like streams) abstract the underlying logic, making the code cleaner and more expressive.

#### Example: Filtering and Transforming Data

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class DeclarativeVsImperativeExample {

    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        System.out.println("Imperative approach:");
        List<Integer> evenSquaresImperative = getEvenSquaresImperative(numbers);
        System.out.println(evenSquaresImperative);

        System.out.println("\nDeclarative approach:");
        List<Integer> evenSquaresDeclarative = getEvenSquaresDeclarative(numbers);
        System.out.println(evenSquaresDeclarative);
    }

    // Imperative approach
    private static List<Integer> getEvenSquaresImperative(List<Integer> numbers) {
        List<Integer> result = new java.util.ArrayList<>();
        for (Integer number : numbers) {
            if (number % 2 == 0) {
                result.add(number * number);
            }
        }
        return result;
    }

    // Declarative approach
    private static List<Integer> getEvenSquaresDeclarative(List<Integer> numbers) {
        return numbers.stream()
                .filter(n -> n % 2 == 0)
                .map(n -> n * n)
                .collect(Collectors.toList());
    }
}
```
#### In this imperative approach:
1. We create a new list to store the results.
2. We explicitly iterate through each number in the input list.
3. We check if each number is even using an if statement.
4. If it's even, we square it and add it to the result list.
5. Finally, we return the result list.
This approach tells the computer exactly how to perform each step of the process.

#### In this declarative approach:
1. We start with the input list and create a stream from it.
2. We declare that we want to filter the stream to keep only even numbers.
3. We declare that we want to transform (map) each remaining number to its square.
4. We declare that we want to collect the results into a list.

#### The key differences are:
1. We don't explicitly manage the iteration; the stream handles that for us.
2. We don't create an intermediate list; the stream pipeline manages the data flow.
3. We describe what we want (even numbers squared), not how to do it step-by-step.
4. The code reads more like a problem statement: "Give me a list of even numbers squared."

### 4. Pure Functions: Ensuring Predictability

A pure function is deterministic, producing the same output for the same input and having no side effects.
The concept of pure functions existed in Java before Java 8, but Java 8 introduced features that made it easier to write and use pure functions. Let's compare pure functions before and after Java 8:

#### Example: Pure Function

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Function;

public class PureFunctionExample {

    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5);

        System.out.println("Before Java 8:");
        List<Integer> squaredBefore = squareListBefore(numbers);
        System.out.println(squaredBefore);

        System.out.println("\nAfter Java 8:");
        List<Integer> squaredAfter = squareListAfter(numbers);
        System.out.println(squaredAfter);
    }

    // Pure function before Java 8
    private static List<Integer> squareListBefore(List<Integer> numbers) {
        List<Integer> result = new ArrayList<>();
        for (Integer number : numbers) {
            result.add(square(number));
        }
        return result;
    }

    // Helper pure function
    private static int square(int x) {
        return x * x;
    }

    // Pure function after Java 8
    private static List<Integer> squareListAfter(List<Integer> numbers) {
        return numbers.stream()
                .map(x -> x * x)
                .toList();
    }
}
```

This example demonstrates pure functions before and after Java 8:

1. Before Java 8:
   - We use a traditional loop to iterate through the list.
   - The `squareListBefore` function is pure as it doesn't modify the input list and always produces the same output for the same input.
   - The `square` helper function is also pure, demonstrating function composition.

2. After Java 8:
   - We use the Stream API and lambda expressions.
   - The `squareListAfter` function is pure, concise, and more declarative.
   - It uses the `map` operation with a lambda expression to square each number.

Both approaches ensure predictability by:
   - Not modifying the input list (immutability).
   - Always producing the same output for the same input (determinism).
   - Having no side effects.

The Java 8 version is more concise and leverages functional programming constructs, making it easier to read and maintain while preserving the purity of the function.

### 5. Lazy Evaluation: Optimizing Performance

Lazy evaluation defers computation until the result is actually needed, which can significantly improve performance by avoiding unnecessary calculations. Traditional approaches often compute eagerly, even if the results are not used, leading to potential inefficiencies.

#### Example: Lazy Evaluation with Streams

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class StreamLazyEvaluationExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // Traditional Approach (Eager Evaluation)
        for (int number : numbers) {
            if (number % 2 == 0) {
                System.out.println("Traditional: " + number);
            }
        }

        // Stream Approach (Lazy Evaluation)
        Stream<Integer> stream = numbers.stream()
            .filter(n -> {
                System.out.println("Filtering " + n);
                return n % 2 == 0;
            })
            .map(n -> {
                System.out.println("Mapping " + n);
                return n * n;
            });

        System.out.println("Stream created, but no computation performed yet");

        // Terminal operation triggers the evaluation
        int firstEvenSquare = stream.findFirst().orElse(0);
        System.out.println("First even square: " + firstEvenSquare);
    }
}

```
In this example, the filter and map operations are lazy. They don't execute until the terminal operation findFirst() is called. Moreover, only the elements needed to produce the result are processed.
#### Benefits
1. **Performance Optimization:**  Lazy evaluation can improve performance by avoiding unnecessary computations.

2. **Working with Infinite Streams:** It allows for the creation of potentially infinite streams of data.

3. **Memory Efficiency:** Only the required data is processed and stored in memory.
#### Challenges
1. **Debugging:** The delayed execution can make debugging more challenging.

2. **Side Effects:** Care must be taken with side effects in lazy operations, as their execution order may not be as expected.

3. **Resource Management:** When working with resources (like file handles), lazy evaluation requires careful management to ensure proper closure.
Understanding and utilizing lazy evaluation in Java can lead to more efficient and expressive code, particularly when working with large datasets or complex computations.

### Conclusion

By embracing functional programming principles, Java developers can write cleaner, more robust, and maintainable code. Concepts like immutability, pure functions, and declarative programming empower developers to focus on *what* needs to be done rather than *how*. Explore these principles and integrate them into your projects for better, modern Java applications.


