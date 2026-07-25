# Chapter 12: Final

---

## ❓ Question
**What is a `final` Variable?**

A `final` variable can only be assigned **once**. Once initialized, its value (for primitives) or reference (for objects) cannot be changed.

```java
final int MAX_USERS = 100;
// MAX_USERS = 200; // COMPILE ERROR — cannot reassign a final variable

final Car car = new Car();
car.color = "Red";   // allowed — object state can still change
// car = new Car();   // COMPILE ERROR — reference itself is fixed
```

---

## ❓ Question
**What is a `final` Method?**

A `final` method **cannot be overridden** by any subclass. Used when a class wants to lock down specific behavior to guarantee it can't be altered through inheritance.

```java
class Vehicle {
    final void startEngine() {
        System.out.println("Engine starting sequence — cannot be changed");
    }
}

class Car extends Vehicle {
    // void startEngine() { } // COMPILE ERROR — cannot override a final method
}
```

---

## ❓ Question
**What is a `final` Class?**

A `final` class **cannot be extended/subclassed** at all. Used when a class's implementation must remain exactly as-is, often for security or immutability guarantees.

```java
final class Utility {
    static int square(int n) { return n * n; }
}

// class SpecialUtility extends Utility { } // COMPILE ERROR — cannot extend a final class
```

`String`, `Integer`, and other wrapper classes are `final` in the JDK — this is part of what guarantees their immutability and behavioral consistency across the platform.

---

## ❓ Question
**What is a Blank Final Variable?**

A blank final is a `final` field declared **without an initial value** at declaration — it must be assigned exactly once, either in every constructor or in an instance initializer block, before the object is considered constructed.

```java
class Employee {
    final String employeeId; // blank final — no value yet

    Employee(String id) {
        this.employeeId = id; // must be assigned here (or in an initializer block)
    }
}
```

If a constructor path doesn't assign it, that's a compile error — the compiler verifies **every** path guarantees exactly one assignment.

---

## ❓ Question
**How is `final` used to create Constants?**

Combining `static` and `final` creates a true **compile-time constant** — shared across all instances and unchangeable, conventionally named in `UPPER_SNAKE_CASE`.

```java
class MathConstants {
    static final double PI = 3.14159;
    static final int MAX_RETRIES = 3;
}

double area = MathConstants.PI * radius * radius;
```

```
static       → one shared copy at the class level (not per object)
final        → cannot be reassigned once set
static final → the standard pattern for true constants
```

---

# 🎯 40 Interview Questions — Final

## ❓ Question
**1. (TCS) Can a `final` variable be initialized in a constructor instead of at declaration?**

Yes — this is exactly the "blank final" pattern, as long as every constructor assigns it exactly once.

---

## ❓ Question
**2. (Infosys) Can a `final` method be overloaded?**

Yes — `final` only prevents overriding; overloading (same name, different parameters) is unaffected and fully allowed.

---

## ❓ Question
**3. (Wipro) Can an abstract class be `final`?**

No — this is contradictory: `abstract` requires subclassing to be usable, but `final` forbids subclassing entirely; combining them is a compile error.

---

## ❓ Question
**4. (Amazon) Is a `final` reference variable's object immutable?**

No — `final` only locks the **reference**, not the object's internal state, unless the object's own class is separately designed to be immutable.

---

## ❓ Question
**5. (Google) Why are wrapper classes like `Integer` and `String` declared `final`?**

To guarantee immutability and prevent subclasses from breaking assumptions the JVM and libraries rely on (like caching, hashing behavior, and security in contexts like classloading).

---

## ❓ Question
**6. (Accenture) Can a `final` variable be a loop counter?**

No — a variable reassigned on each iteration (like a typical `for` loop counter) cannot be `final`, since `final` allows only a single assignment.

---

## ❓ Question
**7. (Cognizant) Can method parameters be declared `final`?**

Yes — `final` parameters cannot be reassigned within the method body, which is often used to signal (and enforce) that a parameter's value shouldn't be mutated during the method's logic.

---

## ❓ Question
**8. (Capgemini) What is "effectively final" in the context of lambdas?**

A local variable that is never reassigned after initialization, even without the `final` keyword — Java requires variables captured by lambdas/anonymous classes to be final or effectively final.

---

## ❓ Question
**9. (Deloitte) Can `final` fields be modified via reflection?**

Technically yes, using `Field.setAccessible(true)` combined with reflection tricks (and removing the `final` modifier flag in older JDKs) — though this is considered a hack and is increasingly restricted in modern JVMs for safety.

---

## ❓ Question
**10. (Oracle) Does declaring a variable `final` improve runtime performance?**

Not significantly by itself — the main benefit is compile-time safety and clearer intent; some JIT optimizations may benefit slightly, but it's not the primary reason to use `final`.

---

## ❓ Question
**11. (Microsoft) Can a `final` class implement an interface?**

Yes — `final` only restricts a class from being extended; it can still freely implement any number of interfaces.

---

## ❓ Question
**12. (IBM) What happens if you don't initialize a blank final field in one of several constructors?**

Compile-time error — the compiler requires that every possible constructor path assigns the blank final field exactly once.

---

## ❓ Question
**13. (HCL) Can a `final` static field be initialized in an instance constructor?**

No — a `static final` field must be initialized either at declaration or within a **static block**, not an instance constructor, since it belongs to the class, not any instance.

---

## ❓ Question
**14. (Mindtree) Why might you make a method `final` in a security-sensitive class?**

To prevent subclasses from overriding critical logic (e.g., authentication or validation checks) in a way that could bypass or weaken the original security guarantees.

---

## ❓ Question
**15. (LTI) Can `final` variables be part of switch-case constant expressions?**

Yes, but only if they are `static final` **compile-time constants** (primitives or Strings with constant values) — a non-constant final variable can't be used as a case label.

---

## ❓ Question
**16. (Amazon) Is `final` the same as `const` in other languages like C++?**

Conceptually similar for variables, but Java has no true `const` keyword; `final` is more limited — it doesn't provide deep immutability for objects the way `const` correctness can in C++.

---

## ❓ Question
**17. (Flipkart) Can you declare an array as `final` and still modify its elements?**

Yes — `final` fixes the reference to the array object, but individual elements inside the array can still be changed freely.

---

## ❓ Question
**18. (Paytm) Can constructors be declared `final`?**

No — `final` on constructors is meaningless/not allowed, since constructors are never inherited or overridden in the first place.

---

## ❓ Question
**19. (Zoho) What's the benefit of marking instance fields `final` in an immutable class design?**

It guarantees the field is set exactly once during construction and never changes afterward, which is a core requirement for building truly immutable objects.

---

## ❓ Question
**20. (Freshworks) Can a `final` variable be assigned inside an instance initializer block instead of the constructor?**

Yes — instance initializer blocks run before the constructor body and are a valid place to assign a blank final field exactly once.

---

## ❓ Question
**21. (Adobe) Why does making a class `final` sometimes improve JIT optimization?**

Because the JVM knows no subclass can override its methods, it can more safely inline method calls and avoid virtual dispatch overhead in some cases.

---

## ❓ Question
**22. (SAP) Can you have a `final` interface?**

No — interfaces are meant to be implemented, and `final` would forbid that, so it's not a valid modifier combination for interfaces.

---

## ❓ Question
**23. (JPMorgan) What is the difference between compile-time constants and `final` runtime-assigned variables?**

A compile-time constant (`static final` primitive/String with a literal value) is inlined directly into bytecode wherever used. A `final` variable assigned at runtime (e.g., from a method call) is not inlined and is evaluated normally at execution time.

---

## ❓ Question
**24. (Goldman Sachs) Why can inlined compile-time constants cause version-mismatch bugs?**

If Class A references a `static final` constant from Class B, and B's constant value changes later, A must be **recompiled** — otherwise A still uses the old inlined value from when it was compiled, silently going stale.

---

## ❓ Question
**25. (Morgan Stanley) Can `final` be applied to enum values?**

Enum constants are already implicitly `static final`; you don't (and can't) explicitly add `final` to them again.

---

## ❓ Question
**26. (Infosys) Does `final` prevent an object from being garbage collected?**

No — `final` has nothing to do with reachability/GC eligibility; it only controls reassignment of the variable itself.

---

## ❓ Question
**27. (Wipro) Can you override a `final` method from a class in a completely different package?**

No — `final` prevents overriding universally, regardless of the subclass's package or access level.

---

## ❓ Question
**28. (TCS) What is the purpose of `final` in a try-with-resources variable?**

Resources declared in a try-with-resources statement are implicitly final (or effectively final); they can't be reassigned within the try block, ensuring the same resource that was opened is the one that gets closed.

---

## ❓ Question
**29. (Capgemini) Can a `final` local variable be declared but assigned later, conditionally?**

Yes, as long as the compiler can prove it's assigned exactly once along every possible execution path before use — e.g., in both branches of an if-else, but not in only one.

---

## ❓ Question
**30. (Cognizant) Why is `String` immutability tied to being `final`?**

Because if `String` could be subclassed, a subclass could override methods to violate the immutability contract (e.g., changing internal char arrays), breaking assumptions the JVM/security model relies on (like safe use as HashMap keys or in security contexts).

---

## ❓ Question
**31. (Amazon) Can you use `final` to prevent method hiding of static methods?**

No — `final` doesn't apply meaningfully to prevent hiding; static methods are hidden (not overridden) regardless of `final`, though marking a static method `final` does still prevent a subclass from declaring the same signature.

---

## ❓ Question
**32. (Google) Does the JVM enforce `final` restrictions at runtime, or only at compile time?**

Primarily at compile time via the Java compiler; the JVM bytecode verifier also performs some checks, but the main enforcement (like preventing subclassing of a final class) happens during compilation.

---

## ❓ Question
**33. (Oracle) Can a record's components be non-final?**

No — record components are always implicitly `private final`; there's no way to make them mutable, since immutability is core to what records are designed for.

---

## ❓ Question
**34. (Microsoft) What's the relationship between `final` and thread safety?**

Properly published `final` fields have special guarantees under the Java Memory Model — once a constructor finishes, other threads are guaranteed to see the fully initialized final field value, which helps avoid certain visibility bugs without extra synchronization.

---

## ❓ Question
**35. (Deloitte) Can a `final` class have `static` methods?**

Yes — `final` only restricts inheritance of the class; it has no effect on whether the class can contain static (or instance) methods, which work exactly as normal.

---

## ❓ Question
**36. (Accenture) What's a real-world example of using `final` for defensive design in Spring Boot?**

Marking injected dependency fields (`private final SomeService service;`) so they must be set via constructor injection, guaranteeing they're never null and never reassigned after bean construction.

---

## ❓ Question
**37. (IBM) Can you declare a `final` variable without ever using it?**

Yes — Java allows unused final variables; it may produce a compiler warning in some tools/IDEs, but it's not a compile error.

---

## ❓ Question
**38. (HCL) Why might overusing `final` on every local variable be considered excessive in typical code style?**

While it can slightly improve clarity/immutability intent, applying it everywhere adds visual noise; most teams reserve explicit `final` for genuinely important immutability signals (like constructor parameters in DI) rather than every local variable.

---

## ❓ Question
**39. (Mindtree) Can you have a `final` variable of a mutable custom class and still violate encapsulation?**

Yes — if that class exposes public setters or mutable internal collections, external code can still change the object's state through those methods even though the reference itself is `final`.

---

## ❓ Question
**40. (LTI) How does `final` support the Builder design pattern?**

The object being built often has `final` fields set only once inside the builder's `build()` method (via the target object's constructor), ensuring the final constructed object is safely immutable once returned.

---
