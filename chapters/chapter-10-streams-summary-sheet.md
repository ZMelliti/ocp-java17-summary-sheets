# 📘 Chapter 10: Streams

### 🎯 Chapter 10 Focus

This chapter is all about **Java Streams**, a powerful tool for functional-style operations:

- Creating and processing streams
- Working with Optional and functional interfaces
- Stream pipelines and chaining
- Primitive streams
- Collecting and grouping data

* * *

### 🔗 Stream Pipeline Basics

- A **stream** is a sequence of elements supporting sequential and parallel operations.
- Consists of **source**, **intermediate operations**, and **terminal operation**.

> ✅ Example:

```
List<String> names = List.of("Toby", "Anna", "Leroy", "Alex");
names.stream()
     .filter(s -> s.length() == 4)
     .sorted()
     .limit(2)
     .forEach(System.out::println); // Anna, Alex
```

#### Pipeline Parts:

| Part | Description |
| --- | --- |
| Source | Creates stream (e.g. `List.stream()`) |
| Intermediate | Transforms stream (e.g. `filter()`) |
| Terminal | Produces result (e.g. `forEach()`) |

* * *

### ⚡ Creating Streams

#### ✅ From Collections:

```
Stream<String> stream = list.stream();
```

#### ✅ From static methods:

```
Stream<Integer> s = Stream.of(1, 2, 3);
Stream<Double> infinite = Stream.generate(Math::random);
```

#### ✅ Parallel Streams:

```
list.parallelStream();
```

* * *

### 🌐 Common Intermediate Operations

| Method | Purpose |
| --- | --- |
| `filter()` | Filters elements based on predicate |
| `map()` | Transforms each element |
| `flatMap()` | Flattens nested structures |
| `distinct()` | Removes duplicates |
| `sorted()` | Sorts elements |
| `peek()` | Used for debugging |
| `limit()` | Limits the number of elements |
| `skip()` | Skips first n elements |

* * *

### 📅 Common Terminal Operations

| Method | Purpose |
| --- | --- |
| `forEach()` | Performs action for each element |
| `count()` | Returns count |
| `collect()` | Reduces to collection |
| `reduce()` | Combines elements using accumulator |
| `min()` / `max()` | Finds min/max using comparator |
| `anyMatch()` | Returns true if any match |
| `allMatch()` | Returns true if all match |
| `noneMatch()` | Returns true if none match |
| `findAny()` / `findFirst()` | Retrieves element (Optional) |

> ✅ Reduce Example:

```
int sum = Stream.of(1, 2, 3).reduce(0, (a, b) -> a + b); // 6
```

* * *

### 🤖 Primitive Streams

- Specialized for performance: `IntStream`, `LongStream`, `DoubleStream`

#### ✅ Methods:

| Method | Description |
| --- | --- |
| `sum()` | Total of elements |
| `average()` | Returns `OptionalDouble` |
| `range(a,b)` | Returns \[a, b) |
| `rangeClosed()` | Returns \[a, b\] |

> ✅ Example:

```
IntStream.range(1, 4).forEach(System.out::print); // 123
```

* * *

### 🔍 Optional in Streams

- Return type of terminal ops like `findFirst()`, `min()`
- Optional methods:
-   `isPresent()`, `orElse()`, `orElseThrow()`
-   `ifPresent(Consumer)`

> ✅ Example:

```
Optional<String> opt = Stream.of("a", "b").findFirst();
opt.ifPresent(System.out::println);
```

* * *

### ➕ Collectors and Grouping

| Collector | Purpose |
| --- | --- |
| `toList()` | Collects to List |
| `joining()` | Joins strings |
| `counting()` | Counts elements |
| `mapping()` | Maps and collects |
| `groupingBy()` | Groups by function |
| `partitioningBy()` | Groups by boolean test |
| `teeing()` (Java 12+) | Merges two collectors into one |

> ✅ Grouping Example:

```
Map<Integer, List<String>> map =
  Stream.of("lion", "tiger", "bear")
        .collect(Collectors.groupingBy(String::length));
```

* * *

### ⚠️ Common Pitfalls and Exam Traps

- Streams can be used **once**; reuse = `IllegalStateException`
- `peek()` shouldn't change state (for debugging only)
- `filter()` must return `boolean`
- Cannot access effectively non-final variables in lambdas
- Don’t call terminal ops inside intermediate ops
- Avoid **stateful** operations in parallel streams (unpredictable results)

> ❌️ Example of bad stateful lambda:

```
List<Integer> data = new ArrayList<>();
IntStream.range(1, 5).parallel().forEach(data::add); // unpredictable
```

* * *

### 🧠 Concepts to Watch (Review Table)

| Concept | Explanation + Example |
| --- | --- |
| Stream Pipeline | Source → 0+ Intermediates → Terminal |
| Optional with Stream | Use `orElse`, `orElseThrow`, `ifPresent` |
| Lazy Evaluation | Nothing runs until terminal op |
| Collectors API | `toList`, `joining`, `groupingBy`, `teeing` |
| Primitive Streams | Efficient versions with `IntStream`, `DoubleStream` |
| peek() | Use only for debugging, don’t modify state |
| Terminal Ops | `collect()`, `reduce()`, `forEach()`, `count()` |
| Parallel Streams | Fast but avoid side-effects |