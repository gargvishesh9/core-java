

## Introduction to Interfaces
- An **interface** in Java is a blueprint of a class that contains **abstract methods** (methods without body) and **constants**.
- Interfaces allow you to achieve **full abstraction**.
- They support **multiple inheritance**, which classes alone cannot.

**Key Points:**
- Cannot instantiate interfaces.
- Methods are **implicitly public and abstract** (except default and static methods).
- Variables are **public, static, and final** by default.

---

## Syntax and Example

```java
interface Animal {
    int legs = 4; // public static final by default

    void eat();   // public abstract by default
    void sleep();
}

class Dog implements Animal {
    public void eat() {
        System.out.println("Dog eats bones");
    }

    public void sleep() {
        System.out.println("Dog sleeps in kennel");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();
        d.sleep();
        System.out.println("Legs: " + Dog.legs);
    }
}
````

**Output:**

```
Dog eats bones
Dog sleeps in kennel
Legs: 4
```

---

## Multiple Inheritance with Interfaces

* Java **does not support multiple inheritance with classes** to avoid ambiguity.
* But **interfaces allow multiple inheritance**.

```java
interface A {
    void methodA();
}

interface B {
    void methodB();
}

class C implements A, B {
    public void methodA() {
        System.out.println("Method A");
    }
    public void methodB() {
        System.out.println("Method B");
    }
}
```

**Diagram (ASCII UML style):**

```
  A       B
   \     /
    \   /
      C
```

---

## Abstraction in Java

* **Abstraction** hides implementation details and shows functionality only.
* Achieved using **abstract classes** and **interfaces**.

```java
abstract class Vehicle {
    abstract void start();
}

interface Electric {
    void charge();
}

class Tesla extends Vehicle implements Electric {
    void start() {
        System.out.println("Tesla starts silently");
    }
    public void charge() {
        System.out.println("Charging Tesla battery");
    }
}
```

---

## Abstract Class vs Interface

| Feature              | Abstract Class              | Interface                            |
| -------------------- | --------------------------- | ------------------------------------ |
| Multiple inheritance | No                          | Yes                                  |
| Method body          | Can have concrete methods   | Only default/static methods allowed  |
| Variables            | Can have instance variables | Only `public static final` constants |
| Constructor          | Yes                         | No                                   |
| Inheritance keyword  | `extends`                   | `implements`                         |

---

## Methods and Variables in Interface

* **Methods**:

  * By default: `public abstract`
  * From Java 8: `default` and `static` methods allowed
  * From Java 9: `private` methods allowed

```java
interface TestInterface {
    int num = 10; // public static final

    void show(); // public abstract

    default void display() {
        System.out.println("Default method in interface");
    }

    static void greet() {
        System.out.println("Static method in interface");
    }
}
```

---

## Default and Static Methods

```java
interface A {
    default void hello() {
        System.out.println("Hello from A");
    }
    static void bye() {
        System.out.println("Goodbye from A");
    }
}

class B implements A {
    public void hello() {
        System.out.println("Hello from B");
    }
}

public class Main {
    public static void main(String[] args) {
        B obj = new B();
        obj.hello(); // overridden default method
        A.bye();     // static method called with interface name
    }
}
```

---

## Functional Interfaces

* A **functional interface** has **exactly one abstract method**.
* Used for **lambda expressions**.

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

---

## Common Interview Questions

1. **Can an interface extend another interface?**

   * Yes, using `extends`. Multiple inheritance is allowed.

2. **Can an interface contain a constructor?**

   * No, interfaces cannot have constructors.

3. **Can a class implement multiple interfaces?**

   * Yes, a class can implement multiple interfaces using `implements`.

4. **What are default methods?**

   * Methods with a body inside an interface. Introduced in Java 8.

---

## Tricky Questions

1. **Q:** If two interfaces have the same default method, and a class implements both, what happens?
2. 
   **A:** The class must override the method, else **compile-time error** occurs.

3. **Q:** Can interface variables be changed?
4. 
   **A:** No, they are `public static final` by default (constants).

5. **Q:** Can an interface extend multiple interfaces?
6. 
   **A:** Yes, e.g., `interface C extends A, B {}`

7. **Q:** Can we declare private methods in interface?
8. 
   **A:** Yes, from Java 9 onwards. Useful for code reuse within interface.

---

```
