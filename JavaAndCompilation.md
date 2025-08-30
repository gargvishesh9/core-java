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


