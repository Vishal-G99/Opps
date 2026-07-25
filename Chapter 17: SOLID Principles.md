# Chapter 17: SOLID Principles

---

## ❓ Question
**What is the Single Responsibility Principle (SRP)?**

A class should have **only one reason to change** — it should be responsible for exactly one piece of functionality.

```java
// VIOLATES SRP — this class handles both business logic AND persistence AND formatting
class Invoice {
    void calculateTotal() { /* ... */ }
    void saveToDatabase() { /* ... */ }
    void printAsPdf() { /* ... */ }
}
```

```java
// FOLLOWS SRP — each responsibility is a separate class
class Invoice {
    void calculateTotal() { /* ... */ }
}

class InvoiceRepository {
    void save(Invoice invoice) { /* ... */ }
}

class InvoicePdfExporter {
    void export(Invoice invoice) { /* ... */ }
}
```

---

## ❓ Question
**What is the Open-Closed Principle (OCP)?**

A class should be **open for extension, but closed for modification** — you should be able to add new behavior without changing existing, tested code.

```java
// VIOLATES OCP — adding a new shape requires modifying this method every time
class AreaCalculator {
    double calculate(Object shape) {
        if (shape instanceof Circle) { /* ... */ }
        else if (shape instanceof Square) { /* ... */ }
        // every new shape means editing this class again
        return 0;
    }
}
```

```java
// FOLLOWS OCP — new shapes extend the abstraction; no existing code changes
interface Shape {
    double area();
}

class Circle implements Shape {
    double radius;
    public double area() { return Math.PI * radius * radius; }
}

class Square implements Shape {
    double side;
    public double area() { return side * side; }
}

class AreaCalculator {
    double calculate(Shape shape) {
        return shape.area(); // works for ANY current or future Shape
    }
}
```

---

## ❓ Question
**What is the Liskov Substitution Principle (LSP)?**

Subclasses must be **substitutable for their base class** without breaking correctness — any code using the parent type should work correctly even if given a subclass instance.

```java
// VIOLATES LSP — Square breaks Rectangle's expected behavior
class Rectangle {
    protected int width, height;
    void setWidth(int w) { width = w; }
    void setHeight(int h) { height = h; }
    int getArea() { return width * height; }
}

class Square extends Rectangle {
    @Override
    void setWidth(int w) { width = height = w; } // unexpectedly changes height too!
    @Override
    void setHeight(int h) { width = height = h; }
}

// Code expecting Rectangle behavior breaks silently when given a Square
Rectangle r = new Square();
r.setWidth(5);
r.setHeight(10);
System.out.println(r.getArea()); // expected 50, actually 100 — LSP violated
```

**Fix:** don't force `Square` to inherit from `Rectangle` at all — they don't share true "is-a" behavioral compatibility; use a common `Shape` interface instead.

---

## ❓ Question
**What is the Interface Segregation Principle (ISP)?**

Clients should **not be forced to depend on methods they don't use** — prefer several small, specific interfaces over one large, general-purpose one.

```java
// VIOLATES ISP — forces ALL printers to implement scan/fax even if they can't
interface Machine {
    void print();
    void scan();
    void fax();
}

class OldPrinter implements Machine {
    public void print() { /* works */ }
    public void scan() { throw new UnsupportedOperationException(); } // forced, unwanted
    public void fax() { throw new UnsupportedOperationException(); }
}
```

```java
// FOLLOWS ISP — segregated, focused interfaces
interface Printer { void print(); }
interface Scanner { void scan(); }
interface Fax { void fax(); }

class OldPrinter implements Printer {
    public void print() { /* only implements what it actually supports */ }
}

class ModernPrinter implements Printer, Scanner, Fax {
    public void print() { /* ... */ }
    public void scan() { /* ... */ }
    public void fax() { /* ... */ }
}
```

---

## ❓ Question
**What is the Dependency Inversion Principle (DIP)?**

High-level modules should **not depend on low-level modules** — both should depend on **abstractions** (interfaces), not concrete implementations.

```java
// VIOLATES DIP — OrderService is tightly coupled to a specific concrete implementation
class MySqlDatabase {
    void save(String data) { /* ... */ }
}

class OrderService {
    private MySqlDatabase db = new MySqlDatabase(); // hardcoded dependency
    void placeOrder(String order) {
        db.save(order);
    }
}
```

```java
// FOLLOWS DIP — OrderService depends on an abstraction, not a concrete class
interface Database {
    void save(String data);
}

class MySqlDatabase implements Database {
    public void save(String data) { /* ... */ }
}

class MongoDatabase implements Database {
    public void save(String data) { /* ... */ }
}

class OrderService {
    private final Database db; // depends on the abstraction

    OrderService(Database db) { // injected — swap implementations freely
        this.db = db;
    }

    void placeOrder(String order) {
        db.save(order);
    }
}
```

---

## ❓ Question
**How do the SOLID principles map to Spring examples?**

```java
// DIP + OCP in action via Spring's Dependency Injection
public interface PaymentGateway {
    void pay(double amount);
}

@Component
class StripeGateway implements PaymentGateway {
    public void pay(double amount) { System.out.println("Paid via Stripe: " + amount); }
}

@Component
class PayPalGateway implements PaymentGateway {
    public void pay(double amount) { System.out.println("Paid via PayPal: " + amount); }
}

@Service
class CheckoutService {
    private final PaymentGateway gateway; // depends on abstraction (DIP)

    @Autowired
    CheckoutService(PaymentGateway gateway) { // Spring injects whichever bean is configured
        this.gateway = gateway;
    }

    void checkout(double amount) {
        gateway.pay(amount); // works with ANY current or future PaymentGateway (OCP)
    }
}
```

Spring's entire IoC container is essentially DIP applied at the framework level — your classes declare dependencies on abstractions, and the container decides which concrete implementation to inject.

---

# 🎯 40 Interview Questions — SOLID Principles

## ❓ Question
**1. (TCS) What does SOLID stand for?**

Single Responsibility, Open-Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion.

---

## ❓ Question
**2. (Infosys) What's a real symptom of an SRP violation in a codebase?**

A class that changes for multiple, unrelated business reasons — e.g., a `User` class that must be edited both when validation rules change AND when the database schema changes AND when email formatting changes.

---

## ❓ Question
**3. (Wipro) How does OCP relate to the Strategy design pattern?**

The Strategy pattern is a direct implementation of OCP — new algorithms/behaviors are added as new Strategy implementations without modifying the class that uses them.

---

## ❓ Question
**4. (Amazon) What's a common way LSP violations manifest at runtime?**

A subclass throwing `UnsupportedOperationException` for a method inherited from the parent, or silently behaving inconsistently with the parent's documented contract/expectations.

---

## ❓ Question
**5. (Google) Why does ISP discourage large "fat" interfaces?**

Because implementing classes are forced to provide (often meaningless or exception-throwing) implementations for methods they don't actually need, creating unnecessary coupling and confusing contracts.

---

## ❓ Question
**6. (Accenture) What's the difference between Dependency Inversion and Dependency Injection?**

Dependency Inversion is the **design principle** (depend on abstractions, not concretions). Dependency Injection is one common **technique/mechanism** used to achieve it (supplying dependencies from outside rather than creating them internally).

---

## ❓ Question
**7. (Cognizant) How does SRP improve testability?**

A class with a single responsibility has fewer reasons to change and fewer dependencies to mock, making unit tests simpler, more focused, and less likely to break due to unrelated changes.

---

## ❓ Question
**8. (Capgemini) What's a practical sign that OCP is being followed well in a codebase?**

Adding new features rarely requires modifying existing, already-tested classes — new functionality is added via new classes/implementations plugged into existing abstractions.

---

## ❓ Question
**9. (Deloitte) Can LSP violations still compile successfully?**

Yes — LSP violations are typically **behavioral**, not syntactic; the code compiles fine but behaves incorrectly or unexpectedly when a subclass is substituted for its parent.

---

## ❓ Question
**10. (Oracle) How would you refactor a large `Worker` interface with `work()` and `eat()` methods to follow ISP, for robots that don't eat?**

Split it into separate `Workable` and `Eatable` interfaces; `Robot` implements only `Workable`, while `Human` implements both.

---

## ❓ Question
**11. (Microsoft) Why is DIP considered the foundation that makes frameworks like Spring possible?**

Because IoC containers rely on classes depending on abstractions (interfaces) rather than instantiating concrete dependencies themselves — this is exactly what allows the container to inject appropriate implementations at runtime.

---

## ❓ Question
**12. (IBM) What's an example of violating SRP in a Spring Boot `@Controller`?**

A controller that handles HTTP request parsing AND business logic AND direct database queries all in one class, instead of delegating business logic to a `@Service` and persistence to a `@Repository`.

---

## ❓ Question
**13. (HCL) How does OCP relate to plugin-based architectures?**

Plugin systems are a direct real-world application of OCP — the core system defines an extension point (interface), and new plugins can be added without modifying the core system's code at all.

---

## ❓ Question
**14. (Mindtree) Can you give an LSP violation example involving exceptions?**

A subclass method that throws a new checked exception type not declared by the overridden parent method — code written to handle only the parent's exceptions would break unexpectedly when given the subclass.

---

## ❓ Question
**15. (LTI) Why might role interfaces (small, behavior-specific interfaces) be preferred in modern API design?**

They follow ISP naturally — clients depend only on the specific capability they need (e.g., `Readable`, `Closeable`), rather than a monolithic interface bundling unrelated behaviors.

---

## ❓ Question
**16. (Amazon) How does DIP make unit testing easier?**

Because high-level classes depend on interfaces, test doubles (mocks/stubs) can be substituted for real implementations without modifying the class under test at all.

---

## ❓ Question
**17. (Flipkart) What's a code smell suggesting SRP is being violated in a method (not just a class)?**

A single method doing multiple unrelated things — e.g., validating input, then calling an external API, then formatting output — often indicated by a long method with several distinct "sections."

---

## ❓ Question
**18. (Paytm) Why is `final` sometimes used alongside OCP-compliant designs?**

To lock down internal implementation details of concrete classes (preventing further modification via subclassing) while the abstraction/interface layer remains open for extension through new implementations.

---

## ❓ Question
**19. (Zoho) How would violating LSP affect a collections-based codebase, e.g., `List<Shape>`?**

If some `Shape` subclass behaves inconsistently with the expected `Shape` contract (e.g., `area()` returns nonsensical values for edge cases), code iterating and processing the list generically could produce silently incorrect results.

---

## ❓ Question
**20. (Freshworks) What's a real-world ISP example in Java's standard library evolution?**

The `Iterator` interface only requires `hasNext()`/`next()`, with `remove()` given a default (optional) implementation — avoiding forcing every iterator implementation to support removal.

---

## ❓ Question
**21. (Adobe) How does constructor injection support DIP better than field injection?**

It makes dependencies explicit and mandatory at object creation time (visible in the constructor signature), reinforcing that the class depends on an abstraction that must be supplied — field injection can hide this and make testing/immutability harder.

---

## ❓ Question
**22. (SAP) Can SRP be applied at a level broader than individual classes?**

Yes — the same principle applies to modules, packages, and microservices: each should have a single, well-defined reason to change/responsibility at its respective scope.

---

## ❓ Question
**23. (JPMorgan) What's a common OCP violation in legacy financial systems using large `switch`/`if-else` chains on transaction types?**

Adding a new transaction type requires modifying a central processing method's conditional logic directly, risking regressions in unrelated existing transaction types — better solved via polymorphic transaction handler classes.

---

## ❓ Question
**24. (Goldman Sachs) How does the Template Method pattern relate to LSP?**

It defines an algorithm's skeleton in a base class while letting subclasses override specific steps — properly designed, subclasses remain behaviorally substitutable since they only customize well-defined extension points, not the overall contract.

---

## ❓ Question
**25. (Morgan Stanley) Why might strict adherence to ISP lead to a proliferation of small interfaces, and is that a problem?**

Yes, it can increase the number of types in a codebase; it's a worthwhile trade-off when it genuinely reduces unwanted coupling, but over-fragmenting into excessively granular interfaces can also hurt readability if taken too far.

---

## ❓ Question
**26. (Infosys) How would you detect a DIP violation during code review?**

Look for high-level business logic classes directly instantiating (`new SomeConcreteClass()`) low-level implementation details (database clients, external API clients) instead of receiving them as injected abstractions.

---

## ❓ Question
**27. (Wipro) Can a single class violate multiple SOLID principles simultaneously?**

Yes — e.g., a "God class" handling many responsibilities (SRP violation) that also directly instantiates its own database connections (DIP violation) and has a bloated public interface (ISP violation) is a common real-world anti-pattern.

---

## ❓ Question
**28. (TCS) How does the Decorator pattern support OCP?**

It allows adding new behavior to an object dynamically by wrapping it in decorator classes, without modifying the original object's class at all — new decorators can be added freely as extensions.

---

## ❓ Question
**29. (Capgemini) What's an LSP-safe alternative design for the classic Rectangle/Square inheritance problem?**

Avoid inheritance between them entirely — both implement a common `Shape` interface with an `area()` method, without one being modeled as a specialized version of the other.

---

## ❓ Question
**30. (Cognizant) How does dependency inversion apply at the architecture level, beyond individual classes?**

In layered/hexagonal architecture, core business logic depends on abstract "ports," while infrastructure details (databases, external APIs) implement those ports as "adapters" — inverting the traditional dependency direction so business logic doesn't depend on infrastructure.

---

## ❓ Question
**31. (Amazon) Why is SRP sometimes described as "a class should have only one reason to change" rather than "a class should do only one thing"?**

Because "one thing" is often too granular/subjective in practice; "one reason to change" better captures the intent — grouping cohesive behavior that changes together for the same underlying business reason.

---

## ❓ Question
**32. (Google) How can excessive SRP application lead to over-engineering?**

Splitting a class into too many tiny, overly granular classes for trivial responsibilities can add unnecessary indirection and complexity without meaningful benefit — SRP should be balanced with pragmatism.

---

## ❓ Question
**33. (Oracle) What's the relationship between OCP and unit test stability?**

Since OCP-compliant code extends via new classes rather than modifying existing ones, previously-passing tests for existing functionality remain stable and unaffected when new features are added.

---

## ❓ Question
**34. (Microsoft) Can LSP violations occur with return types, not just method behavior?**

Yes — if an overriding method's return type is less specific/compatible than expected, or if a subclass method returns null/throws where the parent's contract implies it wouldn't, that's also an LSP violation (covariant return types are fine; broken contracts are not).

---

## ❓ Question
**35. (Deloitte) How does ISP relate to microservice API design?**

Well-designed microservice APIs expose focused, purpose-specific endpoints/contracts rather than one giant "do everything" API — consumers depend only on the specific operations they actually need.

---

## ❓ Question
**36. (Accenture) What's a DIP-related benefit when swapping a real payment gateway for a mock/test double?**

Because `CheckoutService` depends on the `PaymentGateway` interface (not a concrete class), you can inject a test mock implementing that same interface without changing `CheckoutService`'s code at all.

---

## ❓ Question
**37. (IBM) How do all five SOLID principles work together to reduce coupling overall?**

SRP reduces internal complexity per class; OCP and LSP ensure safe extensibility without breaking existing code; ISP avoids unnecessary interface coupling; DIP inverts dependency direction toward stable abstractions — together they minimize the ripple effect of changes across a codebase.

---

## ❓ Question
**38. (HCL) Why is SOLID considered guidance rather than strict, universal rules?**

Because rigidly applying every principle everywhere can sometimes add unnecessary complexity for genuinely simple problems — SOLID is most valuable in codebases expected to grow, change, and be maintained by teams over time.

---

## ❓ Question
**39. (Mindtree) How would you explain DIP to a junior developer using a simple analogy?**

A lamp plugs into a standard wall socket (the abstraction) rather than being wired directly into the power plant (the concrete low-level detail) — the lamp and the power source both depend on the standardized socket interface, not on each other directly.

---

## ❓ Question
**40. (LTI) What's the overall business value SOLID principles provide to a software organization?**

They reduce the cost and risk of changing/extending software over time — fewer regressions, easier onboarding for new developers, more testable code, and systems that can adapt to new requirements without extensive rewrites.

---
