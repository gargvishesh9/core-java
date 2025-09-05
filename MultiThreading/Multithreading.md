
# Multithreading and Related Concepts in Java

## 1. CPU
- The **CPU** (Central Processing Unit), often called the *brain of the computer*, is responsible for executing instructions from programs.  
- It performs:
  - Arithmetic operations  
  - Logic operations  
  - Control operations  
  - Input/Output operations  

---

## 2. Core
- A **core** is an individual processing unit inside the CPU.  
- Modern CPUs can have multiple cores, allowing them to perform tasks in parallel.  
- Example: A **quad-core processor** has 4 cores.  
  - One core may handle a web browser  
  - Another plays music  
  - Another manages downloads  
  - Another runs background updates  

---

## 3. Program
- A **program** is a set of instructions written in a programming language.  
- It tells the computer how to perform a **specific task**.  
- Example:  
  - *Microsoft Word* is a program that allows users to create and edit documents.  

---

## 4. Process
- A **process** is an instance of a program that is **running**.  
- When a program is started, the OS creates a process to manage its execution.  
- Example:  
  - Opening *Microsoft Word* creates a process in the operating system.  

---

## 5. Thread
- A **thread** is the **smallest unit of execution** within a process.  
- A process can contain **multiple threads**, sharing the same memory/resources but running independently.  
- Example:  
  - A browser (like Chrome) may run each tab as a separate thread for better responsiveness.  

---

## 6. Multitasking
- **Multitasking** allows the operating system to run multiple processes simultaneously.  
- **On single-core CPUs:**  
  - Achieved using *time-sharing* → the CPU rapidly switches between tasks (illusion of parallelism).  
- **On multi-core CPUs:**  
  - True parallel execution, with processes distributed across cores.  
- Example:  
  - Browsing the internet, listening to music, and downloading a file at the same time.  

---

## 7. Multithreading
- **Multithreading** is the ability to execute **multiple threads within a single process concurrently**.  
- Benefits:  
  - Better CPU utilization  
  - Improved responsiveness  
  - Faster execution  
- Example (Browser):  
  - One thread renders the page  
  - Another runs JavaScript  
  - Another manages user inputs  

### Multitasking vs Multithreading
- **Multitasking** → Running multiple applications (process-level).  
- **Multithreading** → Running multiple threads inside the same application (thread-level).  

### Single-core vs Multi-core
- **Single-core:** Threads and processes share CPU using time slicing + context switching.  
- **Multi-core:** Threads and processes can run in **true parallel** on different cores.  

---

## 8. Time Slicing
- **Definition:** Divides CPU time into small intervals called **time slices (quanta)**.  
- **Function:** The OS scheduler allocates slices to processes/threads.  
- **Purpose:** Prevents one task from monopolizing CPU, ensures fair execution.  

---

## 9. Context Switching
- **Definition:** Saving the state of a currently running process/thread and loading the state of the next one.  
- **Function:** Happens when a time slice expires or a higher-priority task arrives.  
- **Purpose:** Enables multiple processes/threads to share CPU resources, creating the illusion of simultaneous execution.  

---

## 10. Summary
- **CPU → Cores → Program → Process → Thread → Multitasking → Multithreading → Time Slicing → Context Switching**  
- Together, these concepts explain how modern operating systems achieve **parallelism and concurrency**.  
```


