# 🔤 ASCII Values & Type Conversion in Java

---

## 📖 ASCII Values in Java
- **ASCII (American Standard Code for Information Interchange)** assigns a unique integer value to characters.
- In Java, **char** is a 16-bit data type (Unicode), but ASCII values (0–127) are fully supported.

### Example:
```java
public class ASCIIExample {
    public static void main(String[] args) {
        char ch = 'A';
        int ascii = ch;   // implicit conversion (char → int)
        System.out.println("ASCII value of " + ch + " = " + ascii);
    }
}
```
✅ Output:
```
ASCII value of A = 65
```

---

## 🔄 Type Conversion in Java

### 1️⃣ Implicit Conversion (Widening)
- Happens automatically when a **smaller data type → larger data type**.
- No data loss, safe conversion.

**Example:**
```java
public class ImplicitConversion {
    public static void main(String[] args) {
        int num = 100;
        double d = num;  // int → double (widening)
        System.out.println(d);  // 100.0
    }
}
```

---

### 2️⃣ Explicit Conversion (Narrowing)
- Programmer explicitly converts larger type → smaller type.
- May cause **data loss** or **precision loss**.
- Syntax: `(targetType) value`

**Example:**
```java
public class ExplicitConversion {
    public static void main(String[] args) {
        double d = 99.99;
        int num = (int) d;  // double → int (narrowing)
        System.out.println(num);  // 99
    }
}
```

---

## 📝 Summary Table

| Conversion Type | Direction            | Data Loss | Example                 |
|-----------------|----------------------|-----------|-------------------------|
| Implicit (Widening) | Smaller → Larger | ❌ No     | int → double            |
| Explicit (Narrowing) | Larger → Smaller | ⚠️ Yes   | double → int            |

---

## 🎯 Interview Questions

1. What is the ASCII value of `'a'` and `'A'`?  
   - `'a'` = 97, `'A'` = 65

2. What is the difference between implicit and explicit conversion?  
   - Implicit: automatic, safe.  
   - Explicit: manual, may cause data loss.

3. Can every narrowing conversion cause data loss?  
   - Not always, but it **can** if the value exceeds target type’s range.

4. Why does `char` store Unicode but still support ASCII?  
   - Because ASCII is a subset of Unicode.

---
