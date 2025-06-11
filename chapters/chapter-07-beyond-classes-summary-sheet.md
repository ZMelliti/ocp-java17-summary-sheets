# 📘 Chapter 7: Beyond Classes

### 🎯 Chapter 7 Focus

This chapter extends object-oriented programming by exploring top-level types **other than classes**:

- Interfaces
- Enums
- Sealed classes and interfaces
- Records
- Nested classes
- Polymorphism

* * *

### 🔐 Interfaces

- Define a **contract** that a class must implement.
- Can contain **abstract methods**, **default methods**, **static methods**, and **constants**.
- Can extend multiple interfaces (multiple inheritance).

> ✅ Example:

```
interface Flyable {
  int MAX_SPEED = 100; // implicitly public static final
  void fly();           // implicitly public abstract
  default void hover() {
    System.out.println("Hovering");
  }
}
```

#### Types of Interface Members:

| Member Type | Notes |
| --- | --- |
| Constants | `public static final` by default |
| Abstract Methods | `public abstract` by default |
| Default Methods | Include a body; must be `default` |
| Static Methods | Called on interface, not instance |
| Private Methods | Helper methods inside interface |
| Private Static | Static helper methods inside interface |

* * *

### 🔢 Enums

- Represent **fixed set of constants**.
- Can have constructors, fields, methods, and implement interfaces.
- Can be used in `switch` statements.

> ✅ Example:

```
enum Day {
  MON, TUE, WED;
  public boolean isWeekday() {
    return this != SAT && this != SUN;
  }
}
```

- Enums **cannot extend classes**, but can implement interfaces.
- If method is `abstract`, each enum must implement it.

* * *

### 🔒 Sealed Classes & Interfaces (Java 17)

- Used to **restrict inheritance**.
- Subclasses must be marked `final`, `sealed`, or `non-sealed`.

> ✅ Example:

```
public sealed class Animal permits Dog, Cat {}
final class Dog extends Animal {}
non-sealed class Cat extends Animal {}
```

- `permits` clause can be omitted if all subclasses are in the same file.
- Sealed **interfaces** work similarly.

* * *

### 📜 Records (Java 16+)

- Concise way to create **immutable data classes**.
- Compiler auto-generates constructor, accessors, `equals()`, `hashCode()`, `toString()`.

> ✅ Example:

```
public record Person(String name, int age) {}
```

- You can define custom constructors and methods.
- All fields are `final`.
- Records **cannot have instance variables** outside of the header.

* * *

### 📚 Nested Classes

#### Types of Nested Classes:

|  Type   |  Needs Outer Instance   |  Can Be Static   |  Access to Outer   |  Typical Use   |
| --- | --- | --- | --- | --- |
| Inner | Yes | No  | Yes | Grouping logic together |
| Static Nested | No  | Yes | No  | Helper classes |
| Local | Yes (in method) | No  | Yes | One-time local logic |
| Anonymous | Yes (in method) | No  | Yes | Inline class definitions |

> ✅ Example:

```
class Outer {
  class Inner {}
  static class StaticNested {}
}
```

#### Nested Access Notes:

- Local and anonymous classes can only access **final or effectively final variables** from enclosing method.
- Inner classes can access private members of the outer class.

* * *

### 🛠️ 6. Polymorphism

- One object can be referred to by different types:
-   Its own type
-   A superclass
-   An interface it implements

#### Example:

```
interface Walkable { void walk(); }
class Animal {}
class Dog extends Animal implements Walkable {
  public void walk() {}
}

Walkable w = new Dog();
Animal a = new Dog();
```

#### Casting:

- **Upcasting** (to parent): done automatically
- **Downcasting** (to child): needs explicit cast

> ✅ Example:

```
Animal a = new Dog();
Dog d = (Dog) a;
```

* * *

### ⚠️ Common Exam Tips and Traps

- `interface` methods are **public abstract** by default
- You **can’t instantiate interfaces or abstract classes**
- **Enums can have constructors and methods**, but cannot extend classes
- `sealed` classes must permit all subclasses explicitly
- **Records are immutable**: no setters, only accessors
- **Nested classes** create multiple `.class` files (like `Outer$Inner.class`)
- **Anonymous classes** can’t extend multiple interfaces or classes
- **Use** `instanceof` **carefully** when casting

* * *

### 🧠 Concepts to Watch

| Concept | Quick Summary + Tip |
| --- | --- |
| Interface Members | Implicitly public, abstract (methods), final static (variables) |
| Enum Behavior | Fixed constants, switch-friendly, can define methods |
| Sealed Rules | Subtypes must be final/sealed/non-sealed |
| Records | Immutable classes with built-in constructor & accessors |
| Nested Access | Inner = access outer; local = needs effectively final variables |
| Polymorphism | Use parent or interface references to point to subclass objects |
| Method Hiding | Static = hidden, Instance = overridden |