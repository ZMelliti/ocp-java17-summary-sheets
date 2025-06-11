# 📘 Chapter 2: Operators and Decision Constructs

### 🎯 Chapter 2 Focus

#### ⚙️ Types of operators

| Type | Examples | Description |
| --- | --- | --- |
| Unary | `+`, `-`, `++`, `--`, `!`, `~` | One operand (e.g., negate, increment) |
| Binary | `+`, `-`, `*`, `/`, `%`, `==` | Two operands (math, comparison) |
| Ternary | `?:` | Three operands; compact `if-else` |
| Assignment | `=`, `+=`, `-=`, etc. | Assign or modify variables |

#### 🔝 Operator precedence (simplified)

| Priority | Operators | Notes |
| --- | --- | --- |
| 1   | `++`, `--`, `+`, `-`, `!`, `~` | Unary operators |
| 2   | `*`, `/`, `%` | Multiplication/division/modulus |
| 3   | `+`, `-` | Addition/subtraction |
| 4   | `<`, `<=`, `>`, `>=`, `instanceof` | Relational |
| 5   | `==`, `!=` | Equality |
| 6   | `&`, `^`, `&&` | Logical (bitwise and conditional) |
| 7   | `?:` | Ternary |
| 8   | `=`, `+=`, `-=`, `*=`, `/=`, etc. | Assignment |

#### ✅ Unary operators

| Symbol | Name | Action |
| --- | --- | --- |
| `+` | Unary plus | No change (e.g., `+5` stays `5`) |
| `-` | Unary minus | Negates the value (e.g., `-3`) |
| `++` | Increment | Adds 1 (prefix/postfix changes behavior) |
| `--` | Decrement | Subtracts 1 |
| `!` | Logical NOT | Inverts a boolean (`true` → `false`) |
| `~` | Bitwise complement | Flips bits (e.g., `~3` becomes `-4`) |

#### 🔄 Casting & numeric promotion

- Smaller types (`byte`, `short`, `char`) promote to `int` in expressions.
- Casting is required when assigning larger types to smaller types:
```
byte b = (byte)(intVar + longVar);
```

* * *

### ➕ Binary operators

#### Arithmetic

| Operator | Function |
| --- | --- |
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulus |

#### Comparison & equality

| Operator | Use case |
| --- | --- |
| `==` | Value equality |
| `!=` | Value inequality |
| `<`, `>` | Relational comparisons |
| `<=`, `>=` | Less/greater than or equals |
| `instanceof` | Object type checking |

#### Logical

| Operator | Type | Description |
| --- | --- | --- |
| `&`, `^` | Logical/Bitwise | Evaluates both sides |
| `&&` | Short-circuit | Right-hand side evaluated only if needed |

* * *

### 🚦 Ternary operator

```
boolean test = true;
int result = test ? 1 : 2;
```

- Format: `condition ? valueIfTrue : valueIfFalse`
- Only one branch is evaluated.
- **Exam trap**: Watch for side effects in unused branches.

```
int x = 1, y = 1;
int z = (x < 10) ? x++ : y++;
// Only x is incremented because (x < 10) is true
```

* * *

### 🔧 Assignment operators

| Operator | Description |
| --- | --- |
| `=` | Basic assignment |
| `+=`, `-=`, `*=` etc. | Compound assignment |

- Compound assignments include implicit casts:
```
int x = 1;
x += 2.5; // implicit cast from double to int
```

* * *

### ❓ Realistic exam question traps

- Mixing data types without casting:
```
byte b = 10;
b = b + 1; // does NOT compile unless cast explicitly
```
- Logical operators on wrong types:
```
int a = 1;
boolean b = !a; // does NOT compile
```
- Ternary with different return types:
```
int value = (true ? 1 : "str"); // does NOT compile
```

* * *

### 🔍 Exam concepts to watch

| Concept | Summary |
| --- | --- |
| Operator precedence | Determines evaluation order; use parentheses for clarity |
| Short-circuiting | `&&` skips second operand if result is known |
| Numeric promotion | Small types auto-promoted to `int`; beware unexpected results |
| Casting | Needed when narrowing types (e.g., `int` to `byte`) |
| Ternary conditions | Use with simple expressions; only one branch is evaluated |
| Increment/decrement | Prefix updates before use, postfix after use |
| Compound assignments | May include implicit casts; careful with types |

* * *

### ✨ Personal tips & study hacks

- Practice **operator precedence** with complex expressions.
- Memorize the **truth tables** for `&&`, `||`, `^`, `&`, and `|`.
- Write **mini programs** to explore ternary operator side effects.
- Know **data type sizes** and default types.
- Review **bitwise behavior** with small binary numbers.
- Use diagrams to track post/pre increments during evaluation.

* * *

### 🔒 OCP exam essentials

- Understand precedence, associativity, and which operators return boolean vs. numeric.
- Identify unreachable code due to side effects and short-circuiting.
- Grasp ternary operator use in conditional logic.
- Note operator overloading limitations (Java doesn’t allow it).
- Be aware of implicit/explicit casting in assignments and expressions.
- Use parentheses for clarity in order of operations.