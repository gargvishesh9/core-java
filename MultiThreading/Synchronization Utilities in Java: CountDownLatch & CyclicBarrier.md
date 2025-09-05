
# Mastering Synchronization Utilities in Java: CountDownLatch & CyclicBarrier

Modern Java concurrency provides high-level synchronization tools beyond `synchronized` and `wait/notify`.

Two commonly used tools are:

- **CountDownLatch** → One-time latch for waiting until tasks finish.
- **CyclicBarrier** → Reusable barrier where threads wait for each other.

---

## 🔹 CountDownLatch

### ✅ What is it?
- A **synchronization aid** that allows one or more threads to **wait until a set of operations** being performed in other threads **completes**.
- Initialized with a **count** → number of tasks to wait for.
- Each worker thread calls `countDown()`.  
- The waiting thread calls `await()` → blocks until count reaches zero.

---

### Example: Service Initialization with CountDownLatch

```java
import java.util.concurrent.*;

public class Test {
    public static void main(String[] args) throws InterruptedException {
        int n = 3; // number of services
        ExecutorService executorService = Executors.newFixedThreadPool(n);
        CountDownLatch latch = new CountDownLatch(n);

        executorService.submit(new DependentService(latch));
        executorService.submit(new DependentService(latch));
        executorService.submit(new DependentService(latch));

        // Main thread waits until all services are ready
        latch.await();
        System.out.println("Main thread proceeds after all services are started.");

        executorService.shutdown();
    }
}

class DependentService implements Callable<String> {
    private final CountDownLatch latch;
    public DependentService(CountDownLatch latch) {
        this.latch = latch;
    }

    @Override
    public String call() throws Exception {
        try {
            System.out.println(Thread.currentThread().getName() + " service started.");
            Thread.sleep(2000); // simulate work
        } finally {
            latch.countDown(); // reduce latch count
        }
        return "ok";
    }
}
````

**Output:**

```
pool-1-thread-1 service started.
pool-1-thread-2 service started.
pool-1-thread-3 service started.
Main thread proceeds after all services are started.
```

---

### ⚡ Key Points

* One-time use: Once count reaches `0`, latch cannot be reset.
* Common use cases:

  * Wait for **N services** to start before main thread proceeds.
  * Simulate **join points** (e.g., multiple tasks finishing before reporting).
* Methods:

  * `await()` → waits until count reaches zero.
  * `countDown()` → decreases the count.
  * `getCount()` → current count.

---

## 🔹 CyclicBarrier

### ✅ What is it?

* A synchronization aid that allows a set of threads to all wait for each other to reach a **common barrier point**.
* Unlike `CountDownLatch`, a **CyclicBarrier is reusable** after the barrier is released.
* Useful in scenarios like **multi-phase computations**.

---

### Example: System Startup with CyclicBarrier

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        int numberOfSubsystems = 4;

        // barrier action runs once all threads reach the barrier
        CyclicBarrier barrier = new CyclicBarrier(numberOfSubsystems, () -> {
            System.out.println("All subsystems are up and running. System startup complete.");
        });

        Thread webServerThread = new Thread(new Subsystem("Web Server", 2000, barrier));
        Thread databaseThread = new Thread(new Subsystem("Database", 4000, barrier));
        Thread cacheThread = new Thread(new Subsystem("Cache", 3000, barrier));
        Thread messagingServiceThread = new Thread(new Subsystem("Messaging Service", 3500, barrier));

        webServerThread.start();
        databaseThread.start();
        cacheThread.start();
        messagingServiceThread.start();
    }
}

class Subsystem implements Runnable {
    private final String name;
    private final int initializationTime;
    private final CyclicBarrier barrier;

    public Subsystem(String name, int initializationTime, CyclicBarrier barrier) {
        this.name = name;
        this.initializationTime = initializationTime;
        this.barrier = barrier;
    }

    @Override
    public void run() {
        try {
            System.out.println(name + " initialization started.");
            Thread.sleep(initializationTime); // simulate time taken to initialize
            System.out.println(name + " initialization complete.");
            barrier.await(); // wait until all subsystems are ready
        } catch (InterruptedException | BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}
```

**Output:**

```
Web Server initialization started.
Database initialization started.
Cache initialization started.
Messaging Service initialization started.
Web Server initialization complete.
Cache initialization complete.
Messaging Service initialization complete.
Database initialization complete.
All subsystems are up and running. System startup complete.
```

---

### ⚡ Key Points

* Reusable: After all threads reach the barrier, it resets automatically.
* `CyclicBarrier` can take a **Runnable action** that executes once all threads reach the barrier.
* Methods:

  * `await()` → waits until all parties reach the barrier.
  * `reset()` → resets the barrier.
  * `getNumberWaiting()` → number of threads currently waiting.

---

## 🔹 UML Comparison

```text
+--------------------+            +--------------------+
|  CountDownLatch    |            |   CyclicBarrier    |
+--------------------+            +--------------------+
| - count: int       |            | - parties: int     |
| + await()          |            | + await()          |
| + countDown()      |            | + reset()          |
| + getCount()       |            | + getNumberWaiting()|
+--------------------+            +--------------------+

CountDownLatch → one-time use
CyclicBarrier → reusable
```

---

## 🔹 Interview Questions

### CountDownLatch

1. What is the difference between `join()` and `CountDownLatch`?
2. Can a `CountDownLatch` be reset?
3. How does `CountDownLatch` achieve synchronization internally?
4. Practical use case: waiting for multiple services to initialize.

### CyclicBarrier

1. How is `CyclicBarrier` different from `CountDownLatch`?
2. Can `CyclicBarrier` be reused?
3. What is the role of the barrier action?
4. What happens if one thread fails to reach the barrier?

---

## ✅ Summary

* **CountDownLatch**: One-time latch, threads wait until count reaches zero.
* **CyclicBarrier**: Reusable barrier, threads wait for each other.
* Both are crucial in **multi-threaded coordination problems**.

