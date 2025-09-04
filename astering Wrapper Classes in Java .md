
# Mastering Wrapper Classes in Java

## Table of Contents
1. [Introduction to Wrapper Classes](#introduction-to-wrapper-classes)
2. [Why Wrapper Classes?](#why-wrapper-classes)
3. [Types of Wrapper Classes](#types-of-wrapper-classes)
4. [Autoboxing](#autoboxing)
5. [Unboxing](#unboxing)
6. [Commonly Used Methods](#commonly-used-methods)
7. [UML Representation](#uml-representation)
8. [Comparison: Primitive vs Wrapper](#comparison-primitive-vs-wrapper)
9. [Common Interview Questions](#common-interview-questions)
10. [Tricky Questions](#tricky-questions)

---

## Introduction to Wrapper Classes
- Wrapper classes **convert primitive data types into objects**.
- Each primitive type has a corresponding wrapper class in `java.lang` package.
- Enables use of primitives in **collections (like ArrayList, HashMap)** which require objects.

---

## Why Wrapper Classes?
- **Object representation** of primitives.
- **Collections support** (cannot store primitives directly).
- **Utility methods** for conversions, parsing, comparisons.
- **Autoboxing & Unboxing** for smooth primitive–object interchange.

---

## Types of Wrapper Classes

| Primitive | Wrapper Class |
|-----------|---------------|
| `byte`    | `Byte`        |
| `short`   | `Short`       |
| `int`     | `Integer`     |
| `long`    | `Long`        |
| `float`   | `Float`       |
| `double`  | `Double`      |
| `char`    | `Character`   |
| `boolean` | `Boolean`     |

---

## Autoboxing
- Automatic **conversion of primitive → wrapper object**.
- Introduced in Java 5.

```java
public class Main {
    public static void main(String[] args) {
        int x = 10;
        Integer y = x; // Autoboxing
        System.out.println("Wrapper: " + y);
    }
}
````

**Output:**

```
Wrapper: 10
```

---

## Unboxing

* Automatic **conversion of wrapper object → primitive**.

```java
public class Main {
    public static void main(String[] args) {
        Integer x = 20;
        int y = x; // Unboxing
        System.out.println("Primitive: " + y);
    }
}
```

**Output:**

```
Primitive: 20
```

---

## Commonly Used Methods

### Integer Class

```java
public class Main {
    public static void main(String[] args) {
        Integer num = 100;

        // Conversion
        System.out.println(Integer.toString(num));   // "100"
        System.out.println(Integer.parseInt("123")); // 123
        System.out.println(Integer.valueOf("50"));   // 50

        // Constants
        System.out.println(Integer.MAX_VALUE); // 2147483647
        System.out.println(Integer.MIN_VALUE); // -2147483648

        // Bit count
        System.out.println(Integer.bitCount(15)); // 4 (1111)
    }
}
```

### Character Class

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(Character.isDigit('5'));  // true
        System.out.println(Character.isLetter('A')); // true
        System.out.println(Character.toLowerCase('X')); // x
    }
}
```

### Boolean Class

```java
public class Main {
    public static void main(String[] args) {
        Boolean b1 = Boolean.valueOf("true");
        Boolean b2 = Boolean.FALSE;

        System.out.println(b1 && b2); // false
    }
}
```

---

## UML Representation

```
        Wrapper Classes
--------------------------------
Primitive <--> Wrapper <--> Object
--------------------------------
int      <--> Integer
char     <--> Character
boolean  <--> Boolean
float    <--> Float
... and so on
```

---

## Comparison: Primitive vs Wrapper

| Feature              | Primitive          | Wrapper Class      |
| -------------------- | ------------------ | ------------------ |
| Memory               | Stored in stack    | Stored in heap     |
| Default values       | 0, false, '\u0000' | null               |
| Null handling        | Not applicable     | Can be null        |
| Usage in collections | Not allowed        | Allowed            |
| Methods available    | No                 | Yes (utility APIs) |

---

## Common Interview Questions

1. **Q:** Why do we need wrapper classes in Java?
   **A:** To use primitives as objects, especially in Collections, and to leverage utility methods.

2. **Q:** Difference between `parseInt()` and `valueOf()`?
   **A:** `parseInt()` returns primitive `int`, `valueOf()` returns `Integer` object.

3. **Q:** What is Autoboxing and Unboxing?
   **A:** Automatic conversion between primitive and wrapper classes.

4. **Q:** Are wrapper classes immutable?
   **A:** Yes, they are immutable and final.

---

## Tricky Questions

1. **Q:** Why is `Integer` caching between -128 to 127?
   **A:** For performance, Java caches these values in the Integer pool.

```java
Integer a = 100, b = 100;
System.out.println(a == b); // true (cached)

Integer x = 200, y = 200;
System.out.println(x == y); // false (not cached)
```

2. **Q:** Difference between `==` and `.equals()` for wrapper classes?

   * `==` compares references.
   * `.equals()` compares values.

3. **Q:** Can null be assigned to a wrapper? What happens if unboxed?

   * Yes, wrapper can be null.
   * If unboxed, it throws **NullPointerException**.

```java
Integer n = null;
int x = n; // NullPointerException
```

---


