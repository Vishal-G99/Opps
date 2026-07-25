# Chapter 21: Advanced OOP

---

## ❓ Question
**What is Reflection in Java?**

Reflection allows code to **inspect and manipulate classes, methods, fields, and constructors at runtime**, even without knowing them at compile time.

```java
import java.lang.reflect.*;

class Person {
    private String name = "Alice";
    private void greet() { System.out.println("Hi, I'm " + name); }
}

Class<?> clazz = Person.class;
Object instance = clazz.getDeclaredConstructor().newInstance();

Method greetMethod = clazz.getDeclaredMethod("greet");
greetMethod.setAccessible(true); // bypass private access
greetMethod.invoke(instance);     // "Hi, I'm Alice"

Field nameField = clazz.getDeclaredField("name");
nameField.setAccessible(true);
System.out.println(nameField.get(instance)); // "Alice"
```

Powers frameworks like Spring (dependency injection), Hibernate (ORM mapping), and JUnit (test discovery).

---

## ❓ Question
**What are Annotations, and how do they relate to Reflection?**

Annotations are **metadata attached to code** (classes, methods, fields) that frameworks read via reflection to drive behavior, without you writing that logic manually.

```java
@Retention(RetentionPolicy.RUNTIME) // must be RUNTIME to be readable via reflection
@Target(ElementType.METHOD)
@interface Loggable {
    String value() default "";
}

class Service {
    @Loggable("Important operation")
    void process() { System.out.println("Processing..."); }
}

Method m = Service.class.getDeclaredMethod("process");
if (m.isAnnotationPresent(Loggable.class)) {
    Loggable annotation = m.getAnnotation(Loggable.class);
    System.out.println("Log message: " + annotation.value());
}
```

This is exactly how Spring reads `@Autowired`/`@Service`, and JUnit reads `@Test` — annotations declare intent, reflection reads and acts on it.

---

## ❓ Question
**What are Serialization and Deserialization?**

Serialization converts an object into a **byte stream** (for storage or transmission); deserialization reconstructs the object from that byte stream.

```java
import java.io.*;

class User implements Serializable {
    String name;
    transient String password; // excluded from serialization
    User(String name, String password) { this.name = name; this.password = password; }
}

// Serialize
User user = new User("Alice", "secret123");
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("user.ser"));
out.writeObject(user);
out.close();

// Deserialize
ObjectInputStream in = new ObjectInputStream(new FileInputStream("user.ser"));
User restored = (User) in.readObject();
in.close();

System.out.println(restored.name);     // "Alice"
System.out.println(restored.password); // null — transient fields are skipped
```

---

## ❓ Question
**What is an Object Graph?**

An object graph is the network of interconnected objects formed by their references to one another — visualizing how objects relate through composition/association.

```java
class Address { String city; }
class Employee { String name; Address address; }
class Department { String name; List<Employee> employees; }

Department dept = new Department();
```

```
Object Graph:
Department
   ├── name: "Engineering"
   └── employees: [
         Employee { name: "Alice", address: Address{city:"NYC"} },
         Employee { name: "Bob",   address: Address{city:"LA"} }
       ]
```

Serialization must traverse this **entire graph** (recursively serializing every referenced object), which is why every class in the graph needs to be `Serializable`.

---

## ❓ Question
**What are High Cohesion and Loose Coupling?**

**High Cohesion** — a class's responsibilities are closely related and focused (a single, well-defined purpose). **Loose Coupling** — classes depend on each other minimally, ideally through abstractions, so changes in one don't ripple through others.

```java
// LOW cohesion + TIGHT coupling — one class does everything, directly wired together
class OrderManager {
    void validateOrder() { }
    void saveToMySQL() { }        // tightly coupled to a specific database
    void sendEmailViaSmtp() { }    // tightly coupled to a specific email mechanism
    void generateInvoicePdf() { }
}
```

```java
// HIGH cohesion + LOOSE coupling — focused classes, connected via abstractions
interface OrderRepository { void save(Order order); }
interface NotificationSender { void send(String message); }

class OrderService {
    private final OrderRepository repository;      // depends on abstraction
    private final NotificationSender notifier;       // depends on abstraction

    OrderService(OrderRepository repository, NotificationSender notifier) {
        this.repository = repository;
        this.notifier = notifier;
    }

    void placeOrder(Order order) {
        repository.save(order);
        notifier.send("Order placed: " + order.getId());
    }
}
```

---

## ❓ Question
**What does Composition over Inheritance mean, in an Advanced context?**

Beyond the basic "has-a vs. is-a" idea, this principle emphasizes building complex behavior by **combining small, focused, swappable objects** rather than relying on deep, rigid inheritance hierarchies that are hard to change safely.

```java
// Inheritance-heavy design — rigid, fragile as behaviors multiply
class FlyingRobot extends Robot { void fly() { } }
class SwimmingRobot extends Robot { void swim() { } }
class FlyingSwimmingRobot extends ??? // combinatorial explosion problem

// Composition-based design — flexible, mix-and-match capabilities
interface Flyable { void fly(); }
interface Swimmable { void swim(); }

class Robot {
    private final List<Object> capabilities = new ArrayList<>();

    void addCapability(Object capability) { capabilities.add(capability); }
}

class Drone implements Flyable {
    public void fly() { System.out.println("Flying"); }
}

class AmphibiousRobot implements Flyable, Swimmable {
    public void fly() { System.out.println("Flying"); }
    public void swim() { System.out.println("Swimming"); }
}
```

---

# 🎯 40 Interview Questions — Advanced OOP

## ❓ Question
**1. (TCS) Why does Reflection bypass normal access control (like `private`)?**

`setAccessible(true)` explicitly tells the JVM to suppress standard Java language access checks — intended for frameworks/tools with a legitimate need, not general application code.

---

## ❓ Question
**2. (Infosys) What's a performance downside of using Reflection heavily?**

Reflective method calls and field access are significantly slower than direct calls, since they involve additional lookup, security checks, and lack the JIT compiler's usual optimization opportunities (though modern JVMs have improved this over time).

---

## ❓ Question
**3. (Wipro) Why must an annotation have `@Retention(RUNTIME)` to be usable via reflection?**

Annotations default to `CLASS` retention (kept in bytecode but not loaded into the JVM at runtime); only `RUNTIME` retention ensures the annotation metadata is available for reflective inspection while the program executes.

---

## ❓ Question
**4. (Amazon) What is `transient` used for in Serialization?**

Marks a field to be **excluded** from the default serialization process — commonly used for sensitive data (passwords), non-serializable fields, or derived/cacheable values that can be recomputed after deserialization.

---

## ❓ Question
**5. (Google) What is `serialVersionUID`, and why is it important?**

A version identifier for a `Serializable` class; if it doesn't match between the serialized data and the currently loaded class definition, deserialization throws `InvalidClassException` — explicitly declaring it prevents accidental incompatibility from unrelated class changes.

---

## ❓ Question
**6. (Accenture) Why is deserializing untrusted data considered a security risk?**

Maliciously crafted serialized data can trigger unexpected code execution or object states during deserialization (a well-known class of vulnerabilities), which is why many systems now prefer safer formats like JSON with strict schema validation instead of native Java serialization.

---

## ❓ Question
**7. (Cognizant) How does an Object Graph relate to deep vs. shallow copying?**

A deep copy must traverse and duplicate the entire object graph (every reachable nested object), while a shallow copy only duplicates the top-level object, leaving the rest of the graph shared with the original.

---

## ❓ Question
**8. (Capgemini) What's a real-world symptom of low cohesion in a class?**

The class has many unrelated methods/fields, is difficult to name meaningfully (often ending up as a vague "Manager" or "Helper" class), and changes for many unrelated reasons.

---

## ❓ Question
**9. (Deloitte) How does loose coupling improve a system's ability to handle changing requirements?**

Since classes interact through stable abstractions rather than concrete implementation details, one component can be modified, replaced, or extended without requiring corresponding changes throughout the rest of the system.

---

## ❓ Question
**10. (Oracle) Why is "favor composition over inheritance" especially relevant for combining multiple, orthogonal behaviors?**

Inheritance can only model a single linear hierarchy per class in Java (no multiple class inheritance), leading to combinatorial explosion or awkward hierarchies when a class needs several independent, mixable behaviors — composition avoids that entirely.

---

## ❓ Question
**11. (Microsoft) How does Spring use Reflection during application startup?**

To scan for annotated classes (`@Component`, `@Service`, etc.), instantiate them via their constructors, inspect and inject annotated (`@Autowired`) fields/setters, and invoke lifecycle methods (`@PostConstruct`) — all without the classes needing to implement any special framework interface.

---

## ❓ Question
**12. (IBM) Can Reflection be used to modify `final` fields?**

In limited cases historically yes (via reflection hacks removing the final modifier flag), but modern JVMs increasingly restrict this for safety/security reasons, especially for fields initialized as compile-time constants.

---

## ❓ Question
**13. (HCL) What's the difference between `Class.forName()` and `SomeClass.class`?**

`SomeClass.class` is resolved at compile time (the class must be known/available). `Class.forName("com.app.SomeClass")` loads and resolves the class dynamically at runtime from its fully-qualified name, useful when the class isn't known until runtime (e.g., JDBC driver loading).

---

## ❓ Question
**14. (Mindtree) Why do annotation processors (like Lombok) operate differently from runtime reflection-based annotations?**

Annotation processors run at **compile time**, generating or modifying source/bytecode before the program even runs, whereas reflection-based annotations are read and acted upon dynamically while the program is executing.

---

## ❓ Question
**15. (LTI) What's a common use case for `WeakReference` in combination with reflection-heavy frameworks?**

Avoiding memory leaks when caching reflective metadata (like `Method`/`Field` objects) tied to dynamically loaded classes — weak references allow the cache entries to be collected if the associated class is no longer needed/loaded.

---

## ❓ Question
**16. (Amazon) How does JSON serialization (e.g., Jackson) typically differ from native Java Serialization in terms of coupling to class structure?**

JSON serialization is more loosely coupled to the exact class implementation (fields can be renamed/reordered with annotations, and cross-language/version compatibility is easier), while native Java Serialization is tightly bound to the exact class bytecode structure and `serialVersionUID`.

---

## ❓ Question
**17. (Flipkart) Can circular references in an object graph cause issues during Serialization?**

Native Java serialization handles cycles correctly by tracking already-serialized object references internally — but naive custom (e.g., some JSON) serialization implementations can infinite-loop on cyclic graphs unless specifically designed to detect and handle them.

---

## ❓ Question
**18. (Paytm) What's an example of High Cohesion done well in a typical layered architecture?**

A `PasswordValidator` class focused solely on validating password rules (length, complexity) — every method and field directly relates to that single, well-defined responsibility.

---

## ❓ Question
**19. (Zoho) How does Dependency Injection directly support Loose Coupling?**

Classes declare dependencies on abstractions and receive concrete implementations from an external source (constructor injection/IoC container), rather than instantiating or hardcoding specific implementations themselves.

---

## ❓ Question
**20. (Freshworks) Why might a codebase with excessive inheritance hierarchies be described as "fragile"?**

Changes to a base class can have unpredictable ripple effects across many subclasses (the "fragile base class problem"), especially in deep hierarchies where subclasses depend heavily on specific parent implementation details.

---

## ❓ Question
**21. (Adobe) How does composition make unit testing individual behaviors easier compared to deep inheritance?**

Each composed component/interface can be mocked or substituted independently, letting you test a class's coordination logic without needing to exercise an entire inheritance chain's combined behavior.

---

## ❓ Question
**22. (SAP) What's a real-world Reflection use case outside of frameworks, in application code?**

Building generic utility functions like a "deep equals" comparator, generic object-to-string debuggers, or plugin-loading systems that discover and instantiate classes by name at runtime from configuration.

---

## ❓ Question
**23. (JPMorgan) Why might financial systems avoid native Java Serialization for inter-service communication in favor of Protocol Buffers or JSON?**

Native serialization is JVM/language-specific, less human-readable, more vulnerable to deserialization exploits, and less suitable for cross-language, cross-version compatibility needed in distributed, polyglot microservice architectures.

---

## ❓ Question
**24. (Goldman Sachs) How does annotation-driven configuration (like `@Transactional`) reduce coupling to a specific transaction management implementation?**

The business code only declares intent via the annotation; the actual transaction management logic (JDBC, JTA, etc.) is handled transparently by the framework's underlying infrastructure, which can be swapped without touching annotated business classes.

---

## ❓ Question
**25. (Morgan Stanley) What's the tradeoff of using Reflection for a plugin architecture versus a more explicit Service Provider Interface (SPI) approach?**

Reflection-based dynamic loading offers maximum flexibility (classes discovered purely by name/configuration) but sacrifices compile-time safety; Java's built-in `ServiceLoader`/SPI mechanism offers a more structured, still-flexible middle ground with better tooling support.

---

## ❓ Question
**26. (Infosys) Why is `Object[]` sometimes used in reflective method invocation, and what's a downside?**

`Method.invoke(target, args)` requires arguments as an `Object[]`, boxing any primitive parameters — this introduces both a small performance cost and loses compile-time type checking for the invocation arguments.

---

## ❓ Question
**27. (Wipro) How does an Object Graph's depth/complexity affect Garbage Collection performance?**

Deeper, more complex graphs require the GC to traverse more reference chains during reachability analysis, which can slightly increase GC scan time — though modern collectors are generally well-optimized for typical real-world object graph sizes.

---

## ❓ Question
**28. (TCS) What is the "Law of Demeter," and how does it relate to coupling?**

A guideline stating an object should only communicate with its immediate collaborators, not "reach through" them to access their internals (avoiding chains like `a.getB().getC().doSomething()`) — reducing coupling to the internal structure of unrelated objects.

---

## ❓ Question
**29. (Capgemini) How does composition support runtime behavior swapping in a way inheritance cannot?**

A composed field/reference can be reassigned to a different implementation of the same interface at runtime (e.g., swapping a `PaymentStrategy`), while an object's inherited class hierarchy is fixed permanently once the object is created.

---

## ❓ Question
**30. (Cognizant) What's a common mistake when implementing custom `readObject()`/`writeObject()` methods for Serialization?**

Forgetting to call `defaultReadObject()`/`defaultWriteObject()` first (to handle the standard fields) before adding custom logic, which can silently corrupt or skip normal field serialization.

---

## ❓ Question
**31. (Amazon) Why might a highly cohesive class still need to depend on several other classes?**

Cohesion is about a class having a single, focused responsibility — it can still legitimately collaborate with multiple other well-defined classes to fulfill that responsibility, as long as those collaborations are all genuinely relevant to its one core purpose.

---

## ❓ Question
**32. (Google) How does Reflection enable generic frameworks like JUnit to discover test methods without requiring inheritance from a specific base class?**

JUnit scans for methods annotated with `@Test` using reflection at runtime, invoking them dynamically — meaning test classes don't need to extend any particular framework superclass, keeping test code loosely coupled to the testing framework itself.

---

## ❓ Question
**33. (Oracle) What's the relationship between Object Graphs and the Visitor design pattern?**

The Visitor pattern is often used to traverse a complex object graph (like an AST or document structure), performing operations on each node type without modifying the node classes themselves — separating traversal/operation logic from the graph's structural classes.

---

## ❓ Question
**34. (Microsoft) Why is high coupling particularly problematic in large, multi-team codebases?**

Changes made by one team can unexpectedly break code owned by another team if their components are tightly coupled, making coordinated releases and independent development significantly harder to manage safely.

---

## ❓ Question
**35. (Deloitte) How does the concept of "Ports and Adapters" (Hexagonal Architecture) apply Loose Coupling at a system level?**

Core business logic depends only on abstract "ports" (interfaces); concrete "adapters" (database drivers, external APIs, UI frameworks) implement those ports, meaning the core logic remains entirely decoupled from any specific infrastructure choice.

---

## ❓ Question
**36. (Accenture) Can Reflection be used to violate the Liskov Substitution Principle at runtime, even if code compiles correctly?**

Yes — reflective code can invoke methods, cast objects, or manipulate state in ways that bypass normal compile-time type safety guarantees, potentially violating a class's intended behavioral contracts if used carelessly.

---

## ❓ Question
**37. (IBM) What's a practical tradeoff between using inheritance-based Template Method versus a composition-based Strategy for similar customization needs?**

Template Method is simpler for a fixed, well-understood algorithm skeleton with a few customizable steps; Strategy offers more flexibility for swapping entire algorithms at runtime and avoids the rigidity of a fixed inheritance hierarchy as variation grows.

---

## ❓ Question
**38. (HCL) How does excessive reliance on Reflection undermine the benefits of static typing in Java?**

Reflective code often loses compile-time type checking (using `Object`, casting, and string-based member lookup), pushing potential errors from compile time to runtime, which is precisely the safety net static typing is meant to provide.

---

## ❓ Question
**39. (Mindtree) Why is understanding Object Graphs important when designing REST API DTOs (Data Transfer Objects)?**

Poorly designed DTOs can accidentally expose or serialize deep, unintended parts of an internal object graph (like lazy-loaded entity relationships), leaking implementation details or causing performance/serialization issues (like the classic Hibernate `LazyInitializationException`).

---

## ❓ Question
**40. (LTI) How do High Cohesion, Loose Coupling, and Composition over Inheritance work together as a unified advanced OOP philosophy?**

They collectively push toward small, focused, independently testable components (cohesion) that interact through stable abstractions (loose coupling) and are combined flexibly at runtime (composition) — the foundation of maintainable, scalable, real-world enterprise Java systems.

---
