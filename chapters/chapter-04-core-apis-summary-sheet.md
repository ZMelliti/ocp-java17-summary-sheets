# 📘 Chapter 4: Core APIs

### 🎯 Chapter 4 Focus

This chapter explores the most commonly used APIs in Java:

- `String` and `StringBuilder`
- Arrays (1D and multi-dimensional)
- The `Math` class
- Date and Time APIs (LocalDate, LocalTime, LocalDateTime, Period, Duration, ZonedDateTime, etc.)

* * *

### 📖 Working with Strings

- `String` is **immutable**: once created, it cannot be changed.
- You can create a String with `String s = "hello";` or `String s = new String("hello");`.
- Java supports **text blocks** using triple quotes:

```
String text = """
  This is a text block
  spanning multiple lines""";
```

#### Key String Methods:

| Method | Description | Example |
| --- | --- | --- |
| `length()` | Returns string length | `"Java".length()` → 4 |
| `charAt(int index)` | Returns character at position | `"Java".charAt(1)` → 'a' |
| `substring()` | Gets part of a string | `"Java".substring(1, 3)` → "av" |
| `indexOf()` | Finds index of a char or substring | `"hello".indexOf('e')` → 1 |
| `toUpperCase()` | Converts to upper case | `"java".toUpperCase()` → "JAVA" |
| `equals()` | Checks if contents are equal | `"abc".equals("abc")` → true |
| `equalsIgnoreCase()` | Ignores case while comparing | `"abc".equalsIgnoreCase("ABC")` → true |
| `startsWith()` / `endsWith()` | Checks beginning or end | `"hello".startsWith("he")` → true |
| `contains()` | Checks if string contains text | `"hello".contains("ll")` → true |
| `replace()` | Replaces characters or substrings | `"abc".replace("a", "z")` → "zbc" |

* * *

### 🔨 StringBuilder

- **Mutable** version of String: can change value without creating new object.
- More efficient when doing many string modifications.
- Common in loops and string processing tasks.

> ✅ Example:

```
StringBuilder sb = new StringBuilder("hello");
sb.append(" world").insert(5, ",").reverse();
System.out.println(sb); // prints reversed version
```

#### Key StringBuilder Methods:

|  Method   |  What It Does   |
| --- | --- |
| `append()` | Adds to the end of the builder |
| `insert()` | Inserts at a specific index |
| `delete()` | Deletes characters from the builder |
| `reverse()` | Reverses the characters |
| `toString()` | Converts builder to string |

* * *

### ⚖️ Understanding Equality

|  Comparison   |  Behavior   |
| --- | --- |
| `==` | Compares if two references point to the same object |
| `.equals()` | Compares actual contents |

- For `String`, `.equals()` compares characters.
- For `StringBuilder`, `.equals()` is not overridden, so it behaves like `==`.

> ✅ Example:

```
String a = "abc";
String b = new String("abc");
System.out.println(a == b);       // false
System.out.println(a.equals(b));  // true
```

* * *

### 🧰 Arrays

- Arrays hold **fixed-size** elements of the same type.

> ✅ Example:

```
int[] numbers = new int[3];
numbers[0] = 10;
int[] values = {1, 2, 3};
```

#### Array Utility Methods:

|  Method   |  Purpose   |
| --- | --- |
| `Arrays.sort()` | Sorts an array |
| `Arrays.binarySearch()` | Searches sorted array, returns index |
| `Arrays.equals()` | Compares two arrays |
| `Arrays.compare()` | Returns <0, 0, or >0 for order comparison |
| `Arrays.mismatch()` | Finds first index of mismatch |

#### Multidimensional Arrays:

- Example:

```
int[][] grid = new int[2][3];
grid[0][0] = 5;
```

- Arrays can be **jagged** (inner arrays of different lengths).

* * *

### 🧩 Varargs

- Special syntax that allows passing **zero or more arguments** to a method.
- Declared as the **last** parameter: `public void printNames(String... names)`

> ✅ Example:

```
printNames("Tom", "Sue", "Bob");
```

* * *

### 📐 Math APIs

|  Method   |  Purpose   |  Example   |
| --- | --- | --- |
| `Math.max(a, b)` | Returns the larger of two | `Math.max(3, 5)` → 5 |
| `Math.min(a, b)` | Returns the smaller | `Math.min(3, 5)` → 3 |
| `Math.round(x)` | Rounds to nearest whole number | `Math.round(2.6)` → 3 |
| `Math.ceil(x)` | Rounds up | `Math.ceil(2.1)` → 3.0 |
| `Math.floor(x)` | Rounds down | `Math.floor(2.9)` → 2.0 |
| `Math.pow(a, b)` | Raises a to the power of b | `Math.pow(2, 3)` → 8.0 |
| `Math.random()` | Random number \[0.0, 1.0) | → 0.1234567 (example) |

* * *

### ⏳ Working with Dates and Times

Java 8+ introduced powerful immutable date/time APIs:

- `LocalDate` → date without time
- `LocalTime` → time without date
- `LocalDateTime` → date and time
- `Period` → date difference (years, months, days)
- `Duration` → time difference (hours, minutes, seconds)
- `ZonedDateTime` → full timestamp with timezone

> ✅ Example:

```
LocalDate date = LocalDate.of(2024, 12, 25);
LocalTime time = LocalTime.of(15, 30);
LocalDateTime dt = LocalDateTime.of(date, time);
Period p = Period.ofMonths(2);
date = date.plus(p); // adds 2 months
```

#### Common Date-Time Methods:

|  Method   |  Purpose   |
| --- | --- |
| `now()` | Gets current date/time |
| `of(...)` | Creates specific date/time |
| `plusX()` / `minusX()` | Adds or subtracts units |
| `isBefore()` / `isAfter()` | Compares dates/times |

* * *

### 🔎 Exam Tips & Traps

- String is immutable, StringBuilder is mutable.
- Use `.equals()` to compare contents, not `==`.
- Arrays must be sorted before binary search.
- Varargs must be the **last** parameter.
- `Period` is used with dates, `Duration` with times.
- Date/time classes are immutable: must assign the result!
- Know time zone daylight saving adjustments with `ZonedDateTime`.