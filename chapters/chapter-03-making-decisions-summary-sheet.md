# 📘 Chapter 3: Making Decisions

### 🎯 Chapter 3 Focus

#### ⚖️ DECISION-MAKING STATEMENTS

| Statement | What It Does |
| --- | --- |
| `if` | Runs a block of code only if the condition is true. |
| `else` | Runs a different block if the `if` condition is false. |
| `if-else if` | Checks multiple conditions in order. Runs the first true condition block. |
| Pattern Match | Simplifies `instanceof` checks by creating a variable of that type. |

> ✅ Example:

```
int score = 85;
if (score > 90) {
    System.out.println("Great job!");
} else if (score > 75) {
    System.out.println("Nice work!");
} else {
    System.out.println("Keep trying!");
}
```

#### 🔄 SWITCH STATEMENTS & EXPRESSIONS

- Switch is used to choose one of many code blocks based on a variable's value.
- Enhanced switch (`->`) is shorter and avoids fall-through.
- `yield` allows returning a value from a switch block.
- Traditional switch requires `break` to avoid running the next case.

> ✅ Example (enhanced):

```
String day = "FRI";
String result = switch(day) {
    case "MON" -> "Start of week";
    case "FRI" -> "End of week";
    default -> "Midweek";
};
System.out.println(result);
```

#### ✅ ALLOWED SWITCH TYPES

- Legal: `byte`, `short`, `int`, `char`, `String`, `enum`, and wrapper types (e.g., `Integer`).
- Illegal: `boolean`, `long`, `float`, `double`, arrays, objects (unless enums).

> ✅ Example:

```
int num = 2;
switch (num) {
    case 1 -> System.out.println("One");
    case 2 -> System.out.println("Two");
    default -> System.out.println("Other");
}
```

* * *

### 🔁 LOOPS (REPEATING CODE)

#### while & do/while

- `while`: checks the condition first and then runs the loop.
- `do/while`: runs the loop once before checking the condition.

> ✅ Example:

```
int i = 0;
while (i < 3) {
  System.out.println("While: " + i);
  i++;
}

i = 0;
do {
  System.out.println("Do/While: " + i);
  i++;
} while (i < 3);
```

#### for Loop (Classic)

- Has 3 parts: initialization, condition, and increment/decrement.
- Best when you know how many times to loop.

> ✅ Example:

```
for (int i = 0; i < 5; i++) {
    System.out.println("Count: " + i);
}
```

#### for-each Loop

- Simplifies looping through arrays or collections.
- You can read each element but not modify the array directly.

> ✅ Example:

```
String[] fruits = {"Apple", "Banana", "Cherry"};
for (String fruit : fruits) {
    System.out.println(fruit);
}
```

* * *

### 🔃 FLOW CONTROL KEYWORDS

|     |     |     |
| --- | --- | --- |
| Keyword | What It Does | Example Use |
| `break` | Stops the current loop or switch block immediately. | `if (x > 10) break;` |
| `continue` | Skips the current loop step and goes to the next. | `if (x % 2 == 0) continue;` |
| `return` | Exits the method and optionally returns a value. | `return "Done";` |
| Label | Names a loop to target with `break`/`continue`. | `outer: for(...) { break outer; }` |

> ✅ Example:

```
outer: for (int i = 0; i < 3; i++) {
  for (int j = 0; j < 3; j++) {
    if (j == 1) break outer;
    System.out.println(i + "," + j);
  }
}
```

* * *

### 🧠 PATTERN MATCHING (`instanceof`)

- Allows checking an object's type and assigning it to a variable in one step.
- The variable is scoped only to the `if` block.

> ✅ Example:

```
Object data = "Java";
if (data instanceof String text) {
    System.out.println(text.toLowerCase());
} // 'text' not accessible here
```

* * *

### ❌ UNREACHABLE CODE

- Code that cannot be executed causes a compile error.
- Common cases: code after `return`, `break`, or `throw`.

> ✅ Example:

```
return;
System.out.println("This is unreachable"); // Compile error
```

* * *

### ❓ Realistic Exam Traps

- `instanceof` pattern variable used outside the `if` → compile error
- `switch(null)` → NullPointerException
- Using unsupported types in `switch` like `boolean`, `float` → compile error
- Redefining a variable in the same scope → compile error
- Modifying an array in a for-each loop → won’t change original array

* * *

### 🔍 Concepts to Understand for the Exam

|     |     |
| --- | --- |
| Concept | Simple Explanation + Example |
| Pattern Matching | Combines check and cast: `if (x instanceof String s)` |
| switch expressions | Clean syntax using `->`; use `yield` for blocks. |
| Infinite loops | `while(true)` loops forever unless there’s a `break`. |
| Loop labels | Help control nested loops: `break outer;` |
| for-each vs for | for-each: clean read; for: more control and flexibility. |

* * *

### ✨ Easy Tips for Remembering

- Think of `switch` as smart `if-else` chains.
- Always trace loop condition flow to avoid infinite loops.
- Pattern matching = cleaner `instanceof` + cast.
- Use `do/while` when code must run at least once.
- Practice identifying when a loop should break or continue.

* * *

### 🔐 What to Know for the OCP Exam

- Know valid types for `switch` and pattern matching rules.
- Understand flow control with `return`, `break`, `continue`, and labels.
- Always spot unreachable code or incorrect block scopes.
- Recognize the difference between `while`, `do/while`, `for`, and `for-each`.