# 📘 Chapter 9: Collections and Generics

### 🎯 Chapter 9 Focus

This chapter covers the **Java Collections Framework** and **generics**, two of the most heavily tested and conceptually challenging areas for the OCP exam:

- Lists, Sets, Queues, Maps
- Collection operations
- Sorting with `Comparable` and `Comparator`
- Type safety and generic methods/classes

* * *

### 🔑 Core Collection Interfaces

#### ✅ Collection Types

| Interface | Description | Allows Duplicates | Ordered? | Index Access |
| --- | --- | --- | --- | --- |
| List | Ordered collection with index access | Yes | Yes | Yes |
| Set | No duplicate elements | No  | No  | No  |
| Queue | FIFO order, elements for processing | Yes | Yes | Sometimes |
| Deque | Double-ended queue | Yes | Yes | No  |
| Map | Key-value pairs | Keys: No, Values: Yes | No  | Key-based |

#### ✅ Implementations

| Type | Features | Notes |
| --- | --- | --- |
| ArrayList | Fast random access, resizable | Backed by array |
| LinkedList | Efficient inserts/removals at both ends | Implements both List and Deque |
| HashSet | Unordered, backed by hash table | Allows one null |
| TreeSet | Sorted, no nulls allowed | Uses `compareTo()` or `Comparator` |
| ArrayDeque | Fast for add/remove at both ends | Does not allow null elements |
| HashMap | Unordered key-value store | Allows one null key, many null values |
| TreeMap | Sorted by key, no null keys | Throws `NullPointerException` if key is null |

> ✅ Example:

```
List<String> names = new ArrayList<>();
names.add("John");
names.add("John"); // List allows duplicates

Set<String> ids = new HashSet<>();
ids.add("001");
ids.add("001"); // Duplicate ignored
```

* * *

### 🛠️ Common Collection Operations

| Operation | Method | Example |
| --- | --- | --- |
| Add element | `add()` | `list.add("x")` |
| Remove element | `remove()` | `set.remove("x")` |
| Check presence | `contains()` | `queue.contains("y")` |
| Size | `size()` | `map.size()` |
| Empty check | `isEmpty()` | `stack.isEmpty()` |
| Clear collection | `clear()` | `list.clear()` |
| Condition remove | `removeIf()` | `list.removeIf(x -> x.isEmpty())` |
| Get element by key | `get(key)` | `map.get("id")` |

> ✅ Example:

```
List<String> birds = new ArrayList<>();
birds.add("hawk");
birds.remove("hawk");
birds.remove("cardinal"); // false, not found
```

* * *

### 🔃 Sorting Collections

#### Comparable (Natural Order)

- Implement `Comparable<T>` interface
- Defines how elements are compared naturally
- `compareTo()` returns:
-   `0` if equal
-   Negative if less than
-   Positive if greater

> ✅ Example:

```
public class Duck implements Comparable<Duck> {
  private String name;
  public int compareTo(Duck d) {
    return name.compareTo(d.name);
  }
}
```

#### Comparator (Custom Order)

- Implement custom sorting externally using lambda or class
- Used with `Collections.sort()` or `TreeSet/TreeMap`

> ✅ Example:

```
List<String> list = Arrays.asList("b", "a", "c");
Collections.sort(list, (a, b) -> b.compareTo(a)); // descending
```

* * *

### 🎲 Using Generics

- Provide type safety at compile time
- Avoids casting and runtime exceptions
- Work with generic classes, methods, wildcards

> ✅ Generic declaration:

```
List<String> names = new ArrayList<>();
String first = names.get(0); // No casting needed
```

#### Wildcards in Generics

| Wildcard | Meaning | Usage |
| --- | --- | --- |
| `<?>` | Unknown type | Read-only access |
| `<? extends T>` | Unknown subtype of T (upper bound) | Safe for reading, not writing |
| `<? super T>` | Unknown supertype of T (lower bound) | Allows writing T or subtype |

> ✅ Example:

```
List<? extends Number> list = List.of(1, 2.0); // Read-only
List<? super Integer> list2 = new ArrayList<>();
list2.add(10); // OK
```

* * *

### ✏️ Creating Generic Types and Methods

#### Generic Class:

```
public class Crate<T> {
  private T item;
  public void pack(T item) { this.item = item; }
  public T unpack() { return item; }
}
```

#### Generic Method:

```
public <T> T pick(List<T> items) {
  return items.get(0);
}
```

- You can use different type parameters across methods and classes.
- Type inference helps reduce verbosity.

* * *

### ⚡ Diamond Operator (<>)

- Tells the compiler to infer the type from context.

```
List<String> list = new ArrayList<>();
```

- Reduces redundancy and improves readability.

* * *

### ⚠️ Exam Traps and Tips

- Cannot create instances of a generic type directly: `new T()` ❌
- Cannot use primitives as generic types: `List<int>` ❌ (Use `List<Integer>`)
- Watch for heap pollution when using raw types.
- Cannot add to a `List<? extends T>` (only read allowed)
- `TreeSet`/`TreeMap` must use comparable elements or a comparator
- `Map` is **not** a subtype of `Collection`

* * *

### 🧠 Concepts to Watch (Mini-Review Table)

| Concept | Description/Example |
| --- | --- |
| Collection vs. Map | Map not a subtype of Collection |
| `compareTo()` vs `compare()` | Natural vs. custom sort order |
| Diamond operator | Shortens code: `new ArrayList<>()` |
| Generics wildcards | `extends` = read-only, `super` = write allowed |
| Lower bounded wildcard | `List<? super String>` allows String and above |
| Upper bounded wildcard | `List<? extends Number>` allows Number and below |
| TreeSet, TreeMap | Require Comparable or Comparator, do NOT allow `null` |
| Varargs with Generics | Can cause warning due to type erasure |