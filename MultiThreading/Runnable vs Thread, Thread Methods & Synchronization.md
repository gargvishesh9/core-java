
# Mastering Runnable vs Thread, Thread Methods & Synchronization in Java

## Runnable vs Thread
- **Runnable (Preferred Choice)**  
  - Use when you want to **separate task from the thread**.  
  - Allows the class to **extend another class** (since Java does not support multiple inheritance).  
  - More flexible and widely used in real-world projects.  

- **Thread (Less Flexible)**  
  - Use when you need **direct control over the thread** or want to override Thread-specific methods.  
  - Extending `Thread` limits inheritance (you can’t extend any other class).  

👉 **Best Practice**: Implement `Runnable` unless you have a strong reason to extend `Thread`.

---

## Commonly Used Thread Methods

### 1. `start()`
- Starts a new thread and calls the `run()` method.
- Without `start()`, calling `run()` directly will execute in the **main thread**.

### 2. `run()`
- The entry point for the thread’s task.
- Defined by overriding in `Thread` or `Runnable`.

---

### 3. `sleep(long millis)`
- Pauses execution for a specific duration.

### 4. `join()`
- Waits for another thread to **finish execution** before continuing.

---

### 5. `setPriority(int newPriority)`
- Sets the priority (`1` to `10`) of a thread.  
- `Thread.MIN_PRIORITY = 1`, `Thread.NORM_PRIORITY = 5`, `Thread.MAX_PRIORITY = 10`.

```java
public class MyThread extends Thread {
    public MyThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        System.out.println("Thread is Running...");
        for (int i = 1; i <= 5; i++) {
            System.out.println(
                Thread.currentThread().getName() +
                " - Priority: " + Thread.currentThread().getPriority() +
                " - count: " + i
            );
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        MyThread l = new MyThread("Low Priority Thread");
        MyThread m = new MyThread("Medium Priority Thread");
        MyThread n = new MyThread("High Priority Thread");

        l.setPriority(Thread.MIN_PRIORITY);
        m.setPriority(Thread.NORM_PRIORITY);
        n.setPriority(Thread.MAX_PRIORITY);

        l.start();
        m.start();
        n.start();
    }
}
````

---

### 6. `interrupt()`

* Interrupts a thread that is blocked in `sleep()`, `wait()`, or `join()`.

### 7. `yield()`

* Suggests the thread scheduler to give other threads of the **same or higher priority** a chance to run.
* Not guaranteed, depends on JVM and OS.

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " is running...");
            Thread.yield();
        }
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();
        t1.start();
        t2.start();
    }
}
```

---

### 8. `setDaemon(boolean)`

* Marks a thread as **daemon thread**.
* Daemon threads run in the background (like Garbage Collector).
* JVM exits when only daemon threads remain.

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        while (true) {
            System.out.println("Hello world!");
        }
    }

    public static void main(String[] args) {
        MyThread daemonThread = new MyThread();
        daemonThread.setDaemon(true); // daemon thread
        daemonThread.start();

        MyThread userThread = new MyThread();
        userThread.start(); // user thread
        System.out.println("Main Done");
    }
}
```

---

## Synchronization in Java

When multiple threads access a **shared resource**, race conditions may occur.

### Without Synchronization

```java
class Counter {
    private int count = 0; // shared resource

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class MyThread extends Thread {
    private Counter counter;

    public MyThread(Counter counter) {
        this.counter = counter;
    }

    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            counter.increment();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        MyThread t1 = new MyThread(counter);
        MyThread t2 = new MyThread(counter);

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println(counter.getCount()); // Expected: 2000, Actual: random < 2000
    }
}
```

### With Synchronization

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

Now, only one thread can access `increment()` at a time, ensuring correct output (`2000`).

---

## Interview Questions & Tricky Questions

### Q1. Difference between `Runnable` and `Thread`?

* `Runnable`: Task and execution are separated, allows multiple inheritance.
* `Thread`: Direct thread control but restricts inheritance.

---

### Q2. What happens if you call `run()` instead of `start()`?

* `run()` executes in the **main thread** → no concurrency.
* `start()` creates a **new thread**.

---

### Q3. What is a daemon thread?

* Background threads that support user threads (e.g., GC).
* JVM exits when only daemon threads remain.

---

### Q4. Why is synchronization needed?

* Prevents **race conditions** when multiple threads access shared resources.

---

### Q5. Tricky One:

What happens if two threads call a synchronized method on **different objects**?

* Both threads will execute **concurrently**, since synchronization is per-object, not per-method/class.

---

