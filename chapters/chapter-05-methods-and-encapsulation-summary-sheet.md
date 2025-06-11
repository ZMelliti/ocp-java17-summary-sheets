# 📘 Chapter 5: Methods and Encapsulation

### 🎯 Chapter 5 Focus

This chapter explores core concepts about defining, calling, and organizing methods in Java. It also touches on access control, overloading, static members, and data passing.

* * *

### 🔍 Method Declaration Components

A method declaration includes:

- **Access Modifier**: `public`, `private`, `protected`, or default (package-private).
- **Optional Specifiers**: `static`, `final`, etc.
- **Return Type**: `void` or a specific type.
- **Method Name**: Follows identifier rules.
- **Parameter List**: Types and variable names.
- **Exception List**: Optional.
- **Method Body**: Code block `{}`.

> ✅ Example:

```
public static int add(int x, int y) throws Exception {
    return x + y;
}
```

* * *

### 🔑 Access Modifiers

| Modifier | Who Can Access |
| --- | --- |
| `private` | Same class only |
| (default) | Same package |
| `protected` | Same package or subclasses |
| `public` | Anywhere |

> ✅ Example:

```
class Animal {
  private void sleep() {}
  protected void run() {}
  public void breathe() {}
}
```

* * *

### ✨ Static Methods and Variables

- `static` members belong to the **class**, not an instance.
- Called using the class name (e.g., `Math.random()`).
- Static blocks are used for initialization.

> ✅ Example:

```
class MathUtils {
  static int count = 0;
  static void increment() { count++; }
}
```

* * *

### ✍️ Declaring and Using Local & Instance Variables

- **Local variables** exist inside a method. Must be initialized before use.
- **Instance variables** are declared in the class but outside any method.
- **Effectively final**: local variables that are not reassigned after being initialized.

> ✅ Example:

```
void greet() {
  final String message = "Hi";
  System.out.println(message);
}
```

* * *

### 🧩 Varargs (Variable-Length Arguments)

- Declared with `...`, e.g., `print(String... names)`
- Must be the **last** parameter.
- Treated like an array inside the method.

> ✅ Example:

```
public void printNames(String... names) {
  for (String name : names) {
    System.out.println(name);
  }
}
```

* * *

### 🌟 Overloading Methods

- Multiple methods with **same name** but **different parameters**.
- Can differ in type, number, or order of parameters.
- Return type alone is NOT enough.

> ✅ Example:

```
void eat(int qty) {}
void eat(String food) {}
void eat(String food, int qty) {}
```

#### Overload Resolution Order:

1. Exact match
2. Larger primitive
3. Autoboxed match
4. Varargs

* * *

### 🔧 Passing and Returning Data

- **Primitive types** are passed by value.
- **Objects**: reference is copied, but points to same object.

> ✅ Example:

```
void modify(int x) { x++; } // does not affect caller's x
void modify(StringBuilder sb) { sb.append("!"); } // affects original object
```

* * *

### 🛡️ Final and Effectively Final

- `final` variables cannot be reassigned.
- **Effectively final** means the variable is never reassigned, even without `final` keyword.

> ✅ Example:

```
void sayHi() {
  String greeting = "Hello"; // effectively final
  Runnable r = () -> System.out.println(greeting);
}
```

* * *

### 🏷️ Method Signature

- Combination of method name and **parameter types** (not return type).
- Used to resolve overloading.

> ✅ Example:

```
void bake(int time) {}
void bake(double time) {} // valid overload
```

* * *

### ⚠️ Common Exam Traps and Tips

- You can **only have one varargs** parameter, and it must come last.
- Method **return type is NOT part of the signature**.
- Overloaded methods must differ in **more than just return type**.
- Local variables must be **initialized before use**.
- `final` applies to the variable reference, not the object itself.

* * *

### 🧠 Watch For These Concepts

|  Concept   |  Quick Reminder + Example   |
| --- | --- |
| Static Access | `static` methods cannot access instance variables directly |
| Final Variable | Can’t be reassigned: `final int x = 5;` |
| Method Overload | Same name, different parameters |
| Varargs Rules | One per method, last in the parameter list |
| Access Levels | Know differences: private vs. package vs. protected |