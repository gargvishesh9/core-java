
# Advanced Concurrency in Java: Future, Callable, Atomic Classes & Volatile

Java provides high-level concurrency utilities to simplify multithreading while ensuring **performance** and **thread safety**.

---

## 🔹 Future & Callable

### Problem
- With `Runnable`, tasks **don’t return a result** and **cannot throw checked exceptions**.  
- We often need to **return results** or **check task status** after execution.

### Solution
- **`Callable<V>`** → Similar to Runnable, but returns a value (`V`) and can throw checked exceptions.  
- **`Future<V>`** → Represents the result of an asynchronous computation.  

---

### Example 1: Using Runnable with Future

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executorService = Executors.newSingleThreadExecutor();

        Future<?> future = executorService.submit(() -> System.out.println("Hello")); // Runnable task

        System.out.println(future.get()); // blocking call → returns null (Runnable doesn’t return anything)

        if (future.isDone()) {
            System.out.println("Task is done!");
        }

        executorService.shutdown();
    }
}
````

**Output:**

```
Hello
null
Task is done!
```

---

### Example 2: Using Callable with Future

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executorService = Executors.newSingleThreadExecutor();

        Future<String> future = executorService.submit(() -> "Hello from Callable"); // Callable returns value

        System.out.println(future.get()); // blocking call → waits until result is available

        if (future.isDone()) {
            System.out.println("Task is done!");
        }

        executorService.shutdown();
    }
}
```

**Output:**

```
Hello from Callable
Task is done!
```

---

### Key Methods of Future

* `get()` → Waits for task completion and retrieves result.
* `get(timeout, unit)` → Waits for a maximum time, else throws `TimeoutException`.
* `isDone()` → Returns `true` if task is finished.
* `isCancelled()` → Checks if task was cancelled.
* `cancel(boolean mayInterrupt)` → Cancels task execution.

---

## 🔹 Atomic Classes

### Problem

Using `synchronized` can reduce performance due to blocking. For simple counters or references, it’s **overkill**.

### Solution

Java provides **atomic classes** (e.g., `AtomicInteger`, `AtomicLong`, `AtomicBoolean`) in `java.util.concurrent.atomic` package.
These use **lock-free, hardware-level CAS (Compare-And-Swap)** operations for thread safety.

---

### Example: AtomicInteger Counter

```java
import java.util.concurrent.atomic.AtomicInteger;

public class VolatileCounter {
    private AtomicInteger counter = new AtomicInteger(0);

    public void increment() {
        counter.incrementAndGet();
    }

    public int getCounter() {
        return counter.get();
    }

    public static void main(String[] args) throws InterruptedException {
        VolatileCounter vc = new VolatileCounter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) vc.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) vc.increment();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println(vc.getCounter()); // Always 2000
    }
}
```

✅ No race conditions, no need for synchronization.
✅ Lightweight & fast.

---

## 🔹 Volatile Keyword

### Problem

* Threads may cache variables locally, so changes made by one thread may not be visible to others.
* Example: A loop may never end because the updated value isn’t seen by another thread.

### Solution

* Declaring a variable as **`volatile`** ensures:

  * Visibility → All reads/writes happen directly from main memory.
  * No caching between threads.

⚠️ **Volatile doesn’t provide atomicity.** For compound operations like `count++`, use `AtomicInteger` instead.

---

### Example: Volatile Flag

```java
class SharedObj {
    private volatile boolean flag = false;

    public void setFlagTrue() {
        System.out.println("Writer thread made the flag true!");
        flag = true;
    }

    public void printIfFlagTrue() {
        while (!flag) {
            // busy wait until flag becomes true
        }
        System.out.println("Flag is true!");
    }
}

public class VolatileExample {
    public static void main(String[] args) {
        SharedObj sharedObj = new SharedObj();

        Thread writerThread = new Thread(() -> {
            try {
                Thread.sleep(1000); // simulate delay
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            sharedObj.setFlagTrue();
        });

        Thread readerThread = new Thread(() -> sharedObj.printIfFlagTrue());

        writerThread.start();
        readerThread.start();
    }
}
```

**Output:**

```
Writer thread made the flag true!
Flag is true!
```

---

## 🔹 UML Diagram

```text
            +--------------------+
            |   ExecutorService  |
            +--------------------+
                 /\
                 ||
        +-----------------+
        | Future<T>       |
        +-----------------+
        | + get()         |
        | + cancel()      |
        | + isDone()      |
        +-----------------+

        +-----------------+
        | Callable<V>     |
        +-----------------+
        | + call() : V    |
        +-----------------+

        +-------------------------+
        | AtomicInteger           |
        +-------------------------+
        | + incrementAndGet()     |
        | + decrementAndGet()     |
        | + get()                 |
        +-------------------------+

        +-----------------+
        | volatile var    |
        +-----------------+
        | + visibility    |
        | - no atomicity  |
        +-----------------+
```

---

## 🔹 Interview Questions

### Future & Callable

1. Difference between Runnable and Callable?
2. What happens if you call `future.get()`?
3. How do you handle timeout with `Future`?
4. Can `Future` be cancelled? How?
5. What is the difference between `submit()` and `execute()`?

### Atomic Classes

1. Why use AtomicInteger instead of synchronized?
2. Explain how AtomicInteger works internally (CAS).
3. Can Atomic classes completely replace locks?
4. Difference between `incrementAndGet()` and `getAndIncrement()`.

### Volatile

1. Difference between `volatile` and `synchronized`.
2. Does volatile make a compound operation (like `count++`) atomic?
3. When would you prefer `volatile` over `AtomicInteger`?
4. Can volatile prevent race conditions?
5. Does `volatile` affect performance?

---

## 🔹 Tricky Questions

1. If you mark an `AtomicInteger` as `volatile`, is it useful?
   → No, Atomic classes already ensure visibility and atomicity.

2. If you use `volatile int counter` and do `counter++` in multiple threads, will it be safe?
   → No, because `counter++` is not atomic. Use `AtomicInteger`.

3. What happens if you call `future.get()` before task completes?
   → It blocks until result is available.

4. Can Future handle checked exceptions?
   → Yes, `future.get()` wraps them inside `ExecutionException`.

5. If a thread is waiting on `future.get()`, and the task is cancelled, what happens?
   → `CancellationException` is thrown.

---

# ✅ Summary

* **Future + Callable** → Manage asynchronous tasks and get results.
* **Atomic classes** → Provide lock-free, thread-safe counters and variables.
* **Volatile** → Ensures visibility, not atomicity.

Together, they form the building blocks of **modern Java concurrency**.

