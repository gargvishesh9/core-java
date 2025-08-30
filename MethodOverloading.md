# ⚡ Method Overloading in Java

---

## 📖 Introduction
**Method Overloading** in Java is a feature that allows a class to have **multiple methods with the same name**, but with **different parameter lists** (number of parameters or type of parameters).

It is a type of **compile-time polymorphism**.

---

## ✅ Key Points
- Methods must have the **same name** but **different parameter list**.
- Can differ by:
  - **Number of parameters**  
  - **Type of parameters**  
  - **Order of parameters**  
- **Return type alone** is **not sufficient** to distinguish overloaded methods.
- It improves **readability** and **code reusability**.

---

## 📝 Example

```java
class Calculator {
    // Method with 2 int parameters
    int add(int a, int b) {
        return a + b;
    }

    // Method with 3 int parameters
    int add(int a, int b, int c) {
        return a + b + c;
    }

    // Method with 2 double parameters
    double add(double a, double b) {
        return a + b;
    }
}

public class MethodOverloadingDemo {
    public static void main(String[] args) {
        Calculator calc = new Calculator();

        System.out.println("Sum of 2 integers: " + calc.add(10, 20));
        System.out.println("Sum of 3 integers: " + calc.add(10, 20, 30));
        System.out.println("Sum of 2 doubles: " + calc.add(5.5, 6.5));
    }
}
```
---

## 🔑 Why Use Method Overloading?
- Provides **flexibility** in method calling.  
- **Increases readability** by using the same method name for similar actions.  
- Helps achieve **compile-time polymorphism**.  

---

## ⚠️ Important Notes
- Overloading happens **within the same class** (or child class via inheritance).  
- **Return type** does not play a role in method overloading resolution.  
- Overloading ≠ Overriding (Overriding is **runtime polymorphism**).  

---

## 💡 Interview Questions

1. **What is method overloading in Java?**  
   Method overloading is when two or more methods in the same class have the same name but different parameter lists.

2. **Can method overloading depend only on return type?**  
   ❌ No, return type alone cannot distinguish overloaded methods.

3. **Is method overloading compile-time or runtime polymorphism?**  
   ✅ Compile-time polymorphism.

4. **Can we overload `main()` method in Java?**  
   ✅ Yes, we can overload it, but JVM will only call the `main(String[] args)` version.

5. **Can constructors be overloaded in Java?**  
   ✅ Yes, constructor overloading is allowed.

6. **What happens if two overloaded methods have the same number of parameters but different data types?**  
   Java decides which method to call based on the **argument types** passed during method invocation.

7. **Difference between method overloading and method overriding?**  
   - Overloading → Same name, different parameters, **compile-time polymorphism**.  
   - Overriding → Same name, same parameters, **runtime polymorphism**.  

---


