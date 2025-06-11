# 📘 Chapter 1: Building Blocks

### 🎯 Chapter 1 Focus

#### 🧱 CLASS STRUCTURE

| Element | Rule |
| --- | --- |
| `public class` | Only one per file; filename must match class name |
| Fields | Can be instance, static, final |
| Methods | Contain logic; can access fields; must be inside classes |
| Comments | `//`, `/* */`, `/** */` for Javadoc |
| Order | `package` → `import` → `class` |

#### 🧮 DATA TYPES

| Type | Size | Default Value | Wrapper Class |
| --- | --- | --- | --- |
| `byte` | 8-bit | `0` | `Byte` |
| `short` | 16-bit | `0` | `Short` |
| `int` | 32-bit | `0` | `Integer` |
| `long` | 64-bit | `0L` | `Long` |
| `float` | 32-bit | `0.0f` | `Float` |
| `double` | 64-bit | `0.0d` | `Double` |
| `char` | 16-bit | `'\u0000'` | `Character` |
| `boolean` | 1-bit | `false` | `Boolean` |

#### 📝 VARIABLE DECLARATION

| Scope | Keyword | Notes |
| --- | --- | --- |
| Local | (none) | Must be initialized before use |
| Instance | (none) or `this.` | Default-initialized |
| Static | `static` | Shared across instances |
| Final | `final` | Must be assigned once |
| Inferred | `var` | Type inferred, only for local variables |

#### 🔁 ORDER OF INITIALIZATION

```
// Class loading:
static fields → static blocks

// Object instantiation:
instance fields → instance blocks → constructor
```

#### 🗑️ GARBAGE COLLECTION

| Concept | Explanation |
| --- | --- |
| Eligibility | No live references to object |
| `null` effect | Makes object eligible, doesn’t trigger GC |
| Method-created | Local objects eligible after method ends (unless returned) |

#### 📦 PACKAGES & IMPORTS

| Element | Notes |
| --- | --- |
| `package` | Must be the first line (non-comment) |
| `import` | Comes after `package` declaration |
| Wildcards | Only imports classes, not sub-packages |
| Redundancy | Duplicate imports are ignored |

#### 🧾 TEXT BLOCKS

```
String html = """
  <html>
    <body>Hello, Java 17!</body>
  </html>""";
```

- Preserves formatting, no need for escape characters.

#### ⚠️ COMMON MISTAKES

| Error Type | Example | Fix/Explanation |
| --- | --- | --- |
| Invalid constructor | `public void Dog() {}` | Remove return type |
| `var` misuse | `var name;` | Must initialize with value |
| Reassign `final` | `final int x = 1; x = 2;` | `final` variables can't be reassigned |
| Bad ordering | `import` before `package` | `package` must come first |
| Uninitialized local | `int x; System.out.println(x);` | Local variables must be initialized |

#### ✅ VALID IDENTIFIERS

| Valid | Invalid | Reason |
| --- | --- | --- |
| `myVariable` | `3data` | Can't start with digit |
| `_temp` | `class` | `class` is a reserved keyword |
| `$price` | `void` | `void` is a keyword |

* * *

### 🧠 Key Concepts & Features in Java 17

#### **Java Class Structure**
-   Blueprint for objects; defines fields and methods.
-   One public class per file; file must match public class name.
-   Inner classes can be static or non-static.
#### **Text Blocks**
-   Defined with `"""`.
-   Preserve formatting and reduce use of escape sequences.
#### **main() Method**
-   Must follow exact signature: `public static void main(String[] args)`
-   Program entry point.
#### **Execution**
-   Compile with `javac`, run with `java`.
-   Always match file and class name for public classes.

* * *

### 📌 Important Topics Covered

- **Packages and Imports**: Use to organize and access classes.
- **Constructors**: No return type. Auto-added if none is provided.
- **Object Creation**: Uses `new` keyword.
- **Variables**: Understand primitive vs. reference, scope, and default values.
- **var**: Type inference for local variables.
- **Garbage Collection**: JVM handles memory cleanup; objects must be unreachable.
- **Initialization Order**: Know how static/instance blocks and constructors are triggered.

* * *

### 💡 Notable Practices

- Use `var` wisely; improves readability but can hide type complexity.
- Always initialize local variables.
- Be cautious with constructor overloading.
- Watch for shadowing between local and instance variables.

* * *

### 🆕 Java 17 vs. Earlier Versions

- Introduces officially supported text blocks.
- Supports local variable type inference (`var`).
- Better GC performance and modern syntax features.

* * *

### 📝 Exam Tips

- Know exact `main()` method signature.
- Watch for invalid constructors disguised as methods.
- Understand how and when variables are initialized.
- GC does not mean memory is immediately freed.
- `this` is used to refer to the current object instance.

* * *

### ❓ Realistic Exam Question Traps

- Confusing constructor/method:
```
public void Dog() {} // method, NOT a constructor!
```
- Using `var` without initialization:
```
var name; // does not compile
```
- Final reassignment:
```
final int a = 1;
a = 2; // compilation error
```
- Accessing uninitialized local variables:
```
int x;
System.out.println(x); // error
```

* * *

### 🔍 Key Exam Concepts

| Concept | Summary |
| --- | --- |
| `var` | Local variable type inference, not allowed on fields or method params. |
| Garbage Collection | Based on reachability, not null assignment. |
| main() Signature | Must be `public static void main(String[] args)` exactly. |
| Initialization Order | Static → Instance → Constructor. |
| Identifier Rules | Can't start with digits, avoid keywords, `$` and `_` allowed. |
| Scope | Local > instance if name overlaps. |
| Reference vs Primitive | Reference types point to objects, primitives hold actual data. |