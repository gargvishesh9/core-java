# 🔐 Access Modifiers in Java & Singleton Pattern

---

## 📖 Introduction

**Access Modifiers** in Java determine the **visibility and accessibility of classes, methods, and variables**. They help in **encapsulation and security**.

Java provides **4 types of access modifiers:**

1. **private**

   * Accessible **only within the same class**.
   * Promotes **data hiding**.

2. **default (package-private)**

   * No keyword used. Accessible **within the same package only**.

3. **protected**

   * Accessible **within the same package** and **by subclasses (even in different packages)**.

4. **public**

   * Accessible **from anywhere**.

---

## ✅ Access Modifiers Table

| Modifier  | Same Class | Same Package | Subclass | Everywhere |
| --------- | ---------- | ------------ | -------- | ---------- |
| private   | ✅          | ❌            | ❌        | ❌          |
| default   | ✅          | ✅            | ❌        | ❌          |
| protected | ✅          | ✅            | ✅        | ❌          |
| public    | ✅          | ✅            | ✅        | ✅          |

---

## 📝 Example: Access Modifiers

```java
class Person {
    private String name;           // accessible only in this class
    int age;                        // default access (package-private)
    protected String address;       // accessible in package & subclasses
    public String email;            // accessible everywhere

    // Getter for private variable
    public String getName() {
        return name;
    }

    // Setter for private variable
    public void setName(String name) {
        this.name = name;
    }
}

public class AccessDemo {
    public static void main(String[] args) {
        Person p = new Person();
        p.setName("Vishesh");
        System.out.println("Name: " + p.getName());
        p.age = 24;
        p.address = "Delhi";
        p.email = "vishesh@example.com";
        System.out.println(p.age + ", " + p.address + ", " + p.email);
    }
}
```

### Output

```
Name: Vishesh
24, Delhi, vishesh@example.com
```

---

## 🏛 Singleton Pattern Example (Limit to One Object)

```java
class Singleton {
    private static Singleton instance; // single instance

    private Singleton() {
        // private constructor to prevent instantiation
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    public void showMessage() {
        System.out.println("Singleton object accessed");
    }
}

public class SingletonDemo {
    public static void main(String[] args) {
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();

        s1.showMessage();
        System.out.println(s1 == s2); // true, same object
    }
}
```

### Output

```
Singleton object accessed
true
```

---

## 🔑 Why Use Access Modifiers & Singleton?

* Control **visibility** and **access** to class members.
* Protect **sensitive data**.
* Maintain **encapsulation** and **modular code**.
* Singleton pattern ensures **only one instance exists**, useful for **database connections, logging, configuration classes**.

---

## ⚠️ Important Notes

* Private members: accessible **only inside class**.
* Default: accessible **within package**.
* Protected: accessible **within package & subclasses**.
* Public: accessible **everywhere**.
* Singleton: **lazy initialization** ensures instance is created **only when needed**.
* Use **synchronized getInstance()** for **thread safety** if required.

---

## 💡 Interview Questions

**1. What are access modifiers in Java?**
They determine the **visibility and accessibility** of classes, methods, and variables to control encapsulation.

**2. What are the types of access modifiers?**
Private, Default (package-private), Protected, Public.

**3. Difference between private, protected, default, and public?**

* Private → within class only
* Default → within package
* Protected → within package + subclasses
* Public → everywhere

**4. Can top-level classes be private?**
❌ No, top-level classes can only be **public** or **default**.

**5. What is the Singleton pattern?**
Singleton ensures that **only one object of a class is created** throughout the application.

**6. How is Singleton implemented?**

* Private constructor to restrict instantiation
* Private static instance variable
* Public static method (getInstance()) to return single instance

**7. Why use Singleton?**

* Database connections
* Logging
* Configuration classes

**8. How does Singleton relate to access modifiers?**

* Uses **private constructor** to restrict instantiation
* Uses **public static method** to provide controlled access

**9. Can Singleton be broken using reflection or serialization?**
✅ Yes, but can be prevented using **enums or defensive coding**.

**10. Difference between Singleton and normal class?**

* Singleton → only one instance, private constructor
* Normal class → multiple instances allowed
