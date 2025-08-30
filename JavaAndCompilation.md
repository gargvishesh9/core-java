# ☕ Java & Compilation Process

---

## 📖 Introduction
Java is a **high-level, object-oriented, platform-independent** programming language.  
It follows the principle of **WORA (Write Once, Run Anywhere)** using the **JVM**.

---

## 🔑 Core Components

### 1️⃣ JDK (Java Development Kit)
- Complete package for development.  
- Includes:
  - Compiler (`javac`)
  - JRE
  - Tools (debugger, jar, etc.)

### 2️⃣ JRE (Java Runtime Environment)
- Required to **run Java programs**.  
- Contains:
  - JVM
  - Core libraries  
- Does **not** have compiler/dev tools.

### 3️⃣ JVM (Java Virtual Machine)
- Executes **bytecode**.  
- Provides:
  - Memory management  
  - Garbage collection  
  - Security  

### 4️⃣ JIT (Just-In-Time Compiler)
- Part of JVM.  
- Converts **bytecode → native machine code** at runtime.  
- Improves performance by caching hot code.

---

## ⚙️ Compilation & Execution Flow

```mermaid
flowchart LR
    A[Write Code: Hello.java] --> B[javac Compiler]
    B --> C[Bytecode: Hello.class]
    C --> D[JVM]
    D --> E[JIT Compiler]
    E --> F[Native Machine Code]
    F --> G[Program Runs 🎉]
```

---

## 🔍 Breaking Down the `main` Method

```java
public static void main(String[] args)
```

- **public** → JVM calls this method from outside the class, so it must be accessible everywhere.  
- **static** → Allows JVM to call `main()` without creating an object.  
- **void** → No return value; JVM doesn’t expect anything back.  
- **main** → Special predefined name. JVM looks for this as the entry point.  
- **String[] args** → Stores command-line arguments (e.g., `java Hello Vishesh`).  

---

## 🖨️ `System.out.println`

- **System** → A predefined class in `java.lang` package.  
- **out** → A static object of `PrintStream` class, defined in `System`.  
- **println()** → A method of `PrintStream` that prints data to the console and moves to a new line.  

Example:  
```java
System.out.println("Hello, Java!");
```

Output:  
```
Hello, Java!
```

---

## 🎯 Common Interview Questions

### 🔹 Basics
1. **What is Java and why is it platform-independent?**  
   - Java compiles to **bytecode**, which runs on any JVM regardless of OS.  

2. **Differentiate between JDK, JRE, JVM.**  
   - **JDK** = Development tools + JRE  
   - **JRE** = JVM + libraries (to run programs)  
   - **JVM** = Executes bytecode  

3. **What is the role of the Just-In-Time (JIT) compiler?**  
   - Converts frequently executed bytecode into native machine code at runtime for better performance.  

---

### 🔹 main Method
4. **Why is `main()` method static in Java?**  
   - Because JVM can call it **without creating an object** of the class.  

5. **Can we run a Java class without `main()` method?**  
   - In modern Java (>=7), **No**, JVM requires `main()`.  
   - Older versions allowed `static initializers` as entry, but that’s deprecated.  

6. **Why is the return type of `main()` method void?**  
   - Because JVM does not expect a return value from `main()`.  

---

### 🔹 System.out.println
7. **Explain `System.out.println` in detail.**  
   - `System` = class  
   - `out` = static reference to `PrintStream`  
   - `println()` = method to print data  

8. **Difference between `print()` and `println()`?**  
   - `print()` → prints on the same line  
   - `println()` → prints and moves to the next line  


