# Chapter 9: `super` Keyword

---

## ❓ Question
**What is the `super` keyword in Java?**

`super` is a reference to the **immediate parent class object**. It's used inside a subclass to access the parent's constructor, variables, and methods — especially when the subclass has hidden or overridden them.

```java
class Animal {
    String type = "Animal";
    void sound() { System.out.println("Some generic sound"); }
}

class Dog extends Animal {
    String type = "Dog";
    void sound() { System.out.println("Bark"); }

    void show() {
        System.out.println(type);         // Dog (subclass field)
        System.out.println(super.type);   // Animal (parent field)
        sound();                          // Bark
        super.sound();                    // Some generic sound
    }
}
```

---

## ❓ Question
**How does `super()` call the Parent Constructor?**

`super()` invokes the parent class's constructor. It must be the **first statement** in the subclass constructor. If you don't write it explicitly, Java inserts an implicit `super()` (no-arg) call automatically.

```java
class Animal {
    Animal(String name) {
        System.out.println("Animal constructor: " + name);
    }
}

class Dog extends Animal {
    Dog() {
        super("Rex");  // must call parent's parameterized constructor explicitly
        System.out.println("Dog constructor");
    }
}

// new Dog();
// Output:
// Animal constructor: Rex
// Dog constructor
```

If `Animal` has **no** no-arg constructor and `Dog` doesn't call `super(...)` explicitly, it's a **compile error** — Java can't silently insert `super()` if no matching no-arg constructor exists.

---

## ❓ Question
**How does `super` access Parent Variables?**

When a subclass declares a field with the **same name** as the parent's field, the parent's field isn't overridden — it's **hidden** (field hiding, not polymorphic like methods). `super.fieldName` lets you reach the parent's version explicitly.

```java
class Vehicle {
    int wheels = 4;
}

class Bike extends Vehicle {
    int wheels = 2;

    void printWheels() {
        System.out.println(wheels);        // 2
        System.out.println(super.wheels);  // 4
    }
}
```

Note: field access is resolved at **compile time** based on reference type, not runtime type — unlike overridden methods.

---

## ❓ Question
**How does `super` access Parent Methods?**

`super.methodName()` explicitly calls the parent's version of a method, even if the subclass has overridden it. This is common when you want to **extend**, not fully replace, parent behavior.

```java
class Shape {
    void draw() { System.out.println("Drawing a shape"); }
}

class Circle extends Shape {
    @Override
    void draw() {
        super.draw();                 // reuse parent logic
        System.out.println("Drawing a circle"); // add subclass-specific logic
    }
}

// new Circle().draw();
// Output:
// Drawing a shape
// Drawing a circle
```

---

## ❓ Question
**What is Constructor Chaining with `super()`?**

Constructor chaining means constructors call each other in sequence — `super()` chains up the inheritance hierarchy, ensuring every parent class is properly initialized **before** the child class runs its own logic.

```java
class A {
    A() { System.out.println("A constructor"); }
}

class B extends A {
    B() {
        super();  // implicit or explicit — always runs first
        System.out.println("B constructor");
    }
}

class C extends B {
    C() {
        super();
        System.out.println("C constructor");
    }
}

// new C();
// Output:
// A constructor
// B constructor
// C constructor
```

```
Execution order:  Object → A → B → C
(Top of hierarchy initializes first, then works down to the most derived class)
```

---

## ❓ Question
**How does `super` relate to Method Overriding?**

When a method is overridden, calling it via a subclass reference invokes the **subclass's** version (dynamic/runtime polymorphism). `super.method()` is the escape hatch to deliberately call the **parent's original implementation** from within the override.

```java
class Employee {
    double calculateBonus() { return 1000; }
}

class Manager extends Employee {
    @Override
    double calculateBonus() {
        return super.calculateBonus() + 500; // extends parent logic instead of duplicating it
    }
}
```

This avoids duplicating the base logic — a common real-world pattern for extending behavior instead of rewriting it.

---

# 🎯 40 Interview Questions — `super` Keyword

## ❓ Question
**1. (Infosys) Can `super()` and `this()` be used together in the same constructor?**

No. Both must be the **first statement** in a constructor, so only one can be used — never both in the same constructor.

---

## ❓ Question
**2. (TCS) What happens if a parent class has no default constructor and the child doesn't call `super()` explicitly?**

Compile-time error — Java tries to insert an implicit `super()`, but if no no-arg constructor exists in the parent, compilation fails.

---

## ❓ Question
**3. (Amazon) Can `super` be used in a static method?**

No. `super` refers to an instance of the parent class, and static methods don't operate on instances — using `super` there is a compile error.

---

## ❓ Question
**4. (Google) Is calling `super()` mandatory in every subclass constructor?**

Not explicitly — if omitted, the compiler automatically inserts `super()` (no-arg) as the first statement, provided the parent has an accessible no-arg constructor.

---

## ❓ Question
**5. (Wipro) Can you call `super.super.method()` to reach the grandparent class?**

No, Java does not support multi-level `super` chaining like `super.super`. You can only access the immediate parent.

---

## ❓ Question
**6. (Accenture) What's the difference between field hiding and method overriding regarding `super`?**

Field access with `super.field` is resolved at **compile time** based on the declared reference type. Method calls are resolved at **runtime** based on actual object type — `super.method()` deliberately bypasses that to force the parent's version.

---

## ❓ Question
**7. (Cognizant) Can `super` be used inside a constructor to access parent instance variables before `this()`?**

`super` referring to parent fields can only be used **after** the constructor's first line (`super()`/`this()` call), since the object isn't fully constructed until then.

---

## ❓ Question
**8. (Capgemini) If a child class doesn't override a method, does calling it use the parent's version automatically?**

Yes — if not overridden, the method call simply resolves to the inherited parent implementation; no explicit `super` needed.

---

## ❓ Question
**9. (Deloitte) Can constructors be inherited in Java?**

No. Constructors are **not inherited**. Each class must define its own; `super()` is used to invoke the parent's constructor logic, not to inherit it directly.

---

## ❓ Question
**10. (Oracle) What happens during constructor chaining if a middle class in the hierarchy has no explicit constructor?**

Java provides an implicit default no-arg constructor for that class, which itself implicitly calls `super()` — the chain still works transparently.

---

## ❓ Question
**11. (Microsoft) Can `super` be used to access a private member of the parent class?**

No. Private members aren't inherited/visible to subclasses at all — `super` cannot access them regardless.

---

## ❓ Question
**12. (IBM) Why is `super()` always called before the child constructor's own logic executes?**

To guarantee the parent portion of the object is fully initialized first, since the child class may depend on (or override behavior built on top of) that parent state.

---

## ❓ Question
**13. (HCL) Can you skip calling the parent constructor entirely?**

No — every object construction always invokes some parent constructor, either explicitly via `super(...)` or implicitly via the compiler-inserted `super()`.

---

## ❓ Question
**14. (Mindtree) What is the output order when three classes chain via `super()` and each prints in its constructor?**

Parent-most class's constructor logic executes first, then each subclass constructor runs in order down to the most derived class (top-down execution).

---

## ❓ Question
**15. (LTI) Can `super` be used in an interface's default method?**

Yes, but with special syntax: `InterfaceName.super.methodName()` — used to disambiguate when a class implements multiple interfaces with the same default method.

---

## ❓ Question
**16. (Amazon) What happens if both parent and child classes have a field with the same name, and you access it via a parent-typed reference to a child object?**

The **parent's field** is returned, because field access is resolved based on the reference/declared type at compile time, not the actual object type.

---

## ❓ Question
**17. (Flipkart) Can `super()` call a constructor further up than the immediate parent?**

No. `super()` can only invoke the **immediate parent's** constructor — not a grandparent's, even indirectly.

---

## ❓ Question
**18. (Paytm) Is it valid to write code before `super()` in a constructor?**

No — `super()` (or `this()`) must always be the very first statement; any code before it is a compile error.

---

## ❓ Question
**19. (Zoho) Why might you use `super.method()` instead of just calling `method()` from a subclass?**

To explicitly invoke the parent's implementation when the method is overridden — useful for extending rather than replacing base behavior.

---

## ❓ Question
**20. (Freshworks) Can a subclass access a protected parent field directly, or does it need `super`?**

It can access it directly without `super` unless the field name is shadowed by a same-named subclass field — in that case `super` is needed to disambiguate.

---

## ❓ Question
**21. (Adobe) What is the compiler-inserted default constructor's behavior regarding `super()`?**

If a class has no constructor at all, the compiler auto-generates a public no-arg constructor whose only statement is an implicit `super()` call.

---

## ❓ Question
**22. (SAP) Does `super` work with static (class-level) methods for method hiding?**

Static methods are not overridden but **hidden**. `super.staticMethod()` can technically call it, though this is unusual style — normally you'd call `ParentClass.staticMethod()` directly.

---

## ❓ Question
**23. (JPMorgan) Can constructor chaining cause infinite loops?**

Not with `super()`, since it always moves strictly upward in the hierarchy. However, misusing `this()` to call constructors within the same class circularly can cause a compile-time cyclic constructor invocation error.

---

## ❓ Question
**24. (Goldman Sachs) What happens if the parent class constructor throws a checked exception?**

The child class constructor must either declare that exception in its own `throws` clause or handle it, since the exception propagates immediately through the mandatory `super()` call.

---

## ❓ Question
**25. (Morgan Stanley) Can `super` be assigned to a variable, like `Object obj = super;`?**

No. `super` is not an object reference/expression you can assign — it is a keyword usable only in the specific contexts of constructor calls, field access, and method calls.

---

## ❓ Question
**26. (Infosys) In multiple inheritance via interfaces, how does `super` resolve ambiguity?**

Java requires explicit disambiguation using `InterfaceName.super.method()` when a class implements two interfaces with conflicting default methods.

---

## ❓ Question
**27. (Wipro) Does using `super()` affect performance?**

Negligibly — it's a direct method call resolved at compile time, not a runtime lookup, so the overhead is the same as any normal constructor call.

---

## ❓ Question
**28. (TCS) Can an abstract class's constructor be called via `super()`?**

Yes. Even though you can't instantiate an abstract class directly with `new`, its constructor still runs via `super()` when a concrete subclass is instantiated.

---

## ❓ Question
**29. (Capgemini) What's the difference between `this()` and `super()`?**

`this()` calls another constructor **within the same class** (constructor overloading chaining). `super()` calls a constructor **in the immediate parent class**. Both must be the first statement, so they're mutually exclusive per constructor.

---

## ❓ Question
**30. (Cognizant) If a subclass overrides `toString()`, how would you include the parent's default output too?**

Call `super.toString()` inside the override and concatenate/append it with the subclass-specific details.

---

## ❓ Question
**31. (Amazon) Why can't `super` be used in a top-level class with no explicit parent?**

Every class implicitly extends `Object` (unless it's `Object` itself), so `super` is still valid there and refers to `Object`'s constructor/methods.

---

## ❓ Question
**32. (Google) Can you use `super` to access a parent's static variable?**

Technically yes (`super.staticVar`), but it's discouraged — static members belong to the class, not instances, so accessing them via `ClassName.staticVar` is the recommended style.

---

## ❓ Question
**33. (Oracle) What happens if the parent class is `final`?**

You cannot extend a `final` class at all, so the question of using `super` inside a subclass never arises — there can be no subclass.

---

## ❓ Question
**34. (Microsoft) Is `super` available inside static nested classes?**

Only in the context of that nested class's own inheritance hierarchy — it doesn't let a static nested class reach the enclosing outer class's members; that requires `OuterClass.this` instead (for non-static inner classes).

---

## ❓ Question
**35. (Deloitte) Can `super()` be conditional, e.g., inside an `if` block?**

No — `super()` must be an unconditional first statement in the constructor; it cannot be wrapped in conditional logic.

---

## ❓ Question
**36. (Accenture) How does `super` behave differently for fields versus methods regarding polymorphism?**

Fields are **not polymorphic** (resolved by reference type at compile time), so `super.field` simply grabs the parent's declared field. Methods **are polymorphic** by default, so `super.method()` is the deliberate way to bypass dynamic dispatch and force the parent's version.

---

## ❓ Question
**37. (IBM) What's a real-world use case for `super()` with parameters in enterprise code?**

Common in layered domain models — e.g., a `PremiumCustomer` subclass calling `super(name, id, tier)` to initialize shared `Customer` fields before adding premium-specific setup like loyalty points.

---

## ❓ Question
**38. (HCL) Does overriding `equals()` typically call `super.equals()`?**

Often yes, especially in class hierarchies, to first ensure the parent's structural checks pass (like type checks) before comparing subclass-specific fields.

---

## ❓ Question
**39. (Mindtree) Can `super` be used within a lambda expression inside a subclass method?**

Yes — lambdas don't create their own `this`/`super` context (unlike anonymous inner classes), so `super` inside a lambda refers to the enclosing class's parent, same as normal code there.

---

## ❓ Question
**40. (LTI) What's the risk of overusing `super.method()` calls throughout a codebase?**

It can tightly couple subclasses to specific parent implementation details, making refactoring the parent class riskier — a sign that composition might be a better design choice than deep inheritance chains.

---
