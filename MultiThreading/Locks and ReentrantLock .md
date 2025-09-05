
# Mastering Locks and ReentrantLock in Java

## Limitations of `synchronized` Keyword
The `synchronized` keyword provides **basic thread safety**, but comes with drawbacks:
- Locks entire method/block → **coarse-grained locking** (performance hit).
- No **try-lock mechanism** → threads block indefinitely → risk of **deadlock**.
- Only a **single monitor per object** → limited to `wait()` / `notify()` for coordination.
- Cannot support **multiple condition variables**.

---

## Lock Interface
The `Lock` interface (from `java.util.concurrent.locks`) provides:
- Finer control over locking/unlocking.
- Non-blocking `tryLock()` option.
- Timed locking → avoids indefinite waiting.
- Multiple condition variables for advanced coordination.

### Example: BankAccount with `ReentrantLock`
```java
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class BankAccount {
    private int balance = 100;
    private final Lock lock = new ReentrantLock();

    public void withdraw(int amount) {
        System.out.println(Thread.currentThread().getName() + " attempting to withdraw " + amount);
        try {
            if (lock.tryLock(1000, TimeUnit.MILLISECONDS)) { // try-lock with timeout
                try {
                    if (balance >= amount) {
                        System.out.println(Thread.currentThread().getName() + " proceeding with withdrawal");
                        Thread.sleep(3000); // simulate processing time
                        balance -= amount;
                        System.out.println(Thread.currentThread().getName() +
                                " completed withdrawal. Remaining balance: " + balance);
                    } else {
                        System.out.println(Thread.currentThread().getName() + " insufficient balance");
                    }
                } finally {
                    lock.unlock(); // always release lock in finally
                }
            } else {
                System.out.println(Thread.currentThread().getName() +
                        " could not acquire the lock, will try later");
            }
        } catch (Exception e) {
            Thread.currentThread().interrupt();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount sbi = new BankAccount();

        Runnable task = () -> sbi.withdraw(50);

        Thread t1 = new Thread(task, "Thread 1");
        Thread t2 = new Thread(task, "Thread 2");

        t1.start();
        t2.start();
    }
}
````

---

## ReentrantLock

A **ReentrantLock** allows the same thread to acquire the lock **multiple times** without deadlock.
This is useful when nested methods require the same lock.

### Example: Nested Locking with ReentrantLock

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantExample {
    private final Lock lock = new ReentrantLock();

    public void outerMethod() {
        lock.lock();
        try {
            System.out.println("Outer method");
            innerMethod(); // re-acquires the same lock
        } finally {
            lock.unlock();
        }
    }

    public void innerMethod() {
        lock.lock();
        try {
            System.out.println("Inner method");
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) {
        ReentrantExample example = new ReentrantExample();
        example.outerMethod();
    }
}
```

---

## Methods of `ReentrantLock`

### 1. `lock()`

* Acquires the lock, **blocking until available**.
* If held by another thread → waits indefinitely.
* Use cautiously to avoid deadlocks.

### 2. `tryLock()`

* Attempts to acquire lock **immediately**.
* Returns `true` if successful, `false` otherwise.
* **Non-blocking**.

### 3. `tryLock(long timeout, TimeUnit unit)`

* Waits for lock up to given time.
* Returns `true` if acquired, `false` if timeout expires.
* Useful for avoiding **deadlock**.

### 4. `unlock()`

* Releases the lock.
* **Always call in `finally` block** to avoid leaving locks unreleased.

### 5. `lockInterruptibly()`

* Acquires the lock unless the thread is interrupted.
* Unlike `lock()`, allows interruption while waiting.

---

## Comparison: `synchronized` vs `ReentrantLock`

| Feature                  | `synchronized` | `ReentrantLock`                 |
| ------------------------ | -------------- | ------------------------------- |
| Implicit Locking         | ✅ Yes          | ❌ No (manual)                   |
| Explicit Unlock Required | ❌ No           | ✅ Yes (finally)                 |
| Reentrancy               | ✅ Yes          | ✅ Yes                           |
| Fairness Policy          | ❌ No           | ✅ Yes (optional in constructor) |
| Try-Lock Support         | ❌ No           | ✅ Yes                           |
| Timed Locking            | ❌ No           | ✅ Yes                           |
| Multiple Condition Vars  | ❌ No           | ✅ Yes                           |

---

## Interview Questions & Tricky Questions

### Q1. What is the difference between `synchronized` and `ReentrantLock`?

* `synchronized` → simple, automatic, but limited.
* `ReentrantLock` → flexible, supports tryLock, fairness, multiple conditions.

---

### Q2. Why must `unlock()` be placed in a `finally` block?

* To ensure lock is released even if exceptions occur.
* Otherwise → deadlock risk.

---

### Q3. What is Reentrancy?

* The ability of the same thread to **acquire the same lock multiple times** without deadlock.

---

### Q4. Tricky One:

What happens if a thread calls `lock()` twice without `unlock()`?

* Thread acquires the lock twice (ReentrantLock allows re-entrance).
* But it must call `unlock()` **twice** to release it completely.

---



