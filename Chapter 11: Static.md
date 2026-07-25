# Chapter 11: Static

---

## ❓ Question
**What are Static Variables?**

A static variable belongs to the **class itself**, not to any individual object. All objects of the class share **one single copy** of it, stored in the Metaspace (as part of class-level data), not duplicated per object.

```java
class Counter {
    static int count = 0;   // shared across all objects
    int id;                  // unique per object

    Counter() {
        count++;
        id = count;
    }
}

Counter c1 = new Counter();
Counter c2 = new Counter();
Counter c3 = new Counter();

System.out.println(Counter.count); // 3 — shared counter
System.out.println(c1.id);          // 1
System.out.println(c3.id);          // 3
```

---

## ❓ Question
**What are Static Methods?**

A static method belongs to the class and can be called **without creating an object**. It cannot directly access instance (non-static) variables or methods, since it has no `this` context.

```java
class MathUtils {
    static int square(int n) {
        return n * n;
    }
}

int result = MathUtils.square(5); // called directly on the class — no object needed
```

```java
class Demo {
    int x = 10;
    static void show() {
        // System.out.println(x); // COMPILE ERROR — no instance context
    }
}
```

---

## ❓ Question
**What are Static Blocks?**

A static block runs **once**, when the class is first loaded by the JVM — used for one-time static initialization logic more complex than a simple field assignment.

```java
class Config {
    static Map<String, String> settings;

    static {
        settings = new HashMap<>();
        settings.put("env", "production");
        System.out.println("Static block executed");
    }
}
```

```
Class loading order:
1. Static variables get default values
2. Static blocks execute (in the order they're written)
3. Class is now fully initialized, ready for use
```

---

## ❓ Question
**What are Static Nested Classes?**

A static nested class is a class defined inside another class, marked `static`. Unlike a regular (inner) class, it does **not** hold an implicit reference to an instance of the outer class — it behaves like a normal top-level class, just namespaced inside the outer one.

```java
class Outer {
    static class Node {
        int value;
        Node(int value) { this.value = value; }
    }
}

Outer.Node node = new Outer.Node(10); // no Outer instance needed
```

Commonly used for helper/builder classes, like `Map.Entry` or a linked-list `Node`.

---

## ❓ Question
**What is Static Import?**

Static import lets you use static members of a class **without qualifying them** with the class name, by importing them directly.

```java
import static java.lang.Math.sqrt;
import static java.lang.Math.PI;

double r = sqrt(25);      // instead of Math.sqrt(25)
double area = PI * 4 * 4;  // instead of Math.PI
```

Useful for readability with frequently used utility constants/methods, but overuse can hurt code clarity since the origin of the member becomes less obvious.

---

## ❓ Question
**Where does Static Memory live, and how does JVM Loading handle it?**

Static members live in the **Metaspace** (method area), not the heap — because they belong to the class, not to any individual object.

```
Class Loading Process for static members:
1. Loading    → JVM reads the .class file bytecode
2. Linking    → Verification, preparation (static vars get default values: 0, null, false)
3. Initialization → static blocks run + static variables get their actual assigned values
```

```
Metaspace
┌───────────────────────────┐
│ Counter.class               │
│   static int count = 3       │  ← ONE shared copy, not per-object
└───────────────────────────┘

Heap
┌───────────────────────────┐
│ Counter@1  { id: 1 }          │
│ Counter@2  { id: 2 }          │
│ Counter@3  { id: 3 }          │
└───────────────────────────┘
```

A class is loaded **only once** per ClassLoader, and static initialization happens exactly once — the very first time the class is actively used (instantiated, or one of its static members accessed).

---

# 🎯 40 Interview Questions — Static

## ❓ Question
**1. (TCS) Can a static method be overridden?**

No — static methods are resolved at compile time based on reference type (method hiding), not overridden polymorphically like instance methods.

---

## ❓ Question
**2. (Infosys) What happens if a subclass declares a static method with the same signature as the parent's static method?**

This is called **method hiding**, not overriding — which version runs depends on the reference type used to call it, not the actual object type.

---

## ❓ Question
**3. (Wipro) Can a static method access instance variables directly?**

No — a static method has no implicit `this`, so it cannot access instance variables/methods without first having an object reference.

---

## ❓ Question
**4. (Amazon) Can a static block throw a checked exception?**

No — static blocks cannot throw checked exceptions directly; doing so causes an `ExceptionInInitializerError` wrapping any exception thrown during static initialization.

---

## ❓ Question
**5. (Google) When exactly does static initialization happen?**

The first time the class is actively used — either instantiated, or one of its static fields/methods is accessed — not necessarily when the program starts.

---

## ❓ Question
**6. (Accenture) Can a `main` method be non-static?**

No — the JVM calls `main` without creating an object first, so it must be `public static void main(String[] args)`.

---

## ❓ Question
**7. (Cognizant) Are static variables thread-safe by default?**

No — since they're shared across all threads, concurrent modification without synchronization can cause race conditions; explicit synchronization or `AtomicInteger`-style classes are needed.

---

## ❓ Question
**8. (Capgemini) Can you have multiple static blocks in one class?**

Yes — they execute in the order they appear in the source file, top to bottom.

---

## ❓ Question
**9. (Deloitte) Can a static nested class access the outer class's instance members?**

Not directly — it has no implicit reference to an outer instance; it would need an explicit `Outer` object reference passed to it.

---

## ❓ Question
**10. (Oracle) What's the difference between a static nested class and a non-static inner class?**

A static nested class doesn't hold a reference to an outer instance and can exist independently. A non-static inner class implicitly holds a reference to its enclosing outer object and requires one to be instantiated.

---

## ❓ Question
**11. (Microsoft) Can an interface have static methods?**

Yes, since Java 8 — interfaces can define `static` methods, callable only via the interface name, not inherited by implementing classes.

---

## ❓ Question
**12. (IBM) Is it valid to call a static method using an object reference?**

Syntactically yes (`obj.staticMethod()`), but it's discouraged — it's resolved based on the reference's declared type, not the actual object, and can be misleading.

---

## ❓ Question
**13. (HCL) Can constructors be static?**

No — constructors are inherently tied to instance creation, so `static` is not a valid modifier for them.

---

## ❓ Question
**14. (Mindtree) What is a static factory method?**

A static method that returns an instance of the class, often used instead of a public constructor (e.g., `Integer.valueOf(5)`), allowing caching, meaningful naming, or returning subtypes.

---

## ❓ Question
**15. (LTI) Why might static factory methods be preferred over constructors?**

They can have descriptive names, return cached/reused instances instead of always creating new ones, and can return a subtype of the declared return type — none of which a constructor can do.

---

## ❓ Question
**16. (Amazon) Can you use `this` inside a static method?**

No — `this` refers to the current instance, and static methods have no associated instance.

---

## ❓ Question
**17. (Flipkart) What happens to static variables during Garbage Collection?**

They persist as long as the class itself is loaded — they aren't GC'd like normal heap objects unless the entire class is unloaded (rare, typically only with custom ClassLoaders).

---

## ❓ Question
**18. (Paytm) Can a static variable be `final` and still change value?**

If `static final` and a primitive/String assigned a **compile-time constant**, it can't change. If assigned at runtime (e.g., via a static block) or referencing a mutable object, the reference is fixed but the object's internal state can still change.

---

## ❓ Question
**19. (Zoho) Are static methods and static variables loaded lazily or eagerly?**

By default, JVM class loading is generally lazy (on first active use), though the exact timing can vary — the key guarantee is initialization happens exactly once, before first active use.

---

## ❓ Question
**20. (Freshworks) Can you overload static methods?**

Yes — overloading only depends on the method signature (parameters), independent of whether the method is static or instance-level.

---

## ❓ Question
**21. (Adobe) What's the difference between static import and a regular import?**

A regular import lets you use a class name without its full package path. Static import lets you use a class's static members (fields/methods) without prefixing them with the class name at all.

---

## ❓ Question
**22. (SAP) Can an abstract method be static?**

No — `static` and `abstract` are contradictory: abstract methods require overriding by subclasses, but static methods can't be overridden (only hidden), so combining them is a compile error.

---

## ❓ Question
**23. (JPMorgan) How do static members affect memory footprint in long-running applications?**

Since static data lives for the entire life of the class (often the whole application), large static collections (e.g., caches) can cause memory bloat or leaks if not carefully bounded/cleared.

---

## ❓ Question
**24. (Goldman Sachs) Can a static variable be accessed before the class is fully initialized?**

If accessed during static initialization itself (e.g., inside another static block referencing it earlier in the order), it may still hold its default value rather than the intended assigned value — this is a common source of subtle bugs.

---

## ❓ Question
**25. (Morgan Stanley) Why is the Singleton pattern often implemented with a static field?**

Because a `private static` instance field, combined with a `private` constructor and a `public static` accessor method, guarantees exactly one shared instance across the entire application.

---

## ❓ Question
**26. (Infosys) Can static methods be called through an interface reference?**

No — interface static methods must be called via the interface name directly (`InterfaceName.method()`), not through instances or implementing class references.

---

## ❓ Question
**27. (Wipro) What's the difference between class variables and instance variables in terms of memory?**

Class (static) variables have exactly one copy shared in Metaspace regardless of object count. Instance variables get a fresh copy on the heap for every object created.

---

## ❓ Question
**28. (TCS) Can you synchronize a static method?**

Yes — `synchronized static void method()` locks on the **Class object's monitor** (`ClassName.class`), rather than an individual instance's monitor.

---

## ❓ Question
**29. (Capgemini) What real-world problem do static utility classes (like `Collections`, `Math`) solve?**

They group stateless, reusable helper logic that doesn't need object identity or state, avoiding the overhead of instantiating objects just to call a function-like operation.

---

## ❓ Question
**30. (Cognizant) Can enum constants be considered static?**

Yes — enum constants are implicitly `public static final`, each representing a single shared instance of the enum type.

---

## ❓ Question
**31. (Amazon) What happens if two classes have circular static initialization dependencies?**

It can lead to one class seeing the other's static fields still at default values (0/null) during initialization, since the JVM detects the cycle and proceeds without waiting — a subtle, hard-to-debug issue.

---

## ❓ Question
**32. (Google) Why can't static methods be part of polymorphic (runtime) dispatch?**

Because static methods are resolved via the class itself (compile-time binding), and polymorphism specifically relies on the object's runtime type via the virtual method table — static methods don't participate in that mechanism.

---

## ❓ Question
**33. (Oracle) Can local variables inside a method be static?**

No — `static` only applies to class-level members, not to variables declared inside method bodies.

---

## ❓ Question
**34. (Microsoft) How does the JVM guarantee thread-safe class initialization?**

The JVM uses an internal locking mechanism during class loading/initialization, ensuring only one thread performs the static initialization while others wait — this is specified by the JLS's class initialization guarantees.

---

## ❓ Question
**35. (Deloitte) Can you have a static variable inside a method (not a static field)?**

No — Java doesn't support "static local variables" the way C/C++ does; the closest equivalent is a static class-level field instead.

---

## ❓ Question
**36. (Accenture) Why might excessive use of static state be considered bad OOP practice?**

It breaks encapsulation and testability — shared mutable static state can create hidden coupling between unrelated parts of code and makes unit testing harder (state persists between tests unless carefully reset).

---

## ❓ Question
**37. (IBM) Can a static field be shadowed by an instance field with the same name?**

Yes — similar to regular field hiding, an instance field can share a name with a static field of the same class, though it's confusing style and generally avoided.

---

## ❓ Question
**38. (HCL) Does marking a variable `static` improve performance?**

It can slightly reduce memory usage (one shared copy instead of per-object copies) but the main purpose is semantic (shared state), not primarily a performance optimization.

---

## ❓ Question
**39. (Mindtree) How does Spring Boot discourage overuse of static fields for dependency management?**

Spring promotes dependency injection through instance fields managed by the IoC container instead, since static fields bypass the container entirely, making mocking/testing and lifecycle management much harder.

---

## ❓ Question
**40. (LTI) What's a common real-world bug caused by static mutable collections in web applications?**

A `static List`/`Map` used as a shared cache without synchronization can cause data corruption or `ConcurrentModificationException` under concurrent requests, since all threads/requests share that single instance.

---
