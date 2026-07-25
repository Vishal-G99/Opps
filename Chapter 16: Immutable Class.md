# Chapter 16: Immutable Class

---

## ❓ Question
**How does `String` demonstrate immutability?**

Once a `String` is created, its internal character data can never change. Any operation that "modifies" a String actually returns a **new** String object.

```java
String s1 = "Hello";
String s2 = s1.concat(" World");

System.out.println(s1); // "Hello" — unchanged
System.out.println(s2); // "Hello World" — a brand-new String object
```

```
s1 ──► "Hello" (heap, unchanged)
s2 ──► "Hello World" (new heap object, s1 never touched)
```

This immutability is why Strings are safe as `HashMap` keys, safe to share across threads, and cacheable in the String Pool.

---

## ❓ Question
**How do Wrapper Classes demonstrate immutability?**

`Integer`, `Long`, `Double`, `Boolean`, etc. are all immutable — once created, their wrapped value can never change.

```java
Integer a = 100;
Integer b = a;
a = a + 1; // creates a NEW Integer object; doesn't mutate the original

System.out.println(a); // 101
System.out.println(b); // 100 — completely unaffected
```

This is why wrapper classes are also `final` (cannot be subclassed to break the immutability guarantee).

---

## ❓ Question
**How do you build a Custom Immutable Class?**

Follow these rules:

```
1. Declare the class as `final`             → prevents subclassing that could add mutability
2. Make all fields `private final`           → set once, never reassigned
3. No setter methods                         → no way to change state after construction
4. Initialize all fields via the constructor  → object is fully valid from creation
5. For mutable field types, return DEFENSIVE COPIES from getters (not the actual reference)
```

```java
import java.util.Date;

final class Employee {
    private final String name;
    private final Date joiningDate; // Date is mutable — needs defensive copying

    public Employee(String name, Date joiningDate) {
        this.name = name;
        this.joiningDate = new Date(joiningDate.getTime()); // defensive copy IN
    }

    public String getName() {
        return name;
    }

    public Date getJoiningDate() {
        return new Date(joiningDate.getTime()); // defensive copy OUT
    }
}
```

Without the defensive copies, external code could do `employee.getJoiningDate().setTime(...)` and silently mutate the "immutable" object's internal state.

---

## ❓ Question
**Why does Immutability improve Thread Safety?**

An immutable object's state can never change after construction, so **multiple threads can safely share and read it concurrently** without any synchronization, locks, or risk of race conditions.

```java
final class Point {
    private final int x, y;
    public Point(int x, int y) { this.x = x; this.y = y; }
    public int getX() { return x; }
    public int getY() { return y; }
}

// Safe to share across any number of threads — no thread can ever
// see a partially-updated or inconsistent Point, because it never changes
static final Point ORIGIN = new Point(0, 0);
```

If a "change" is needed, a new immutable instance is simply created and used instead — no in-place mutation ever happens, eliminating an entire category of concurrency bugs.

---

## ❓ Question
**What are Best Practices for designing Immutable Classes?**

- Mark the class `final` (or use private constructors + static factory methods if subclassing control is handled differently).
- Keep **all** fields `private final`.
- Never expose mutable internal fields (collections, `Date`, arrays) directly — return unmodifiable views or defensive copies.
- Prefer immutable field types themselves where possible (`String`, wrapper types, other immutable classes, `LocalDate` instead of `Date`).
- For collections, use `List.copyOf(...)` or `Collections.unmodifiableList(...)` when exposing them.
- Consider using Java `record` types (Java 16+) for simple immutable data carriers — they handle most of this boilerplate automatically.

```java
import java.util.List;

final class Team {
    private final String name;
    private final List<String> members;

    public Team(String name, List<String> members) {
        this.name = name;
        this.members = List.copyOf(members); // immutable defensive copy
    }

    public List<String> getMembers() {
        return members; // already unmodifiable — safe to return directly
    }
}
```

---

# 🎯 40 Interview Questions — Immutable Class

## ❓ Question
**1. (TCS) What are the five core rules for creating an immutable class?**

Make the class final, make all fields private and final, provide no setters, initialize everything via the constructor, and defensively copy mutable fields on both input and output.

---

## ❓ Question
**2. (Infosys) Why must an immutable class be `final` (or otherwise prevent subclassing)?**

To stop a subclass from adding mutable state or overriding methods in a way that breaks the immutability contract the base class guarantees.

---

## ❓ Question
**3. (Wipro) Is every class with only `final` fields automatically immutable?**

No — if any `final` field holds a reference to a **mutable** object (like a `Date` or `List`), the object it points to can still be changed internally, breaking true immutability unless defensive copies are used.

---

## ❓ Question
**4. (Amazon) Why is `String` concatenation with `+` in a loop considered inefficient?**

Each concatenation creates a brand-new `String` object (since Strings are immutable), so repeated concatenation in a loop creates many discarded intermediate objects — `StringBuilder` is preferred for that use case.

---

## ❓ Question
**5. (Google) How does immutability enable safe use as `HashMap` keys?**

Since an immutable object's `hashCode()` never changes after insertion, it will always be found in the same hash bucket — a mutable key's hash could change after insertion, "losing" the entry.

---

## ❓ Question
**6. (Accenture) What is a defensive copy, and why is it necessary in immutable class design?**

A copy of a mutable object made specifically to prevent external code from holding a reference that could later mutate the immutable class's internal state — necessary both when receiving mutable objects in the constructor and when returning them via getters.

---

## ❓ Question
**7. (Cognizant) Can an immutable class have mutable instance fields internally, as long as they're never exposed?**

In principle yes, if genuinely never exposed or mutated after construction — but this is risky and fragile; the safer, standard approach is to avoid mutable field types entirely or always defensively copy.

---

## ❓ Question
**8. (Capgemini) Why are Java `record` types well suited for immutability?**

Records automatically generate `private final` fields, a canonical constructor, accessor methods, and correct `equals()`/`hashCode()`/`toString()` — implementing most immutable class rules by default, though you must still add defensive copying manually for mutable component types.

---

## ❓ Question
**9. (Deloitte) Does immutability guarantee an object is also `final`-safe from reflection-based mutation?**

No — reflection can bypass `private`/`final` restrictions in some cases (via `setAccessible(true)`), meaning immutability is a language/design-level guarantee, not an absolute security boundary.

---

## ❓ Question
**10. (Oracle) Why is `LocalDate` preferred over `Date` in modern immutable class design?**

`LocalDate` (and the rest of `java.time`) is genuinely immutable, unlike the legacy mutable `Date` class, eliminating the need for manual defensive copying when using it as a field.

---

## ❓ Question
**11. (Microsoft) Can an immutable object's `hashCode()` be safely cached/computed once?**

Yes — since the object's state never changes, its hash code is guaranteed to stay constant, so it can be computed once (often lazily on first access) and reused, which `String` actually does internally for performance.

---

## ❓ Question
**12. (IBM) What happens if you try to add an element to a `List` returned by `Collections.unmodifiableList()`?**

It throws `UnsupportedOperationException` at runtime — the list is a read-only wrapper view, though note the underlying original list could still be mutated separately if a reference to it is retained elsewhere.

---

## ❓ Question
**13. (HCL) What's the difference between `Collections.unmodifiableList()` and `List.copyOf()` regarding true immutability?**

`unmodifiableList()` wraps the original list — if the original is later mutated, the "unmodifiable" view reflects those changes too. `List.copyOf()` creates a genuinely independent, immutable copy that's unaffected by later changes to the source list.

---

## ❓ Question
**14. (Mindtree) Why can immutable objects be freely cached and reused (like `Integer` caching -128 to 127)?**

Because their state never changes, sharing the same instance across multiple references carries zero risk — any code holding that reference will always see the same, correct value.

---

## ❓ Question
**15. (LTI) Can an immutable class have methods that appear to "modify" it, like `String.toUpperCase()`?**

Yes — these methods always return a **new** instance representing the transformed value, leaving the original object completely untouched.

---

## ❓ Question
**16. (Amazon) Why is immutability especially valuable in functional-style Java (streams, lambdas)?**

Functional operations often pass objects between multiple stages/threads; immutable objects guarantee no stage can unexpectedly alter shared data mid-pipeline, making behavior predictable and side-effect free.

---

## ❓ Question
**17. (Flipkart) What's a performance trade-off of immutability?**

Every "change" requires creating a new object rather than modifying in place, which can increase memory allocation and garbage collection pressure for frequently-changing data (mitigated for Strings via `StringBuilder`, and generally acceptable given modern GC efficiency).

---

## ❓ Question
**18. (Paytm) Can an immutable class implement `Cloneable`?**

It's unnecessary and generally discouraged — since the object never changes, there's no need to clone it; you can simply share the same reference safely instead of copying it.

---

## ❓ Question
**19. (Zoho) How does the Builder pattern help construct complex immutable objects?**

It allows step-by-step, readable construction of an object with many fields (avoiding huge constructor parameter lists), while still producing a single, fully-formed immutable instance at the end via `.build()`.

---

## ❓ Question
**20. (Freshworks) Why don't immutable objects need `synchronized` getters?**

Because their state is fixed at construction and safely published, concurrent reads from multiple threads can never observe an inconsistent or partially-updated value — synchronization is only needed to protect *mutable* shared state.

---

## ❓ Question
**21. (Adobe) What is "effective immutability" in the context of local variables and lambdas?**

A variable that is technically mutable (not declared `final`) but is never actually reassigned after initialization — Java requires this for variables captured inside lambda expressions.

---

## ❓ Question
**22. (SAP) Can you make an immutable class using only `private` fields without `final`?**

Not safely — without `final`, nothing prevents the class's own methods from accidentally reassigning the field later; `final` is the compiler-enforced guarantee that no reassignment can occur.

---

## ❓ Question
**23. (JPMorgan) Why are immutable Value Objects common in Domain-Driven Design (DDD)?**

Because Value Objects (like `Money`, `Address`) are defined by their attributes, not identity — immutability ensures two value objects with equal state are truly interchangeable and safe to share/cache without unexpected side effects.

---

## ❓ Question
**24. (Goldman Sachs) How does immutability simplify equals()/hashCode() correctness for HashMap-based caching?**

Since the object's fields (used in equals/hashCode) can never change after being inserted as a key, the cache entry remains reliably retrievable for the object's entire lifetime.

---

## ❓ Question
**25. (Morgan Stanley) What's a subtle immutability bug with arrays as fields, even if declared `final`?**

`final` only prevents reassigning the array reference itself — individual array elements (`arr[0] = ...`) can still be modified freely, so arrays require deep defensive copying (`Arrays.copyOf`), not just a `final` declaration.

---

## ❓ Question
**26. (Infosys) Can an immutable class have static mutable fields?**

Technically yes, but doing so is dangerous and generally against the spirit of immutability — shared mutable static state can still introduce race conditions even if instance-level state is perfectly immutable.

---

## ❓ Question
**27. (Wipro) How would you make an immutable class that wraps a `Map`?**

Accept a `Map` in the constructor, store `Map.copyOf(inputMap)` (or wrap with `Collections.unmodifiableMap` over a defensive copy) internally, and return that same unmodifiable reference from any getter.

---

## ❓ Question
**28. (TCS) Why is immutability considered a core principle in functional programming, which Java has increasingly adopted?**

Because pure functions avoid side effects, and immutable data structures guarantee that passing data between functions never causes unexpected external state changes — aligning naturally with functional design.

---

## ❓ Question
**29. (Capgemini) Does `final` alone guarantee thread safety for an object?**

No — `final` alone (on the reference) doesn't guarantee the referenced object's internal state is immutable; true thread-safety via immutability requires the entire object graph to be immutable, not just the top-level reference.

---

## ❓ Question
**30. (Cognizant) What's the benefit of "with-style" copy methods (e.g., `withName("NewName")`) on immutable classes?**

They let you create a modified copy of an immutable object concisely, without a full manual constructor call, while still preserving true immutability (the original object is untouched, and a new instance is returned).

---

## ❓ Question
**31. (Amazon) How does Kotlin's `data class` compare to Java's approach to immutability?**

Kotlin's `data class` with `val` properties provides built-in immutability, equals/hashCode/toString/copy() generation similar to Java records — Java's `record` (since 16) closes much of this historical gap.

---

## ❓ Question
**32. (Google) Why might you choose composition of several small immutable Value Objects over one large immutable class?**

It improves reusability, readability, and testability — smaller immutable objects (like `Money`, `Address`) can be composed into larger immutable aggregates (`Order`) while each piece stays focused and independently verifiable.

---

## ❓ Question
**33. (Oracle) Can an immutable class safely expose an `Iterator` over its internal collection?**

Only if that iterator doesn't support `remove()` (or the underlying collection is itself unmodifiable) — otherwise external code could mutate the "immutable" object's internal collection through the iterator.

---

## ❓ Question
**34. (Microsoft) Does immutability eliminate the need for `equals()`/`hashCode()` overrides?**

No — immutability doesn't automatically define logical equality; you still need to explicitly override `equals()`/`hashCode()` based on the object's fields unless using a `record`, which generates them automatically.

---

## ❓ Question
**35. (Deloitte) How does immutability help with safe object publication across threads without explicit synchronization?**

The Java Memory Model guarantees that properly constructed immutable objects (all fields `final`, no leaking `this` during construction) are safely visible to other threads once a reference is published, without needing additional synchronization.

---

## ❓ Question
**36. (Accenture) What's a real-world caching benefit of immutability in Spring Boot applications?**

Immutable DTOs/configuration objects can be safely cached and shared across concurrent HTTP requests without risk of one request's processing corrupting another's view of the same cached object.

---

## ❓ Question
**37. (IBM) Can you achieve immutability while still supporting the Builder pattern for optional/many fields?**

Yes — the builder collects all field values through a fluent API, then the final `build()` call constructs a single, fully-initialized immutable object in one atomic step, keeping intermediate builder state separate from the final immutable result.

---

## ❓ Question
**38. (HCL) Why is "leaking `this`" during construction dangerous for immutable class safety?**

If a constructor passes `this` to another object/thread before construction fully completes (e.g., registering a listener), that external code could observe a partially-initialized object, violating the immutability/safe-publication guarantee.

---

## ❓ Question
**39. (Mindtree) How does immutability interact with serialization for security-sensitive classes?**

Immutable classes are less vulnerable to certain deserialization attacks (like modifying state post-deserialization), though care is still needed with custom `readObject()` implementations to preserve invariants during deserialization.

---

## ❓ Question
**40. (LTI) What's the summarized real-world argument for defaulting to immutability wherever practical?**

It eliminates entire categories of bugs (unexpected mutation, race conditions, aliasing issues), simplifies reasoning about code correctness, and works naturally with modern Java features (records, streams, functional style) — mutable state should be the deliberate exception, not the default.

---
