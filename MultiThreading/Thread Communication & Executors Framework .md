
# Thread Communication & Executors Framework in Java

Java provides mechanisms to simplify and efficiently manage **multithreading** and **concurrency**.  
Two important topics are:

1. **Thread Communication** – Coordination between threads using `wait()`, `notify()`, and `notifyAll()`.  
2. **Executors Framework** – Simplifies thread management and improves scalability.  

---

## 🔹 Thread Communication

### Problem:
When multiple threads share a resource, they must communicate to avoid:
- **Race conditions**  
- **Data inconsistency**  
- **Busy waiting** (wasting CPU cycles)

### Solution:
Java provides **inter-thread communication methods** inside `Object` class:
- `wait()` → Causes the current thread to wait until another thread calls `notify()`/`notifyAll()`.  
- `notify()` → Wakes up **one waiting thread**.  
- `notifyAll()` → Wakes up **all waiting threads**.  

---

### Example: Producer-Consumer Problem

```java
class SharedResource {
    private int data;
    private boolean hasData;

    public synchronized void produce(int value) {
        while (hasData) {
            try {
                wait(); // wait until consumer consumes
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        data = value;
        hasData = true;
        System.out.println("Produced: " + value);
        notify(); // notify consumer
    }

    public synchronized int consume() {
        while (!hasData) {
            try {
                wait(); // wait until producer produces
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        hasData = false;
        System.out.println("Consumed: " + data);
        notify(); // notify producer
        return data;
    }
}

class Producer implements Runnable {
    private SharedResource resource;

    public Producer(SharedResource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            resource.produce(i);
        }
    }
}

class Consumer implements Runnable {
    private SharedResource resource;

    public Consumer(SharedResource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            int value = resource.consume();
        }
    }
}

public class ThreadCommunication {
    public static void main(String[] args) {
        SharedResource resource = new SharedResource();
        Thread producerThread = new Thread(new Producer(resource));
        Thread consumerThread = new Thread(new Consumer(resource));

        producerThread.start();
        consumerThread.start();
    }
}
````

✅ Ensures that:

* Producer waits if data is already produced.
* Consumer waits if no data is available.
* Prevents **race conditions**.

---

## 🔹 Executors Framework

### Problem:

Manually creating and managing threads can be error-prone:

* Too many threads → Performance issues.
* Too few threads → Poor resource utilization.
* Need to manage **shutdowns, reusability, and scalability**.

### Solution:

The **Executors Framework** (`java.util.concurrent`) provides a higher-level API for managing threads.

### Benefits:

* ✅ Avoids manual thread management
* ✅ Reuses threads (thread pool)
* ✅ Better resource management
* ✅ Built-in error handling
* ✅ Supports scaling for CPU or I/O-bound tasks

---

### Example: Using ExecutorService

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ExecutorFrameWork {

    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        
        ExecutorService executor = Executors.newFixedThreadPool(3); // 3 threads

        for (int i = 1; i < 10; i++) {
            int finalI = i;
            executor.submit(() -> {
                long result = factorial(finalI);
                System.out.println("Factorial of " + finalI + " = " + result);
            });
        }

        executor.shutdown();
        try {
            executor.awaitTermination(1, TimeUnit.SECONDS);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        System.out.println("Total time " + (System.currentTimeMillis() - startTime));
    }

    private static long factorial(int n) {
        try {
            Thread.sleep(1000); // simulate delay
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        long result = 1;
        for (int i = 1; i <= n; i++) {
            result *= i;
        }
        return result;
    }
}
```

✅ Executes tasks with a **thread pool of size 3**.
✅ Threads are **reused** for multiple tasks.
✅ Improves **performance and scalability**.

---

## 🔹 Interview Questions

### Thread Communication

1. What is the difference between `sleep()` and `wait()`?
2. Why must `wait()` and `notify()` be called from a synchronized block?
3. Difference between `notify()` and `notifyAll()`.
4. Can a thread call `wait()` without holding a lock?
5. What happens if `notify()` is called when no thread is waiting?

### Executors Framework

1. Difference between `execute()` and `submit()` methods.
2. Explain types of thread pools in Executors:

   * FixedThreadPool
   * CachedThreadPool
   * ScheduledThreadPool
   * SingleThreadExecutor
3. How does Executors framework improve performance?
4. How do you properly shut down an ExecutorService?
5. What happens if you don’t call `shutdown()` on ExecutorService?

---

## 🔹 Tricky Questions

1. **Can you replace `notify()` with `notifyAll()` in Producer-Consumer?**
   → Yes, but `notifyAll()` wakes all waiting threads → higher context switching.

2. **What happens if Producer calls `notify()` but Consumer hasn’t yet called `wait()`?**
   → Notification is lost. Consumer may wait forever → solution is to use a condition (`while` loop).

3. **Which is better for performance: creating new threads manually or using Executors?**
   → Executors are better since they reuse threads and reduce overhead.

4. **Can ExecutorService execute tasks asynchronously?**
   → Yes, tasks submitted to `ExecutorService` run asynchronously.

5. **What if `awaitTermination()` times out before tasks complete?**
   → Remaining tasks may still run if `shutdown()` is called (graceful), but with `shutdownNow()`, tasks are interrupted.

---

# ✅ Summary

* **Thread Communication** (wait/notify) prevents race conditions and coordinates Producer-Consumer logic.
* **Executors Framework** simplifies thread management and improves scalability with thread pools.
* Both are essential for writing efficient, production-grade concurrent Java applications.

```
