# 🔠 Java Data Types & Variables

---

## 📖 Variables in Java
- A **variable** is a name given to a memory location.
- It is used to store data which can be modified during program execution.

### 🔑 Declaration & Initialization
```java
int age = 24;      // declaration + initialization
String name = "Vishesh";
```

---

## 🧩 Types of Variables
1. **Local Variable**
   - Declared inside a method, constructor, or block.
   - Must be initialized before use.
   ```java
   void show() {
       int x = 10; // local variable
       System.out.println(x);
   }
   ```

2. **Instance Variable**
   - Declared inside a class, but outside methods.
   - Each object gets its own copy.
   ```java
   class Student {
       int age;   // instance variable
   }
   ```

3. **Static Variable**
   - Declared with `static` keyword.
   - Shared among all objects of the class.
   ```java
   class Student {
       static String college = "ABC University"; 
   }
   ```

---

## 🔢 Data Types in Java

Java supports two categories:  
1. **Primitive Data Types** (8 built-in)  
2. **Non-Primitive Data Types** (user-defined)

---

### 📊 Primitive Data Types

| Data Type | Size | Range | Default Value | Example |
|-----------|------|---------------------------|---------------|---------|
| byte      | 1 byte | -128 to 127 | 0 | `byte b = 10;` |
| short     | 2 bytes | -32,768 to 32,767 | 0 | `short s = 1000;` |
| int       | 4 bytes | -2³¹ to 2³¹-1 | 0 | `int i = 100000;` |
| long      | 8 bytes | -2⁶³ to 2⁶³-1 | 0L | `long l = 100000L;` |
| float     | 4 bytes | approx 6–7 decimal digits | 0.0f | `float f = 10.5f;` |
| double    | 8 bytes | approx 15 decimal digits | 0.0d | `double d = 20.123;` |
| char      | 2 bytes | 0 to 65,535 (Unicode) | '\u0000' | `char c = 'A';` |
| boolean   | 1 bit (JVM-dependent) | true/false | false | `boolean flag = true;` |

---

### 🧮 Non-Primitive Data Types
- **String** → `String name = "Java";`  
- **Arrays** → `int[] arr = {1,2,3};`  
- **Classes** → User-defined objects  
- **Interfaces**  

---

## 📝 Example Program

```java
public class DataTypesExample {
    int instanceVar = 50;          // instance variable
    static String college = "ABC"; // static variable

    public void demo() {
        int localVar = 10;         // local variable
        System.out.println("Local: " + localVar);
    }

    public static void main(String[] args) {
        DataTypesExample obj = new DataTypesExample();
        obj.demo();
        System.out.println("Instance: " + obj.instanceVar);
        System.out.println("Static: " + college);
    }
}
```

---

## 🎯 Interview Questions

1. **What are the different types of variables in Java?**  
   - Local, Instance, Static.  

2. **What are the 8 primitive data types in Java?**  
   - byte, short, int, long, float, double, char, boolean.  

3. **Why is `char` 2 bytes in Java?**  
   - To support Unicode characters (not just ASCII).  

4. **What is the default value of local variables?**  
   - They **don’t** have default values, must be initialized.  

5. **Difference between primitive and non-primitive data types?**  
   - Primitive are predefined & simple (int, float…), non-primitive are user-defined (String, arrays…).  
