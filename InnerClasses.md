
# Mastering Java Inner Classes

## Table of Contents
1. [Introduction to Inner Classes](#introduction-to-inner-classes)
2. [Types of Inner Classes](#types-of-inner-classes)
   - [Member Inner Classes](#member-inner-classes)
   - [Static Nested Classes](#static-nested-classes)
   - [Local Inner Classes](#local-inner-classes)
   - [Anonymous Inner Classes](#anonymous-inner-classes)
3. [UML Diagrams](#uml-diagrams)
4. [Comparison of Inner Classes](#comparison-of-inner-classes)
5. [Common Interview Questions](#common-interview-questions)
6. [Tricky Questions](#tricky-questions)

---

## Introduction to Inner Classes
- **Inner classes** are classes defined **within another class**.  
- They help logically group classes that are only used in one place.  
- They can access **private members** of the outer class.  

**Key Points:**
- Inner classes improve **encapsulation**.
- Can be **non-static** or **static**.

---

## Types of Inner Classes

### Member Inner Classes
- Defined **inside a class but outside any method**.
- Can access **all members (including private)** of the outer class.
- Non-static, so an object of outer class is required.

```java
class Outer {
    private String msg = "Hello from Outer";

    class Inner {
        void display() {
            System.out.println(msg); // can access private member
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner(); // syntax to create object
        inner.display();
    }
}
````

**Output:**

```
Hello from Outer
```

---

### Static Nested Classes

* Declared `static` inside the outer class.
* Can only access **static members** of outer class.
* No need for outer class instance to create.

```java
class Outer {
    static String msg = "Static Outer";

    static class Nested {
        void display() {
            System.out.println(msg);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.Nested nested = new Outer.Nested(); // direct access
        nested.display();
    }
}
```

**Output:**

```
Static Outer
```

---

### Local Inner Classes

* Defined **inside a method**.
* Can access **final or effectively final variables** from the method.

```java
class Outer {
    void outerMethod() {
        String localMsg = "Local Inner";

        class LocalInner {
            void print() {
                System.out.println(localMsg);
            }
        }

        LocalInner li = new LocalInner();
        li.print();
    }
}

public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.outerMethod();
    }
}
```

**Output:**

```
Local Inner
```

---

### Anonymous Inner Classes

* **No name** for the class.
* Usually used to **override methods** or implement **interfaces** inline.

```java
interface Greeting {
    void sayHello();
}

public class Main {
    public static void main(String[] args) {
        Greeting g = new Greeting() {
            public void sayHello() {
                System.out.println("Hello from Anonymous Class");
            }
        };

        g.sayHello();
    }
}
```

**Output:**

```
Hello from Anonymous Class
```

* Can also extend a class anonymously:

```java
class Printer {
    void print() {
        System.out.println("Printing...");
    }
}

public class Main {
    public static void main(String[] args) {
        Printer p = new Printer() {
            void print() {
                System.out.println("Anonymous Printing");
            }
        };
        p.print();
    }
}
```

---

## UML Diagrams

**Member Inner Class**

```
Outer
----------------
- msg: String
+Outer()
+outerMethod()
----------------
  Inner
  ----------------
  +display()
```

**Static Nested Class**

```
Outer
----------------
+ msg: static String
----------------
  Nested (static)
  ----------------
  +display()
```

**Local/Anonymous Inner Classes**

* Not explicitly shown in UML (method-local scope).

---

## Comparison of Inner Classes

| Feature                   | Member Inner Class | Static Nested Class   | Local Inner Class       | Anonymous Inner Class       |
| ------------------------- | ------------------ | --------------------- | ----------------------- | --------------------------- |
| Requires outer instance?  | Yes                | No                    | No                      | No (inline)                 |
| Can access outer members? | Yes                | Only static members   | Final/effectively final | Outer + interface/class     |
| Can have name?            | Yes                | Yes                   | Yes                     | No                          |
| Usage                     | Logical grouping   | Group related classes | Method-specific logic   | Quick inline implementation |

---

## Common Interview Questions

1. **Q:** Difference between member and static nested class?
   **A:** Member requires outer instance, can access all members; static nested class can only access static members.

2. **Q:** Can a local inner class be declared static?
   **A:** No, only member nested class can be static.

3. **Q:** Can anonymous inner class access local variables?
   **A:** Yes, but they must be **final or effectively final**.

4. **Q:** How to instantiate a member inner class?
   **A:** `Outer.Inner inner = outer.new Inner();`

---

## Tricky Questions

1. **Q:** Can a static nested class access non-static members of outer class?
   **A:** No, only static members.

2. **Q:** Can an inner class have static methods?
   **A:** Only if it’s a **static nested class**. Member, local, or anonymous cannot have static methods.

3. **Q:** Can an anonymous inner class implement multiple interfaces?
   **A:** No, it can **only extend one class or implement one interface**.

4. **Q:** What happens if a local variable in outer method is modified after inner class uses it?
   **A:** Compile-time error. It must remain **effectively final**.

---

```
