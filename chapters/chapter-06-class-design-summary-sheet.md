# 📘 Chapter 6: Class Design

### 🎯 Chapter 6 Focus

This chapter focuses on **object-oriented class design principles**:

- Inheritance
- Class modifiers (`final`, `abstract`)
- Access modifiers and `this`/`super`
- Constructors and initialization order
- Overriding and hiding methods/variables
- Abstract classes
- Creating immutable objects

* * *

### 🔍 Understanding Inheritance

- Inheritance allows a subclass to access public/protected members of a superclass.
- All classes (except `Object`) inherit from another class.

> ✅ Example:

```
public class Animal {
  protected String name;
}

public class Dog extends Animal {
  public void bark() {
    System.out.println(name + " says Woof!");
  }
}
```

* * *

### ✉️ Declaring a Subclass

- Use `extends` to inherit from another class.
- A class can only extend **one** class (single inheritance).
- Subclasses inherit accessible members (not `private`).

> ✅ Example:

```
public class Lion extends Animal {
  public void roar() {
    System.out.println("Roar! I am " + name);
  }
}
```

* * *

### 💪 Accessing `this` and `super`

- `this` refers to the current instance.
- `super` refers to the parent class.
- Use `super()` to call parent constructors.

> ✅ Example:

```
public class Reptile {
  public Reptile(String type) {
    System.out.println("Created a " + type);
  }
}

public class Lizard extends Reptile {
  public Lizard() {
    super("Lizard");
  }
}
```

* * *

### 📚 Constructors and Initialization

- If no constructor is written, Java adds a **no-arg** constructor.
- The **first line** of any constructor must be `this()` or `super()`.
- If you forget to call a parent's constructor that doesn't have a no-arg version, you'll get a compile error.

> ✅ Example:

```
class Parent {
  public Parent(int x) {}
}

class Child extends Parent {
  public Child() {
    super(10); // Required, no default Parent()
  }
}
```

* * *

### 🔄 Order of Initialization

1. Static fields/blocks (top to bottom)
2. Instance fields/blocks (top to bottom)
3. Constructor runs last

> ✅ Example:

```
class Example {
  static { System.out.print("1"); }
  { System.out.print("2"); }
  public Example() { System.out.print("3"); }
}

// Output: 123
```

* * *

### ➕ Overriding Methods

- Child method has **same name, parameters, and return type** (or subtype).
- Cannot override `final` methods.
- Overridden method must be **at least as accessible**.

> ✅ Example:

```
class Bird {
  public void fly() { System.out.println("Bird flies"); }
}

class Eagle extends Bird {
  @Override
  public void fly() { System.out.println("Eagle soars"); }
}
```

* * *

### ❌ Hiding Methods and Variables

- Static methods are **hidden**, not overridden.
- Variables are **hidden** (reference type matters).
- Don’t hide variables/methods in practice: it’s confusing.

> ✅ Example:

```
class Parent {
  static void greet() { System.out.println("Hi from Parent"); }
}

class Child extends Parent {
  static void greet() { System.out.println("Hi from Child"); }
}

Parent p = new Child();
p.greet(); // prints: Hi from Parent
```

* * *

### 📜 Abstract Classes

- Use `abstract` when you want a class to be a blueprint.
- Cannot instantiate.
- May include **abstract methods** (no body) and regular methods.

> ✅ Example:

```
abstract class Animal {
  abstract void makeSound();
}

class Cat extends Animal {
  void makeSound() { System.out.println("Meow"); }
}
```

* * *

### 🔒 Immutable Objects

- Use `final` fields, no setters.
- Defensive copy to protect from outside mutation.

> ✅ Example:

```
public class Animal {
  private final List<String> foods;

  public Animal(List<String> input) {
    this.foods = new ArrayList<>(input); // Defensive copy
  }
  public List<String> getFoods() {
    return List.copyOf(foods);
  }
}
```

* * *

### ⚠️ Common Exam Traps and Tips

- Abstract class can have both abstract and non-abstract methods.
- Calling `super()` must be first line of constructor.
- Overridden methods must match signature and have equal or looser access.
- `final` class can't be extended.
- Class can't be `private` or `protected` if it's top-level.

* * *

### 🧠 Concepts to Watch

| Concept | Summary + Example |
| --- | --- |
| Inheritance | Subclasses get access to non-private members of superclass |
| Access Modifiers | private < package < protected < public |
| Constructor Order | static → instance → constructor |
| Overriding vs Hiding | Static = hiding, Instance = overriding |
| `final` Class/Method | Prevents extension or overriding |
| `super()` / `this()` | Must be first line in constructor |
| Abstract Class | Cannot instantiate, used for base behavior |
| Immutable Design | Use final fields, no setters, defensive copies |