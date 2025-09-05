
# Mastering Multithreading in Java

## 1. Introduction
- **Multithreading** in Java allows applications to perform multiple tasks simultaneously.  
- Improves **performance** and **responsiveness**.  
- Supported in the `java.lang` package.  
- Threads are **lightweight processes** managed by the **JVM** and **Operating System**.  

---

## 2. Multithreading in Different Environments

### Single-core
- JVM + OS perform **time-slicing** to give illusion of concurrency.  
- Only one thread executes at a time, but CPU switches rapidly between threads.  

### Multi-core
- Threads can run in **true parallelism**, distributed across multiple cores.  
- The JVM assigns different threads to different cores.  

---

## 3. Main Thread in Java
- When a Java program starts, one thread runs immediately: the **main thread**.  
- It executes the `main()` method.  

```java
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello world !");
    }
}
````

---

## 4. Creating Threads in Java

### Method 1: Extending `Thread` class

* Create a class extending `Thread`.
* Override the `run()` method.
* Call `start()` to begin execution.

```java
public class Test {
    public static void main(String[] args) {
        World world = new World();
        world.start();
        for (;;) {
            System.out.println("Hello");
        }
    }
}

class World extends Thread {
    @Override
    public void run() {
        for (;;) {
            System.out.println("World");
        }
    }
}
```

---

### Method 2: Implementing `Runnable` interface

* Create a class implementing `Runnable`.
* Override the `run()` method.
* Create a `Thread` object, passing the `Runnable` instance.
* Call `start()` on the `Thread` object.

```java
public class Test {
    public static void main(String[] args) {
        World world = new World();
        Thread thread = new Thread(world);
        thread.start();
        for (;;) {
            System.out.println("Hello");
        }
    }
}

class World implements Runnable {
    @Override
    public void run() {
        for (;;) {
            System.out.println("World");
        }
    }
}
```

---

## 5. Thread Lifecycle in Java

A thread goes through several states during execution:

1. **NEW** – Created but not started.
2. **RUNNABLE** – Ready to run, waiting for CPU time.
3. **RUNNING** – Currently executing.
4. **BLOCKED / WAITING / TIMED\_WAITING** – Waiting for resources or a condition.
5. **TERMINATED** – Execution finished.

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("RUNNING"); // RUNNING
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            System.out.println(e);
        }
    }

    public static void main(String[] args) throws InterruptedException {
        MyThread t1 = new MyThread();
        System.out.println(t1.getState()); // NEW
        t1.start();
        System.out.println(t1.getState()); // RUNNABLE
        Thread.sleep(100);
        System.out.println(t1.getState()); // TIMED_WAITING
        t1.join();
        System.out.println(t1.getState()); // TERMINATED
    }
}
```

---

## 6. Interview Questions

### Q1. What is the difference between a process and a thread?

* **Process**: Independent execution unit with its own memory space.
* **Thread**: Lightweight unit inside a process, shares memory and resources.

### Q2. Which is better: Extending `Thread` or Implementing `Runnable`?

* **Runnable** is preferred because:

  * Allows multiple inheritance (a class can implement multiple interfaces).
  * Decouples task from thread management.

### Q3. What is the difference between `start()` and `run()`?

* `start()` → Creates a new thread and calls `run()` internally.
* `run()` → Executes in the current thread like a normal method call.

### Q4. What are daemon threads?

* Background service threads (e.g., garbage collector).
* JVM exits when only daemon threads remain.

### Q5. How do you make a thread sleep or wait?

* `Thread.sleep(ms)` → Pauses for given milliseconds.
* `wait()` → Waits until another thread calls `notify()`.

---

## 7. Tricky Questions

### Q1. Can we restart a thread in Java?

* No, once a thread is terminated, it **cannot be restarted**.

### Q2. What happens if we call `run()` directly instead of `start()`?

* `run()` executes like a normal method in the current thread.
* No new thread is created.

### Q3. Can constructors be synchronized?

* No, constructors cannot be synchronized. Only methods/blocks can.

### Q4. What is thread starvation?

* When a low-priority thread never gets CPU time because high-priority threads keep executing.

### Q5. What is the difference between user threads and daemon threads?

* **User thread**: Keeps JVM alive until execution completes.
* **Daemon thread**: JVM exits once only daemon threads remain.

---

## 8. Summary

* Java provides strong support for **multithreading** via `Thread` and `Runnable`.
* Threads can be managed through lifecycle states.
* Understanding threading is critical for building **high-performance and responsive applications**.


```
