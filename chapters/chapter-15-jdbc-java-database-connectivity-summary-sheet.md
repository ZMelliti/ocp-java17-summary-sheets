# 📘 Chapter 15: JDBC (Java Database Connectivity)

### 🎯 Chapter 15 Focus

- Relational databases and SQL CRUD operations
- JDBC architecture and key interfaces
- Building a JDBC URL and connecting to databases
- Working with `PreparedStatement` and `CallableStatement`
- Executing queries and reading `ResultSet`
- Managing transactions and savepoints
- Closing database resources

* * *

### 🗄️ JDBC and SQL Overview

#### What is JDBC?

- JDBC is a Java API that enables interaction with relational databases.
- It works by sending SQL statements to a database and handling results.

#### CRUD SQL Operations:

| Operation | SQL Keyword | Purpose |
| --- | --- | --- |
| Create | INSERT | Add new rows |
| Read | SELECT | Retrieve data |
| Update | UPDATE | Modify existing data |
| Delete | DELETE | Remove rows |

#### Example SQL:

```
INSERT INTO exhibits VALUES (3, 'Asian Elephant', 7.5);
SELECT name FROM exhibits;
UPDATE exhibits SET num_acres = 10 WHERE id = 3;
DELETE FROM exhibits WHERE id = 3;
```

* * *

### 🔍 JDBC Architecture and Key Interfaces

#### Five Core Interfaces (from java.sql):

| Interface | Role |
| --- | --- |
| `Driver` | Establishes a DB connection |
| `Connection` | Represents a connection to DB |
| `PreparedStatement` | Executes parameterized SQL queries |
| `CallableStatement` | Calls stored procedures |
| `ResultSet` | Retrieves results from SQL queries |

You do NOT use concrete classes directly. Implementations come from the DB-specific JDBC driver.

* * *

### 🔗 Connecting to a Database

#### JDBC URL Format:

```
jdbc:<subprotocol>:<subname>
```

Examples:

```
jdbc:hsqldb:file:zoo
jdbc:mysql://localhost:3306/zoo
```

#### Creating a Connection:

```
String url = "jdbc:hsqldb:file:zoo";
try (Connection conn = DriverManager.getConnection(url)) {
    System.out.println(conn);
}
```

- You can also pass username/password as:

```
DriverManager.getConnection(url, "user", "pass");
```

* * *

### 🎩 Using `PreparedStatement`

- Prevents SQL injection by using placeholders (`?`) for variables.

#### Example:

```
String sql = "SELECT name FROM exhibits WHERE id = ?";
try (PreparedStatement ps = conn.prepareStatement(sql)) {
    ps.setInt(1, 3);
    try (ResultSet rs = ps.executeQuery()) {
        while (rs.next())
            System.out.println(rs.getString("name"));
    }
}
```

- Use `executeUpdate()` for INSERT/UPDATE/DELETE → returns rows affected.
- Use `executeQuery()` for SELECT → returns a `ResultSet`.
- Use `execute()` if you don’t know the statement type → returns boolean.

* * *

### 🔢 Retrieving Data with `ResultSet`

#### Iterating and Reading Data:

```
while (rs.next()) {
    String name = rs.getString("name");
    int id = rs.getInt("id");
}
```

- Column indexes start at 1, not 0.
- `rs.getObject(column)` can be used for any column type.

* * *

### ⚡ Calling Stored Procedures

#### Using `CallableStatement`:

```
CallableStatement cs = conn.prepareCall("{ call getTotal(?) }");
cs.registerOutParameter(1, Types.INTEGER);
cs.execute();
int total = cs.getInt(1);
```

- Use `registerOutParameter()` for OUT/INOUT parameters.
- Format for return values: `{? = call funcName(?)}`

* * *

### ✅ Transaction Management

#### Default Behavior:

- JDBC uses **auto-commit** mode by default: every statement is committed immediately.

#### Manual Transaction:

```
conn.setAutoCommit(false);
Savepoint sp = conn.setSavepoint();
try {
    // Execute statements
    conn.commit();
} catch (SQLException e) {
    conn.rollback(sp);
}
```

- Use `setAutoCommit(false)` to disable auto-commit.
- Use `commit()`, `rollback()`, and `setSavepoint()` to manage transactions manually.

> 🔸 If you close a connection without commit/rollback, the result depends on the JDBC driver. Don’t rely on implicit behavior!

* * *

### 🛅 Closing JDBC Resources

- Always use **try-with-resources**.
- Closing a `Connection` also closes its `PreparedStatement` and `ResultSet`.
- Closing a `Statement` also closes its `ResultSet`.

Bad Example:

```
Connection conn = DriverManager.getConnection(url);
// If exception occurs before try-with-resources, resources won't close
```

Good Example:

```
try (Connection conn = DriverManager.getConnection(url);
     PreparedStatement ps = conn.prepareStatement(sql);
     ResultSet rs = ps.executeQuery()) {
    // Use the resources
}
```

* * *

### ⚠️ Exam Traps & Tips

- Know that column indexes start at **1**, not 0
- `setAutoCommit(true)` commits current transaction immediately
- `getConnection()` requires driver to be on classpath
- Know difference between `executeQuery()`, `executeUpdate()`, `execute()`
- `setX()` vs `registerOutParameter()` → don't confuse in exam
- Avoid resource leaks → never declare JDBC resources outside try-with-resources
- JDBC classes are in the `java.sql` module/package