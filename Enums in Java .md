
# Mastering Enums in Java

## Table of Contents
1. [Introduction to Enums](#introduction-to-enums)
2. [Internal Working of Enums](#internal-working-of-enums)
3. [Enum Basics](#enum-basics)
4. [Important Enum Methods](#important-enum-methods)
   - [toString()](#tostring)
   - [values()](#values)
   - [ordinal()](#ordinal)
   - [valueOf()](#valueof)
5. [Enums with Fields and Methods](#enums-with-fields-and-methods)
6. [Enums in Switch Statements](#enums-in-switch-statements)
7. [Enum Implementing Interfaces](#enum-implementing-interfaces)
8. [Enum and Singleton Design Pattern](#enum-and-singleton-design-pattern)
9. [UML Representation](#uml-representation)
10. [Common Interview Questions](#common-interview-questions)
11. [Tricky Questions](#tricky-questions)

---

## Introduction to Enums
- An **enum (enumeration)** is a special Java type used to define a collection of **constants**.
- Introduced in Java 5.
- Enums are **type-safe**, meaning you can’t assign values outside defined constants.

---

## Internal Working of Enums
- Enums are **implicitly final and extend `java.lang.Enum`**.
- They **cannot extend other classes** because they already extend `Enum`.
- Enums can **implement interfaces**.
- Each constant in enum is an **object** of that enum type, created implicitly.

```java
enum Color {
    RED, GREEN, BLUE;
}
````

Internally:

```java
final class Color extends Enum<Color> {
    public static final Color RED = new Color("RED", 0);
    public static final Color GREEN = new Color("GREEN", 1);
    public static final Color BLUE = new Color("BLUE", 2);

    private Color(String name, int ordinal) {
        super(name, ordinal);
    }
}
```

---

## Enum Basics

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

public class Main {
    public static void main(String[] args) {
        Day today = Day.FRIDAY;
        System.out.println(today);
    }
}
```

**Output:**

```
FRIDAY
```

---

## Important Enum Methods

### toString()

* Returns name of enum constant.

```java
Day d = Day.MONDAY;
System.out.println(d.toString()); // MONDAY
```

---

### values()

* Returns array of all constants.

```java
for (Day d : Day.values()) {
    System.out.println(d);
}
```

**Output:**

```
MONDAY
TUESDAY
...
SUNDAY
```

---

### ordinal()

* Returns **position (0-based index)** of constant.

```java
System.out.println(Day.MONDAY.ordinal()); // 0
System.out.println(Day.FRIDAY.ordinal()); // 4
```

---

### valueOf()

* Returns enum constant for given string.

```java
Day d = Day.valueOf("SUNDAY");
System.out.println(d); // SUNDAY
```

---

## Enums with Fields and Methods

Enums can have **constructors, fields, and methods**.

```java
enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    EARTH(5.976e+24, 6.37814e6),
    MARS(6.421e+23, 3.3972e6);

    private final double mass;   // in kilograms
    private final double radius; // in meters

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }

    double surfaceGravity() {
        final double G = 6.67300E-11;
        return G * mass / (radius * radius);
    }
}

public class Main {
    public static void main(String[] args) {
        for (Planet p : Planet.values()) {
            System.out.println(p + " has gravity: " + p.surfaceGravity());
        }
    }
}
```

---

## Enums in Switch Statements

```java
enum TrafficLight { RED, YELLOW, GREEN }

public class Main {
    public static void main(String[] args) {
        TrafficLight signal = TrafficLight.RED;

        switch (signal) {
            case RED:
                System.out.println("STOP!");
                break;
            case YELLOW:
                System.out.println("READY!");
                break;
            case GREEN:
                System.out.println("GO!");
                break;
        }
    }
}
```

---

## Enum Implementing Interfaces

```java
interface Greeting {
    void sayHello();
}

enum Language implements Greeting {
    ENGLISH, HINDI, FRENCH;

    public void sayHello() {
        switch (this) {
            case ENGLISH: System.out.println("Hello"); break;
            case HINDI:   System.out.println("Namaste"); break;
            case FRENCH:  System.out.println("Bonjour"); break;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Language.ENGLISH.sayHello();
        Language.HINDI.sayHello();
    }
}
```

---

## Enum and Singleton Design Pattern

Enums are best for implementing **Singleton** (thread-safe, serialization safe).

```java
enum Singleton {
    INSTANCE;

    public void showMessage() {
        System.out.println("Hello from Singleton Enum");
    }
}

public class Main {
    public static void main(String[] args) {
        Singleton s = Singleton.INSTANCE;
        s.showMessage();
    }
}
```

---

## UML Representation

```
          java.lang.Enum
                 ^
                 |
             <<enum>>
              Day
 ----------------------------
 MONDAY, TUESDAY, WEDNESDAY...
 ----------------------------
 +values(): Day[]
 +valueOf(String): Day
 +ordinal(): int
 +toString(): String
```

---

## Common Interview Questions

1. **Q:** Can enums extend classes?
   **A:** No, they implicitly extend `java.lang.Enum`.

2. **Q:** Can enums implement interfaces?
   **A:** Yes, they can.

3. **Q:** Are enums thread-safe?
   **A:** Yes, because constants are implicitly `public static final`.

4. **Q:** Default values of enum fields?
   **A:** All enum constants are initialized when the enum class is loaded.

---

## Tricky Questions

1. **Q:** Can enums have constructors?
   **A:** Yes, but constructors are always **private** or package-private.

2. **Q:** Difference between `==` and `.equals()` for enums?
   **A:** Both compare references, but since enums are singleton objects, both give same result.

3. **Q:** Can we create enum inside a class?
   **A:** Yes, enums can be declared inside classes just like inner classes.

4. **Q:** Can enum constants have different behaviors?
   **A:** Yes, using **abstract methods** in enum.

```java
enum Operation {
    PLUS {
        double apply(double x, double y) { return x + y; }
    },
    MINUS {
        double apply(double x, double y) { return x - y; }
    };

    abstract double apply(double x, double y);
}

public class Main {
    public static void main(String[] args) {
        System.out.println(Operation.PLUS.apply(3, 4));  // 7.0
        System.out.println(Operation.MINUS.apply(9, 2)); // 7.0
    }
}
```

