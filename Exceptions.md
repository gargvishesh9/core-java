
# Mastering Exceptions in Java

## Table of Contents
1. [Introduction to Exceptions](#introduction-to-exceptions)
2. [Types of Exceptions](#types-of-exceptions)
3. [Try-Catch Block](#try-catch-block)
4. [Finally Block](#finally-block)
5. [Throw Keyword](#throw-keyword)
6. [Throws Keyword](#throws-keyword)
7. [Try-with-Resources](#try-with-resources)
8. [Custom Exceptions](#custom-exceptions)
9. [UML Flow Diagrams](#uml-flow-diagrams)
10. [Common Interview Questions](#common-interview-questions)
11. [Tricky Questions](#tricky-questions)

---

## Introduction to Exceptions
- An **exception** is an event that disrupts the normal flow of a program.  
- Java uses **exception handling mechanism** to handle runtime errors.  

**Hierarchy (simplified):**
```

Throwable
├── Error (not recoverable, e.g. OutOfMemoryError)
└── Exception
├── Checked (IOException, SQLException)
└── Unchecked/Runtime (NullPointerException, ArithmeticException)

````

---

## Types of Exceptions
- **Checked exceptions**: Checked at compile-time (e.g., FileNotFoundException).  
- **Unchecked exceptions**: Occur at runtime (e.g., ArithmeticException).  
- **Errors**: Serious problems (e.g., StackOverflowError).

---

## Try-Catch Block
- `try` block contains risky code.  
- `catch` block handles the exception.

```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // risky code
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e);
        }
    }
}
````

**Output:**

```
Exception caught: java.lang.ArithmeticException: / by zero
```

---

## Finally Block

* `finally` always executes, regardless of exception occurrence.
* Useful for **cleanup code** (closing DB connections, files, etc.).

```java
try {
    int arr[] = new int[2];
    System.out.println(arr[5]);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Exception: " + e);
} finally {
    System.out.println("Finally block executed");
}
```

---

## Throw Keyword

* Used to **explicitly throw an exception**.

```java
class Main {
    static void validateAge(int age) {
        if (age < 18) {
            throw new ArithmeticException("Not eligible to vote");
        } else {
            System.out.println("Eligible to vote");
        }
    }

    public static void main(String[] args) {
        validateAge(15);
    }
}
```

**Output:**

```
Exception in thread "main" java.lang.ArithmeticException: Not eligible to vote
```

---

## Throws Keyword

* Declares exceptions a method can throw.
* Caller must handle them.

```java
import java.io.*;

class Test {
    void readFile() throws IOException {
        FileReader fr = new FileReader("test.txt");
    }
}

public class Main {
    public static void main(String[] args) {
        Test t = new Test();
        try {
            t.readFile();
        } catch (IOException e) {
            System.out.println("Handled IOException: " + e);
        }
    }
}
```

---

## Try-with-Resources

* Introduced in Java 7.
* Used for automatic resource management (`AutoCloseable` interface).
* Resources are closed automatically.

```java
import java.io.*;

public class Main {
    public static void main(String[] args) {
        try (FileReader fr = new FileReader("test.txt")) {
            int data = fr.read();
            while (data != -1) {
                System.out.print((char) data);
                data = fr.read();
            }
        } catch (IOException e) {
            System.out.println("Exception: " + e);
        }
    }
}
```

---

## Custom Exceptions

* User-defined exceptions by extending `Exception` or `RuntimeException`.

```java
class InvalidAgeException extends Exception {
    InvalidAgeException(String message) {
        super(message);
    }
}

public class Main {
    static void validate(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age is not valid");
        }
        System.out.println("Welcome to voting portal");
    }

    public static void main(String[] args) {
        try {
            validate(16);
        } catch (InvalidAgeException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }
}
```

**Output:**

```
Caught Exception: Age is not valid
```

---

## UML Flow Diagrams

**General Try-Catch Flow**

```
   Start
     |
     v
   try block ---- Exception? ---> catch block --> recover
     |
     v
 finally block
     |
     v
   End
```

**Throw/Throws**

```
Method A ----(throws Exception)----> Method B
     |                                    |
     v                                    v
caller must handle                  try-catch OR further throws
```

**Try-with-Resources**

```
try(resource) {
   use resource
}
catch(Exception e) {
   handle exception
}
resource closed automatically
```

---

## Common Interview Questions

1. **Q:** Difference between `throw` and `throws`?
   **A:** `throw` is used to **actually throw** an exception, `throws` is used in method declaration to specify possible exceptions.

2. **Q:** Can we have multiple catch blocks?
   **A:** Yes, but they should be ordered from **specific to general**.

3. **Q:** Can finally block be skipped?
   **A:** Yes, if JVM shuts down (e.g., `System.exit(0)`).

4. **Q:** Difference between checked and unchecked exceptions?
   **A:** Checked are handled at **compile-time**, unchecked at **runtime**.

---

## Tricky Questions

1. **Q:** What happens if both catch and finally have return statements?
   **A:** The `finally` return overrides the `catch` return.

2. **Q:** Can we catch multiple exceptions in one block?
   **A:** Yes, using multi-catch (`catch(IOException | SQLException e)`).

3. **Q:** What if we don’t catch a checked exception?
   **A:** Compiler error, unless declared with `throws`.

4. **Q:** Can a constructor use `throws`?
   **A:** Yes, constructors can declare exceptions using `throws`.

---

```
