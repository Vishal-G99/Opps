# Inheritance

## ❓ Question

**What is Inheritance?**

Inheritance is one of the four fundamental pillars of Object-Oriented
Programming (OOP). It allows one class (child/subclass) to acquire the
properties and behaviors of another class (parent/superclass).

### Key Points

-   Promotes code reusability.
-   Represents an **IS-A** relationship.
-   Reduces duplicate code.
-   Supports method overriding.
-   Enables runtime polymorphism.

### Syntax

``` java
class Parent {
}

class Child extends Parent {
}
```

### Example

``` java
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat();
        dog.bark();
    }
}
```

**Output**

    Animal is eating
    Dog is barking

------------------------------------------------------------------------

# IS-A Relationship

## ❓ Question

**What is an IS-A Relationship?**

An IS-A relationship means one object is a specialized version of
another object.

Examples:

-   Dog IS-A Animal
-   Car IS-A Vehicle
-   Manager IS-A Employee

``` java
class Vehicle {
    void start() {
        System.out.println("Vehicle Started");
    }
}

class Car extends Vehicle {
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.start();
    }
}
```

------------------------------------------------------------------------

# Types of Inheritance

Java supports:

1.  Single Inheritance
2.  Multilevel Inheritance
3.  Hierarchical Inheritance

Java does **not** support Multiple and Hybrid inheritance using classes
because of the Diamond Problem. They are achievable using interfaces.

------------------------------------------------------------------------

# Single Inheritance

## ❓ Question

**What is Single Inheritance?**

A child class inherits from one parent class.

``` java
class Animal {
    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking...");
    }
}
```

------------------------------------------------------------------------

# Multilevel Inheritance

## ❓ Question

**What is Multilevel Inheritance?**

A class inherits from another derived class.

``` java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking");
    }
}

class Puppy extends Dog {
    void weep() {
        System.out.println("Weeping");
    }
}
```

------------------------------------------------------------------------

# Hierarchical Inheritance

## ❓ Question

**What is Hierarchical Inheritance?**

Multiple child classes inherit from one parent class.

``` java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
}

class Cat extends Animal {
}
```

------------------------------------------------------------------------

# Multiple Inheritance

## ❓ Question

**Does Java support Multiple Inheritance?**

Java does not support multiple inheritance using classes.

❌ Invalid

``` java
class A {
}

class B {
}

class C extends A, B {
}
```

Reason:

-   Diamond Problem
-   Method ambiguity
-   Complexity

Using interfaces:

``` java
interface A {
    void display();
}

interface B {
    void display();
}

class C implements A, B {
    public void display() {
        System.out.println("Implemented");
    }
}
```

------------------------------------------------------------------------

# Hybrid Inheritance

## ❓ Question

**What is Hybrid Inheritance?**

Hybrid inheritance combines more than one inheritance type.

Java supports hybrid inheritance through interfaces.

``` java
interface A {
}

interface B extends A {
}

interface C {
}

class D implements B, C {
}
```

------------------------------------------------------------------------

# Method Overriding

## ❓ Question

**What is Method Overriding?**

When a child class provides its own implementation of a parent method.

``` java
class Animal {
    void sound() {
        System.out.println("Animal Sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog Bark");
    }
}
```

Rules:

-   Same method name
-   Same parameters
-   Cannot reduce access modifier
-   Return type must be same or covariant

------------------------------------------------------------------------

# Dynamic Binding

## ❓ Question

**What is Dynamic Binding?**

The JVM decides which overridden method to call at runtime.

``` java
class Animal {
    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.sound();
    }
}
```

Output

    Dog

------------------------------------------------------------------------

# Constructor Calling

## ❓ Question

**How are constructors called in inheritance?**

When a child object is created:

1.  Parent constructor executes first.
2.  Child constructor executes second.

``` java
class Animal {
    Animal() {
        System.out.println("Animal Constructor");
    }
}

class Dog extends Animal {
    Dog() {
        System.out.println("Dog Constructor");
    }
}
```

Output

    Animal Constructor
    Dog Constructor

------------------------------------------------------------------------

# super Keyword

## ❓ Question

**What is the super keyword?**

Used to access parent members.

Uses:

-   Call parent constructor
-   Call parent method
-   Access parent variable

``` java
class Animal {
    String name = "Animal";

    void display() {
        System.out.println("Animal Display");
    }
}

class Dog extends Animal {

    String name = "Dog";

    Dog() {
        super();
    }

    void show() {
        System.out.println(super.name);
        super.display();
    }
}
```

------------------------------------------------------------------------

# Object Class

## ❓ Question

**What is the Object class?**

Every class in Java directly or indirectly extends `Object`.

Common methods:

-   toString()
-   equals()
-   hashCode()
-   getClass()
-   clone()
-   wait()
-   notify()
-   notifyAll()

``` java
class Student {
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student();
        System.out.println(s.toString());
        System.out.println(s.getClass());
    }
}
```

------------------------------------------------------------------------

# instanceof Operator

## ❓ Question

**What is instanceof?**

Checks whether an object belongs to a specific class.

``` java
Animal a = new Dog();

System.out.println(a instanceof Dog);
System.out.println(a instanceof Animal);
```

Output

    true
    true

------------------------------------------------------------------------

# Spring Boot Example

## ❓ Question

**How is inheritance used in Spring Boot?**

``` java
class BaseEntity {

    private Long id;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }
}

class User extends BaseEntity {

    private String username;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```

Benefits:

-   Shared audit fields
-   Common entity logic
-   Reduced duplication
-   Easier maintenance

------------------------------------------------------------------------

# Best Practices

-   Favor composition over inheritance when appropriate.
-   Override methods carefully.
-   Keep inheritance hierarchies shallow.
-   Use `@Override`.
-   Prefer interfaces for multiple behavior inheritance.


# Java Inheritance – 60 Company Interview Questions & Answers

> Covers beginner, intermediate, advanced, Spring Boot, JVM, and scenario-based questions.

---

## 1. What is inheritance in Java?

**Answer:**  
Inheritance is an OOP mechanism where one class acquires the fields and methods of another class using the `extends` keyword (or `implements` for interfaces). It promotes code reuse, extensibility, and polymorphism.

---

## 2. Why is inheritance used?

**Answer:**
- Code reuse
- Method overriding
- Runtime polymorphism
- Easier maintenance
- Better code organization

---

## 3. What is an IS-A relationship?

**Answer:**  
Inheritance represents an **IS-A** relationship.

Example:
```java
class Animal {}
class Dog extends Animal {}
```

Dog **IS-A** Animal.

---

## 4. Which keyword is used for inheritance?

**Answer:** `extends`

Example:
```java
class Child extends Parent {}
```

---

## 5. Can Java support multiple inheritance using classes?

**Answer:**  
No. Java does not support multiple inheritance with classes because of the Diamond Problem.

---

## 6. How does Java achieve multiple inheritance?

**Answer:**  
Using interfaces.

```java
interface A {}
interface B {}
class C implements A, B {}
```

---

## 7. What types of inheritance exist in Java?

**Answer:**
- Single
- Multilevel
- Hierarchical
- Multiple (through interfaces)
- Hybrid (using interfaces)

---

## 8. What is single inheritance?

**Answer:** One child inherits one parent.

---

## 9. What is multilevel inheritance?

**Answer:** A chain of inheritance.

```text
A
↓
B
↓
C
```

---

## 10. What is hierarchical inheritance?

**Answer:** Multiple child classes inherit the same parent.

---

## 11. What is hybrid inheritance?

**Answer:** Combination of inheritance types using interfaces.

---

## 12. Why doesn't Java support multiple inheritance with classes?

**Answer:** To avoid ambiguity caused by the Diamond Problem.

---

## 13. What is the Diamond Problem?

**Answer:** When two parent classes define the same method and a child inherits both, Java cannot determine which implementation to use.

---

## 14. What is method overriding?

**Answer:** Child class provides its own implementation of a parent method.

```java
@Override
public void display(){}
```

---

## 15. What is runtime polymorphism?

**Answer:** Overridden methods are resolved at runtime.

---

## 16. What is dynamic binding?

**Answer:** JVM decides which overridden method to invoke during runtime.

---

## 17. What is compile-time binding?

**Answer:** Method selection happens during compilation (static/final/private methods).

---

## 18. Can constructors be inherited?

**Answer:** No.

---

## 19. Why are constructors not inherited?

**Answer:** Constructors initialize objects and belong only to their own class.

---

## 20. Which constructor executes first?

**Answer:** Parent constructor executes before child constructor.

---

## 21. What is `super`?

**Answer:** Refers to the immediate parent object.

---

## 22. What is `super()`?

**Answer:** Calls the parent constructor.

---

## 23. Can `super()` and `this()` appear together?

**Answer:** No. Both must be the first statement.

---

## 24. What is `super.method()`?

**Answer:** Invokes the parent implementation.

---

## 25. Can private members be inherited?

**Answer:** No, they are inaccessible directly.

---

## 26. Are protected members inherited?

**Answer:** Yes.

---

## 27. Are public members inherited?

**Answer:** Yes.

---

## 28. Can static methods be overridden?

**Answer:** No. They are hidden, not overridden.

---

## 29. Can final methods be overridden?

**Answer:** No.

---

## 30. Can final classes be inherited?

**Answer:** No.

---

## 31. Can abstract classes be inherited?

**Answer:** Yes.

---

## 32. Must abstract methods be overridden?

**Answer:** Yes, unless the child is also abstract.

---

## 33. Difference between overriding and overloading?

**Answer:**
- Overloading → same class, different parameters.
- Overriding → parent-child, same signature.

---

## 34. What is upcasting?

**Answer:**
```java
Animal a = new Dog();
```

---

## 35. What is downcasting?

**Answer:**
```java
Dog d = (Dog)a;
```

---

## 36. Why use `instanceof` before downcasting?

**Answer:** Prevents `ClassCastException`.

---

## 37. What is Object class?

**Answer:** Root class of every Java class.

---

## 38. Which methods come from Object?

**Answer:**
- toString()
- equals()
- hashCode()
- clone()
- wait()
- notify()
- notifyAll()

---

## 39. Can Object reference hold child objects?

**Answer:** Yes.

---

## 40. What is covariant return type?

**Answer:** Child override can return a subclass type.

---

## 41. Can private methods be overridden?

**Answer:** No.

---

## 42. Can constructors be overridden?

**Answer:** No.

---

## 43. Can static variables be inherited?

**Answer:** Yes.

---

## 44. Is inheritance transitive?

**Answer:** Yes.

---

## 45. Can an interface extend another interface?

**Answer:** Yes.

---

## 46. Can a class extend an interface?

**Answer:** No. It implements interfaces.

---

## 47. Can an interface extend multiple interfaces?

**Answer:** Yes.

---

## 48. Which is better: inheritance or composition?

**Answer:** Prefer composition unless there is a true IS-A relationship.

---

## 49. What is composition?

**Answer:** HAS-A relationship.

---

## 50. Explain IS-A vs HAS-A.

**Answer:**
- IS-A → inheritance
- HAS-A → composition

---

## 51. What is constructor chaining with inheritance?

**Answer:** Parent constructor executes automatically before child.

---

## 52. What is the order of execution?

**Answer:**
Static Block
→ Parent Static
→ Child Static
→ Parent Instance
→ Parent Constructor
→ Child Instance
→ Child Constructor

---

## 53. Why use `@Override`?

**Answer:** Compile-time verification of overriding.

---

## 54. Can interfaces have default methods?

**Answer:** Yes (Java 8+).

---

## 55. How are default method conflicts resolved?

**Answer:** Child class must override.

---

## 56. How is inheritance used in Spring Boot?

**Answer:** Common base entity/service classes reduce duplication.

Example:
```java
class BaseEntity{
    Long id;
}

class User extends BaseEntity{}
```

---

## 57. Where is inheritance commonly used in enterprise projects?

**Answer:**
- BaseEntity
- BaseController
- BaseService
- Exception hierarchy
- DTO hierarchy

---

## 58. What are disadvantages of inheritance?

**Answer:**
- Tight coupling
- Fragile hierarchy
- Reduced flexibility

---

## 59. When should inheritance be avoided?

**Answer:** When relationship is HAS-A instead of IS-A.

---

## 60. Which inheritance questions are most frequently asked in companies?

**Answer:**
1. Why Java doesn't support multiple inheritance?
2. Diamond Problem
3. IS-A relationship
4. Overriding vs Overloading
5. Dynamic Binding
6. `super` keyword
7. Constructor calling order
8. `instanceof`
9. Object class
10. Inheritance vs Composition
