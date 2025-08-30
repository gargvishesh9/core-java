# 🔒 Encapsulation in Java

---

## 📖 Introduction
**Encapsulation** is one of the **four fundamental OOP concepts** in Java.  
It is the **mechanism of wrapping data (variables) and code (methods) together as a single unit**, and restricting direct access to some of an object's components.  

Encapsulation provides:
- **Data hiding**  
- **Controlled access** through methods  
- **Increased security** and maintainability  

---

## ✅ Key Points
- Achieved using **private variables** and **public getter/setter methods**.
- Helps **control how data is accessed or modified**.
- Promotes **modularity** and **maintainability**.
- Supports **abstraction** by hiding internal implementation.  

---

## 📝 Example Code

```java
class Person {
    // Private variables
    private String name;
    private int age;

    // Public getter and setter for name
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    // Public getter and setter for age
    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if(age > 0) {          // validation
            this.age = age;
        }
    }
}

public class EncapsulationDemo {
    public static void main(String[] args) {
        Person p = new Person();

        // Using setter methods to set values
        p.setName("Vishesh");
        p.setAge(24);

        // Using getter methods to retrieve values
        System.out.println("Name: " + p.getName());
        System.out.println("Age: " + p.getAge());
    }
}
```

## 🔑 Why Use Encapsulation?

- Protects sensitive data from unauthorized access.  
- Control changes using setter validation.  
- Easy maintenance and code modification.  
- Promotes modularity and data hiding.  

---

## ⚠️ Important Notes

- Private variables **cannot be accessed directly** from outside the class.  
- Public getters and setters provide **controlled access**.  
- Encapsulation is the foundation of **OOP security** and maintainable code.  

---

## 💡 Interview Questions

**1. What is encapsulation in Java?**  
Encapsulation is the process of wrapping variables and methods together and restricting direct access to some of the object’s components.

**2. How is encapsulation achieved in Java?**  
By declaring variables as **private** and providing **public getter and setter methods**.

**3. Why use encapsulation?**  
- Data hiding  
- Controlled access  
- Validation before modifying data  
- Easy maintenance

**4. Can private variables be accessed outside the class?**  
❌ No, only via getter and setter methods.

**5. Is encapsulation related to security?**  
✅ Yes, it helps in protecting sensitive data and controlling access.

**6. Can encapsulation be applied to constructors?**  
✅ Yes, constructors can also be private for **singleton pattern** or controlled instantiation.

---




