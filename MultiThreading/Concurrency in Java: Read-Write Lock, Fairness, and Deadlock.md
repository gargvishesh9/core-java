
# Concurrency in Java: Read-Write Lock, Fairness, and Deadlock

Java provides advanced concurrency mechanisms to handle multi-threaded environments efficiently.  
Some important concepts are **Read-Write Locks**, **Fairness in Locks**, and **Deadlocks**.  
Let’s explore each in detail with examples.

---

## 🔹 Read-Write Lock

A **Read-Write Lock** allows:
- Multiple threads to **read** data concurrently.
- Only **one thread** to **write** at a time (exclusive access).
- Provided by `ReentrantReadWriteLock`.

This is useful in **read-heavy applications** (e.g., caching, analytics systems) where reads dominate writes.

### Example: ReadWriteCounter
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteCounter {
    private int count = 0;
    private final ReadWriteLock lock = new ReentrantReadWriteLock();
    private final Lock readLock = lock.readLock();
    private final Lock writeLock = lock.writeLock();

    public void increment() {
        writeLock.lock();
        try {
            count++;
            Thread.sleep(50); // simulate delay
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        } finally {
            writeLock.unlock();
        }
    }

    public int getCount() {
        readLock.lock();
        try {
            return count;
        } finally {
            readLock.unlock();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        ReadWriteCounter counter = new ReadWriteCounter();

        Runnable readTask = () -> {
            for (int i = 0; i < 10; i++) {
                System.out.println(Thread.currentThread().getName() + " read: " + counter.getCount());
            }
        };

        Runnable writeTask = () -> {
            for (int i = 0; i < 10; i++) {
                counter.increment();
                System.out.println(Thread.currentThread().getName() + " incremented");
            }
        };

        Thread writerThread = new Thread(writeTask);
        Thread readerThread1 = new Thread(readTask);
        Thread readerThread2 = new Thread(readTask);

        writerThread.start();
        readerThread1.start();
        readerThread2.start();

        writerThread.join();
        readerThread1.join();
        readerThread2.join();

        System.out.println("Final count: " + counter.getCount());
    }
}
````

✅ Multiple readers don’t block each other.
✅ Writers get **exclusive access**.
✅ Prevents inconsistent data reads.

---

## 🔹 Fairness of Locks

* **Fair Lock**: Threads acquire the lock **in the order they requested it** (FIFO). Prevents starvation but may reduce throughput.
* **Non-Fair Lock** (default): Allows threads to "jump ahead," improving performance but risking starvation.

### Example: Fair Lock

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class FairnessLockExample {
    private final Lock lock = new ReentrantLock(true); // fair lock

    public void accessResource() {
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + " acquired the lock.");
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            System.out.println(Thread.currentThread().getName() + " released the lock.");
            lock.unlock();
        }
    }

    public static void main(String[] args) {
        FairnessLockExample example = new FairnessLockExample();

        Runnable task = () -> example.accessResource();

        Thread thread1 = new Thread(task, "Thread 1");
        Thread thread2 = new Thread(task, "Thread 2");
        Thread thread3 = new Thread(task, "Thread 3");

        thread1.start();
        thread2.start();
        thread3.start();
    }
}
```

✅ Ensures fairness.
❌ Slightly slower due to maintaining queue order.

---

## 🔹 Deadlock

A **Deadlock** occurs when two or more threads are **blocked forever**, each waiting for a resource held by the other.

### Example: Pen and Paper Deadlock

```java
class Pen {
    public synchronized void writeWithPenAndPaper(Paper paper) {
        System.out.println(Thread.currentThread().getName() + " is using pen " + this + " and trying to use paper " + paper);
        paper.finishWriting();
    }

    public synchronized void finishWriting() {
        System.out.println(Thread.currentThread().getName() + " finished using pen " + this);
    }
}

class Paper {
    public synchronized void writeWithPaperAndPen(Pen pen) {
        System.out.println(Thread.currentThread().getName() + " is using paper " + this + " and trying to use pen " + pen);
        pen.finishWriting();
    }

    public synchronized void finishWriting() {
        System.out.println(Thread.currentThread().getName() + " finished using paper " + this);
    }
}

class Task1 implements Runnable {
    private Pen pen;
    private Paper paper;

    public Task1(Pen pen, Paper paper) {
        this.pen = pen;
        this.paper = paper;
    }

    @Override
    public void run() {
        pen.writeWithPenAndPaper(paper); // Thread1 locks pen, waits for paper
    }
}

class Task2 implements Runnable {
    private Pen pen;
    private Paper paper;

    public Task2(Pen pen, Paper paper) {
        this.pen = pen;
        this.paper = paper;
    }

    @Override
    public void run() {
        synchronized (pen) {
            paper.writeWithPaperAndPen(pen); // Thread2 locks paper, waits for pen
        }
    }
}

public class DeadlockExample {
    public static void main(String[] args) {
        Pen pen = new Pen();
        Paper paper = new Paper();

        Thread thread1 = new Thread(new Task1(pen, paper), "Thread-1");
        Thread thread2 = new Thread(new Task2(pen, paper), "Thread-2");

        thread1.start();
        thread2.start();
    }
}
```

✅ Demonstrates **deadlock** where both threads wait indefinitely.

---

## 🔹 Interview Questions

1. **What is a Read-Write Lock? How is it different from a normal Lock?**
2. **Can multiple threads acquire a Read Lock simultaneously?**
3. **What is fairness in locks? Which is faster: fair or non-fair locks?**
4. **What is a deadlock? How can it occur?**
5. **How do you detect and resolve deadlocks in Java?**
6. **What are the ways to avoid deadlock?**

   * Lock ordering
   * Timeout with `tryLock()`
   * Deadlock detection algorithms
   * Using higher-level concurrency utilities (`java.util.concurrent`)

---

## 🔹 Tricky Questions

1. If multiple threads are reading and one thread requests a write lock, what happens?
   → Readers finish, then writer gets lock. No new readers allowed until writer is done.

2. Can a thread upgrade from **read lock to write lock** in `ReentrantReadWriteLock`?
   → No, upgrading causes **deadlock risk**. Only downgrade (write → read) is safe.

3. Which one is the default: Fair Lock or Non-Fair Lock?
   → Non-Fair (better performance, but risk of starvation).

4. Can deadlocks happen with only **one lock**?
   → No, deadlock requires at least **two resources** with circular waiting.

5. How to simulate **deadlock-free design**?
   → Always acquire locks in a **consistent global order**.

---

# ✅ Summary

* **ReadWriteLock** improves performance in **read-heavy** scenarios.
* **Fairness** prevents starvation but lowers throughput.
* **Deadlocks** are dangerous and must be avoided using design strategies.

