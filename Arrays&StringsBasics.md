# 📚 Java Strings, Arrays & Jagged Arrays

---

## 🔤 Strings in Java

- A **String** is a sequence of characters.  
- In Java, Strings are **objects** of the `String` class.  
- Strings are **immutable** (cannot be changed after creation).

### ✅ Creating Strings
```java
String s1 = "Hello";               // String literal
String s2 = new String("World");   // Using new keyword
```

### ✅ Common Methods
```java
String str = "Java Programming";

System.out.println(str.length());         // 16
System.out.println(str.toUpperCase());    // JAVA PROGRAMMING
System.out.println(str.toLowerCase());    // java programming
System.out.println(str.charAt(0));        // J
System.out.println(str.substring(0, 4));  // Java
System.out.println(str.contains("Prog")); // true
System.out.println(str.equals("Java"));   // false
```

---

## 📦 Arrays in Java

- An **array** is a collection of elements of the **same type** stored in contiguous memory.  
- Arrays in Java are **objects**.  

### ✅ Declaring & Initializing
```java
int[] arr1 = new int[5];            // declaration + size
int[] arr2 = {10, 20, 30, 40, 50};  // direct initialization
```

### ✅ Accessing Elements
```java
System.out.println(arr2[2]);  // 30
arr2[1] = 25;                 // modifying element
```

### ✅ Looping Through Arrays
```java
for(int i=0; i<arr2.length; i++) {
    System.out.println(arr2[i]);
}

for(int num : arr2) {   // enhanced for loop
    System.out.println(num);
}
```

---

## 🧩 Jagged Arrays (Array of Arrays)

- A **jagged array** is an array of arrays with **different column sizes**.  
- Useful when each row has varying number of elements.

### ✅ Example
```java
public class JaggedArrayExample {
    public static void main(String[] args) {
        int[][] jagged = new int[3][];
        
        jagged[0] = new int[2];  // row 0 has 2 columns
        jagged[1] = new int[3];  // row 1 has 3 columns
        jagged[2] = new int[4];  // row 2 has 4 columns

        int value = 1;
        for(int i=0; i<jagged.length; i++) {
            for(int j=0; j<jagged[i].length; j++) {
                jagged[i][j] = value++;
            }
        }

        // printing
        for(int i=0; i<jagged.length; i++) {
            for(int j=0; j<jagged[i].length; j++) {
                System.out.print(jagged[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

✅ Output:
```
1 2
3 4 5
6 7 8 9
```

---

## 🎯 Interview Questions

1. Why are Strings immutable in Java?  
   - For **security, caching, and thread-safety**.

2. Difference between `String`, `StringBuilder`, and `StringBuffer`?  
   - `String` → immutable  
   - `StringBuilder` → mutable, not thread-safe (faster)  
   - `StringBuffer` → mutable, thread-safe (slower)

3. How are arrays stored in memory?  
   - Contiguous memory blocks with fixed size.

4. Difference between normal 2D array and jagged array?  
   - 2D → all rows have same number of columns.  
   - Jagged → rows can have different column sizes.

---
