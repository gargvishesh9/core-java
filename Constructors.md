# 🔨 Constructors and Constructor Overloading in Java

---

## 📖 Introduction

A **constructor** in Java is a **special method** used to **initialize objects**.  
- It has the **same name as the class**.  
- **No return type** (not even void).  
- Automatically called when an object is created.

**Types of Constructors:**
1. **Default constructor** – No parameters.  
2. **Parameterized constructor** – Takes arguments to initialize object with specific values.  

---

## ✅ Key Points

- Constructors **initialize object state**.  
- **Constructor overloading** allows a class to have **multiple constructors with different parameter lists**.  
- Helps **create objects in different ways**.  
- Overloaded constructors must differ by **number or type of parameters**, **not return type**.  

---

## 📝 Example: Constructor and Constructor Overloading

```java
class Student {
    String name;
    int age;

    // Default constructor
    Student() {
        name = "Unknown";
        age = 0;
    }

    // Parameterized constructor
    Student(String n, int a) {
        name = n;
        age = a;
    }

    // Another parameterized constructor (overloaded)
    Student(String n) {
        name = n;
        age = 18; // default age
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class ConstructorDemo {
    public static void main(String[] args) {
        Student s1 = new Student();              // default constructor
        Student s2 = new Student("Vishesh", 24);// parameterized constructor
        Student s3 = new Student("Riya");       // overloaded constructor

        s1.display();
        s2.display();
        s3.display();
    }
}
```



---

## 🔑 Why Use Constructor Overloading?

- Provides **flexibility** in object creation.  
- Avoids the need to write **multiple initialization methods**.  
- Helps **initialize objects with different sets of data**.  

---

## ⚠️ Important Notes

- Constructor name must **match the class name**.  
- Constructors **do not have a return type**.  
- Overloading is resolved at **compile-time**.  
- Default constructor is provided by compiler **only if no constructor is explicitly defined**.  

---

## 💡 Interview Questions

**1. What is a constructor in Java?**  
A constructor is a special method used to **initialize objects of a class**. It has the **same name as the class** and **no return type**.

**2. What is constructor overloading?**  
Constructor overloading is when a class has **multiple constructors with different parameter lists**.

**3. Can constructors have a return type?**  
❌ No, constructors cannot have a return type (not even void).

**4. When is the default constructor provided?**  
The compiler provides a default constructor **only if no constructor is explicitly defined**.

**5. Can a constructor call another constructor?**  
✅ Yes, using the `this()` keyword.

**6. Difference between method overloading and constructor overloading?**  
- Method overloading → multiple methods with same name, different parameters.  
- Constructor overloading → multiple constructors with same name, different parameters.  
