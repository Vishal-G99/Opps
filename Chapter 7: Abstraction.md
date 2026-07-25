# Chapter 7: Abstraction in Java

> Complete Interview Notes (Markdown)

## Table of Contents
1. Definition
2. Abstract Classes
3. Abstract Methods
4. Interface
5. Functional Interface
6. Marker Interface
7. Default Methods
8. Static Methods in Interfaces
9. Private Methods in Interfaces
10. Multiple Inheritance using Interfaces
11. Spring Boot Interface Examples

---

# 1. Definition

## ❓ What is Abstraction?

**Answer**

Abstraction is an Object-Oriented Programming (OOP) principle that hides implementation details and exposes only essential functionality.

### Benefits
- Hides complexity
- Improves maintainability
- Promotes loose coupling
- Supports polymorphism
- Easier testing and extension

### Example

```java
abstract class Vehicle {
    abstract void start();

    void stop() {
        System.out.println("Vehicle stopped");
    }
}
```

---

# 2. Abstract Classes

## ❓ What is an Abstract Class?

An abstract class is declared using the `abstract` keyword. It cannot be instantiated and may contain both abstract and concrete methods.

### Features
- Can have constructors
- Can have fields
- Can have static/final methods
- Can have abstract and non-abstract methods

```java
abstract class Animal {
    abstract void sound();

    void sleep() {
        System.out.println("Sleeping...");
    }
}
```

---

# 3. Abstract Methods

## ❓ What is an Abstract Method?

An abstract method has no implementation and must be implemented by the first concrete subclass.

```java
abstract class Shape {
    abstract double area();
}
```

---

# 4. Interface

## ❓ What is an Interface?

An interface defines a contract that implementing classes must follow.

```java
interface PaymentService {
    void pay();
}

class CardPayment implements PaymentService {
    public void pay() {
        System.out.println("Paid by card");
    }
}
```

### Advantages
- Loose coupling
- Multiple inheritance
- Better testing
- Dependency Injection support

---

# 5. Functional Interface

## ❓ What is a Functional Interface?

A functional interface contains exactly one abstract method.

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

Used with:
- Lambda expressions
- Method references
- Streams API

Common examples:
- Runnable
- Callable
- Comparator
- Predicate
- Function
- Consumer
- Supplier

---

# 6. Marker Interface

## ❓ What is a Marker Interface?

A marker interface has no methods or constants. It marks a class with special behavior.

Examples:
- Serializable
- Cloneable
- Remote

```java
class Student implements java.io.Serializable {}
```

---

# 7. Default Methods

## ❓ What are Default Methods?

Default methods (Java 8+) provide an implementation inside an interface.

```java
interface Printer {
    default void print() {
        System.out.println("Printing...");
    }
}
```

Purpose:
- Backward compatibility
- Shared behavior

---

# 8. Static Methods

## ❓ Can interfaces contain static methods?

Yes.

```java
interface MathUtil {
    static int square(int n) {
        return n * n;
    }
}
```

Usage:

```java
MathUtil.square(5);
```

---

# 9. Private Methods

## ❓ Can interfaces contain private methods?

Yes (Java 9+).

```java
interface Logger {

    private void log(String msg){
        System.out.println(msg);
    }

    default void info(){
        log("Information");
    }
}
```

Used to avoid duplicate code among default methods.

---

# 10. Multiple Inheritance using Interfaces

## ❓ How does Java support multiple inheritance?

Java allows a class to implement multiple interfaces.

```java
interface A {
    void show();
}

interface B {
    void print();
}

class Demo implements A, B {

    public void show() {}

    public void print() {}
}
```

### Default Method Conflict

```java
interface A{
    default void display(){}
}

interface B{
    default void display(){}
}

class C implements A,B{

    @Override
    public void display(){
        A.super.display();
    }
}
```

---

# 11. Spring Boot Interface Examples

## Service Layer

```java
public interface UserService {
    User save(User user);
}

@Service
class UserServiceImpl implements UserService {

    @Override
    public User save(User user){
        return user;
    }
}
```

## Dependency Injection

```java
@RestController
class UserController {

    private final UserService service;

    UserController(UserService service){
        this.service = service;
    }
}
```

Benefits:
- Loose coupling
- Easy testing with mocks
- Better scalability

---

# Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|--------|----------------|-----------|
| Constructor | Yes | No |
| Instance Variables | Yes | Constants only |
| Multiple Inheritance | No | Yes |
| Default Methods | N/A | Yes |
| Static Methods | Yes | Yes |
| Private Methods | Yes | Yes (Java 9+) |
| State | Yes | No mutable state |

---

# Frequently Asked Interview Questions

1. What is abstraction?
2. Why is abstraction important?
3. Abstract class vs interface.
4. Can an abstract class have constructors?
5. Can an abstract class have final methods?
6. Can interfaces have variables?
7. What is a functional interface?
8. What is a marker interface?
9. Why were default methods introduced?
10. Can interfaces have static methods?
11. Can interfaces have private methods?
12. How does Java support multiple inheritance?
13. What happens when two interfaces have the same default method?
14. Why are interfaces heavily used in Spring Boot?
15. When should you choose an abstract class over an interface?






# Chapter 7: Abstraction - 100 Company Interview Questions & 20 Practical Programs
## Company Coverage
- TCS, Infosys, Wipro, Cognizant, Capgemini, Accenture, IBM, Deloitte, Oracle, Amazon, Microsoft, Google

## ❓ Question 1
**What is abstraction in Java?**

**Answer:**
Abstraction is the OOP concept of hiding implementation details while exposing only essential behavior. It is achieved using abstract classes and interfaces.
## ❓ Question 2
**Difference between abstraction and encapsulation?**

**Answer:**
Abstraction hides implementation complexity; encapsulation hides data using access modifiers.
## ❓ Question 3
**What is an abstract class?**

**Answer:**
An abstract class cannot be instantiated and may contain both abstract and concrete methods.
## ❓ Question 4
**What is an interface?**

**Answer:**
An interface defines a contract that implementing classes must follow. Java interfaces support multiple inheritance.
## ❓ Question 5
**Abstract class vs Interface?**

**Answer:**
Use an abstract class for shared state/behavior. Use an interface for contracts, loose coupling, and multiple inheritance.
## ❓ Question 6
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 7
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 8
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 9
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 10
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 11
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 12
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 13
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 14
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 15
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 16
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 17
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 18
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 19
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 20
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 21
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 22
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 23
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 24
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 25
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 26
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 27
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 28
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 29
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 30
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 31
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 32
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 33
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 34
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 35
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 36
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 37
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 38
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 39
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 40
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 41
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 42
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 43
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 44
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 45
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 46
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 47
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 48
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 49
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 50
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 51
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 52
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 53
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 54
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 55
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 56
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 57
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 58
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 59
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 60
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 61
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 62
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 63
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 64
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 65
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 66
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 67
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 68
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 69
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 70
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 71
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 72
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 73
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 74
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 75
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 76
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 77
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 78
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 79
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 80
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 81
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 82
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 83
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 84
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 85
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 86
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 87
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 88
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 89
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 90
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.
## ❓ Question 91
**What is a functional interface?**

**Answer:**
A functional interface contains exactly one abstract method and is used with lambda expressions.
## ❓ Question 92
**What is a marker interface?**

**Answer:**
A marker interface contains no methods and marks a class for special JVM/framework behavior, e.g., Serializable.
## ❓ Question 93
**Why does Spring Boot prefer interfaces?**

**Answer:**
Interfaces enable loose coupling, easier testing, and dependency injection.
## ❓ Question 94
**How does dependency injection use interfaces?**

**Answer:**
Spring injects an interface while choosing an implementation at runtime, enabling runtime polymorphism and flexibility.
## ❓ Question 95
**How is multiple inheritance achieved in Java?**

**Answer:**
A class can implement multiple interfaces, avoiding the diamond problem of class inheritance.
## ❓ Question 96
**Can an abstract class have constructors?**

**Answer:**
Yes. Constructors initialize inherited state and are invoked when a subclass object is created.
## ❓ Question 97
**Can abstract classes contain final methods?**

**Answer:**
Yes. Final methods provide common behavior that subclasses cannot override.
## ❓ Question 98
**Can interfaces have default methods?**

**Answer:**
Yes. Default methods were introduced in Java 8 to add behavior without breaking existing implementations.
## ❓ Question 99
**Can interfaces have static methods?**

**Answer:**
Yes. Static methods belong to the interface and are called using the interface name.
## ❓ Question 100
**Can interfaces have private methods?**

**Answer:**
Yes. Private methods (Java 9+) reduce code duplication among default methods.

# 20 Practical Programs
### Program 1: Abstract Class Example
**Problem:** Create an abstract Shape class and implement Circle and Rectangle.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 2: Interface Example
**Problem:** Implement PaymentService using CardPayment and UpiPayment.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 3: Functional Interface
**Problem:** Create a Calculator functional interface and invoke it with a lambda.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 4: Marker Interface
**Problem:** Implement Serializable and serialize an object.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 5: Default Method
**Problem:** Create an interface with a default method and override it.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 6: Static Interface Method
**Problem:** Call a static utility method from an interface.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 7: Private Interface Method
**Problem:** Reuse logic between two default methods.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 8: Multiple Interfaces
**Problem:** Implement two interfaces in one class.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 9: Abstract Class + Constructor
**Problem:** Show constructor execution order.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 10: Spring Service Interface
**Problem:** Create UserService and UserServiceImpl.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 11: Spring Repository
**Problem:** Inject JpaRepository into a service.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 12: Dependency Injection
**Problem:** Inject interface implementation using constructor injection.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 13: Notification System
**Problem:** EmailNotifier and SmsNotifier implementations.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 14: Vehicle Hierarchy
**Problem:** Abstract Vehicle with Car and Bike.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 15: Employee Payroll
**Problem:** Abstract Employee with FullTime and Contract.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 16: Bank Account
**Problem:** Abstract Account with Savings and Current.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 17: Logger Interface
**Problem:** Different logger implementations.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 18: File Parser
**Problem:** CSVParser and JSONParser implementing Parser.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 19: Strategy Pattern
**Problem:** Payment strategy using interfaces.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```
### Program 20: Factory Pattern
**Problem:** Create objects through interface-based factory.
```java
// Implement the classes and demonstrate abstraction using abstract classes or interfaces.
```




# Chapter 7 - Abstraction: 20 Practical Programs (Complete Code)
## Program 1: Abstract Class Example

**Problem:** Create an abstract Shape class and implement Circle and Rectangle.

```java
abstract class Shape {
    abstract double area();
}
class Circle extends Shape {
    double r=5;
    double area(){ return Math.PI*r*r; }
}
class Rectangle extends Shape {
    double l=4,b=6;
    double area(){ return l*b; }
}
public class Main{
    public static void main(String[] args){
        Shape s1=new Circle();
        Shape s2=new Rectangle();
        System.out.println(s1.area());
        System.out.println(s2.area());
    }
}
```
---
## Program 2: Interface Example

**Problem:** Implement PaymentService using CardPayment and UpiPayment.

```java
interface PaymentService{ void pay(double amt); }
class CardPayment implements PaymentService{
 public void pay(double amt){System.out.println("Card: "+amt);}
}
class UpiPayment implements PaymentService{
 public void pay(double amt){System.out.println("UPI: "+amt);}
}
public class Main{
 public static void main(String[] args){
  PaymentService p=new CardPayment(); p.pay(1000);
  p=new UpiPayment(); p.pay(500);
 }
}
```
---
## Program 3: Functional Interface

**Problem:** Create a Calculator functional interface and invoke it with a lambda.

```java
@FunctionalInterface
interface Calculator{ int add(int a,int b); }
public class Main{
 public static void main(String[] args){
  Calculator c=(a,b)->a+b;
  System.out.println(c.add(10,20));
 }
}
```
---
## Program 4: Marker Interface

**Problem:** Implement Serializable and serialize an object.

```java
import java.io.*;
class Student implements Serializable{
 int id=1; String name="John";
}
public class Main{
 public static void main(String[] args)throws Exception{
  Student s=new Student();
  ObjectOutputStream o=new ObjectOutputStream(new FileOutputStream("student.ser"));
  o.writeObject(s); o.close();
 }
}
```
---
## Program 5: Default Method

**Problem:** Create an interface with a default method and override it.

```java
interface Printer{
 default void print(){ System.out.println("Default Print");}
}
class HPPrinter implements Printer{
 public void print(){ System.out.println("HP Print");}
}
public class Main{
 public static void main(String[] args){ new HPPrinter().print(); }
}
```
---
## Program 6: Static Interface Method

**Problem:** Call a static utility method from an interface.

```java
interface MathUtil{
 static int square(int n){ return n*n; }
}
public class Main{
 public static void main(String[] args){
  System.out.println(MathUtil.square(5));
 }
}
```
---
## Program 7: Private Interface Method

**Problem:** Reuse logic between two default methods.

```java
interface Logger{
 private void log(String m){ System.out.println(m); }
 default void info(){ log("INFO"); }
 default void error(){ log("ERROR"); }
}
class AppLogger implements Logger{}
public class Main{
 public static void main(String[] args){
  AppLogger l=new AppLogger(); l.info(); l.error();
 }
}
```
---
## Program 8: Multiple Interfaces

**Problem:** Implement two interfaces in one class.

```java
interface A{void show();}
interface B{void print();}
class Demo implements A,B{
 public void show(){System.out.println("Show");}
 public void print(){System.out.println("Print");}
}
public class Main{
 public static void main(String[] args){
  Demo d=new Demo(); d.show(); d.print();
 }
}
```
---
## Program 9: Abstract Class + Constructor

**Problem:** Show constructor execution order.

```java
abstract class Animal{
 Animal(){System.out.println("Animal Constructor");}
}
class Dog extends Animal{
 Dog(){System.out.println("Dog Constructor");}
}
public class Main{
 public static void main(String[] args){ new Dog(); }
}
```
---
## Program 10: Spring Service Interface

**Problem:** Create UserService and UserServiceImpl.

```java
public interface UserService{
 void save();
}
@Service
class UserServiceImpl implements UserService{
 public void save(){ System.out.println("Saved");}
}
```
---
## Program 11: Spring Repository

**Problem:** Inject JpaRepository into a service.

```java
public interface UserRepository extends JpaRepository<User,Long>{}
@Service
class UserService{
 @Autowired UserRepository repo;
}
```
---
## Program 12: Dependency Injection

**Problem:** Inject interface implementation using constructor injection.

```java
interface Engine{ void start(); }
@Component
class PetrolEngine implements Engine{
 public void start(){}
}
@Service
class CarService{
 private final Engine engine;
 CarService(Engine engine){this.engine=engine;}
}
```
---
## Program 13: Notification System

**Problem:** EmailNotifier and SmsNotifier implementations.

```java
interface Notifier{void send(String msg);}
class EmailNotifier implements Notifier{
 public void send(String msg){System.out.println("Email:"+msg);}
}
class SmsNotifier implements Notifier{
 public void send(String msg){System.out.println("SMS:"+msg);}
}
```
---
## Program 14: Vehicle Hierarchy

**Problem:** Abstract Vehicle with Car and Bike.

```java
abstract class Vehicle{
 abstract void drive();
}
class Car extends Vehicle{
 void drive(){System.out.println("Car");}
}
class Bike extends Vehicle{
 void drive(){System.out.println("Bike");}
}
```
---
## Program 15: Employee Payroll

**Problem:** Abstract Employee with FullTime and Contract.

```java
abstract class Employee{
 abstract double salary();
}
class FullTime extends Employee{
 double salary(){return 50000;}
}
class Contract extends Employee{
 double salary(){return 25000;}
}
```
---
## Program 16: Bank Account

**Problem:** Abstract Account with Savings and Current.

```java
abstract class Account{
 double bal=1000;
 abstract void withdraw(double a);
}
class Savings extends Account{
 void withdraw(double a){bal-=a;}
}
class Current extends Account{
 void withdraw(double a){bal-=a;}
}
```
---
## Program 17: Logger Interface

**Problem:** Different logger implementations.

```java
interface Logger{
 void log(String msg);
}
class ConsoleLogger implements Logger{
 public void log(String msg){System.out.println(msg);}
}
class FileLogger implements Logger{
 public void log(String msg){System.out.println("Write:"+msg);}
}
```
---
## Program 18: File Parser

**Problem:** CSVParser and JSONParser implementing Parser.

```java
interface Parser{void parse(String f);}
class CSVParser implements Parser{
 public void parse(String f){System.out.println("CSV");}
}
class JSONParser implements Parser{
 public void parse(String f){System.out.println("JSON");}
}
```
---
## Program 19: Strategy Pattern

**Problem:** Payment strategy using interfaces.

```java
interface PaymentStrategy{void pay(int amt);}
class CardStrategy implements PaymentStrategy{
 public void pay(int amt){System.out.println("Card");}
}
class UpiStrategy implements PaymentStrategy{
 public void pay(int amt){System.out.println("UPI");}
}
```
---
## Program 20: Factory Pattern

**Problem:** Create objects through interface-based factory.

```java
interface Animal{void sound();}
class Dog implements Animal{
 public void sound(){System.out.println("Bark");}
}
class AnimalFactory{
 static Animal getAnimal(){ return new Dog(); }
}
public class Main{
 public static void main(String[] args){
  Animal a=AnimalFactory.getAnimal();
  a.sound();
 }
}
```
---
