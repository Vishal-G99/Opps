# Chapter 13: Object Class

Every class in Java implicitly extends `java.lang.Object`, which provides a baseline set of methods available to all objects.

---

## ❓ Question
**What is `equals()` in the Object class?**

`equals(Object obj)` checks logical equality. The default `Object` implementation simply does `this == obj` (reference comparison). Classes commonly override it to compare actual field values.

```java
class Point {
    int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Point)) return false;
        Point p = (Point) obj;
        return this.x == p.x && this.y == p.y;
    }
}
```

---

## ❓ Question
**What is `hashCode()` in the Object class?**

Returns an integer hash representation of the object, used by hash-based collections (`HashMap`, `HashSet`). The default implementation typically derives from the object's memory address/identity.

```java
class Point {
    int x, y;

    @Override
    public int hashCode() {
        return Objects.hash(x, y); // combine fields into a hash
    }
}
```

**Golden rule:** if you override `equals()`, you **must** override `hashCode()` too — equal objects must produce equal hash codes, or hash-based collections will behave incorrectly.

---

## ❓ Question
**What is `toString()` in the Object class?**

Returns a String representation of the object. The default implementation returns `ClassName@hexHashCode`, which is rarely useful — commonly overridden for readable debugging/logging output.

```java
class Point {
    int x, y;

    @Override
    public String toString() {
        return "Point(" + x + ", " + y + ")";
    }
}

System.out.println(new Point(3, 4)); // Point(3, 4) — instead of Point@1b6d3586
```

---

## ❓ Question
**What is `clone()` in the Object class?**

Creates and returns a copy of the object. The class must implement the `Cloneable` marker interface, or `clone()` throws `CloneNotSupportedException`. The default behavior performs a **shallow copy**.

```java
class Point implements Cloneable {
    int x, y;

    @Override
    public Point clone() throws CloneNotSupportedException {
        return (Point) super.clone();
    }
}
```

---

## ❓ Question
**What is `finalize()` in the Object class?**

A method the JVM used to call before reclaiming an object's memory during garbage collection, intended for cleanup logic. It is **deprecated since Java 9** and scheduled for removal — it's unreliable (no guaranteed timing or invocation) and has been replaced by `try-with-resources` and `Cleaner`/`PhantomReference`.

```java
class Resource {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Cleanup before GC"); // deprecated, don't rely on this
    }
}
```

---

## ❓ Question
**What are `wait()`, `notify()`, and `notifyAll()`?**

These are methods for **thread synchronization**, used inside `synchronized` blocks to coordinate threads waiting on an object's monitor.

```java
synchronized (lock) {
    while (!conditionMet) {
        lock.wait();       // releases the lock, waits to be notified
    }
    // proceed once notified
}

synchronized (lock) {
    conditionMet = true;
    lock.notify();          // wakes ONE waiting thread
    // lock.notifyAll();     // wakes ALL waiting threads
}
```

- `wait()` — releases the object's monitor and pauses the thread until notified.
- `notify()` — wakes up a single waiting thread (unspecified which one).
- `notifyAll()` — wakes up all waiting threads; they then compete for the lock.

---

## ❓ Question
**What is `getClass()` in the Object class?**

Returns the runtime `Class` object representing the object's actual class — useful for reflection, type checks, and logging.

```java
Point p = new Point(1, 2);
System.out.println(p.getClass().getName()); // Point
System.out.println(p.getClass().getSimpleName()); // Point
```

---

## ❓ Question
**Can you show a Practical Example combining `equals()`, `hashCode()`, and `toString()` properly?**

```java
import java.util.Objects;

class Employee {
    String id;
    String name;

    Employee(String id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Employee)) return false;
        Employee e = (Employee) obj;
        return id.equals(e.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }

    @Override
    public String toString() {
        return "Employee{id='" + id + "', name='" + name + "'}";
    }
}

Set<Employee> employees = new HashSet<>();
employees.add(new Employee("E1", "Alice"));
employees.add(new Employee("E1", "Alice Duplicate")); // treated as duplicate by id
System.out.println(employees.size()); // 1 — because equals()/hashCode() are based on id
```

---

## ❓ Question
**What is the JVM internal contract between `equals()` and `hashCode()`?**

```
Contract Rules:
1. If a.equals(b) is true  → a.hashCode() MUST equal b.hashCode()
2. If a.hashCode() == b.hashCode() → a.equals(b) is NOT required to be true (hash collisions are allowed)
3. hashCode() must return the SAME value consistently, as long as fields used in it don't change
```

```
HashMap bucket lookup process:
1. Compute hashCode() → determine which bucket to check
2. Within that bucket, use equals() → find the exact matching key
```

If `equals()` is overridden without `hashCode()`, two "equal" objects could land in different buckets — breaking `HashMap`/`HashSet` lookups silently.

---

# 🎯 40 Interview Questions — Object Class

## ❓ Question
**1. (TCS) Why must `equals()` and `hashCode()` be overridden together?**

Because hash-based collections rely on the contract that equal objects produce equal hash codes; violating this breaks lookups in `HashMap`/`HashSet`.

---

## ❓ Question
**2. (Infosys) What does the default `Object.equals()` actually do?**

It performs a simple reference comparison (`this == obj`), the same as `==`, unless overridden.

---

## ❓ Question
**3. (Wipro) Why is `clone()` considered problematic in modern Java?**

It's shallow by default, requires implementing the empty marker interface `Cloneable`, throws a checked exception, and doesn't play well with `final` fields — copy constructors or factory methods are often preferred.

---

## ❓ Question
**4. (Amazon) What happens if you call `clone()` without implementing `Cloneable`?**

`CloneNotSupportedException` is thrown at runtime.

---

## ❓ Question
**5. (Google) Why was `finalize()` deprecated?**

It has unreliable timing (no guarantee it ever runs before JVM exit), can resurrect objects, hurts GC performance, and has been superseded by more predictable resource management (`try-with-resources`, `Cleaner`).

---

## ❓ Question
**6. (Accenture) Can `wait()` be called outside a synchronized block?**

No — calling `wait()` without holding the object's monitor throws `IllegalMonitorStateException`.

---

## ❓ Question
**7. (Cognizant) What's the difference between `notify()` and `notifyAll()`?**

`notify()` wakes exactly one arbitrary waiting thread; `notifyAll()` wakes all waiting threads, which then compete to reacquire the lock.

---

## ❓ Question
**8. (Capgemini) Why is `toString()` important for debugging?**

Overriding it provides meaningful output in logs, print statements, and debuggers instead of the default unhelpful `ClassName@hashcode` format.

---

## ❓ Question
**9. (Deloitte) Does `getClass()` return the declared type or the runtime type?**

The **runtime type** — even if a variable is declared as a supertype, `getClass()` returns the actual class of the object it currently references.

---

## ❓ Question
**10. (Oracle) Can `equals()` be used to compare an object with `null` safely?**

Yes, if implemented correctly (`obj instanceof X` returns false for null), calling `obj.equals(null)` should safely return false without throwing an exception — but calling `null.equals(obj)` would throw `NullPointerException`, so order matters.

---

## ❓ Question
**11. (Microsoft) What utility class helps safely compare objects that might be null?**

`java.util.Objects`, e.g., `Objects.equals(a, b)` handles null-safety internally, and `Objects.hash(...)` simplifies hash code generation.

---

## ❓ Question
**12. (IBM) Is `hashCode()` guaranteed to be unique for every object?**

No — different objects can share the same hash code (a "collision"); uniqueness is not guaranteed, only consistency for equal objects.

---

## ❓ Question
**13. (HCL) Why does overriding `equals()` typically also require checking `instanceof`?**

To safely verify the compared object is actually of a compatible type before casting and comparing fields, avoiding a `ClassCastException`.

---

## ❓ Question
**14. (Mindtree) What is the difference between `wait()` and `sleep()`?**

`wait()` releases the object's monitor lock and requires being called within a synchronized context; `Thread.sleep()` does **not** release any lock and simply pauses the current thread for a fixed duration.

---

## ❓ Question
**15. (LTI) Can `clone()` be overridden to perform a deep copy?**

Yes — inside the overridden `clone()`, after calling `super.clone()`, you manually clone any mutable reference fields as well, rather than just copying their references.

---

## ❓ Question
**16. (Amazon) What does `Object.equals()` compare when neither `equals()` nor `hashCode()` is overridden?**

Both effectively fall back to identity comparison — `equals()` behaves like `==`, and `hashCode()` is typically derived from the object's memory address/internal identity.

---

## ❓ Question
**17. (Flipkart) Why should mutable fields generally not be used in `hashCode()` for objects stored in a `HashSet`?**

If a field used in `hashCode()` changes after the object is added, the object's hash bucket becomes inconsistent with its current state, effectively "losing" it in the collection — lookups and removal will silently fail.

---

## ❓ Question
**18. (Paytm) What replaced `finalize()` for reliable resource cleanup?**

`try-with-resources` (for `AutoCloseable` resources) and `java.lang.ref.Cleaner` for more advanced, GC-tied cleanup needs.

---

## ❓ Question
**19. (Zoho) Can `getClass()` be called on a `null` reference?**

No — calling any method, including `getClass()`, on a `null` reference throws `NullPointerException`.

---

## ❓ Question
**20. (Freshworks) What is the "spurious wakeup" problem with `wait()`?**

A waiting thread can wake up without an actual `notify()`/`notifyAll()` call, due to JVM/OS-level behavior — this is why `wait()` should always be called inside a loop re-checking the condition, not a simple `if`.

---

## ❓ Question
**21. (Adobe) Why is `Objects.hash()` convenient but sometimes less performant than a manually written `hashCode()`?**

It internally creates a varargs `Object[]` array and iterates over it, adding minor overhead compared to a hand-rolled combination of field hash codes — usually negligible, but relevant in extremely hot code paths.

---

## ❓ Question
**22. (SAP) Is it mandatory to override `toString()` for every class?**

No — it's optional, but strongly recommended for domain/model classes to aid debugging and logging; utility/internal classes may not need it.

---

## ❓ Question
**23. (JPMorgan) What happens if `equals()` is overridden inconsistently with `compareTo()` in a `Comparable` class?**

It can cause unexpected behavior in sorted collections like `TreeSet`/`TreeMap`, which rely on `compareTo() == 0` for equality checks, potentially differing from `equals()` — leading to subtle bugs like items being "equal" in a sorted set but not by `.equals()`.

---

## ❓ Question
**24. (Goldman Sachs) Can `wait()`/`notify()` be replaced by higher-level concurrency utilities?**

Yes — `java.util.concurrent` classes like `CountDownLatch`, `Semaphore`, `BlockingQueue`, and `Condition` (from `Lock`) offer safer, more expressive alternatives to raw `wait()`/`notify()`.

---

## ❓ Question
**25. (Morgan Stanley) Does overriding `hashCode()` affect object identity (`==`)?**

No — `==` always compares references directly and is unaffected by any override of `equals()` or `hashCode()`.

---

## ❓ Question
**26. (Infosys) What is the correct way to check an object's exact runtime class versus checking if it's an instance of a type (including subtypes)?**

`obj.getClass() == SomeClass.class` checks the exact class only. `obj instanceof SomeClass` returns true for the class and any of its subclasses too.

---

## ❓ Question
**27. (Wipro) Why can a poorly implemented `clone()` break encapsulation?**

If it performs a shallow copy of mutable reference fields, the "cloned" object still shares internal mutable state with the original, so changes to one can unexpectedly affect the other.

---

## ❓ Question
**28. (TCS) Is `Object` itself instantiable directly?**

Yes — `new Object()` is valid; it's a concrete (not abstract) class, though it's rarely useful on its own beyond basic locking/synchronization purposes.

---

## ❓ Question
**29. (Capgemini) How do records simplify `equals()`, `hashCode()`, and `toString()`?**

Records automatically generate correct implementations of all three based on their components, eliminating the boilerplate typically hand-written or IDE-generated for simple data-carrying classes.

---

## ❓ Question
**30. (Cognizant) What's the risk of using a mutable object as a `HashMap` key?**

If the key's fields (used in `hashCode()`/`equals()`) change after insertion, the entry can become unreachable via normal lookup, effectively "lost" in the map even though it's technically still present.

---

## ❓ Question
**31. (Amazon) Can two unequal objects have the same `hashCode()`?**

Yes — this is called a **hash collision**, and it's allowed by the contract; hash-based collections handle it internally by chaining/probing and falling back to `equals()` to disambiguate.

---

## ❓ Question
**32. (Google) Why does `String` override `hashCode()` using a specific formula?**

To provide a well-distributed, deterministic, and cacheable hash (Strings cache their computed hash internally after first computation) since Strings are extremely commonly used as `HashMap` keys.

---

## ❓ Question
**33. (Oracle) Can `finalize()` still technically be called manually, despite deprecation?**

Yes — as a regular (if unusual) method call like `obj.finalize()`, but this doesn't trigger the special GC-related behavior; it's just an ordinary method invocation.

---

## ❓ Question
**34. (Microsoft) What does IntelliJ/Eclipse-generated `equals()`/`hashCode()` typically use under the hood?**

Commonly a `getClass() == obj.getClass()` check (stricter than `instanceof`) plus `Objects.equals()`/`Objects.hash()` for field comparisons, avoiding common pitfalls with subclassing and null fields.

---

## ❓ Question
**35. (Deloitte) Why is `getClass() == obj.getClass()` sometimes preferred over `instanceof` in `equals()`?**

`instanceof` allows a subclass to be considered "equal" to a superclass instance, which can break symmetry (`a.equals(b)` should imply `b.equals(a)`) when subclasses add new fields — `getClass()` comparison avoids that asymmetry.

---

## ❓ Question
**36. (Accenture) What is the "reflexive," "symmetric," and "transitive" contract of `equals()`?**

Reflexive: `x.equals(x)` must be true. Symmetric: `x.equals(y)` implies `y.equals(x)`. Transitive: if `x.equals(y)` and `y.equals(z)`, then `x.equals(z)` must also be true — all part of the formal `equals()` contract.

---

## ❓ Question
**37. (IBM) Can `notify()` wake up a thread that called `Thread.sleep()` instead of `wait()`?**

No — `notify()`/`notifyAll()` only affect threads waiting on that specific object's monitor via `wait()`; sleeping threads are unrelated and unaffected.

---

## ❓ Question
**38. (HCL) What's a real-world example where overriding `toString()` prevents a production debugging headache?**

Logging a list of custom domain objects (e.g., `Order` objects) — without an overridden `toString()`, logs show meaningless `Order@4a5e1a`, making it impossible to see actual order details without attaching a debugger.

---

## ❓ Question
**39. (Mindtree) Why do frameworks like Hibernate/JPA recommend caution when overriding `equals()`/`hashCode()` for entity classes?**

Because entity IDs are often null before being persisted, and lazy-loaded proxies can complicate `getClass()` checks — naive implementations can behave inconsistently before/after persistence, breaking collection behavior.

---

## ❓ Question
**40. (LTI) How does `Object`'s baseline design support Java's "everything is polymorphic" philosophy?**

By guaranteeing every class — regardless of hierarchy — shares a common baseline API (`equals`, `hashCode`, `toString`, `getClass`, etc.), enabling generic collections, reflection, and utilities to operate uniformly on any object in the language.

---
