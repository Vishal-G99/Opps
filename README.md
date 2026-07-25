## ❓ Question
**What is Programming?**

Programming is the process of writing instructions that tell a computer what to do. These instructions are written in programming languages such as Python, Java, C++, or JavaScript.

## ❓ Question
**What is Procedural Programming?**

**Procedural Programming** is a programming paradigm that organizes a program into a collection of **procedures** or **functions**. It follows a step-by-step approach where the program is executed sequentially, and each procedure performs a specific task.

In procedural programming, the focus is on **functions** and the sequence of actions rather than on objects or data. Data is typically shared among functions through variables.

**Examples of Procedural Programming Languages:**
- C
- Pascal
- BASIC
- FORTRAN
- Java (Supports procedural programming, but is primarily Object-Oriented)

## ❓ Question
**Why does Procedural Programming have limitations?**

Procedural Programming has several limitations because it focuses on **functions (procedures)** rather than **data**. As applications become larger and more complex, managing code, data, and dependencies becomes difficult.

### Limitations of Procedural Programming

- **No Data Encapsulation:** Data is often stored in global variables, making it accessible from anywhere in the program. This reduces data security and increases the risk of unintended modifications.

- **Poor Data Security:** Since functions can directly access and modify shared data, it is difficult to protect sensitive information.

- **Difficult to Maintain Large Applications:** As the number of functions grows, understanding, debugging, and maintaining the code becomes more challenging.

- **Limited Code Reusability:** Functions can be reused, but procedural programming lacks features like inheritance and polymorphism, which provide better reusability in Object-Oriented Programming.

- **High Dependency Between Functions:** Changes in one function may affect multiple other functions that depend on it, making maintenance more difficult.

- **Real-World Modeling is Difficult:** Procedural programming does not naturally represent real-world entities such as **Student**, **Employee**, or **Car** because it focuses on procedures instead of objects.

- **Code Scalability Issues:** As the project grows, the codebase becomes harder to organize, extend, and manage.

### Example

Suppose a banking application stores account details in global variables. Any function can modify the account balance directly, increasing the risk of accidental or unauthorized changes. In Object-Oriented Programming, the balance can be made private and accessed only through controlled methods.

### Interview Answer

Procedural Programming has limitations because it focuses on functions rather than data. It lacks data encapsulation, provides poor security, is difficult to maintain for large applications, offers limited code reusability, and does not model real-world entities effectively. These limitations led to the development of Object-Oriented Programming (OOP).


## ❓ Question
**What is OOP (Object-Oriented Programming)?**

**Object-Oriented Programming (OOP)** is a programming paradigm that organizes software around **objects** rather than functions or procedures. An object is an instance of a class that contains both **data (attributes)** and **behavior (methods)**.

OOP helps developers build **modular, reusable, secure, and maintainable** applications by modeling real-world entities as objects.

### Key Features of OOP
- **Class** – A blueprint for creating objects.
- **Object** – An instance of a class.
- **Encapsulation** – Wrapping data and methods into a single unit and restricting direct access to data.
- **Abstraction** – Hiding implementation details and exposing only essential features.
- **Inheritance** – Allowing one class to inherit properties and methods from another class.
- **Polymorphism** – Allowing the same method to behave differently based on the object.

### Advantages of OOP
- Improves code reusability.
- Makes applications easier to maintain and extend.
- Provides better data security through encapsulation.
- Models real-world entities effectively.
- Reduces code duplication.
- Simplifies testing and debugging.

### Example (Java)

```java
class Car {
    String brand;

    void start() {
        System.out.println("Car is starting...");
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.brand = "Toyota";
        car.start();
    }
}
```

### Output

```text
Car is starting...
```

### Real-World Example

Consider a **Car**:
- **Attributes (Data):** Brand, Model, Color, Speed
- **Behaviors (Methods):** Start(), Stop(), Accelerate(), Brake()

In OOP, the **Car** is represented as an **object**, while the **Car class** acts as its blueprint.

### Interview Answer

Object-Oriented Programming (OOP) is a programming paradigm that uses **objects** to design and develop software. It combines data and methods into a single unit called an object and is based on four core principles: **Encapsulation, Abstraction, Inheritance, and Polymorphism**. OOP improves code reusability, security, scalability, and maintainability.

## ❓ Question
**Why is Java Object-Oriented?**

Java is called an **Object-Oriented Programming (OOP)** language because it is designed around the concept of **classes and objects**. In Java, almost everything is represented as an object, and programs are built by creating classes and their objects.

Java follows the **four fundamental principles of OOP**:

- **Encapsulation** – Combines data and methods into a single unit and restricts direct access to data.
- **Abstraction** – Hides implementation details and exposes only the necessary functionality.
- **Inheritance** – Allows one class to inherit properties and behaviors from another class, promoting code reuse.
- **Polymorphism** – Allows the same method to perform different behaviors depending on the object.

### Why Java is Considered Object-Oriented

- Every Java program is written using **classes**.
- Objects are created from classes to represent real-world entities.
- Data and behavior are grouped together in objects.
- Java supports the four core OOP principles.
- Java encourages code reusability, modularity, and maintainability.

### Example (Java)

```java
class Student {
    String name;

    void study() {
        System.out.println(name + " is studying.");
    }
}

public class Main {
    public static void main(String[] args) {
        Student student = new Student();
        student.name = "Rahul";
        student.study();
    }
}
```

### Output

```text
Rahul is studying.
```

### Why Java is Not 100% Object-Oriented

Java is **not considered a pure Object-Oriented language** because it also supports **primitive data types**, which are not objects.

Examples of primitive data types:
- `int`
- `char`
- `boolean`
- `double`
- `float`
- `byte`
- `short`
- `long`

For example:

```java
int age = 25; // Primitive type, not an object
```

Languages like **Smalltalk** are considered **pure Object-Oriented** because everything in those languages is an object.

### Interview Answer

Java is called an Object-Oriented Programming language because it is based on **classes and objects** and supports the four core OOP principles: **Encapsulation, Abstraction, Inheritance, and Polymorphism**. However, Java is **not a purely Object-Oriented language** because it includes primitive data types such as `int`, `char`, and `boolean`, which are not objects.


## ❓ Question
**What are the Characteristics of Object-Oriented Programming (OOP)?**

Object-Oriented Programming (OOP) is based on several key characteristics that make software more **modular, reusable, secure, scalable, and maintainable**. These characteristics help in modeling real-world entities and simplifying software development.

### 1. Class
A **Class** is a blueprint or template used to create objects. It defines the properties (fields) and behaviors (methods) that objects of that class will have.

**Example:** `Car`, `Student`, `Employee`

---

### 2. Object
An **Object** is an instance of a class. It represents a real-world entity with its own state (data) and behavior (methods).

**Example:** A specific car like `Toyota Camry` is an object of the `Car` class.

---

### 3. Encapsulation
**Encapsulation** is the process of combining data (variables) and methods into a single unit (class) while restricting direct access to the data using access modifiers.

**Benefits:**
- Improves data security.
- Prevents unauthorized access.
- Makes code easier to maintain.

---

### 4. Abstraction
**Abstraction** is the process of hiding implementation details and exposing only the essential features to the user.

**Benefits:**
- Reduces complexity.
- Improves code readability.
- Allows implementation changes without affecting users.

---

### 5. Inheritance
**Inheritance** allows one class to inherit the properties and methods of another class.

**Benefits:**
- Promotes code reusability.
- Reduces code duplication.
- Simplifies maintenance.

---

### 6. Polymorphism
**Polymorphism** allows the same method or interface to perform different behaviors depending on the object.

**Types:**
- Compile-time Polymorphism (Method Overloading)
- Runtime Polymorphism (Method Overriding)

**Benefits:**
- Increases flexibility.
- Improves extensibility.
- Simplifies code management.

---

### 7. Message Passing
Objects communicate with each other by invoking methods. This interaction is known as **Message Passing**.

**Example:**
```java
car.start();
student.study();
```

---

### 8. Dynamic Binding
The method to be executed is determined at **runtime**, especially when using method overriding.

**Benefit:**
- Enables Runtime Polymorphism.

---

### 9. Modularity
OOP divides a large application into smaller, independent classes, making the application easier to develop, test, and maintain.

---

### 10. Reusability
Existing classes can be reused through **Inheritance**, **Composition**, and **Polymorphism**, reducing development time and improving code quality.

---

### Summary Table

| Characteristic | Description |
|----------------|-------------|
| Class | Blueprint for creating objects |
| Object | Instance of a class |
| Encapsulation | Hides and protects data |
| Abstraction | Hides implementation details |
| Inheritance | Reuses code from existing classes |
| Polymorphism | One interface, multiple behaviors |
| Message Passing | Objects communicate through methods |
| Dynamic Binding | Method resolved at runtime |
| Modularity | Divides application into independent classes |
| Reusability | Reuses existing code efficiently |

---

### Interview Answer

The main characteristics of Object-Oriented Programming are **Class, Object, Encapsulation, Abstraction, Inheritance, Polymorphism, Message Passing, Dynamic Binding, Modularity, and Reusability**. These characteristics help developers build secure, reusable, scalable, and maintainable software by organizing programs around objects and their interactions.
