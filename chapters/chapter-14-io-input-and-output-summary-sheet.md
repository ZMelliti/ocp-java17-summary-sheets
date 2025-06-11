# 📘 Chapter 14: I/O (Input and Output)

### 🎯 Chapter 14 Focus

- Working with `File` and `Path` (I/O and NIO.2)
- Reading and writing data using streams
- File and directory operations
- Working with attributes and metadata
- Serialization and deserialization
- Reading user input from Console
- Stream types and categories (byte vs character, input vs output)

* * *

### 🔍 File vs Path: I/O and NIO.2

#### ✨ File (java.io.File):

- Legacy class to represent file and directory names.
- Simple APIs for basic file system access.
- Doesn't support symbolic links or metadata inspection.

```
File file = new File("data/zoo.txt");
if (file.exists()) {
    System.out.println("Size: " + file.length());
    System.out.println("Absolute path: " + file.getAbsolutePath());
}
```

#### ✨ Path (java.nio.file.Path):

- Modern file system abstraction introduced in NIO.2.
- Works with `java.nio.file.Files` class for advanced file operations.
- Supports symbolic links, attributes, streaming directories.

```
Path path = Path.of("data/zoo.txt");
if (Files.exists(path)) {
    System.out.println("Size: " + Files.size(path));
    System.out.println("Is writable: " + Files.isWritable(path));
}
```

✅ Use `Path` + `Files` for new development. Use `File` only for legacy compatibility.

* * *

### 🛠️ Common File Operations

| Operation | File | NIO.2 |
| --- | --- | --- |
| Check existence | `file.exists()` | `Files.exists(path)` |
| Is file/dir? | `file.isFile()` | `Files.isRegularFile(path)` |
| List directory | `file.listFiles()` | `Files.list(path)` (Stream) |
| Delete | `file.delete()` | `Files.deleteIfExists(path)` |
| Create directory | `file.mkdir()` | `Files.createDirectory(path)` |
| Move/rename | `file.renameTo(dest)` | `Files.move(source, target)` |

> 🔸 Prefer `Files.*()` operations for better error handling and flexibility (copy options, symbolic link support).

* * *

### 🔀 Stream Types (I/O)

#### 📁 Byte Streams

- Work with **binary data** like images, videos.
- Use subclasses of `InputStream` and `OutputStream`.

```
InputStream is = new FileInputStream("photo.jpg");
OutputStream os = new FileOutputStream("copy.jpg");
```

#### 📖 Character Streams

- Handle **text data** using Unicode.
- Subclasses of `Reader` and `Writer`.

```
Reader reader = new FileReader("data.txt");
Writer writer = new FileWriter("copy.txt");
```

#### ⚙️ Buffered (High-level) Streams

- Wrap lower-level streams to add efficiency.
- `BufferedReader`, `BufferedWriter`, `PrintWriter`, `ObjectOutputStream`.

```
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}
```

➡️ **Buffered streams are preferred** for performance (fewer disk reads/writes).

* * *

### 🚀 NIO.2 and Stream API Integration

#### Reading Lines as Stream:

```
Files.lines(Path.of("log.txt"))
     .filter(line -> line.contains("ERROR"))
     .forEach(System.out::println);
```

- Lazy loading: better for large files.
- Must close stream or use try-with-resources.

#### Reading All Lines (Eager):

```
List<String> lines = Files.readAllLines(Path.of("log.txt"));
```

- Reads everything at once into memory.

#### Writing Lines:

```
Files.write(Path.of("report.txt"), List.of("Summary", "Details"));
```

- Overwrites by default. Use `StandardOpenOption.APPEND` to add.

#### Buffered Reader/Writer with NIO.2:

```
try (var reader = Files.newBufferedReader(Path.of("input.txt"));
     var writer = Files.newBufferedWriter(Path.of("output.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        writer.write(line);
        writer.newLine();
    }
}
```

* * *

### 📤 Serialization

Serialization converts an object to a stream of bytes for storage or transfer.

#### Serializable Class:

```
class Animal implements Serializable {
    private String name;
    private transient int age; // not saved
}
```

- Fields marked `transient` are skipped.

#### Writing and Reading:

```
try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("animal.ser"))) {
    out.writeObject(new Animal("Lion"));
}

try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("animal.ser"))) {
    Animal a = (Animal) in.readObject();
}
```

- Must handle `ClassNotFoundException` and `IOException`.
- Class versioning managed with `serialVersionUID`.

* * *

### 📟 Console Input

Secure way to read user input (esp. passwords).

```
Console console = System.console();
if (console != null) {
    String user = console.readLine("Username: ");
    char[] password = console.readPassword("Password: ");
}
```

- Returns `null` if run inside some IDEs.
- Password returned as `char[]` for better security handling.

* * *

### ⚠️ Exam Traps and Notes

- Mixing `Files.readAllLines()` with streams like `filter()` causes compile error.
- Always close files/streams! Use try-with-resources.
- `Files.lines()` returns a **lazy** stream; `readAllLines()` is **eager**.
- BufferedWriter doesn't flush automatically unless closed.
- `ObjectInputStream.readObject()` returns `Object` → must cast.
- File name may not reflect actual content type (e.g., `zoo.txt` might be a directory).
- Symbolic links are **not** followed by default in NIO.2 unless you add `NOFOLLOW_LINKS`.

* * *

### 📂 NIO.2 Options and Flags

Used with operations like `Files.copy()`, `Files.move()`.

| Option | Purpose |
| --- | --- |
| `NOFOLLOW_LINKS` | Avoid resolving symbolic links |
| `REPLACE_EXISTING` | Overwrite existing file |
| `COPY_ATTRIBUTES` | Preserve timestamps, permissions |
| `ATOMIC_MOVE` | Ensures the move is done in one atomic operation |

```
Files.move(source, target, StandardCopyOption.REPLACE_EXISTING);
```

- You can combine multiple options with commas.