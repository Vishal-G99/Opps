# Chapter 3: Constructors

---

## ❓ Question
**What is a Constructor?**

A **constructor** is a special member of a class that is automatically executed when an object is created. Its primary purpose is to initialize the object's state.

A constructor:

- Has the same name as the class.
- Does **not** have any return type (not even `void`).
- Is automatically invoked when an object is created using the `new` keyword.
- Can be overloaded.
- Cannot be inherited.
- Can call another constructor using `this()` or the parent constructor using `super()`.

### Syntax

```java
class Employee {

    Employee() {
        System.out.println("Constructor Called");
    }
}
```

### Example

```java
class Employee {

    Employee() {
        System.out.println("Employee Object Created");
    }
}

public class Main {

    public static void main(String[] args) {

        Employee emp = new Employee();
    }
}
```

### Output

```
Employee Object Created
```

### Real-Life Example

Think of a constructor as the process of manufacturing a new car.

Whenever a new car is built:

- Engine is installed.
- Wheels are attached.
- Seats are fitted.
- Default software is installed.

Similarly, when an object is created, the constructor prepares it with initial values.

---

## ❓ Question
**Why do we need Constructors?**

Constructors ensure that every newly created object starts in a valid and initialized state.

Without constructors:

```java
Employee emp = new Employee();

emp.id = 101;
emp.name = "Rahul";
```

With constructors:

```java
class Employee {

    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
}

Employee emp = new Employee(101, "Rahul");
```

### Advantages

- Initializes objects automatically.
- Reduces repetitive code.
- Makes objects ready to use immediately.
- Improves code readability.

---

## ❓ Question
**What are the Types of Constructors in Java?**

Java mainly supports two types of constructors.

### 1. Default (No-Argument) Constructor

Does not accept any parameters.

```java
class Student {

    Student() {
        System.out.println("Default Constructor");
    }
}
```

### 2. Parameterized Constructor

Accepts one or more parameters.

```java
class Student {

    int id;

    Student(int id) {
        this.id = id;
    }
}
```

### Additional Constructor Patterns

Although Java officially has only two types, developers commonly use:

- Copy Constructor
- Private Constructor
- Singleton Constructor
- Constructor Chaining

---

## ❓ Question
**What is a Default Constructor?**

A default constructor is a constructor that takes **no arguments**.

If you do not write any constructor, the Java compiler automatically creates one.

Compiler-generated constructor:

```java
class Student {

}
```

Internally becomes

```java
class Student {

    Student() {

    }
}
```

### Example

```java
class Student {

    Student() {
        System.out.println("Object Created");
    }
}

public class Main {

    public static void main(String[] args) {

        Student s = new Student();
    }
}
```

### Output

```
Object Created
```

---

## ❓ Question
**When does Java create a Default Constructor automatically?**

The compiler creates a default constructor **only if** no constructor is written inside the class.

Example

```java
class Employee {

}
```

Compiler adds

```java
Employee() {

}
```

If any constructor already exists,

```java
class Employee {

    Employee(int id){

    }
}
```

then Java **does not** generate another default constructor.

---

## ❓ Question
**What is a Parameterized Constructor?**

A parameterized constructor accepts parameters so that different objects can have different initial values.

Example

```java
class Employee {

    int id;
    String name;

    Employee(int id, String name) {

        this.id = id;
        this.name = name;
    }
}
```

Creating Objects

```java
Employee e1 = new Employee(101, "Rahul");
Employee e2 = new Employee(102, "Amit");
```

Memory

```
Heap

Employee Object
---------------
id = 101
name = Rahul

Employee Object
---------------
id = 102
name = Amit
```

---

## ❓ Question
**What is Constructor Overloading?**

Constructor overloading means defining multiple constructors in the same class with different parameter lists.

Example

```java
class Student {

    Student() {

        System.out.println("Default");
    }

    Student(int id) {

        System.out.println(id);
    }

    Student(int id, String name) {

        System.out.println(id + " " + name);
    }
}
```

Creating Objects

```java
new Student();

new Student(101);

new Student(101, "Rahul");
```

Output

```
Default
101
101 Rahul
```

### Benefits

- Flexible object creation.
- Better readability.
- Multiple initialization options.

---

## ❓ Question
**What is Constructor Chaining?**

Constructor chaining means one constructor calls another constructor.

There are two ways.

- Using `this()`
- Using `super()`

Example

```java
class Student {

    Student() {

        this(100);

        System.out.println("Default");
    }

    Student(int id) {

        System.out.println(id);
    }
}
```

Output

```
100
Default
```

---

## ❓ Question
**What is `this()` in Java?**

`this()` calls another constructor within the same class.

Rules

- Must be the first statement.
- Can only call constructors of the same class.
- Avoids duplicate code.

Example

```java
class Employee {

    Employee() {

        this(101);

        System.out.println("Default");
    }

    Employee(int id) {

        System.out.println(id);
    }
}
```

Output

```
101
Default
```

---

## ❓ Question
**What is `super()` in Java?**

`super()` calls the constructor of the parent class.

Example

```java
class Person {

    Person() {

        System.out.println("Person Constructor");
    }
}

class Employee extends Person {

    Employee() {

        super();

        System.out.println("Employee Constructor");
    }
}
```

Output

```
Person Constructor
Employee Constructor
```

---

## ❓ Question
**What is the difference between `this()` and `super()`?**

| this() | super() |
|---------|----------|
| Calls constructor of same class | Calls constructor of parent class |
| Used for constructor chaining | Used for inheritance |
| Must be first statement | Must be first statement |
| Cannot be used with super() in same constructor | Cannot be used with this() in same constructor |

```

This is the exact Markdown style suitable for GitHub or interview notes.

The remaining topics—**Copy Constructor, Private Constructor, Singleton Constructor, Initialization Blocks, Static Blocks, Constructor Execution Flow, Memory Diagrams, Practical Examples, and 25 Interview Questions with Answers**—are too large to fit in a single response and should be continued in subsequent parts using the same format.



# Constructor Interview Questions and Answers

---

## ❓ Question 1
**What is a Constructor in Java?**

A constructor is a special member of a class that is automatically called when an object is created. It initializes the object's state.

**Key Points**
- Same name as the class.
- No return type.
- Automatically invoked using the `new` keyword.
- Can be overloaded.

---

## ❓ Question 2
**What are the characteristics of a Constructor?**

A constructor has the following characteristics:

- Same name as the class.
- No return type.
- Executes automatically during object creation.
- Can have access modifiers.
- Can be overloaded.
- Cannot be inherited.
- Cannot be overridden.
- First statement can be `this()` or `super()`.

---

## ❓ Question 3
**Why do we use Constructors?**

Constructors initialize objects with default or user-provided values.

### Example

```java
class Employee {
    int id;

    Employee(int id) {
        this.id = id;
    }
}
```

Without constructors, every object would need to be initialized manually.

---

## ❓ Question 4
**How many types of Constructors are there in Java?**

Java mainly has two types:

1. Default (No-Argument) Constructor
2. Parameterized Constructor

Common design patterns include:

- Copy Constructor
- Private Constructor
- Singleton Constructor

---

## ❓ Question 5
**What is the difference between a Constructor and a Method?**

| Constructor | Method |
|-------------|--------|
| Initializes an object | Performs an action |
| Same name as class | Any valid name |
| No return type | Has a return type or `void` |
| Called automatically | Called explicitly |
| Cannot be inherited | Can be inherited |

---

## ❓ Question 6
**Can a Constructor have a return type?**

No.

The following is **not** a constructor.

```java
class Student {

    void Student() {

    }
}
```

This is a normal method because it has a return type (`void`).

---

## ❓ Question 7
**Can Constructors be inherited?**

No.

Constructors belong to the class in which they are declared.

However, child constructors can invoke parent constructors using `super()`.

---

## ❓ Question 8
**Can Constructors be overridden?**

No.

Only methods participate in runtime polymorphism.

Constructors cannot be overridden because they are never inherited.

---

## ❓ Question 9
**Can Constructors be overloaded?**

Yes.

A class can have multiple constructors with different parameter lists.

```java
class Student {

    Student(){}

    Student(int id){}

    Student(int id,String name){}
}
```

---

## ❓ Question 10
**What happens if you don't write any Constructor?**

The Java compiler automatically creates a default constructor.

```java
class Student{

}
```

Compiler generates:

```java
Student(){

}
```

This happens only if no constructor exists.

---

## ❓ Question 11
**What happens if a Parameterized Constructor exists but no Default Constructor exists?**

Java will not generate a default constructor.

```java
class Student{

    Student(int id){

    }
}
```

The following will produce a compilation error.

```java
Student s = new Student();
```

Error:

```
constructor Student() is undefined
```

---

## ❓ Question 12
**What is Constructor Chaining?**

Constructor chaining means calling one constructor from another constructor.

It can be achieved using:

- `this()`
- `super()`

It helps avoid duplicate initialization code.

---

## ❓ Question 13
**What is `this()` in Constructors?**

`this()` invokes another constructor of the same class.

Rules:

- Must be the first statement.
- Used for constructor chaining.

Example:

```java
Student(){

    this(10);
}
```

---

## ❓ Question 14
**What is `super()` in Constructors?**

`super()` invokes the parent class constructor.

Example:

```java
class Person{

    Person(){

    }
}

class Employee extends Person{

    Employee(){

        super();
    }
}
```

---

## ❓ Question 15
**Can `this()` and `super()` be used together?**

No.

Both must be the first statement of a constructor.

The following is illegal.

```java
Employee(){

    this();

    super();
}
```

Compilation error occurs.

---

## ❓ Question 16
**What is a Copy Constructor in Java?**

Java does not provide a built-in copy constructor.

Developers create one manually.

```java
class Employee{

    int id;

    Employee(Employee e){

        this.id=e.id;
    }
}
```

Used for creating copies of existing objects.

---

## ❓ Question 17
**Why is a Private Constructor used?**

A private constructor prevents object creation from outside the class.

Common uses:

- Singleton Pattern
- Utility Classes
- Factory Classes

Example:

```java
class Utility{

    private Utility(){

    }
}
```

---

## ❓ Question 18
**What is the Singleton Pattern?**

Singleton ensures only one object exists throughout the application.

Example:

```java
class Singleton{

    private static Singleton obj=new Singleton();

    private Singleton(){

    }

    static Singleton getInstance(){

        return obj;
    }
}
```

---

## ❓ Question 19
**What is the Constructor Execution Order?**

Execution order is:

```
Static Block

↓

Parent Static Block

↓

Child Static Block

↓

Parent Instance Block

↓

Parent Constructor

↓

Child Instance Block

↓

Child Constructor
```

For a single class:

```
Static Block

↓

Instance Block

↓

Constructor
```

---

## ❓ Question 20
**What are the most common mistakes with Constructors?**

Common mistakes include:

- Giving a return type.

```java
void Student(){

}
```

- Forgetting to create a default constructor.

- Calling `this()` after another statement.

```java
Student(){

    System.out.println();

    this(10);   // Error
}
```

- Using both `this()` and `super()` together.

- Assuming constructors are inherited.

- Assuming constructors can be overridden.

---

# ⭐ Company-Level Interview Questions

### Amazon

**Q:** Why can't constructors be overridden?

**Answer:** Constructors are not inherited. Since overriding requires inheritance, constructors cannot be overridden.

---

### Oracle

**Q:** Explain constructor chaining with an execution flow.

**Answer:** Constructor chaining is calling another constructor using `this()` or `super()`. The called constructor executes first, then control returns to the caller.

---

### Infosys

**Q:** When does Java generate a default constructor?

**Answer:** Only when no constructor is declared in the class.

---

### TCS

**Q:** What is the difference between `this()` and `super()`?

**Answer:**

- `this()` calls another constructor in the same class.
- `super()` calls the parent class constructor.

---

### Accenture

**Q:** Why should `this()` be the first statement?

**Answer:** The object must complete constructor chaining before executing remaining initialization code.

---

### Wipro

**Q:** Can constructors be final?

**Answer:** No. Constructors cannot be inherited, so `final` has no meaning.

---

### Cognizant

**Q:** Can constructors be static?

**Answer:** No. Constructors belong to object creation, not the class.

---

### Capgemini

**Q:** Can constructors throw exceptions?

**Answer:** Yes. Constructors can declare checked or unchecked exceptions using `throws`.

---

### Deloitte

**Q:** What is the use of a private constructor?

**Answer:** To restrict object creation, commonly used in Singleton and Utility classes.

---

### IBM

**Q:** Which constructor executes first in inheritance?

**Answer:** The parent constructor always executes before the child constructor.
