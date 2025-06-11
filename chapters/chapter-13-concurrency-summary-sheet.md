# 📘 Chapter 13: Concurrency

### 🎯 Chapter 13 Focus

- Thread lifecycle and state management
- Creating and managing threads (Runnable, Callable, ExecutorService)
- Synchronization and thread safety
- Concurrent collections
- Fork/Join framework
- Parallel streams
- Thread-related exam traps and best practices

* * *

### ⏱️ Thread Basics

#### Thread Lifecycle States:

| State | Description |
| --- | --- |
| NEW | Thread is created but not started |
| RUNNABLE | Thread is eligible to run (either running or waiting for CPU) |
| BLOCKED | Thread is waiting to enter a synchronized block because another thread owns it |
| WAITING | Thread waits indefinitely until another thread notifies it (`join()`, `wait()`) |
| TIMED\_WAITING | Thread waits for a fixed amount of time (`sleep()`, `join(1000)`) |
| TERMINATED | Thread has completed execution or was aborted |

#### Creating Threads with `Runnable`

```
Runnable task = () -> System.out.println("Hello from thread");
Thread t = new Thread(task);
t.start();
```

- `start()` initiates a new thread; `run()` executes in the current thread.

#### Implementing `Runnable` vs `Callable`

| Trait | `Runnable` | `Callable<T>` |
| --- | --- | --- |
| Returns Value? | No  | Yes (`T`) |
| Can Throw? | No checked exceptions | Yes (checked exceptions allowed) |

```
Callable<String> callable = () -> "Callable result";
```

* * *

### 🚀 ExecutorService

Allows you to manage a thread pool rather than manually creating threads.

#### Creating an Executor:

```
ExecutorService service = Executors.newFixedThreadPool(2);
service.execute(() -> System.out.println("Task running"));
service.shutdown();
```

#### `submit()` vs `execute()`:

```
Future<Integer> future = service.submit(() -> 10 + 20);
Integer result = future.get(); // 30
```

| Method | Returns | Can Handle Exception? |
| --- | --- | --- |
| `execute()` | void | No (logs to stderr) |
| `submit()` | `Future<T>` | Yes (via `get()` + try/catch) |

#### Shutdown Methods:

- `shutdown()`: gracefully stop accepting new tasks
- `shutdownNow()`: attempts to cancel currently running tasks

* * *

### 🔒 Thread Safety & Synchronization

#### Why Thread Safety Matters

Concurrent modification of shared data leads to unpredictable results.

#### Common Race Condition Example:

```
int count = 0;
Runnable task = () -> count++; // not atomic
```

#### Fixes:

1. **Synchronized blocks:**

```
synchronized (this) {
   count++;
}
```

1. **Atomic classes:**

```
AtomicInteger atomicCount = new AtomicInteger();
Runnable task = () -> atomicCount.incrementAndGet();
```

- Atomic classes (`AtomicInteger`, `AtomicBoolean`, etc.) are lock-free and efficient.

* * *

### 🔁 Concurrent Collections

Used in multi-threaded environments where normal collections may cause data corruption.

| Class | Description |
| --- | --- |
| `ConcurrentHashMap` | Thread-safe map with segment-based locking |
| `CopyOnWriteArrayList` | Creates a copy of the array for every write |
| `ConcurrentLinkedQueue` | Non-blocking queue, safe for concurrent operations |

#### When to Use:

- Use `CopyOnWriteArrayList` for **more reads than writes**.
- Use `ConcurrentHashMap` for high-concurrency key-value access.

* * *

### 🔱 Fork/Join Framework

Designed for tasks that can be **recursively broken into subtasks**.

#### Key Classes:

- `RecursiveAction`: no return value
- `RecursiveTask<T>`: returns a result

#### Example: Recursive Summation

```
class SumTask extends RecursiveTask<Integer> {
    int[] numbers;
    int start, end;
    SumTask(int[] nums, int s, int e) {
        numbers = nums; start = s; end = e;
    }
    protected Integer compute() {
        if (end - start <= 5) {
            int sum = 0;
            for (int i = start; i < end; i++) sum += numbers[i];
            return sum;
        }
        int mid = (start + end) / 2;
        SumTask left = new SumTask(numbers, start, mid);
        SumTask right = new SumTask(numbers, mid, end);
        left.fork();
        return right.compute() + left.join();
    }
}
```

* * *

### ☰ Parallel Streams

Parallel streams split a stream into multiple substreams processed by different threads.

#### Example:

```
List<Integer> nums = List.of(1, 2, 3, 4, 5);
nums.parallelStream()
    .map(n -> n * 2)
    .forEach(System.out::println);
```

#### Guidelines:

- Avoid modifying shared variables.
- Best for stateless operations with large data sets.
- Performance gain is context-sensitive (hardware & task dependent).

* * *

### ⚠️ Common Concurrency Pitfalls

| Issue | Description |
| --- | --- |
| Reusing threads | Calling `start()` twice throws `IllegalThreadStateException` |
| Unclosed ExecutorService | `shutdown()` is required, or app may hang |
| Shared state modification | Without sync leads to race conditions |
| Non-thread-safe collections | Can throw `ConcurrentModificationException` |
| Blocking in parallel stream | Can lead to deadlocks or poor performance |

* * *

### 🧠 Exam Tips & Tricks

| Topic | Key Tip |
| --- | --- |
| `submit()` vs `execute()` | `submit()` returns `Future`; `execute()` does not |
| `shutdown()` vs `shutdownNow()` | One waits, one interrupts tasks |
| Callable vs Runnable | `call()` can return value and throw checked exceptions |
| Parallel streams | Avoid modifying shared variables |
| Synchronization | `synchronized` keyword or atomic classes for shared vars |
| Thread lifecycle | Know all 6 states (`NEW`, `RUNNABLE`, etc.) |
| Future.get() | Can throw `ExecutionException` or `InterruptedException` |