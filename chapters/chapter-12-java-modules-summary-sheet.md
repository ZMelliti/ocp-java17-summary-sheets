# 📘 Chapter 12: Java Modules

### 🎯 Chapter 12 Focus

- Java Platform Module System (JPMS)
- `module-info.java` syntax and directives
- Named, automatic, and unnamed modules
- Compiling/running modules with `javac`, `java`, `jar`, `jdeps`, `jlink`
- Creating modular applications and services
- Circular dependencies and migration strategies
- Common pitfalls and exam questions

* * *

### 🔍 What is a Module?

A **module** is a logical group of related Java packages that can explicitly declare dependencies and control which packages are exposed to other modules.

#### 📦 Module Includes:

- Java `.class` files inside packages
- A `module-info.java` file
- Optional resources (images, properties, etc.)

#### ✅ Example Structure:

```
project-root/
└── zoo.animal.feeding/
    ├── module-info.java
    └── zoo/animal/feeding/Task.java
```

#### ✅ Basic Module Declaration:

```
module zoo.animal.feeding {
    exports zoo.animal.feeding;
}
```

* * *

### 🔐 Benefits of Using Modules

- **Strong Encapsulation**: Only exported packages are visible to other modules.
- **Explicit Dependencies**: You must declare all dependencies, avoiding classpath issues.
- **Security**: Prevents unintended access to internal APIs.
- **Performance**: Smaller custom runtime images with `jlink`, better startup time.
- **Service Discovery**: Supports the Service Loader mechanism for pluggable designs.

* * *

### 🧰 Creating and Running a Module

#### ✅ Compilation:

```
javac --module-source-path src -d out $(find src -name "*.java")
```

#### ✅ Running a Module:

```
java --module-path out -m zoo.animal.feeding/zoo.animal.feeding.Task
```

#### 🛠️ Key Flags:

- `--module-path` or `-p`: where the required modules are located
- `--module` or `-m`: identifies the module and main class to run

* * *

### 🧾 Module Directives in module-info.java

Each directive configures access or dependencies.

| Directive | Description |
| --- | --- |
| `requires` | Declares a required module |
| `requires transitive` | Makes a required module visible to downstream modules |
| `exports` | Makes a package available to all modules |
| `exports ... to` | Makes a package available only to specific modules |
| `opens` | Allows runtime reflection (e.g., for serialization or frameworks) |
| `opens ... to` | Reflective access for specific modules only |
| `uses` | Declares service consumption |
| `provides ... with` | Declares service provider implementations |

#### ✅ Full Example:

```
module zoo.staff {
    requires zoo.animal.feeding;                 // imports another module
    requires transitive zoo.api;                 // re-exports dependency
    exports zoo.staff.care;                      // exposes package to all modules
    exports zoo.staff.internal to zoo.admin;     // exposes only to zoo.admin
    opens zoo.staff.records;                     // allows runtime reflection
    opens zoo.staff.logs to zoo.audit;           // reflection access limited to zoo.audit
    uses zoo.api.VetService;                     // consumes a service
    provides zoo.api.VetService with zoo.staff.MyVetImpl; // registers an implementation
}
```

#### 📝 Explanation by Example:

- `requires zoo.animal.feeding;` → This module depends on `zoo.animal.feeding`. Without it, classes in this module using that API won’t compile.
- `requires transitive zoo.api;` → If another module `zoo.care` requires `zoo.staff`, it automatically sees `zoo.api` without declaring it again.
- `exports zoo.staff.care;` → Makes the `zoo.staff.care` package available for use by any other module.
- `exports zoo.staff.internal to zoo.admin;` → Grants visibility of `zoo.staff.internal` **only** to the `zoo.admin` module.
- `opens zoo.staff.records;` → Makes `zoo.staff.records` available for reflection (used in frameworks like Spring, Hibernate).
- `opens zoo.staff.logs to zoo.audit;` → Only the `zoo.audit` module has reflective access to `zoo.staff.logs`.
- `uses zoo.api.VetService;` → Declares interest in consuming a service (e.g., loaded via `ServiceLoader`).
- `provides zoo.api.VetService with zoo.staff.MyVetImpl;` → This module **registers** `MyVetImpl` as a provider of `VetService`.

#### 📌 Additional Use Case Examples:

```
// Expose to multiple modules
exports zoo.api to zoo.ui, zoo.backend;

// Multiple service implementations
provides zoo.api.Scheduler with zoo.impl.DailyScheduler, zoo.impl.WeeklyScheduler;

// Use service in consumer module
uses java.sql.Driver; // JDBC drivers loaded dynamically
```

These examples are crucial for designing large applications where access needs to be controlled explicitly and services can be plugged dynamically.

### 🔄 Cyclic Dependencies

- Cyclic dependencies between **modules** are **not allowed**.
- Java enforces **acyclic module graphs** at compile time.
- Cycles **within a single module** (e.g., package-to-package) are allowed.

> ✅ Legal: Circular references between classes in the same module.  
> ❌ Illegal: Two modules requiring each other.

#### ❌ Example:

```
module zoo.butterfly {
    requires zoo.caterpillar;
}

module zoo.caterpillar {
    requires zoo.butterfly; // ❌ Compile-time error
}
```

* * *

### 🔀 Migration Strategies to Modules

When converting legacy systems to modules, consider these approaches:

| Strategy | Description |
| --- | --- |
| Bottom-Up | Start by modularizing leaf libraries with no dependencies. |
| Top-Down | Modularize your top-level applications first, treat dependencies as auto. |
| Mixed | Modularize core APIs, treat utility libraries as automatic initially. |

#### ✅ Example (Using Automatic Modules):

```
# Treat a regular JAR as an automatic module
java --module-path lib/legacy-lib.jar -m app.module/com.example.Main
```

* * *

### 🧰 Tools and Commands Summary

| Tool | Purpose | Example Command |
| --- | --- | --- |
| `javac` | Compiles modular code | `javac -d out --module-source-path src $(find src -name '*.java')` |
| `java` | Runs a modular application | `java --module-path out -m zoo.staff/zoo.staff.Main` |
| `jar` | Packages a module or app into a JAR file | `jar --create --file zoo.jar -C out/zoo .` |
| `jdeps` | Analyzes class/module dependencies | `jdeps --module-path mods zoo.jar` |
| `jlink` | Builds a minimal Java runtime | `jlink --module-path mods --add-modules zoo.staff -o image` |
| `jmod` | Low-level packaging (for JDK module delivery) | Rarely used by applications |

* * *

### 🧠 Exam Tips & Gotchas

- `module-info.java` must be in the **root directory** of the module source tree.
- A `module` **can export multiple packages** but **not individual classes**.
- `requires transitive` exposes dependencies to consumers of your module.
- Use `opens` or `opens to` when reflection is needed (e.g., with Spring or Jackson).
- `exports` is for compile-time + runtime access; `opens` is for runtime only (reflection).
- You **cannot have cyclic dependencies** between modules.
- `provides ... with` must match an interface and its implementation class.
- `ServiceLoader` loads services declared with `provides`.
- Use `jdeps` to help identify what modules a JAR depends on.
- Only one `module-info.java` per module — no partial declarations.