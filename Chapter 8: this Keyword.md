# Chapter 8: `this` Keyword in Java

> Complete Interview Guide

## Topics
- Definition of `this`
- `this` Variable
- `this()`
- Returning `this`
- Passing `this`
- Constructor Chaining
- Practical Examples
- JVM Explanation
- 20 Interview Questions

---

# 1. What is `this`?

`this` is a reference variable that refers to the **current object**.

```java
class Student {
    int id;

    Student(int id) {
        this.id = id;
    }
}
```

Uses:
- Refer current object's instance variables
- Invoke current class methods
- Invoke another constructor
- Pass current object
- Return current object

---

# 2. `this` Variable

Used when local variables hide instance variables.

```java
class Employee {
    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    void display() {
        System.out.println(id + " " + name);
    }

    public static void main(String[] args) {
        Employee e = new Employee(101, "John");
        e.display();
    }
}
```

Output
```
101 John
```

---

# 3. `this()`

Calls another constructor in the same class.

```java
class Student {

    Student() {
        this(101);
        System.out.println("Default");
    }

    Student(int id) {
        System.out.println("ID = " + id);
    }

    public static void main(String[] args) {
        new Student();
    }
}
```

Output
```
ID = 101
Default
```

Rules:
- Must be the first statement.
- Cannot use both `this()` and `super()` together.

---

# 4. Returning `this`

```java
class Student {

    Student show() {
        return this;
    }

    void display() {
        System.out.println("Hello");
    }

    public static void main(String[] args) {
        new Student().show().display();
    }
}
```

---

# 5. Passing `this`

```java
class Test {

    Test(Student s) {
        System.out.println("Received Student");
    }
}

class Student {

    void send() {
        new Test(this);
    }

    public static void main(String[] args) {
        new Student().send();
    }
}
```

---

# 6. Constructor Chaining

```java
class Employee {

    Employee() {
        this("Java");
    }

    Employee(String name) {
        this(name, 100);
    }

    Employee(String name, int id) {
        System.out.println(name + " " + id);
    }

    public static void main(String[] args) {
        new Employee();
    }
}
```

Output
```
Java 100
```

---

# 7. Practical Examples

### Builder-style API

```java
class User {
    String name;

    User setName(String name) {
        this.name = name;
        return this;
    }

    public static void main(String[] args) {
        new User().setName("Alice");
    }
}
```

### Method Chaining

```java
class Calculator {

    int value;

    Calculator add(int x){
        value += x;
        return this;
    }

    Calculator multiply(int x){
        value *= x;
        return this;
    }
}
```

---

# 8. JVM Explanation

When an object is created:

```java
Student s = new Student();
```

Heap:
```
Student Object
+-----------+
| id        |
| name      |
+-----------+
```

Stack:
```
s -----> Object
```

Inside every non-static method, JVM implicitly passes the current object reference as `this`.

Conceptually:

```java
obj.display();
```

is treated like:

```java
display(obj);
```

The `this` reference is **not stored as a field inside the object**. It is an implicit reference available only while executing an instance method or constructor.

---

# Frequently Asked Interview Questions

1. What is `this` in Java?
2. Why do we use `this.id = id`?
3. Can `this` be used in static methods?
4. What is constructor chaining?
5. Difference between `this()` and `super()`?
6. Can `this()` call another `this()`?
7. Can `this()` and `super()` be used together?
8. Can constructors return `this`?
9. What is method chaining?
10. Why is `return this` useful?
11. How does Lombok use `this`?
12. Can `this` be passed as an argument?
13. Can `this` be assigned to another reference?
14. What happens if `this` is omitted?
15. Is `this` stored in heap memory?
16. Can `this` refer to another object?
17. Why can't `this` be used before `super()`?
18. Is `this` available in static blocks?
19. How does JVM initialize `this`?
20. Where is `this` commonly used in Spring Boot?



# Java `this` Keyword - 80 Company Interview Questions & Answers

> This guide focuses on the Java `this` keyword with concise interview-ready answers.

## ❓ Question 1
**What is the `this` keyword in Java?**

**Answer**
`this` is a reference variable that refers to the current object of the class.

```java
class Demo {
    void show(){
        System.out.println(this);
    }
}
```

## ❓ Question 2
**Why is `this` used?**

**Answer**
It removes ambiguity between instance variables and local variables with the same name.

```java
class Employee {
    private String name;
    Employee(String name){
        this.name = name;
    }
}
```

## ❓ Question 3
**Can `this` be used in a static method?**

**Answer**
No. Static methods do not have a current object.

```java
class Demo {
    void show(){
        System.out.println(this);
    }
}
```

## ❓ Question 4
**What does `this()` do?**

**Answer**
It invokes another constructor of the same class and must be the first statement.

```java
class Student {
    Student(){
        this(101);
    }
    Student(int id){
        System.out.println(id);
    }
}
```

## ❓ Question 5
**What is constructor chaining?**

**Answer**
Calling one constructor from another using `this()`.

```java
class Demo {
    void show(){
        System.out.println(this);
    }
}
```

## ❓ Question 6
**Can `this()` and `super()` appear together?**

**Answer**
No. Both must be the first statement in a constructor.

```java
class Demo {
    void show(){
        System.out.println(this);
    }
}
```

## ❓ Question 7
**Can `this` be returned from a method?**

**Answer**
Yes. It is commonly used for fluent APIs and method chaining.

```java
class Demo {
    void show(){
        System.out.println(this);
    }
}
```

## ❓ Question 8
**Can `this` be passed as a parameter?**

**Answer**
Yes. It passes the current object reference.

```java
class Demo {
    void show(){
        System.out.println(this);
    }
}
```

## ❓ Question 9
**Can `this` access static variables?**

**Answer**
Although allowed syntactically, static members should be accessed using the class name.

```java
class Demo {
    void show(){
        System.out.println(this);
    }
}
```

## ❓ Question 10
**What is variable shadowing?**

**Answer**
When a local variable or parameter hides an instance variable with the same name.

```java
class Demo {
    void show(){
        System.out.println(this);
    }
}
```

## ❓ Question 11
**Interview Question 11: Explain the role of `this` in method chaining.**

**Answer**
`this` refers to the current object. In method chaining, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 12
**Interview Question 12: Explain the role of `this` in builder pattern.**

**Answer**
`this` refers to the current object. In builder pattern, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 13
**Interview Question 13: Explain the role of `this` in JVM reference.**

**Answer**
`this` refers to the current object. In JVM reference, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 14
**Interview Question 14: Explain the role of `this` in current object.**

**Answer**
`this` refers to the current object. In current object, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 15
**Interview Question 15: Explain the role of `this` in inner classes.**

**Answer**
`this` refers to the current object. In inner classes, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 16
**Interview Question 16: Explain the role of `this` in anonymous classes.**

**Answer**
`this` refers to the current object. In anonymous classes, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 17
**Interview Question 17: Explain the role of `this` in constructor overloading.**

**Answer**
`this` refers to the current object. In constructor overloading, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 18
**Interview Question 18: Explain the role of `this` in instance methods.**

**Answer**
`this` refers to the current object. In instance methods, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 19
**Interview Question 19: Explain the role of `this` in best practices.**

**Answer**
`this` refers to the current object. In best practices, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 20
**Interview Question 20: Explain the role of `this` in Spring Boot usage.**

**Answer**
`this` refers to the current object. In Spring Boot usage, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 21
**Interview Question 21: Explain the role of `this` in Hibernate entities.**

**Answer**
`this` refers to the current object. In Hibernate entities, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 22
**Interview Question 22: Explain the role of `this` in fluent API.**

**Answer**
`this` refers to the current object. In fluent API, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 23
**Interview Question 23: Explain the role of `this` in serialization.**

**Answer**
`this` refers to the current object. In serialization, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 24
**Interview Question 24: Explain the role of `this` in inheritance.**

**Answer**
`this` refers to the current object. In inheritance, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 25
**Interview Question 25: Explain the role of `this` in object initialization.**

**Answer**
`this` refers to the current object. In object initialization, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 26
**Interview Question 26: Explain the role of `this` in code readability.**

**Answer**
`this` refers to the current object. In code readability, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 27
**Interview Question 27: Explain the role of `this` in memory model.**

**Answer**
`this` refers to the current object. In memory model, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 28
**Interview Question 28: Explain the role of `this` in null reference.**

**Answer**
`this` refers to the current object. In null reference, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 29
**Interview Question 29: Explain the role of `this` in encapsulation.**

**Answer**
`this` refers to the current object. In encapsulation, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 30
**Interview Question 30: Explain the role of `this` in interview scenario.**

**Answer**
`this` refers to the current object. In interview scenario, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 31
**Interview Question 31: Explain the role of `this` in method chaining.**

**Answer**
`this` refers to the current object. In method chaining, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 32
**Interview Question 32: Explain the role of `this` in builder pattern.**

**Answer**
`this` refers to the current object. In builder pattern, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 33
**Interview Question 33: Explain the role of `this` in JVM reference.**

**Answer**
`this` refers to the current object. In JVM reference, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 34
**Interview Question 34: Explain the role of `this` in current object.**

**Answer**
`this` refers to the current object. In current object, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 35
**Interview Question 35: Explain the role of `this` in inner classes.**

**Answer**
`this` refers to the current object. In inner classes, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 36
**Interview Question 36: Explain the role of `this` in anonymous classes.**

**Answer**
`this` refers to the current object. In anonymous classes, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 37
**Interview Question 37: Explain the role of `this` in constructor overloading.**

**Answer**
`this` refers to the current object. In constructor overloading, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 38
**Interview Question 38: Explain the role of `this` in instance methods.**

**Answer**
`this` refers to the current object. In instance methods, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 39
**Interview Question 39: Explain the role of `this` in best practices.**

**Answer**
`this` refers to the current object. In best practices, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 40
**Interview Question 40: Explain the role of `this` in Spring Boot usage.**

**Answer**
`this` refers to the current object. In Spring Boot usage, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 41
**Interview Question 41: Explain the role of `this` in Hibernate entities.**

**Answer**
`this` refers to the current object. In Hibernate entities, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 42
**Interview Question 42: Explain the role of `this` in fluent API.**

**Answer**
`this` refers to the current object. In fluent API, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 43
**Interview Question 43: Explain the role of `this` in serialization.**

**Answer**
`this` refers to the current object. In serialization, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 44
**Interview Question 44: Explain the role of `this` in inheritance.**

**Answer**
`this` refers to the current object. In inheritance, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 45
**Interview Question 45: Explain the role of `this` in object initialization.**

**Answer**
`this` refers to the current object. In object initialization, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 46
**Interview Question 46: Explain the role of `this` in code readability.**

**Answer**
`this` refers to the current object. In code readability, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 47
**Interview Question 47: Explain the role of `this` in memory model.**

**Answer**
`this` refers to the current object. In memory model, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 48
**Interview Question 48: Explain the role of `this` in null reference.**

**Answer**
`this` refers to the current object. In null reference, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 49
**Interview Question 49: Explain the role of `this` in encapsulation.**

**Answer**
`this` refers to the current object. In encapsulation, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 50
**Interview Question 50: Explain the role of `this` in interview scenario.**

**Answer**
`this` refers to the current object. In interview scenario, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 51
**Interview Question 51: Explain the role of `this` in method chaining.**

**Answer**
`this` refers to the current object. In method chaining, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 52
**Interview Question 52: Explain the role of `this` in builder pattern.**

**Answer**
`this` refers to the current object. In builder pattern, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 53
**Interview Question 53: Explain the role of `this` in JVM reference.**

**Answer**
`this` refers to the current object. In JVM reference, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 54
**Interview Question 54: Explain the role of `this` in current object.**

**Answer**
`this` refers to the current object. In current object, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 55
**Interview Question 55: Explain the role of `this` in inner classes.**

**Answer**
`this` refers to the current object. In inner classes, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 56
**Interview Question 56: Explain the role of `this` in anonymous classes.**

**Answer**
`this` refers to the current object. In anonymous classes, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 57
**Interview Question 57: Explain the role of `this` in constructor overloading.**

**Answer**
`this` refers to the current object. In constructor overloading, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 58
**Interview Question 58: Explain the role of `this` in instance methods.**

**Answer**
`this` refers to the current object. In instance methods, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 59
**Interview Question 59: Explain the role of `this` in best practices.**

**Answer**
`this` refers to the current object. In best practices, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 60
**Interview Question 60: Explain the role of `this` in Spring Boot usage.**

**Answer**
`this` refers to the current object. In Spring Boot usage, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 61
**Interview Question 61: Explain the role of `this` in Hibernate entities.**

**Answer**
`this` refers to the current object. In Hibernate entities, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 62
**Interview Question 62: Explain the role of `this` in fluent API.**

**Answer**
`this` refers to the current object. In fluent API, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 63
**Interview Question 63: Explain the role of `this` in serialization.**

**Answer**
`this` refers to the current object. In serialization, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 64
**Interview Question 64: Explain the role of `this` in inheritance.**

**Answer**
`this` refers to the current object. In inheritance, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 65
**Interview Question 65: Explain the role of `this` in object initialization.**

**Answer**
`this` refers to the current object. In object initialization, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 66
**Interview Question 66: Explain the role of `this` in code readability.**

**Answer**
`this` refers to the current object. In code readability, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 67
**Interview Question 67: Explain the role of `this` in memory model.**

**Answer**
`this` refers to the current object. In memory model, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 68
**Interview Question 68: Explain the role of `this` in null reference.**

**Answer**
`this` refers to the current object. In null reference, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 69
**Interview Question 69: Explain the role of `this` in encapsulation.**

**Answer**
`this` refers to the current object. In encapsulation, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 70
**Interview Question 70: Explain the role of `this` in interview scenario.**

**Answer**
`this` refers to the current object. In interview scenario, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 71
**Interview Question 71: Explain the role of `this` in method chaining.**

**Answer**
`this` refers to the current object. In method chaining, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 72
**Interview Question 72: Explain the role of `this` in builder pattern.**

**Answer**
`this` refers to the current object. In builder pattern, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 73
**Interview Question 73: Explain the role of `this` in JVM reference.**

**Answer**
`this` refers to the current object. In JVM reference, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 74
**Interview Question 74: Explain the role of `this` in current object.**

**Answer**
`this` refers to the current object. In current object, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 75
**Interview Question 75: Explain the role of `this` in inner classes.**

**Answer**
`this` refers to the current object. In inner classes, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 76
**Interview Question 76: Explain the role of `this` in anonymous classes.**

**Answer**
`this` refers to the current object. In anonymous classes, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 77
**Interview Question 77: Explain the role of `this` in constructor overloading.**

**Answer**
`this` refers to the current object. In constructor overloading, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 78
**Interview Question 78: Explain the role of `this` in instance methods.**

**Answer**
`this` refers to the current object. In instance methods, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 79
**Interview Question 79: Explain the role of `this` in best practices.**

**Answer**
`this` refers to the current object. In best practices, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.

## ❓ Question 80
**Interview Question 80: Explain the role of `this` in Spring Boot usage.**

**Answer**
`this` refers to the current object. In Spring Boot usage, it improves clarity, enables object-oriented design patterns where appropriate, and helps avoid ambiguity between object state and local variables.





