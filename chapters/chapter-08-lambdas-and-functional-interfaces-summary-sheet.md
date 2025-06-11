# 📘 Chapter 8: Lambdas and Functional Interfaces

### 🎯 Chapter 8 Focus

This chapter focuses on the powerful **lambda expressions** and **functional interfaces** in Java:

- Writing lambdas and using functional interfaces
- Method references (short-form lambdas)
- Variable scoping rules inside lambdas
- Built-in functional interfaces like `Predicate`, `Consumer`, `Function`, etc.

* * *

### 🧐 What is a Lambda Expression?

- A lambda is an anonymous block of code you can pass around.
- Works with **functional interfaces** (interfaces with exactly **one abstract method**).

> ✅ Example:

```
Predicate<String> p = s -> s.isEmpty();
```

> ✅ Full Syntax:

```
(String a, String b) -> { return a.equals(b); }
```

> ✅ Short Syntax:

```
a -> a.equals("Hello")
```

> ✅ Parameter types are optional. Parentheses can be omitted for **one untyped parameter**.

* * *

### 🖐️ Functional Interfaces

- Any interface with **a single abstract method** (SAM).
- May include default, static, and private methods — they **don’t count** toward the SAM rule.

#### ✅ Common Built-in Functional Interfaces

| Interface | Method Name | Description |
| --- | --- | --- |
| `Predicate<T>` | `boolean test(T)` | Test condition |
| `Consumer<T>` | `void accept(T)` | Accept and act |
| `Supplier<T>` | `T get()` | Provides a value |
| `Function<T,R>` | `R apply(T)` | Maps input to result |
| `UnaryOperator<T>` | `T apply(T)` | One input, one output, same type |
| `BinaryOperator<T>` | `T apply(T,T)` | Two inputs, one output, same type |

> ✅ Example:

```
Function<String, Integer> f = s -> s.length();
```

* * *

### 🧰 Method References

A method reference is shorthand for a lambda expression.

#### ✅ Types of Method References

|     |     |     |
| --- | --- | --- |
| Type | Syntax | Example |
| Static method | `Class::staticMethod` | `Math::abs` |
| Instance on specific obj | `obj::method` | `System.out::println` |
| Instance on param | `Class::instanceMethod` | `String::length` |
| Constructor | `Class::new` | `ArrayList::new` |

> ✅ Example:

```
Consumer<String> c = System.out::println; // same as s -> System.out.println(s)
```

* * *

### 📂 Lambda Syntax Rules

| Rule | Example |
| --- | --- |
| One parameter, no type | `x -> x + 1` |
| One parameter with type | `(int x) -> x + 1` |
| Multiple parameters | `(x, y) -> x + y` |
| With block and return | `(x, y) -> { return x + y; }` |
| Parentheses required if typed | `(String s) -> s.isEmpty()` |
| Braces optional for one line | `x -> x.length()` |
| Return keyword required in block | `{ return x + y; }` |

> ⚠️ Mixing parameter formats is **not allowed**:

```
(var x, y) -> x + y; // ❌ Does not compile
```

* * *

### 📋 Variable Scope in Lambdas

- Lambdas can access:
-   Instance and static variables freely
-   Local variables **only if final or effectively final**

> ✅ Example:

```
void shout(String msg) {
  String mood = "loud";
  Consumer<String> c = s -> System.out.println(msg + mood + s);
}
```

- Changing `msg` or `mood` after the lambda will cause a **compiler error**.

* * *

### 👀 Deferred Execution

- Lambdas store code to run **later**.
- Example: event handlers, stream filters, etc.

> ✅ Example:

```
List<String> list = List.of("a", "bb", "ccc");
list.removeIf(s -> s.length() == 2);
```

* * *

### 💡 Exam Tips and Traps

- You cannot redeclare a lambda parameter name.
- Parentheses optional for 1 untyped param, but required otherwise.
- `return` only allowed in block-form (`{}`) lambdas.
- Method reference must match **parameter and return type** of expected interface.
- Only **functional interfaces** can be used in lambda expressions.
- `@FunctionalInterface` annotation is optional, but helps detect mistakes.

* * *

### 🧠 Exam Concepts to Watch (Mini-Review Table)

| Concept | Summary + Watch Out For |
| --- | --- |
| Lambda Syntax | Shorter form of method for single abstract method |
| Functional Interface | One abstract method; others must be default/static/private |
| Effectively Final | Don't change variables after declaration if used in lambda |
| Method References | Must match functional interface signature |
| var in Lambdas | All params must use `var` if one does |
| Variable Scope | Local vars must be final/effectively final; instance/static always OK |
| Deferred Execution | Lambda runs **when used**, not when passed |