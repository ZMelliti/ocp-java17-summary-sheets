# 📘 Chapter 11: Exceptions and Localization

### 🎯 Chapter 11 Focus

- Handling exceptions: try/catch/finally, try-with-resources, multi-catch
- Exception types: checked, unchecked, errors
- Custom exceptions
- Exception class hierarchy
- Localization: `Locale`, `ResourceBundle`, number/date formatting

* * *

### ❗ Understanding Exceptions

- **Exception:** An event that disrupts the normal flow of the program.
- Types:
-   **Checked exceptions**: Must be handled with a `try-catch` block or declared with `throws`. Compile-time checked.
-   **Unchecked exceptions (Runtime)**: Occur due to programming errors. Not checked at compile time.
-   **Errors**: Indicate serious problems the application shouldn't handle (e.g., JVM issues).

#### ✅ Examples:

```
// Checked Exception
FileReader fr = new FileReader("file.txt"); // throws FileNotFoundException

// Unchecked Exception
String s = null;
s.length(); // throws NullPointerException

// Error (rare in user code)
throw new StackOverflowError();
```

* * *

### 🧱 Exception Hierarchy

```
Throwable
├── Error (unchecked)
│   └── OutOfMemoryError
├── Exception
    ├── RuntimeException (unchecked)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    └── IOException (checked)
        └── FileNotFoundException
```

* * *

### ⚙️ Throwing and Declaring Exceptions

- `throw` keyword: to actually throw an exception.
- `throws` keyword: in method signature to declare possible exceptions.

#### ✅ Example:

```
public void divide(int a, int b) throws ArithmeticException {
    if (b == 0) throw new ArithmeticException("Divide by zero");
    System.out.println(a / b);
}
```

* * *

### 🔁 Try-Catch-Finally Syntax

- `finally` always runs unless JVM shuts down or `System.exit()` is called.

```
try {
    System.out.println("Try");
} catch (Exception e) {
    System.out.println("Catch");
} finally {
    System.out.println("Finally");
}
```

#### ✅ Multi-Catch:

```
try {
    riskyMethod();
} catch (IOException | SQLException e) {
    System.out.println("Caught: " + e);
}
```

#### ❌ Invalid Multi-Catch:

```
catch (IOException | Exception e) // ❌ Exception is a superclass of IOException
```

* * *

### 🔃 Try-With-Resources

- Automatically closes resources.
- Resources must implement `AutoCloseable`.
- Can declare multiple resources.

#### ✅ Example:

```
try (Scanner sc = new Scanner(new File("data.txt"));
     BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
    System.out.println(sc.nextLine());
} catch (IOException e) {
    e.printStackTrace();
}
```

* * *

### 🔍 Recognizing Exception Classes

| Type | Example | Notes |
| --- | --- | --- |
| Checked Exception | `IOException`, `SQLException` | Must be handled or declared |
| Unchecked Exception | `NullPointerException`, `IllegalArgumentException` | Runtime exception |
| Error | `OutOfMemoryError`, `StackOverflowError` | Usually JVM-related |

> ✅ Common use case:

```
public void read(String path) throws IOException {
    Files.readAllLines(Path.of(path));
}
```

* * *

### 🔁 Overriding Methods and Exceptions

- A subclass method can:
-   Throw fewer or no exceptions
-   Throw a **subclass** of an exception in the parent method
- ❌ Cannot throw broader or new checked exceptions

> ✅ Example:

```
class Animal {
  public void sleep() throws IOException {}
}
class Dog extends Animal {
  public void sleep() throws FileNotFoundException {} // allowed
}
```

* * *

### 📦 Printing Exceptions

```
try {
    throw new RuntimeException("Oops!");
} catch (RuntimeException e) {
    System.out.println(e);              // Output: java.lang.RuntimeException: Oops!
    System.out.println(e.getMessage()); // Output: Oops!
    e.printStackTrace();                // Full stack trace
}
```

* * *

### 🌍 Localization Basics

- Java uses `Locale` for region/language settings.
- Locale codes:
-   Language = lowercase (`en`, `fr`, `ja`)
-   Country = uppercase (`US`, `CA`, `JP`)

#### ✅ Examples:

```
Locale locale = new Locale("fr", "FR");
Locale us = Locale.US;
```

* * *

### 📅 Formatting Numbers and Dates

#### NumberFormat

```
NumberFormat nf = NumberFormat.getCurrencyInstance(Locale.US);
System.out.println(nf.format(123456.78)); // $123,456.78
```

#### DateTimeFormatter

```
LocalDate date = LocalDate.now();
DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.FULL);
System.out.println(date.format(formatter));
```

* * *

### 📚 Resource Bundles

- `.properties` files used for internationalization (i18n)
- Java looks for most specific match and falls back:
-   `Zoo_en_US.properties` → `Zoo_en.properties` → `Zoo.properties`

> ✅ Example:

```
# Zoo_en_US.properties
hello=Hello, World!
```

```
ResourceBundle rb = ResourceBundle.getBundle("Zoo", new Locale("en", "US"));
System.out.println(rb.getString("hello"));
```

* * *

### 🧠 Exam Tips and Common Traps

- `throw` = keyword, `throws` = declaration
- Try-with-resources closes in **reverse** order
- `finally` block always runs, even if exception is thrown
- Multi-catch must not contain related types (e.g., `Exception | IOException ❌`)
- `ResourceBundle.getBundle()` fails at runtime if file not found
- Date and number formatting varies by locale → different symbols, separators
- Do NOT use catch blocks to catch `Error` or `Throwable` in real projects