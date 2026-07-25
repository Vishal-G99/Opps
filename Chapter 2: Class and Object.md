
---
 
## ❓ Question
**What is a Class?**
 
A class is a **blueprint or template** used to create objects. It defines the structure (fields/variables) and behavior (methods) that its objects will have. A class itself does not occupy memory for data — it only exists as a template until an object is created from it.
 
```java
class Car {
    // fields (state)
    String color;
    String model;
    int speed;
 
    // methods (behavior)
    void accelerate() {
        speed += 10;
    }
 
    void brake() {
        speed -= 10;
    }
}
```
 
Think of it like an architect's **house blueprint** — the blueprint isn't a house you can live in, but every house built from it follows the same design.
 
---
 
## ❓ Question
**What is an Object?**
 
An object is a **runtime instance of a class**. It is a real entity that has:
- **State** — values stored in fields
- **Behavior** — actions defined by methods
- **Identity** — a unique memory address distinguishing it from other objects
```java
Car myCar = new Car();
myCar.color = "Red";
myCar.model = "Tesla";
```
 
Here, `myCar` is an object — an actual "house" built from the `Car` blueprint, with its own color and model.
 
---
 
## ❓ Question
**How is an Object Created?**
 
Java provides **4 primary ways** to create an object:
 
**1. Using `new` keyword (most common)**
```java
Car car1 = new Car();
```
 
**2. Using `Class.newInstance()` / `Constructor.newInstance()` (Reflection)**
```java
Car car2 = Car.class.getDeclaredConstructor().newInstance();
```
 
**3. Using `clone()`**
```java
Car car3 = (Car) car1.clone(); // Car must implement Cloneable
```
 
**4. Using Deserialization**
```java
ObjectInputStream in = new ObjectInputStream(new FileInputStream("car.ser"));
Car car4 = (Car) in.readObject();
```
 
`new` is the standard approach — it allocates memory on the heap, calls the constructor, and returns a reference to the newly created object.
 
---
 
## ❓ Question
**What is the Object Lifecycle in Java?**
 
An object goes through distinct phases from birth to death:
 
```
1. Creation      → memory allocated on heap via `new`
2. Initialization → constructor runs, fields get values
3. In Use        → object is referenced and manipulated by code
4. Unreachable   → no more references point to it
5. Garbage Collected → JVM reclaims the memory
```
 
```java
Car car = new Car();   // 1 & 2: Created + Initialized
car.accelerate();      // 3: In use
car = null;             // 4: Now unreachable (if no other refs)
// 5: Eligible for GC — reclaimed at JVM's discretion
```
 
---
 
## ❓ Question
**What is Object Identity?**
 
Object identity is what makes each object **unique**, even if two objects have identical field values. It is tied to the object's memory location (reference), not its data.
 
```java
Car a = new Car();
Car b = new Car();
 
a.model = "Tesla";
b.model = "Tesla";
 
System.out.println(a == b);        // false → different identity (different objects)
System.out.println(a.equals(b));   // false by default (Object.equals compares identity too)
```
 
Even though `a` and `b` have the same state, they are **two separate objects** in memory. `hashCode()` (default implementation) is often derived from this identity.
 
---
 
## ❓ Question
**What is Object State?**
 
Object state refers to the **values stored in an object's instance variables (fields)** at a given moment. State can change over the object's lifetime.
 
```java
Car car = new Car();
car.speed = 0;      // initial state
car.accelerate();
car.accelerate();
System.out.println(car.speed); // state changed to 20
```
 
Each object maintains its **own copy** of instance variables — that's why two `Car` objects can have different `speed` values simultaneously.
 
---
 
## ❓ Question
**What is Object Behavior?**
 
Object behavior refers to the **actions an object can perform**, defined through its methods. Behavior often operates on and changes the object's state.
 
```java
class Car {
    int speed;
 
    void accelerate() {   // behavior
        speed += 10;
    }
 
    void displaySpeed() {  // behavior
        System.out.println("Speed: " + speed);
    }
}
```
 
State = "what the object knows." Behavior = "what the object can do."
 
---
 
## ❓ Question
**What is Heap Memory?**
 
Heap memory is the region of JVM memory where **all objects and their instance variables are stored**. It is shared across all threads and managed by the Garbage Collector.
 
```
Heap Memory
┌─────────────────────────────┐
│  Car@1a2b  { color:"Red" }  │
│  Car@3c4d  { color:"Blue" } │
│  Dog@5e6f  { name:"Rex" }   │
└─────────────────────────────┘
```
 
- Objects live here until garbage collected.
- Larger and slower to access than stack memory.
- Divided into generations: **Young Generation (Eden, S0, S1)**, **Old/Tenured Generation**, and (in older JVMs) **PermGen**, now replaced by **Metaspace**.
---
 
## ❓ Question
**What is Stack Memory?**
 
Stack memory stores **method call frames**, local variables, and **reference variables** — not the objects themselves. Each thread has its own private stack.
 
```
Stack Memory (per thread)
┌───────────────────────────┐
│ main()                    │
│   car  ──► points to heap │
│   speed = 10  (primitive) │
└───────────────────────────┘
```
 
- Fast access, LIFO (Last-In-First-Out) structure.
- Automatically cleaned up when a method returns.
- Stores primitives directly; stores only the **reference (pointer)** for objects.
---
 
## ❓ Question
**What is a Reference Variable?**
 
A reference variable holds the **memory address (pointer)** of an object stored in heap memory. It does not hold the object itself.
 
```java
Car myCar = new Car();
```
 
```
Stack                 Heap
┌───────────┐        ┌─────────────────┐
│ myCar ────┼───────►│ Car object       │
└───────────┘        │ color: null      │
                      │ speed: 0         │
                      └─────────────────┘
```
 
Multiple reference variables can point to the **same** object:
 
```java
Car a = new Car();
Car b = a;   // b now points to the same object as a
b.color = "Green";
System.out.println(a.color); // "Green" — because a and b share the same object
```
 
---
 
## ❓ Question
**What is an Anonymous Object?**
 
An anonymous object is an object created **without assigning it to a reference variable**. It's used once and then becomes eligible for garbage collection immediately.
 
```java
new Car().accelerate();          // anonymous object, used once
 
System.out.println(new Car().speed); // no reference stored
```
 
Common in method chaining, one-off utility calls, and anonymous inner classes:
```java
new Thread(() -> System.out.println("Running")).start();
```
 
---
 
## ❓ Question
**What is Garbage Collection (GC)?**
 
Garbage Collection is the JVM's automatic process of **reclaiming heap memory** occupied by objects that are no longer reachable from any live reference.
 
```java
Car car = new Car();
car = null; // object now unreachable → eligible for GC
```
 
**Key points:**
- Java uses **generational GC** — most objects die young (Young Gen), so it's collected frequently and cheaply there.
- Common GC algorithms: **Serial, Parallel, CMS, G1 (default since Java 9), ZGC, Shenandoah**.
- You cannot force GC — `System.gc()` is only a *suggestion* to the JVM.
- An object becomes eligible for GC when it has **zero reachable references** (ignoring cycles, which GC also handles via reachability analysis, not simple reference counting).
```
Young Gen                 Old Gen
┌────────────┐           ┌────────────┐
│ Eden│S0│S1 │  ──promote─►│  Tenured  │
└────────────┘           └────────────┘
  Minor GC (fast)          Major/Full GC (slower)
```
 
---
 
## ❓ Question
**What is the step-by-step JVM Object Creation Process?**
 
When you write `Car car = new Car();`, the JVM performs these steps internally:
 
```
Step 1: Class Loading
   → JVM checks if Car.class is loaded; if not, ClassLoader loads it.
 
Step 2: Memory Allocation
   → JVM allocates memory for the object on the Heap (size based on fields).
 
Step 3: Default Initialization
   → Fields get default values (0, null, false) — NOT constructor values yet.
 
Step 4: Constructor Invocation
   → Instance initializers run, then the constructor body executes,
     assigning actual values to fields.
 
Step 5: Reference Assignment
   → The memory address of the newly created object is returned
     and stored in the reference variable (on the stack).
```
 
```java
class Car {
    int speed;              // default: 0 (Step 3)
    Car() {
        speed = 100;         // actual value (Step 4)
    }
}
 
Car car = new Car(); // Step 1 → 2 → 3 → 4 → 5
```
 
---
 
## ❓ Question
**Can you show a Practical End-to-End Example combining Class, Object, State, Behavior, and Memory?**
 
```java
class BankAccount {
    String owner;      // state
    double balance;     // state
 
    BankAccount(String owner, double balance) {
        this.owner = owner;
        this.balance = balance;
    }
 
    void deposit(double amount) {   // behavior
        balance += amount;
    }
 
    void withdraw(double amount) {  // behavior
        if (amount <= balance) balance -= amount;
    }
 
    void printStatement() {
        System.out.println(owner + "'s balance: " + balance);
    }
}
 
public class Main {
    public static void main(String[] args) {
        BankAccount acc1 = new BankAccount("Alice", 1000); // heap object #1
        BankAccount acc2 = new BankAccount("Bob", 500);     // heap object #2
 
        acc1.deposit(200);
        acc2.withdraw(100);
 
        acc1.printStatement(); // Alice's balance: 1200
        acc2.printStatement(); // Bob's balance: 400
 
        acc1 = null; // acc1's object becomes unreachable → eligible for GC
    }
}
```
 
---
 
## ❓ Question
**Can you show a Memory Diagram for two objects with shared and independent references?**
 
```java
BankAccount acc1 = new BankAccount("Alice", 1000);
BankAccount acc2 = new BankAccount("Bob", 500);
BankAccount acc3 = acc1; // shares the same object as acc1
```
 
```
STACK                          HEAP
┌────────────┐                ┌───────────────────────────┐
│ acc1 ──────┼───────────────►│ BankAccount@0x100          │
│            │        ┌──────►│  owner: "Alice"            │
│ acc3 ──────┼────────┘       │  balance: 1000              │
│            │                └───────────────────────────┘
│ acc2 ──────┼───────────────►┌───────────────────────────┐
└────────────┘                │ BankAccount@0x200          │
                               │  owner: "Bob"               │
                               │  balance: 500                │
                               └───────────────────────────┘
```
 
If `acc1.deposit(200)` is called, `acc3.balance` also reflects `1200` — because `acc1` and `acc3` reference the **same** heap object.
 
---
 
## ❓ Question
**How does Spring Boot manage object creation? (Bean Lifecycle)**
 
In plain Java, **you** create objects using `new`. In Spring Boot, the **IoC (Inversion of Control) container** creates and manages objects — called **Beans** — for you.
 
```java
@Component
class CarService {
    void drive() {
        System.out.println("Driving...");
    }
}
 
@RestController
class CarController {
    private final CarService carService;
 
    // Spring creates CarService object and INJECTS it here — you never call `new`
    @Autowired
    CarController(CarService carService) {
        this.carService = carService;
    }
 
    @GetMapping("/drive")
    String drive() {
        carService.drive();
        return "Done";
    }
}
```
 
**What Spring does internally (simplified):**
```
1. Component Scan   → finds classes annotated @Component/@Service/@Repository
2. Bean Definition  → registers metadata (scope, dependencies) in ApplicationContext
3. Object Creation  → uses reflection to call the constructor (like new, but automated)
4. Dependency Injection → wires required beans into constructors/fields
5. Bean Ready       → object stored as a Singleton (default) in the ApplicationContext
```
 
By default, Spring Beans are **Singletons** — only **one object** is created per bean definition and reused everywhere it's needed (unlike plain Java where every `new` creates a fresh object).
 
```java
@Bean
@Scope("prototype")   // opt-in to a NEW object every time it's requested
CarService carService() {
    return new CarService();
}
```
 
---
 
## ❓ Question
**How does Spring Boot's ApplicationContext relate to Object Lifecycle?**
 
Spring extends the normal Java object lifecycle with its own hooks:
 
```
1. Instantiation        → Spring calls constructor (via reflection)
2. Populate Properties  → @Autowired dependencies injected
3. @PostConstruct        → custom init logic runs
4. Bean in Use           → available via ApplicationContext
5. @PreDestroy            → cleanup logic runs before shutdown
6. Bean Destroyed         → removed from container (on context close)
```
 
```java
@Component
class CacheService {
    @PostConstruct
    void init() {
        System.out.println("Cache warmed up!"); // after object + DI, before use
    }
 
    @PreDestroy
    void cleanup() {
        System.out.println("Cache cleared!"); // before app shutdown
    }
}
```
 
---
 
# 🎯 30 Interview Questions with Answers
 
## ❓ Question
**1. What is the difference between a class and an object?**
 
A class is a blueprint/definition; it consumes no heap memory for instance data. An object is a runtime instance created from that class, occupying heap memory with actual state.
 
---
 
## ❓ Question
**2. Can a class exist without any object?**
 
Yes. A class can be defined and even have static members used without ever instantiating an object, e.g., `Math.max(3, 5)` — no `Math` object is ever created.
 
---
 
## ❓ Question
**3. Can an object exist without a class?**
 
No. In Java, every object must be an instance of some class (even anonymous classes and lambdas are backed by a generated class).
 
---
 
## ❓ Question
**4. What is the difference between `==` and `.equals()` for objects?**
 
`==` compares **reference identity** (do both variables point to the same memory address?). `.equals()` compares **logical equality**, which by default (in `Object`) also checks identity, but is commonly overridden (e.g., in `String`, `Integer`) to compare actual content/state.
 
---
 
## ❓ Question
**5. Where are objects stored in memory — stack or heap?**
 
Objects (and their instance fields) are always stored on the **heap**. Reference variables pointing to those objects, along with local primitives, live on the **stack**.
 
---
 
## ❓ Question
**6. What happens to local variables when a method returns?**
 
The entire stack frame for that method is popped, so local variables (including reference variables) are destroyed immediately. If they were the last reference to a heap object, that object becomes eligible for GC.
 
---
 
## ❓ Question
**7. What triggers Garbage Collection?**
 
An object becomes eligible for GC when it is **unreachable** — no live thread can access it through any chain of references from GC roots (stack variables, static fields, active threads, JNI references). The JVM decides *when* to actually run GC.
 
---
 
## ❓ Question
**8. Can you force Garbage Collection in Java?**
 
Not directly. `System.gc()` only **requests** garbage collection; the JVM may ignore it. There is no guaranteed way to force GC.
 
---
 
## ❓ Question
**9. What is the default value of an object reference field before initialization?**
 
`null`. Instance reference fields default to `null` if not explicitly initialized in the constructor or field declaration.
 
---
 
## ❓ Question
**10. What is the difference between instance variables and local variables?**
 
Instance variables belong to an object, live on the heap, get default values automatically, and persist as long as the object exists. Local variables live on the stack, must be explicitly initialized before use, and disappear when the method exits.
 
---
 
## ❓ Question
**11. What is an anonymous object, and when would you use one?**
 
An object created without storing its reference, e.g., `new Car().accelerate();`. Useful for one-time operations where you don't need to reuse the object afterward — it's immediately eligible for GC after the statement executes.
 
---
 
## ❓ Question
**12. What is object identity vs object equality?**
 
Identity = same object in memory (`==` true). Equality = same logical content (`.equals()` true), even if they're different objects in memory. Two objects can be equal but not identical.
 
---
 
## ❓ Question
**13. Why is `hashCode()` related to object identity?**
 
By default, `Object.hashCode()` derives a value based on the object's memory address/internal identity, ensuring distinct objects (usually) get distinct hash codes — unless overridden to be based on state (as `String` and value-based classes do).
 
---
 
## ❓ Question
**14. What is the difference between shallow copy and deep copy?**
 
A shallow copy (default `clone()`) copies field values as-is — reference fields still point to the **same** nested objects as the original. A deep copy also clones the nested objects themselves, so the copy is fully independent.
 
---
 
## ❓ Question
**15. What are the generations in heap memory, and why do they exist?**
 
Young Generation (Eden + Survivor spaces) and Old/Tenured Generation. This split exists because most objects die young (the "weak generational hypothesis"), so frequent, cheap Minor GCs on Young Gen handle most garbage, while infrequent Major/Full GCs handle long-lived objects in Old Gen.
 
---
 
## ❓ Question
**16. What is a memory leak in Java, if GC is automatic?**
 
A memory leak happens when objects are no longer needed but are still **reachable** (e.g., held in a static collection, unclosed listener, or cache), so GC can't reclaim them even though the program logically doesn't need them anymore.
 
---
 
## ❓ Question
**17. What is the difference between Stack Overflow and Out of Memory errors?**
 
`StackOverflowError` occurs when the call stack exceeds its size limit (commonly from deep/infinite recursion). `OutOfMemoryError` occurs when the heap cannot allocate more space for new objects (too many live objects, memory leak, or heap set too small).
 
---
 
## ❓ Question
**18. Why are Strings treated specially in memory (String Pool)?**
 
Java maintains a **String Constant Pool** (part of heap in modern JVMs) to reuse identical string literals, saving memory. `String s1 = "abc"; String s2 = "abc";` — both point to the same pooled object, while `new String("abc")` forces a new heap object outside the pool.
 
---
 
## ❓ Question
**19. What is the difference between `new String("abc")` and `"abc"`?**
 
`"abc"` (a literal) is placed in/retrieved from the String Pool and reused. `new String("abc")` always creates a **new** object on the heap, separate from the pool, even though its content is equal.
 
---
 
## ❓ Question
**20. Can two reference variables point to the same object? What happens if you modify through one?**
 
Yes. `Car b = a;` makes `b` point to the same heap object as `a`. Any state change made via `b` is visible via `a` too, since there's only **one** underlying object.
 
---
 
## ❓ Question
**21. What is the role of a constructor in object creation?**
 
A constructor initializes an object's state right after memory allocation and default field initialization. It runs exactly once per object creation and can never be called directly like a regular method — only via `new` (or reflection).
 
---
 
## ❓ Question
**22. What is the difference between object creation and object initialization?**
 
Creation = memory allocation on the heap (Step 2 of JVM process) with default field values. Initialization = assigning actual values via instance initializer blocks and the constructor body (Step 4). Creation always precedes initialization.
 
---
 
## ❓ Question
**23. How does the JVM decide where an object's class metadata is stored?**
 
Class metadata (method bytecode, static fields, runtime constant pool) is stored in the **Metaspace** (native memory, replacing PermGen since Java 8) — separate from the heap, which stores only object instances.
 
---
 
## ❓ Question
**24. What is escape analysis, and how does it affect object allocation?**
 
Escape analysis is a JIT compiler optimization that determines whether an object's reference "escapes" the method it's created in. If it doesn't escape, the JVM may allocate it on the **stack** (or eliminate the allocation entirely / use scalar replacement) instead of the heap, improving performance.
 
---
 
## ❓ Question
**25. Why does Spring Boot use Singleton scope for beans by default?**
 
To avoid the overhead of creating a new object for every request and to allow shared, stateless services to be reused efficiently across the application, reducing memory footprint and object-creation cost.
 
---
 
## ❓ Question
**26. What is the difference between `@Component` and manually calling `new`?**
 
`new` creates an object directly and immediately in your code, with you managing its lifecycle. `@Component` registers a class with Spring's IoC container, which creates, wires (injects dependencies into), and manages the object's lifecycle (including destruction) for you.
 
---
 
## ❓ Question
**27. Can a Spring Bean be a `prototype` scope instead of `singleton`? What's the effect?**
 
Yes, via `@Scope("prototype")`. Every time the bean is requested from the container, a **brand-new object** is created (like calling `new` each time), instead of reusing one shared instance.
 
---
 
## ❓ Question
**28. What is the difference between `@PostConstruct` and a constructor in Spring?**
 
The constructor runs during object instantiation, often before all dependencies are guaranteed to be injected (especially with field injection). `@PostConstruct` runs **after** the object is fully constructed and all dependencies are injected — the right place for initialization logic that depends on injected beans.
 
---
 
## ❓ Question
**29. If an object has no references but is part of a reference cycle (A references B, B references A), is it garbage collected?**
 
Yes. Java's GC uses **reachability analysis** from GC roots, not simple reference counting. If neither A nor B is reachable from any GC root, both are eligible for collection even though they reference each other.
 
---
 
## ❓ Question
**30. What is the difference between `final` reference variable immutability and object immutability?**
 
A `final` reference variable means the **reference itself** cannot be reassigned to point to a different object — but the object it points to can still have its internal state mutated (unless the object's class is itself designed to be immutable, like `String`).
 
```java
final Car car = new Car();
car.color = "Red";     // allowed — object state changes
car = new Car();       // COMPILE ERROR — reference reassignment not allowed
```
 
