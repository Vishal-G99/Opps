# SOLID Principles in Java — Complete Explanation with Examples

**SOLID** is a collection of **5 object-oriented design principles** that help you write Java code that is:

* Easy to maintain
* Easy to extend
* Easy to test
* Less tightly coupled
* More flexible and scalable

SOLID is especially important when designing **classes, services, modules, and large Spring Boot applications**.

## SOLID = 5 Principles

| Letter | Principle                       | Main Idea                                                |
| ------ | ------------------------------- | -------------------------------------------------------- |
| **S**  | Single Responsibility Principle | One class should have one responsibility                 |
| **O**  | Open/Closed Principle           | Open for extension, closed for modification              |
| **L**  | Liskov Substitution Principle   | Child classes should properly replace parent classes     |
| **I**  | Interface Segregation Principle | Don't force classes to implement methods they don't need |
| **D**  | Dependency Inversion Principle  | Depend on abstractions, not concrete classes             |

---

# 1. S — Single Responsibility Principle (SRP)

## Definition

> **A class should have only one reason to change.**

This does **not necessarily mean a class can contain only one method**.

It means all methods in a class should belong to **one responsibility**.

### ❌ Bad Example

```java
public class UserService {

    public void saveUser(User user) {
        // Save user into database
    }

    public void sendEmail(User user) {
        // Send email
    }

    public void generateReport(User user) {
        // Generate PDF report
    }
}
```

This class has **3 responsibilities**:

1. User database operations
2. Email sending
3. Report generation

If email logic changes, `UserService` changes.

If report logic changes, `UserService` changes.

This violates SRP.

---

### ✅ Better Design

```java
public class UserService {

    public void saveUser(User user) {
        // Save user
    }
}
```

```java
public class EmailService {

    public void sendEmail(User user) {
        // Send email
    }
}
```

```java
public class ReportService {

    public void generateReport(User user) {
        // Generate report
    }
}
```

Now every class has a clear responsibility.

### Real Spring Boot Example

```java
@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User save(User user) {
        return userRepository.save(user);
    }
}
```

```java
@Service
public class EmailService {

    public void sendWelcomeEmail(String email) {
        // Email logic
    }
}
```

```java
@RestController
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```

A common separation is:

```text
Controller  → HTTP Request/Response
Service     → Business Logic
Repository  → Database Operations
```

This is strongly related to **SRP**.

---

# 2. O — Open/Closed Principle (OCP)

## Definition

> **Software entities should be open for extension but closed for modification.**

Meaning:

You should be able to add new functionality **without modifying existing working code too much**.

## ❌ Bad Example

Suppose you have different payment methods.

```java
public class PaymentService {

    public void pay(String type) {

        if (type.equals("CREDIT_CARD")) {
            System.out.println("Credit Card Payment");
        } 
        else if (type.equals("UPI")) {
            System.out.println("UPI Payment");
        }
    }
}
```

Now you want to add:

```text
PayPal
Net Banking
Crypto
Wallet
```

Every time, you modify:

```java
PaymentService
```

This can create bugs in existing code.

---

## ✅ Better Example Using Interface

```java
public interface Payment {

    void pay();
}
```

### Credit Card

```java
public class CreditCardPayment implements Payment {

    @Override
    public void pay() {
        System.out.println("Payment using Credit Card");
    }
}
```

### UPI

```java
public class UpiPayment implements Payment {

    @Override
    public void pay() {
        System.out.println("Payment using UPI");
    }
}
```

Now later, add PayPal:

```java
public class PayPalPayment implements Payment {

    @Override
    public void pay() {
        System.out.println("Payment using PayPal");
    }
}
```

You don't need to change the existing `Payment` implementations.

Usage:

```java
Payment payment = new UpiPayment();

payment.pay();
```

### Concept

```text
Existing code
     ↓
[ Payment Interface ]
     ↑
     │
 ┌───┴─────────────┐
 │                 │
UPI          Credit Card
 │
PayPal can be added later
```

This is **Open for Extension, Closed for Modification**.

---

# 3. L — Liskov Substitution Principle (LSP)

## Definition

> A child class should be able to replace its parent class without breaking the application.

If:

```java
B extends A
```

Then we should be able to use:

```java
A object = new B();
```

without unexpected behavior.

## ❌ Classic Bad Example

```java
class Bird {

    public void fly() {
        System.out.println("Flying");
    }
}
```

Now:

```java
class Sparrow extends Bird {
}
```

Works fine:

```java
Bird bird = new Sparrow();
bird.fly();
```

But what about:

```java
class Penguin extends Bird {

    @Override
    public void fly() {
        throw new UnsupportedOperationException(
            "Penguin cannot fly"
        );
    }
}
```

Now:

```java
Bird bird = new Penguin();

bird.fly();
```

💥 Exception!

Although `Penguin` is technically a bird, it cannot correctly behave like the parent `Bird` class expects.

This violates LSP.

---

## ✅ Better Design

Separate the abilities.

```java
interface Bird {
    void eat();
}
```

```java
interface FlyingBird extends Bird {
    void fly();
}
```

### Sparrow

```java
class Sparrow implements FlyingBird {

    @Override
    public void eat() {
        System.out.println("Sparrow eating");
    }

    @Override
    public void fly() {
        System.out.println("Sparrow flying");
    }
}
```

### Penguin

```java
class Penguin implements Bird {

    @Override
    public void eat() {
        System.out.println("Penguin eating");
    }
}
```

Now the hierarchy correctly represents behavior.

### Main Point of LSP

When you use:

```java
Parent parent = new Child();
```

The child should not:

* Break expected behavior
* Throw unexpected exceptions
* Remove functionality required by the parent contract
* Return completely unexpected values

---

# 4. I — Interface Segregation Principle (ISP)

## Definition

> A class should not be forced to implement methods it does not need.

## ❌ Bad Example

```java
public interface Worker {

    void work();

    void eat();

    void sleep();
}
```

A human can implement everything:

```java
public class HumanWorker implements Worker {

    public void work() {
        System.out.println("Human working");
    }

    public void eat() {
        System.out.println("Human eating");
    }

    public void sleep() {
        System.out.println("Human sleeping");
    }
}
```

But what about a robot?

```java
public class RobotWorker implements Worker {

    public void work() {
        System.out.println("Robot working");
    }

    public void eat() {
        throw new UnsupportedOperationException();
    }

    public void sleep() {
        throw new UnsupportedOperationException();
    }
}
```

The robot is being forced to implement unnecessary methods.

This violates ISP.

---

## ✅ Better Design

Split the interface.

```java
public interface Workable {

    void work();
}
```

```java
public interface Eatable {

    void eat();
}
```

```java
public interface Sleepable {

    void sleep();
}
```

Now:

### Human

```java
public class HumanWorker
        implements Workable, Eatable, Sleepable {

    public void work() {
        System.out.println("Human working");
    }

    public void eat() {
        System.out.println("Human eating");
    }

    public void sleep() {
        System.out.println("Human sleeping");
    }
}
```

### Robot

```java
public class RobotWorker implements Workable {

    public void work() {
        System.out.println("Robot working");
    }
}
```

Perfect.

### ISP Rule

Prefer:

```text
Small, focused interfaces
```

instead of:

```text
One large interface with many unrelated methods
```

---

# 5. D — Dependency Inversion Principle (DIP)

This is one of the **most important SOLID principles**, especially in **Spring Boot**.

## Definition

> High-level modules should not depend directly on low-level modules. Both should depend on abstractions.

Also:

> Depend on interfaces or abstractions, not concrete implementations.

---

## ❌ Bad Example

```java
public class UserService {

    private MySQLDatabase database =
            new MySQLDatabase();

    public void saveUser() {
        database.save();
    }
}
```

Here:

```text
UserService
     ↓
MySQLDatabase
```

`UserService` is tightly coupled with `MySQLDatabase`.

What if tomorrow you want:

```text
PostgreSQL
Oracle
MongoDB
```

You must modify:

```java
UserService
```

---

## ✅ Better Design

Create an abstraction.

```java
public interface Database {

    void save();
}
```

### MySQL

```java
public class MySQLDatabase implements Database {

    @Override
    public void save() {
        System.out.println("Saving in MySQL");
    }
}
```

### PostgreSQL

```java
public class PostgreSQLDatabase implements Database {

    @Override
    public void save() {
        System.out.println("Saving in PostgreSQL");
    }
}
```

Now:

```java
public class UserService {

    private Database database;

    public UserService(Database database) {
        this.database = database;
    }

    public void saveUser() {
        database.save();
    }
}
```

Usage:

```java
Database database = new MySQLDatabase();

UserService userService =
        new UserService(database);

userService.saveUser();
```

Tomorrow:

```java
Database database = new PostgreSQLDatabase();
```

`UserService` does not need to change.

---

# DIP in Spring Boot

Spring Framework heavily uses this concept.

```java
@Service
public class PaymentService {

    private final PaymentGateway paymentGateway;

    public PaymentService(
            PaymentGateway paymentGateway) {

        this.paymentGateway = paymentGateway;
    }
}
```

Interface:

```java
public interface PaymentGateway {

    void processPayment();
}
```

Implementation:

```java
@Component
public class RazorpayGateway
        implements PaymentGateway {

    @Override
    public void processPayment() {
        System.out.println("Payment processed");
    }
}
```

Spring automatically injects the implementation:

```text
PaymentService
       ↓
PaymentGateway
       ↑
       │
RazorpayGateway
```

This is **Dependency Injection**, which is commonly used to implement **Dependency Inversion**.

---

# Important: DIP vs Dependency Injection

These two are related, but they are **not exactly the same**.

### Dependency Inversion Principle

A **design principle**:

```java
class Service {
    private Database database;
}
```

Where `Database` is an abstraction.

### Dependency Injection

A **technique** for providing dependencies:

```java
public Service(Database database) {
    this.database = database;
}
```

In Spring:

```java
@Autowired
```

or preferably constructor injection:

```java
public Service(Database database) {
    this.database = database;
}
```

So:

```text
DIP = Design Principle
DI  = Implementation Technique
```

---

# Complete SOLID Example

Let's create an order payment system.

## Step 1: SRP

Separate responsibilities.

```java
public class OrderService {

    public void createOrder() {
        System.out.println("Order Created");
    }
}
```

```java
public class EmailService {

    public void sendEmail() {
        System.out.println("Email Sent");
    }
}
```

```java
public class InvoiceService {

    public void generateInvoice() {
        System.out.println("Invoice Generated");
    }
}
```

Each class has a separate responsibility.

---

## Step 2: OCP + DIP

Create an abstraction.

```java
public interface PaymentProcessor {

    void processPayment(double amount);
}
```

### UPI

```java
public class UpiPaymentProcessor
        implements PaymentProcessor {

    @Override
    public void processPayment(double amount) {
        System.out.println(
            "UPI Payment: " + amount
        );
    }
}
```

### Credit Card

```java
public class CreditCardPaymentProcessor
        implements PaymentProcessor {

    @Override
    public void processPayment(double amount) {
        System.out.println(
            "Credit Card Payment: " + amount
        );
    }
}
```

Now the service:

```java
public class PaymentService {

    private final PaymentProcessor paymentProcessor;

    public PaymentService(
            PaymentProcessor paymentProcessor) {

        this.paymentProcessor = paymentProcessor;
    }

    public void pay(double amount) {
        paymentProcessor.processPayment(amount);
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        PaymentProcessor processor =
                new UpiPaymentProcessor();

        PaymentService paymentService =
                new PaymentService(processor);

        paymentService.pay(5000);
    }
}
```

Output:

```text
UPI Payment: 5000.0
```

Later, add:

```java
public class WalletPaymentProcessor
        implements PaymentProcessor {

    @Override
    public void processPayment(double amount) {
        System.out.println(
            "Wallet Payment: " + amount
        );
    }
}
```

No need to modify `PaymentService`.

This demonstrates:

* **OCP** → Add new payment types without changing service
* **DIP** → Service depends on interface
* **SRP** → Different services handle different responsibilities

---

# Easy Way to Remember SOLID

## S — Single Responsibility

```text
One class → One responsibility
```

Example:

```text
UserService → User logic
EmailService → Email logic
```

---

## O — Open/Closed

```text
Add new functionality
without changing old working code
```

Example:

```text
Payment interface
   ├── UPI
   ├── Credit Card
   └── PayPal
```

---

## L — Liskov Substitution

```text
Child should safely replace Parent
```

Example:

```java
Animal animal = new Dog();
```

The `Dog` should behave correctly wherever an `Animal` is expected.

---

## I — Interface Segregation

```text
Don't create fat interfaces.
```

Instead of:

```java
interface Worker {
    work();
    eat();
    sleep();
}
```

Use:

```java
interface Workable
interface Eatable
interface Sleepable
```

---

## D — Dependency Inversion

```text
Depend on Interface
NOT Concrete Class
```

Prefer:

```java
private PaymentProcessor paymentProcessor;
```

Instead of:

```java
private UpiPaymentProcessor paymentProcessor;
```

---

# SOLID in a Spring Boot Project

A typical structure might look like:

```text
Controller
    ↓
Service Interface
    ↓
Service Implementation
    ↓
Repository
    ↓
Database
```

Example:

```java
@RestController
public class UserController {

    private final UserService userService;
}
```

```java
public interface UserService {

    User getUser(Long id);
}
```

```java
@Service
public class UserServiceImpl
        implements UserService {

    private final UserRepository userRepository;

    public UserServiceImpl(
            UserRepository userRepository) {

        this.userRepository = userRepository;
    }

    @Override
    public User getUser(Long id) {
        return userRepository.findById(id)
                .orElseThrow();
    }
}
```

```java
public interface UserRepository
        extends JpaRepository<User, Long> {
}
```

Here, you can see several SOLID concepts working together.

---

## Final Interview Summary

> **SOLID is a set of five object-oriented design principles used to create maintainable, flexible, scalable, and loosely coupled software.**

```text
S → Single Responsibility Principle
O → Open/Closed Principle
L → Liskov Substitution Principle
I → Interface Segregation Principle
D → Dependency Inversion Principle
```

### One-line interview explanation

**SRP:** One class should have one responsibility.

**OCP:** Extend behavior without modifying existing code.

**LSP:** Subclasses should correctly replace their parent classes.

**ISP:** Don't force a class to implement methods it doesn't need.

**DIP:** Depend on abstractions rather than concrete implementations.

For Java interviews, **DIP, OCP, and the difference between DIP and Dependency Injection** are particularly common topics.
