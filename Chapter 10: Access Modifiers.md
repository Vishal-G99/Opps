# Chapter 10: Access Modifiers

---

## ❓ Question
**What is the `private` access modifier?**

`private` restricts access to **within the same class only**. Not even subclasses or other classes in the same package can access a private member directly.

```java
class Account {
    private double balance = 1000;

    private void logTransaction() {
        System.out.println("Transaction logged");
    }

    public double getBalance() { // controlled access via public method
        return balance;
    }
}

// Account acc = new Account();
// acc.balance;         // COMPILE ERROR — not accessible
// acc.getBalance();     // OK — 1000.0
```

Used for **encapsulation** — hiding internal implementation details.

---

## ❓ Question
**What is the `default` (package-private) access modifier?**

When **no modifier** is written, the member is accessible only **within the same package** — not from subclasses or classes in other packages.

```java
package com.bank;

class Account {          // default access — no modifier
    double balance = 1000; // default access field

    void showBalance() {   // default access method
        System.out.println(balance);
    }
}
```

```java
package com.bank;
class Branch {
    void test() {
        Account acc = new Account();
        acc.showBalance(); // OK — same package
    }
}
```

```java
package com.other;
import com.bank.Account;
class Test {
    void test() {
        // Account acc = new Account(); // COMPILE ERROR — class itself not accessible outside package
    }
}
```

---

## ❓ Question
**What is the `protected` access modifier?**

`protected` allows access within the **same package**, plus from **subclasses in other packages** (via inheritance).

```java
package com.bank;

public class Account {
    protected double balance = 1000;
}
```

```java
package com.other;
import com.bank.Account;

class SavingsAccount extends Account {
    void show() {
        System.out.println(balance); // OK — accessible via inheritance, even in a different package
    }
}
```

```java
package com.other;
import com.bank.Account;

class Unrelated {
    void test() {
        Account acc = new Account();
        // System.out.println(acc.balance); // COMPILE ERROR — not a subclass, different package
    }
}
```

---

## ❓ Question
**What is the `public` access modifier?**

`public` means the member (or class) is accessible from **anywhere** — any class, in any package.

```java
package com.bank;

public class Account {
    public double balance = 1000;

    public void deposit(double amt) {
        balance += amt;
    }
}
```

```java
package com.other;
import com.bank.Account;

class Test {
    void test() {
        Account acc = new Account();
        acc.balance = 2000;   // OK — fully public
        acc.deposit(500);      // OK
    }
}
```

---

## ❓ Question
**How do Access Modifiers interact with Packages?**

```
Modifier      Same Class   Same Package   Subclass (diff. package)   Everywhere
─────────────────────────────────────────────────────────────────────────────
private            ✅             ❌                  ❌                  ❌
default (none)     ✅             ✅                  ❌                  ❌
protected          ✅             ✅                  ✅                  ❌
public             ✅             ✅                  ✅                  ✅
```

A **package** groups related classes together and forms a natural access boundary — `default` and `protected` are both defined relative to package membership.

```java
package com.company.model;   // package declaration must be the first line in the file
import com.company.util.Helper; // importing a class from another package
```

---

## ❓ Question
**Can you show a Real Example combining all four modifiers in one class?**

```java
package com.bank;

public class BankAccount {
    private double balance;              // hidden — only this class can touch it directly
    String accountType = "Savings";       // default — visible to other classes in com.bank only
    protected String branch = "Main";      // visible to package + subclasses elsewhere
    public String accountHolder;           // fully open

    public BankAccount(String holder, double balance) {
        this.accountHolder = holder;
        this.balance = balance;
    }

    private boolean isValidAmount(double amt) { // internal helper, hidden from outside
        return amt > 0;
    }

    public void deposit(double amt) {            // public gateway that safely uses the private helper
        if (isValidAmount(amt)) {
            balance += amt;
        }
    }

    public double getBalance() {                  // controlled, read-only access to private state
        return balance;
    }
}
```

This is the standard **encapsulation pattern**: keep fields `private`, expose behavior through carefully chosen `public` methods.

---

## ❓ Question
**What are Best Practices for choosing Access Modifiers?**

- **Default to the most restrictive modifier possible** — start with `private`, widen only when truly needed.
- Fields should almost always be `private`, exposed via `public` getters/setters (or none, for immutability).
- Use `protected` sparingly — it exposes internals to any subclass anywhere, which can make refactoring the parent class riskier.
- Helper/utility methods that aren't part of the public API should be `private` or `default` (package-private), not `public`.
- Classes themselves default to package-private unless they need to be used outside their package — don't make everything `public` by habit.
- In layered architectures (e.g., Spring Boot), keep implementation classes package-private where possible and expose only interfaces publicly.

---

# 🎯 40 Interview Questions — Access Modifiers

## ❓ Question
**1. (TCS) Can a top-level class be declared `private` or `protected`?**

No. Top-level classes can only be `public` or default (package-private) — `private`/`protected` are only valid for members (fields, methods, nested classes), not top-level classes.

---

## ❓ Question
**2. (Infosys) What's the difference between `default` and `protected` access?**

`default` allows access only within the same package. `protected` allows that too, plus access from subclasses even in different packages.

---

## ❓ Question
**3. (Wipro) Can a subclass in a different package override a `protected` method and make it `public`?**

Yes — you can always widen access when overriding (e.g., `protected` → `public`), but you cannot narrow it (e.g., `public` → `protected`).

---

## ❓ Question
**4. (Amazon) Why is it a compile error to reduce visibility when overriding a method?**

It would violate the parent contract (Liskov Substitution Principle) — code relying on the parent's public method being callable would break if a subclass secretly made it less accessible.

---

## ❓ Question
**5. (Google) Can constructors have access modifiers?**

Yes. A `private` constructor is commonly used to enforce the Singleton pattern or prevent instantiation of utility classes.

---

## ❓ Question
**6. (Accenture) What access level do interface methods have by default?**

`public` — all interface methods are implicitly `public` (and abstract, unless `default`/`static`/`private`), even without writing the modifier.

---

## ❓ Question
**7. (Cognizant) Can interface methods be `private`?**

Yes, since Java 9 — `private` interface methods are allowed, but only to share code between the interface's own `default`/`static` methods; they can't be implemented or called externally.

---

## ❓ Question
**8. (Capgemini) Why are fields typically made `private` in real projects?**

To enforce encapsulation — controlling how a field's value can be read or changed via getters/setters, allowing validation, computed values, or future changes without breaking external code.

---

## ❓ Question
**9. (Deloitte) Can a class outside the package access a `protected` field without inheriting the class?**

No — `protected` access from outside the package **requires** the accessing class to be a subclass; unrelated classes in other packages cannot access it directly.

---

## ❓ Question
**10. (Oracle) What happens if you don't specify any access modifier for a class member?**

It gets **default (package-private)** access — visible only within the same package.

---

## ❓ Question
**11. (Microsoft) Is it possible to access a `private` method via reflection?**

Yes, technically — using `setAccessible(true)` on the `Method` object, reflection can bypass normal access control, though this is discouraged outside frameworks/testing tools.

---

## ❓ Question
**12. (IBM) Can two top-level public classes exist in the same `.java` file?**

No — a Java source file can have at most **one** `public` top-level class, and the file name must match that class's name.

---

## ❓ Question
**13. (HCL) What's the access modifier rule for a `.java` file's public class filename?**

The public class name must exactly match the file name (e.g., `public class Account` must be in `Account.java`).

---

## ❓ Question
**14. (Mindtree) Can enum constants have different access levels than the enum itself?**

Enum constants are implicitly `public static final`; you can't apply different modifiers to individual constants, though enum-specific methods/fields can have their own modifiers.

---

## ❓ Question
**15. (LTI) Why might a class be made package-private (default) instead of public in a well-designed library?**

To hide implementation classes that shouldn't be part of the library's public API, reducing the surface area users depend on and allowing internal refactoring freedom.

---

## ❓ Question
**16. (Amazon) Can a `private` field in a parent class be accessed by a subclass at all?**

No — private members are not inherited/visible in subclasses; the subclass would need a `protected` or `public` accessor method in the parent to reach that value.

---

## ❓ Question
**17. (Flipkart) What is the visibility of a nested `private static class`?**

It's accessible only within the enclosing top-level class — commonly used for internal helper classes like `Node` in a linked list implementation.

---

## ❓ Question
**18. (Paytm) Can constructors be `protected`, and why would you do that?**

Yes — often used when you want subclasses (possibly in other packages) to be able to construct the object, but prevent arbitrary external instantiation.

---

## ❓ Question
**19. (Zoho) Does Java enforce access modifiers at compile time, runtime, or both?**

Primarily at **compile time**, but the JVM also performs runtime access checks (e.g., via the `AccessibleObject`/reflection APIs and bytecode verification) as a security layer.

---

## ❓ Question
**20. (Freshworks) Can an abstract method be `private`?**

No — `private` methods can't be overridden (they aren't inherited/visible to subclasses), which directly contradicts what an abstract method requires: to be implemented by subclasses.

---

## ❓ Question
**21. (Adobe) In Spring Boot, why are `@Repository` implementation classes often package-private?**

To hide the underlying data-access implementation details, exposing only the interface publicly — callers depend on the interface's contract, not the concrete class.

---

## ❓ Question
**22. (SAP) Can a `public` method exist inside a `default`-access (package-private) class?**

Yes — the class itself is limited to the package, but a `public` method inside it is still only reachable if you already have access to the class instance (typically via a factory method or interface in the same package).

---

## ❓ Question
**23. (JPMorgan) Why is `protected` considered "riskier" than `default` for library design?**

Because `protected` exposes internals to **any subclass in any package**, meaning external code (which you don't control) can extend your class and depend on that field/method — restricting future changes.

---

## ❓ Question
**24. (Goldman Sachs) Can access modifiers affect method overloading resolution?**

No — overload resolution is based on method signatures (name + parameter types), not access modifiers; different access levels don't distinguish overloads.

---

## ❓ Question
**25. (Morgan Stanley) What access level should utility/helper classes with only static methods have their constructor set to?**

`private` — to explicitly prevent instantiation, since the class is meant to be used only via its static methods (e.g., `Math`-style utility classes).

---

## ❓ Question
**26. (Infosys) Is it valid to have a `public` field with a `private` setter method pattern?**

It's uncommon and generally poor practice — having a directly mutable `public` field defeats the purpose of controlling mutation via a setter; fields are usually `private` with `public` getters/setters instead.

---

## ❓ Question
**27. (Wipro) Can annotations have access modifiers on their elements?**

Annotation type elements are implicitly `public`; you cannot apply other access modifiers to them.

---

## ❓ Question
**28. (TCS) What access modifier does a local variable have?**

None — access modifiers apply only to class members and top-level classes. Local variables are scoped to the block they're declared in, not controlled via `public`/`private`/etc.

---

## ❓ Question
**29. (Capgemini) Can you change a field's access modifier without breaking binary compatibility?**

Widening access (e.g., `private` → `public`) is generally safe; narrowing it (`public` → `private`) can break any external code that depended on the wider access, causing compile errors for dependents.

---

## ❓ Question
**30. (Cognizant) How do access modifiers support the Open-Closed Principle?**

By hiding internal fields/methods as `private` and exposing only stable `public` methods, a class can change its internal implementation freely ("closed for modification" externally) while still being extendable through its public contract.

---

## ❓ Question
**31. (Amazon) Can a subclass access a `protected` constructor from a completely different package to instantiate the parent directly (not via inheritance)?**

No — `protected` constructor access from another package is only usable through the subclass's own constructor chain (via `super(...)`), not by directly calling `new ParentClass()` from unrelated code.

---

## ❓ Question
**32. (Google) Why does Java not have a "package-protected explicitly written" keyword?**

Because omitting any modifier already conventionally means package-private — Java's designers chose to make it the implicit default rather than requiring an explicit keyword like some other languages do.

---

## ❓ Question
**33. (Oracle) Can access modifiers be applied to local inner classes?**

No — local classes (declared inside a method body) cannot have access modifiers like `public`/`private`/`protected`; they're scoped entirely to that method.

---

## ❓ Question
**34. (Microsoft) What's the difference in accessibility between `record` components and traditional class fields?**

Record components are implicitly `private final`, with `public` accessor methods auto-generated — you can't independently choose different access levels for a record's core fields.

---

## ❓ Question
**35. (Deloitte) How do modules (Java 9+ JPMS) interact with `public` access?**

A module can restrict which packages it `exports`; even `public` classes/members in a non-exported package are inaccessible to other modules, adding a layer of encapsulation beyond traditional access modifiers.

---

## ❓ Question
**36. (Accenture) Can getter/setter methods have different access levels than the field they wrap?**

Yes, commonly — a field might be `private` while the getter is `public` but the setter is `protected` or omitted entirely, to allow read access broadly while restricting mutation.

---

## ❓ Question
**37. (IBM) Why might a `public` class have a `private` inner class?**

To encapsulate helper logic used only internally (e.g., iterator implementations, internal node structures) without exposing that implementation detail as part of the class's public API.

---

## ❓ Question
**38. (HCL) Is method overloading affected if two overloaded methods have different access modifiers?**

No — Java allows overloaded methods to have different access modifiers; overload resolution only considers the method signature, not access level.

---

## ❓ Question
**39. (Mindtree) What access modifier issue commonly arises with JavaBeans-style POJOs in frameworks like Hibernate?**

Hibernate requires accessible (often `public` or at least `protected`/package-visible) no-arg constructors and getters/setters via reflection, so overly restrictive `private` constructors can break ORM instantiation unless the framework uses reflection's `setAccessible`.

---

## ❓ Question
**40. (LTI) What's a common real-world mistake with access modifiers in layered Spring Boot applications?**

Making `@Service`/`@Repository` fields or helper methods `public` unnecessarily, exposing internal implementation details that should remain `private`, which increases coupling and makes safe refactoring harder across the codebase.

---
