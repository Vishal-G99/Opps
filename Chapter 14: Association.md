# Chapter 14: Association

---

## ❓ Question
**What is Association?**

Association is a general relationship between two independent classes where one class **uses** or **references** another, without either owning the other's lifecycle.

```java
class Teacher {
    String name;
}

class Student {
    String name;
    Teacher teacher; // Student is "associated with" a Teacher

    Student(String name, Teacher teacher) {
        this.name = name;
        this.teacher = teacher;
    }
}
```

Both `Teacher` and `Student` can exist independently — a `Teacher` object isn't destroyed if a `Student` object is, and vice versa.

---

## ❓ Question
**What is Aggregation?**

Aggregation is a special "**has-a**" form of association representing a **whole-part relationship** where the part can exist **independently** of the whole (a weaker ownership).

```java
class Department {
    String name;
}

class Employee {
    String name;
    Department department; // Employee "has-a" Department, but Department exists independently

    Employee(String name, Department department) {
        this.name = name;
        this.department = department;
    }
}

Department hr = new Department();
Employee emp = new Employee("Alice", hr);
// If emp is deleted, "hr" Department object still exists fine on its own
```

---

## ❓ Question
**What is Composition?**

Composition is a **stronger "has-a"** relationship where the part **cannot exist independently** of the whole — when the owning object is destroyed, its parts are destroyed too.

```java
class Engine {
    void start() { System.out.println("Engine starting"); }
}

class Car {
    private final Engine engine; // Engine is created and owned entirely by Car

    Car() {
        this.engine = new Engine(); // Engine's lifecycle is tied to Car's
    }

    void start() {
        engine.start();
    }
}

// If a Car object is destroyed, its Engine has no independent existence outside it
```

---

## ❓ Question
**How do these relationships look in UML?**

```
Association:    Class A ─────────── Class B
                (plain line — simple "uses/knows about" relationship)

Aggregation:    Class A ◇────────── Class B
                (hollow diamond at the "whole" end — weak "has-a", part can live independently)

Composition:    Class A ◆────────── Class B
                (filled diamond at the "whole" end — strong "has-a", part's lifecycle is bound to the whole)
```

```
Strength of ownership:  Association  <  Aggregation  <  Composition
```

---

## ❓ Question
**How does Spring Boot model these relationships in practice?**

```java
// Association-style: two independently-managed Spring beans referencing each other
@Service
class NotificationService {
    void notifyUser(String msg) { System.out.println(msg); }
}

@Service
class OrderService {
    private final NotificationService notificationService; // association via DI

    @Autowired
    OrderService(NotificationService notificationService) {
        this.notificationService = notificationService;
    }
}
```

```java
// Composition-style: the "part" object is created and fully owned internally,
// never injected or shared externally
@Service
class ReportGenerator {
    private final ReportFormatter formatter = new ReportFormatter(); // owned internally

    String generate() {
        return formatter.format("Report data");
    }
}
```

---

## ❓ Question
**How does Hibernate/JPA model Association, Aggregation, and Composition?**

```java
// Association / Aggregation: @OneToMany with a foreign key, independent lifecycles
@Entity
class Department {
    @Id Long id;
    String name;

    @OneToMany(mappedBy = "department")
    List<Employee> employees; // employees can exist/be reassigned independently
}
```

```java
// Composition: @Embeddable — the "part" has no identity of its own,
// and is fully owned by the parent entity
@Entity
class Order {
    @Id Long id;

    @Embedded
    Address shippingAddress; // Address has no separate table/identity — lives inside Order
}

@Embeddable
class Address {
    String street;
    String city;
}
```

```java
// Strong composition with cascading delete: child rows are deleted when parent is deleted
@Entity
class Invoice {
    @Id Long id;

    @OneToMany(mappedBy = "invoice", cascade = CascadeType.ALL, orphanRemoval = true)
    List<InvoiceLineItem> lineItems; // line items cannot exist without their Invoice
}
```

---

# 🎯 40 Interview Questions — Association

## ❓ Question
**1. (TCS) What's the simplest way to distinguish Association from Aggregation/Composition?**

Association is the general umbrella term for any "uses-a" relationship; Aggregation and Composition are both specific, stronger forms of association representing "has-a" (whole-part) relationships.

---

## ❓ Question
**2. (Infosys) Can Association be bidirectional?**

Yes — e.g., a `Student` referencing a `Teacher`, and that `Teacher` also maintaining a list of `Student` objects, is a bidirectional association.

---

## ❓ Question
**3. (Wipro) What is the key lifecycle difference between Aggregation and Composition?**

In Aggregation, the "part" object can outlive the "whole." In Composition, the "part" is destroyed along with the "whole" — it has no independent existence.

---

## ❓ Question
**4. (Amazon) Can you give a real-world Aggregation example outside of code?**

A `University` "has" `Department` objects — if the university closes, departments could still theoretically be transferred/exist as separate records elsewhere (weak ownership).

---

## ❓ Question
**5. (Google) Can you give a real-world Composition example outside of code?**

A `House` "has" `Room` objects — a room, as modeled, has no meaningful existence independent of the specific house it belongs to (strong ownership).

---

## ❓ Question
**6. (Accenture) Is inheritance the same as composition?**

No — inheritance ("is-a") creates a taxonomic relationship between classes sharing structure/behavior. Composition ("has-a") creates a functional relationship where one object contains/uses another.

---

## ❓ Question
**7. (Cognizant) Why is "favor composition over inheritance" a common design principle?**

Composition offers more flexibility (behavior can be swapped at runtime via interfaces), avoids fragile deep inheritance hierarchies, and reduces tight coupling to a rigid parent class's implementation.

---

## ❓ Question
**8. (Capgemini) In UML, what does the diamond shape's fill (hollow vs. solid) represent?**

A hollow diamond represents aggregation (weak ownership); a filled/solid diamond represents composition (strong ownership).

---

## ❓ Question
**9. (Deloitte) Can an object be both the "whole" in a composition and the "part" in another composition simultaneously?**

Yes — e.g., a `Car` is composed of an `Engine`, while the `Car` itself might be a "part" composed within a `Garage`'s inventory tracking, depending on the domain model's design.

---

## ❓ Question
**10. (Oracle) How would you model Composition using constructors versus setters?**

Composition is typically enforced by creating the "part" object **inside** the "whole's" constructor (or as a field initializer), rather than allowing it to be injected/set externally — reinforcing that its lifecycle is fully owned.

---

## ❓ Question
**11. (Microsoft) In JPA, what's the significance of `cascade = CascadeType.ALL` combined with `orphanRemoval = true`?**

Together they enforce composition semantics — deleting the parent entity (or removing a child from its collection) automatically deletes the corresponding child rows, reflecting that the child has no independent existence.

---

## ❓ Question
**12. (IBM) Is a plain method parameter passed into another class's method considered Association?**

It can be considered a very loose/transient association — the classes "know about" each other during that call, though it's often just called a "dependency" rather than a full structural association if it's not stored as a field.

---

## ❓ Question
**13. (HCL) What's the difference between Association and Dependency in UML?**

Association implies a more persistent structural relationship (often stored as a field). Dependency is typically a temporary, weaker relationship — e.g., a method parameter or local variable used briefly within a method call.

---

## ❓ Question
**14. (Mindtree) Can Aggregation be one-to-many?**

Yes — e.g., a `Team` aggregating multiple `Player` objects, where players can be transferred to other teams and still exist independently.

---

## ❓ Question
**15. (LTI) How does Dependency Injection in Spring typically represent Association rather than Composition?**

Because injected beans are usually created and managed externally by the Spring container (not instantiated internally by the class using them), their lifecycle is independent — a hallmark of association/aggregation rather than tight composition.

---

## ❓ Question
**16. (Amazon) Why is composition generally considered to promote better encapsulation than inheritance?**

Because the "part" object's internal implementation is completely hidden behind the "whole" object's own API — external code interacts only with the whole, unlike inheritance where protected/parent internals can leak into subclasses.

---

## ❓ Question
**17. (Flipkart) Can two classes have a bidirectional Composition relationship?**

Practically no — true composition requires exactly one "owner" whose lifecycle governs the "part." A mutual, symmetric ownership would create ambiguous/cyclic lifecycle dependencies and isn't standard composition design.

---

## ❓ Question
**18. (Paytm) How would you refactor an Aggregation into a Composition if requirements change?**

Change the "part" object's creation from being passed in externally (e.g., via constructor injection) to being instantiated internally within the "whole" class, removing any external reference/reuse of that part elsewhere.

---

## ❓ Question
**19. (Zoho) What's a common anti-pattern when trying to model Composition in Hibernate/JPA?**

Using a regular `@OneToMany` without `cascade`/`orphanRemoval` settings, which allows child records to persist independently even after being removed from the parent's collection — accidentally behaving like aggregation instead of composition.

---

## ❓ Question
**20. (Freshworks) Is a `List<Employee>` field inside a `Department` class necessarily Aggregation?**

Typically yes, if `Employee` objects can exist and be reassigned to other departments independently — but it depends on the actual domain rules governing whether employees can exist without a department.

---

## ❓ Question
**21. (Adobe) How does Association differ when using interfaces versus concrete classes as the referenced type?**

Associating via an interface type (e.g., `PaymentProcessor processor`) allows loose coupling and swappable implementations at runtime, which is generally preferred over associating directly with a concrete class.

---

## ❓ Question
**22. (SAP) What design smell can excessive Association between many classes create?**

High coupling — a "tangled" object graph where changes to one class ripple unpredictably through many others, making the system harder to test, maintain, and reason about.

---

## ❓ Question
**23. (JPMorgan) How do microservice architectures typically avoid tight Composition-style coupling between services?**

By communicating through well-defined APIs/events rather than sharing in-process object references, ensuring each service's internal "parts" remain fully encapsulated and independently deployable.

---

## ❓ Question
**24. (Goldman Sachs) Can Aggregation exist between objects in different bounded contexts/microservices?**

Yes, loosely — e.g., an `Order` service referencing a `CustomerId` from a separate `Customer` service represents a form of aggregation-like association, without either service owning the other's lifecycle or data directly.

---

## ❓ Question
**25. (Morgan Stanley) Why might using Composition make unit testing simpler in some cases, and harder in others?**

Simpler when the "part" is trivial/stateless and doesn't need mocking. Harder when the "part" has complex behavior, since composition (internal instantiation) makes it difficult to substitute a mock/test double, unlike association via constructor injection which allows easy mocking.

---

## ❓ Question
**26. (Infosys) What's the relationship between Composition and the Single Responsibility Principle?**

Composition naturally supports SRP by delegating specific responsibilities to well-defined "part" objects (e.g., an `Engine` handles propulsion logic) rather than piling all behavior into one large class.

---

## ❓ Question
**27. (Wipro) How would you represent "has-a" versus "is-a" in code to decide between composition and inheritance?**

Ask: "Is a `Car` a `Vehicle`?" (yes → inheritance/is-a) versus "Does a `Car` have an `Engine`?" (yes → composition/has-a); mixing these up is a classic OOP design mistake.

---

## ❓ Question
**28. (TCS) Can Aggregation be represented using just a reference field, without any special Java syntax?**

Yes — Java has no dedicated keyword for aggregation/composition; both are expressed identically in code as reference fields. The distinction is purely a **design-level (UML/semantic) concept**, not enforced by the compiler.

---

## ❓ Question
**29. (Capgemini) Why is it misleading to say "composition is always better than association" universally?**

Because composition sacrifices flexibility and reusability of the "part" — if a component genuinely needs to be shared or independently managed (e.g., across multiple services or swapped for testing), forcing composition creates unnecessary rigidity.

---

## ❓ Question
**30. (Cognizant) How do you decide the multiplicity (1-to-1, 1-to-many, many-to-many) in an association?**

Based on real-world domain rules — e.g., "one `Department` has many `Employees`" is 1-to-many, while "many `Students` enroll in many `Courses`" is many-to-many, typically modeled with an intermediate join table/class.

---

## ❓ Question
**31. (Amazon) What's an example of a many-to-many Association, and how is it typically implemented?**

`Student` and `Course` — a student can enroll in many courses, and a course can have many students; typically implemented with a join table (`Enrollment`) or a `@ManyToMany` mapping in JPA.

---

## ❓ Question
**32. (Google) Can Composition help enforce invariants that Aggregation cannot?**

Yes — since the "whole" fully controls the "part's" creation and lifecycle, it can guarantee the part is always in a valid, fully-initialized state, which is harder to guarantee when the part is externally created and injected.

---

## ❓ Question
**33. (Oracle) How does immutability interact well with Composition?**

An immutable "whole" object composed of immutable "parts" (all set once via the constructor) guarantees the entire object graph's state can never change after construction — a common pattern in domain-driven design's Value Objects.

---

## ❓ Question
**34. (Microsoft) What's the risk of exposing a composed "part" object via a public getter?**

It can leak the internal implementation detail and allow external code to mutate the part directly, breaking the encapsulation that composition is meant to provide — often mitigated by returning defensive copies or read-only views.

---

## ❓ Question
**35. (Deloitte) Why is "the whole delegates behavior to its parts" a hallmark of good Composition design?**

It keeps the "whole" class's own logic focused on coordination, while specialized behavior lives in well-encapsulated "part" classes — improving modularity and making each piece easier to test in isolation.

---

## ❓ Question
**36. (Accenture) How does Aggregation support the Dependency Inversion Principle?**

By associating with an abstraction (interface) rather than a concrete class, and having that dependency injected externally, high-level modules don't depend on low-level implementation details directly.

---

## ❓ Question
**37. (IBM) Can a class have Association, Aggregation, and Composition relationships all at once with different classes?**

Yes — a `Car` might have a Composition relationship with `Engine` (owned internally), an Aggregation relationship with `Driver` (can exist independently), and a plain Association with `TrafficService` (used occasionally, no ownership at all).

---

## ❓ Question
**38. (HCL) How would a code reviewer spot Composition being used where Aggregation was actually needed?**

If a "part" object is being instantiated internally (locked into the whole) but the requirements later reveal it should be shareable/reassignable/independently persisted — a sign the relationship should be loosened to aggregation via external injection.

---

## ❓ Question
**39. (Mindtree) Why does Composition typically simplify serialization/deserialization logic?**

Because the "part" object's lifecycle and existence is entirely bound to the "whole," serializing the whole naturally includes its parts without needing to resolve external references/foreign keys separately.

---

## ❓ Question
**40. (LTI) What's a practical Spring Boot example distinguishing Aggregation (via `@Autowired`) versus Composition (via `new` inside the class)?**

A `@Service` receiving a `PaymentGateway` bean via constructor injection is Aggregation (the gateway is externally managed and reusable elsewhere); a `@Service` internally creating `new RequestValidator()` for its own private use is Composition (fully owned, never shared).

---
