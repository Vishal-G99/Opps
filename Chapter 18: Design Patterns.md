# Chapter 18: Design Patterns

---

## ❓ Question
**What is the Singleton Pattern?**

Ensures a class has **exactly one instance** and provides a global access point to it.

```java
class ConfigManager {
    private static final ConfigManager INSTANCE = new ConfigManager();

    private ConfigManager() {} // private constructor prevents external instantiation

    public static ConfigManager getInstance() {
        return INSTANCE;
    }
}

ConfigManager cfg1 = ConfigManager.getInstance();
ConfigManager cfg2 = ConfigManager.getInstance();
System.out.println(cfg1 == cfg2); // true — same single instance
```

---

## ❓ Question
**What is the Factory Pattern?**

Delegates object creation to a factory method/class, decoupling the client code from concrete class instantiation.

```java
interface Shape { void draw(); }
class Circle implements Shape { public void draw() { System.out.println("Circle"); } }
class Square implements Shape { public void draw() { System.out.println("Square"); } }

class ShapeFactory {
    static Shape create(String type) {
        return switch (type) {
            case "circle" -> new Circle();
            case "square" -> new Square();
            default -> throw new IllegalArgumentException("Unknown shape");
        };
    }
}

Shape s = ShapeFactory.create("circle"); // client doesn't know/care about the concrete class
```

---

## ❓ Question
**What is the Builder Pattern?**

Constructs complex objects **step by step**, avoiding huge telescoping constructors, especially useful with many optional fields.

```java
class Pizza {
    private final String size;
    private final boolean cheese;
    private final boolean pepperoni;

    private Pizza(Builder b) {
        this.size = b.size; this.cheese = b.cheese; this.pepperoni = b.pepperoni;
    }

    static class Builder {
        private String size;
        private boolean cheese, pepperoni;

        Builder size(String s) { this.size = s; return this; }
        Builder cheese(boolean c) { this.cheese = c; return this; }
        Builder pepperoni(boolean p) { this.pepperoni = p; return this; }
        Pizza build() { return new Pizza(this); }
    }
}

Pizza pizza = new Pizza.Builder().size("Large").cheese(true).pepperoni(true).build();
```

---

## ❓ Question
**What is the Prototype Pattern?**

Creates new objects by **cloning an existing instance** ("prototype") instead of constructing from scratch — useful when creation is expensive.

```java
class Document implements Cloneable {
    String content;
    Document(String content) { this.content = content; }

    @Override
    public Document clone() throws CloneNotSupportedException {
        return (Document) super.clone();
    }
}

Document template = new Document("Standard contract text");
Document copy = template.clone(); // faster than rebuilding from scratch
```

---

## ❓ Question
**What is the Strategy Pattern?**

Defines a **family of interchangeable algorithms**, encapsulated behind a common interface, selectable at runtime.

```java
interface PaymentStrategy { void pay(double amount); }
class CreditCardStrategy implements PaymentStrategy {
    public void pay(double amount) { System.out.println("Paid by card: " + amount); }
}
class UpiStrategy implements PaymentStrategy {
    public void pay(double amount) { System.out.println("Paid by UPI: " + amount); }
}

class Checkout {
    private PaymentStrategy strategy;
    Checkout(PaymentStrategy strategy) { this.strategy = strategy; }
    void process(double amount) { strategy.pay(amount); }
}

new Checkout(new UpiStrategy()).process(500); // swap strategy freely at runtime
```

---

## ❓ Question
**What is the Adapter Pattern?**

Converts one interface into another that a client expects, allowing incompatible classes to work together.

```java
interface MediaPlayer { void play(String fileName); }

class LegacyVlcPlayer { void playVlc(String fileName) { System.out.println("Playing VLC: " + fileName); } }

class VlcAdapter implements MediaPlayer {
    private LegacyVlcPlayer vlcPlayer = new LegacyVlcPlayer();
    public void play(String fileName) {
        vlcPlayer.playVlc(fileName); // adapts the incompatible interface
    }
}
```

---

## ❓ Question
**What is the Decorator Pattern?**

Adds new behavior to an object **dynamically**, by wrapping it, without modifying the original class.

```java
interface Coffee { double cost(); }
class SimpleCoffee implements Coffee { public double cost() { return 2.0; } }

class MilkDecorator implements Coffee {
    private final Coffee coffee;
    MilkDecorator(Coffee coffee) { this.coffee = coffee; }
    public double cost() { return coffee.cost() + 0.5; }
}

Coffee order = new MilkDecorator(new SimpleCoffee());
System.out.println(order.cost()); // 2.5 — behavior extended without touching SimpleCoffee
```

---

## ❓ Question
**What is the Observer Pattern?**

Defines a **one-to-many dependency** so that when one object (subject) changes state, all its dependents (observers) are automatically notified.

```java
interface Observer { void update(String event); }

class EventPublisher {
    private final List<Observer> observers = new ArrayList<>();
    void subscribe(Observer o) { observers.add(o); }
    void publish(String event) {
        for (Observer o : observers) o.update(event); // notify all
    }
}

EventPublisher publisher = new EventPublisher();
publisher.subscribe(event -> System.out.println("Received: " + event));
publisher.publish("New Order Placed");
```

---

## ❓ Question
**What is the MVC (Model-View-Controller) Pattern?**

Separates an application into three interconnected components: **Model** (data/business logic), **View** (presentation), and **Controller** (handles input, coordinates Model and View).

```
Request → Controller → Model (business logic/data)
                ↓
              View (renders response back to user)
```

```java
@Controller
class OrderController {
    @Autowired OrderService orderService; // delegates to Model layer

    @GetMapping("/orders/{id}")
    String getOrder(@PathVariable Long id, Model model) {
        model.addAttribute("order", orderService.findById(id)); // Model
        return "orderView"; // View template name
    }
}
```

---

## ❓ Question
**How do these Design Patterns map to Spring Boot?**

```
Singleton   → Default Spring Bean scope — one shared instance managed by the container
Factory     → @Bean methods in @Configuration classes act as factories
Builder     → Lombok's @Builder, RestTemplate.Builder, UriComponentsBuilder
Strategy    → Interface + multiple @Component implementations, injected by qualifier
Adapter     → HandlerAdapter in Spring MVC adapts different controller types uniformly
Decorator   → Spring AOP proxies "wrap" beans to add cross-cutting behavior (logging, security)
Observer    → Spring's ApplicationEvent / @EventListener mechanism
MVC         → Spring MVC itself: @Controller, Model, and View templates (Thymeleaf/JSP)
```

```java
@Configuration
class AppConfig {
    @Bean // Factory pattern
    PaymentStrategy paymentStrategy() {
        return new UpiStrategy();
    }
}

@Component
class OrderEventListener {
    @EventListener // Observer pattern
    void onOrderPlaced(OrderPlacedEvent event) {
        System.out.println("Order placed: " + event.getOrderId());
    }
}
```

---

# 🎯 40 Interview Questions — Design Patterns

## ❓ Question
**1. (TCS) What problem does the Singleton pattern solve, and what's a common criticism of it?**

It ensures a single shared instance (e.g., for configuration or logging) — but it's criticized for introducing global state, making unit testing harder, and often being overused where dependency injection would be cleaner.

---

## ❓ Question
**2. (Infosys) How would you make a Singleton thread-safe in Java?**

Using an eagerly-initialized `static final` field (thread-safe by class loading guarantees), or double-checked locking with `volatile`, or the "Bill Pugh" static inner holder class idiom.

---

## ❓ Question
**3. (Wipro) What's the difference between Factory Method and Abstract Factory patterns?**

Factory Method creates one type of product via a single creation method (often overridden by subclasses). Abstract Factory provides an interface for creating **families of related** products without specifying their concrete classes.

---

## ❓ Question
**4. (Amazon) Why is the Builder pattern preferred over telescoping constructors?**

Telescoping constructors (many overloaded constructors for different parameter combinations) become unreadable and error-prone as optional parameters grow; Builder offers a clear, self-documenting, fluent alternative.

---

## ❓ Question
**5. (Google) When would you choose Prototype over Factory for object creation?**

When creating an object from scratch is expensive (e.g., involves heavy computation or I/O) but a similar pre-configured instance already exists — cloning it is cheaper than rebuilding it.

---

## ❓ Question
**6. (Accenture) How does the Strategy pattern support the Open-Closed Principle?**

New strategies (algorithms) can be added as new classes implementing the common interface, without modifying the context class that uses them.

---

## ❓ Question
**7. (Cognizant) What's the key difference between Adapter and Decorator patterns?**

Adapter changes an object's **interface** to make it compatible with what a client expects. Decorator keeps the **same interface** but adds new behavior/responsibility to the object.

---

## ❓ Question
**8. (Capgemini) Can the Decorator pattern be chained with multiple decorators?**

Yes — decorators can be stacked (e.g., `new MilkDecorator(new SugarDecorator(new SimpleCoffee()))`), each adding its own layer of behavior around the previous one.

---

## ❓ Question
**9. (Deloitte) What's the risk of the Observer pattern if observers aren't properly unsubscribed?**

Memory leaks — the subject holds references to observers, preventing them from being garbage collected even after they're logically no longer needed (a classic listener leak).

---

## ❓ Question
**10. (Oracle) How does MVC improve testability compared to a monolithic UI+logic class?**

The Model (business logic) can be unit tested independently of the View (presentation), since they're decoupled — you don't need a running UI to verify business rules.

---

## ❓ Question
**11. (Microsoft) Is Singleton compatible with Spring's default Bean scope?**

Yes — Spring beans are singleton-scoped by default, meaning the container itself manages a single shared instance per bean definition, achieving the same goal as the classic Singleton pattern without the boilerplate.

---

## ❓ Question
**12. (IBM) Why is a private constructor essential to the classic Singleton implementation?**

To prevent external code from calling `new` directly, which would create additional instances and break the "exactly one instance" guarantee.

---

## ❓ Question
**13. (HCL) How does Java's enum-based Singleton implementation avoid common Singleton pitfalls?**

Enums are inherently serialization-safe and reflection-attack-resistant (the JVM guarantees only one instance per enum constant), making `enum Singleton { INSTANCE; }` a robust, simpler alternative to manual implementations.

---

## ❓ Question
**14. (Mindtree) What's a real-world Factory pattern example in the JDK itself?**

`Calendar.getInstance()`, `NumberFormat.getInstance()`, and various `valueOf()` methods act as factory methods, returning appropriate implementations without exposing concrete class construction.

---

## ❓ Question
**15. (LTI) How does the Builder pattern interact with immutability?**

The final `build()` step typically constructs a fully-initialized, immutable object in one atomic step — while the builder itself is mutable during the intermediate construction process.

---

## ❓ Question
**16. (Amazon) What's a downside of the Strategy pattern if there are many strategies?**

It can lead to a proliferation of small classes, and the client must know which concrete strategy to instantiate/inject — often mitigated by combining it with Factory or Dependency Injection for strategy selection.

---

## ❓ Question
**17. (Flipkart) Can Adapter be implemented via composition instead of inheritance?**

Yes — "object adapter" style wraps the incompatible class as a field and delegates calls (shown in the topic example), which is generally preferred over "class adapter" style (multiple inheritance via interfaces), since it's more flexible.

---

## ❓ Question
**18. (Paytm) How does Spring AOP's proxy mechanism relate to the Decorator pattern?**

Spring creates a runtime proxy that "wraps" your bean, adding cross-cutting behavior (like `@Transactional` or logging) around method calls — conceptually the same "wrap and extend behavior" idea as the classic Decorator pattern.

---

## ❓ Question
**19. (Zoho) What's the difference between the Observer pattern and simple direct method calls between objects?**

Observer decouples the subject from its observers — the subject doesn't need to know concrete observer types, just that they implement the observer interface, allowing dynamic subscription/unsubscription at runtime.

---

## ❓ Question
**20. (Freshworks) How does MVC differ from MVVM (Model-View-ViewModel)?**

MVC's Controller handles input and coordinates Model/View directly. MVVM introduces a ViewModel that exposes observable state/commands the View binds to directly, often reducing manual Controller-style coordination code (common in frameworks like Angular/WPF).

---

## ❓ Question
**21. (Adobe) Why might Singleton be considered an anti-pattern in highly testable codebases?**

Global shared state makes it hard to isolate tests (state can leak between test runs), and hardcoded `getInstance()` calls bypass dependency injection, making mocking difficult — DI-managed singletons (like Spring beans) address this better.

---

## ❓ Question
**22. (SAP) What's a practical example of Abstract Factory in a cross-platform UI toolkit?**

A `GUIFactory` interface with `WindowsFactory` and MacFactory` implementations, each producing platform-consistent families of related components (`Button`, `Checkbox`) without the client code knowing which platform it's running on.

---

## ❓ Question
**23. (JPMorgan) How would you apply the Strategy pattern to interest calculation for different account types in a banking system?**

Define an `InterestStrategy` interface with implementations like `SavingsInterestStrategy` and `FixedDepositInterestStrategy`, injected into the account processing logic — new account types/rules can be added without modifying existing calculation code.

---

## ❓ Question
**24. (Goldman Sachs) What's a risk of over-applying design patterns in enterprise codebases?**

"Pattern overuse" can add unnecessary abstraction/indirection for simple problems, making code harder to read and navigate — patterns should solve a genuine, recurring design problem, not be applied by default.

---

## ❓ Question
**25. (Morgan Stanley) How does the Prototype pattern handle deep vs. shallow copying concerns?**

The `clone()` implementation must be carefully written to deep-copy mutable fields if true independence between the prototype and its clones is required, same considerations as general object cloning.

---

## ❓ Question
**26. (Infosys) Why is Factory pattern useful for decoupling client code from library upgrades?**

If the concrete implementation class changes or a new one is introduced, only the factory needs updating — client code depending on the abstract product type remains unaffected.

---

## ❓ Question
**27. (Wipro) Can the Builder pattern be combined with the Director concept from the classic GoF definition?**

Yes — a "Director" class can encapsulate common construction sequences using a Builder, standardizing how certain object configurations are built, though modern fluent builders often skip a separate Director for simplicity.

---

## ❓ Question
**28. (TCS) What's a real-world Decorator example in Java's I/O library?**

`BufferedReader`, `InputStreamReader`, and similar stream-wrapping classes are classic Decorator pattern examples — each wraps another stream to add specific functionality (buffering, character decoding) without modifying the wrapped class.

---

## ❓ Question
**29. (Capgemini) How does Spring's `@EventListener` simplify implementing the Observer pattern compared to manual implementation?**

It eliminates the need to manually maintain a list of observers and call them explicitly — Spring's `ApplicationEventPublisher` and annotated listener methods handle registration and notification automatically via the container.

---

## ❓ Question
**30. (Cognizant) What's a common alternative to MVC in modern single-page-application frontends, and how does the backend's role change?**

Frontend frameworks (React/Angular) often own the View and much of the Controller logic client-side; the backend increasingly exposes a Model via REST/GraphQL APIs rather than rendering full server-side Views.

---

## ❓ Question
**31. (Amazon) How would you test a class designed with the Strategy pattern?**

Inject a mock or simple test-double implementation of the strategy interface, verifying the context class correctly delegates to whatever strategy it's given — no need to test all concrete strategies through the context itself.

---

## ❓ Question
**32. (Google) What's the difference between structural patterns (Adapter, Decorator) and behavioral patterns (Strategy, Observer)?**

Structural patterns focus on how classes/objects are **composed** to form larger structures. Behavioral patterns focus on how objects **communicate and distribute responsibility** at runtime.

---

## ❓ Question
**33. (Oracle) Is Singleton compatible with multi-threaded environments without extra synchronization?**

Only if implemented correctly — eager initialization (`static final` field) is inherently thread-safe due to JVM class-loading guarantees; naive lazy initialization without synchronization can create multiple instances under concurrent access.

---

## ❓ Question
**34. (Microsoft) How does the Decorator pattern avoid the "class explosion" problem that subclassing for every feature combination would cause?**

Instead of creating a subclass for every possible combination of features (e.g., `MilkSugarCoffee`, `MilkCoffee`, `SugarCoffee`...), decorators can be composed dynamically at runtime in any combination using a small, fixed set of decorator classes.

---

## ❓ Question
**35. (Deloitte) What's a real-world Adapter pattern example when integrating a legacy system with a modern microservice?**

An adapter class/service translates the legacy system's outdated data format/protocol into the modern API contract expected by new services, without modifying the legacy system itself.

---

## ❓ Question
**36. (Accenture) How does dependency injection reduce the need for a manual Factory pattern in Spring applications?**

The Spring container itself acts as a sophisticated factory, constructing and wiring beans based on configuration/annotations — application code rarely needs to write explicit factory classes for its own managed beans.

---

## ❓ Question
**37. (IBM) What's the relationship between the Template Method pattern and inheritance?**

Template Method defines an algorithm's overall structure in a base class, with specific steps deferred to abstract/overridable methods implemented by subclasses — a direct, intentional use of inheritance for controlled customization.

---

## ❓ Question
**38. (HCL) How would you choose between Strategy and simple `if-else`/`switch` logic for varying behavior?**

Use Strategy when the number of variations is expected to grow, when each variant has meaningful complexity, or when you want to follow OCP; simple conditional logic is fine for a small, stable, unlikely-to-change set of cases.

---

## ❓ Question
**39. (Mindtree) Why is understanding design patterns valuable even if you rarely implement them from scratch?**

They provide a shared vocabulary for discussing design decisions with other engineers, and recognizing them in frameworks (Spring, JDK libraries) makes understanding and using those frameworks significantly easier.

---

## ❓ Question
**40. (LTI) What's a summarized guideline for choosing when to actually apply a design pattern in real projects?**

Apply a pattern when it genuinely solves a recurring, real design problem you're facing (extensibility, decoupling, object creation complexity) — not preemptively "because it's a known pattern," since unnecessary abstraction has its own maintenance cost.

---
