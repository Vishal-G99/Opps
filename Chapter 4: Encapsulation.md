# Chapter 4: Encapsulation

---

## ❓ Question
**What is Encapsulation?**

Encapsulation is one of the four fundamental principles of Object-Oriented Programming (OOP). It is the process of **wrapping data (variables) and methods (functions) into a single unit (class)** while restricting direct access to the internal data.

The main goal of encapsulation is to **protect the object's state** and ensure that data can only be accessed or modified through controlled methods.

### Key Points

- Bundles data and behavior into a single class.
- Prevents unauthorized access to internal data.
- Achieved using **private fields** and **public getter/setter methods**.
- Improves security and maintainability.
- Allows validation before modifying data.

### Syntax

```java
class Employee {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

### Example

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}

public class Main {

    public static void main(String[] args) {

        BankAccount account = new BankAccount();

        account.deposit(5000);

        System.out.println(account.getBalance());
    }
}
```

**Output**

```
5000.0
```

### Real-Life Example

Think of an **ATM Machine**.

- Your bank balance is hidden.
- You cannot directly modify your balance.
- You interact through options like Deposit, Withdraw, and Check Balance.

The ATM acts as the controlled interface.

---

# Data Hiding

---

## ❓ Question
**What is Data Hiding?**

Data Hiding is the practice of **restricting direct access to an object's data** by making variables private.

Only selected methods are allowed to read or modify the data.

### Key Points

- Protects data from unauthorized access.
- Implemented using the **private** access modifier.
- External classes cannot directly access private fields.
- Access is provided through getters and setters.

### Example

```java
class Student {

    private int marks;

    public void setMarks(int marks) {
        this.marks = marks;
    }

    public int getMarks() {
        return marks;
    }
}

public class Main {

    public static void main(String[] args) {

        Student s = new Student();

        // s.marks = 95; // Compilation Error

        s.setMarks(95);

        System.out.println(s.getMarks());
    }
}
```

### Output

```
95
```

### Why Data Hiding?

Without data hiding:

```java
student.marks = -100;
```

Invalid data enters the object.

With encapsulation:

```java
public void setMarks(int marks) {

    if (marks >= 0 && marks <= 100) {
        this.marks = marks;
    }
}
```

Only valid values are accepted.

---

# Information Hiding

---

## ❓ Question
**What is Information Hiding?**

Information Hiding means **hiding the internal implementation details** and exposing only what is necessary to the user.

The user knows **what** an object does but not **how** it does it.

### Key Points

- Hides implementation.
- Shows only required functionality.
- Makes code easier to change.
- Improves maintainability.

### Example

```java
class Car {

    public void start() {
        startEngine();
        injectFuel();
        igniteSpark();
        System.out.println("Car Started");
    }

    private void startEngine() {
        System.out.println("Engine Started");
    }

    private void injectFuel() {
        System.out.println("Fuel Injected");
    }

    private void igniteSpark() {
        System.out.println("Spark Ignited");
    }
}

public class Main {

    public static void main(String[] args) {

        Car car = new Car();

        car.start();
    }
}
```

Output

```
Engine Started
Fuel Injected
Spark Ignited
Car Started
```

The user only calls

```java
car.start();
```

The internal implementation remains hidden.

---

# Getters

---

## ❓ Question
**What is a Getter?**

A Getter is a public method used to **read the value of a private variable**.

It provides controlled access to private fields.

### Syntax

```java
public DataType getVariableName() {
    return variable;
}
```

### Example

```java
class Employee {

    private String name = "Rahul";

    public String getName() {
        return name;
    }
}

public class Main {

    public static void main(String[] args) {

        Employee emp = new Employee();

        System.out.println(emp.getName());
    }
}
```

Output

```
Rahul
```

### Benefits

- Read-only access
- Security
- Encapsulation

---

# Setters

---

## ❓ Question
**What is a Setter?**

A Setter is a public method used to **modify the value of a private variable**.

It allows validation before updating data.

### Syntax

```java
public void setVariableName(DataType value) {
    this.variable = value;
}
```

### Example

```java
class Employee {

    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

public class Main {

    public static void main(String[] args) {

        Employee emp = new Employee();

        emp.setName("John");

        System.out.println(emp.getName());
    }
}
```

Output

```
John
```

---

# Immutable Objects

---

## ❓ Question
**What is an Immutable Object?**

An Immutable Object is an object **whose state cannot be changed after it is created.**

Once initialized, its data remains constant.

### Characteristics

- Class is final (recommended)
- Fields are private and final
- No setter methods
- Values assigned through constructor

### Example

```java
final class Employee {

    private final int id;
    private final String name;

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}

public class Main {

    public static void main(String[] args) {

        Employee emp = new Employee(1, "Rahul");

        System.out.println(emp.getName());

        // emp.setName("Amit"); // Not Possible
    }
}
```

Output

```
Rahul
```

### Benefits

- Thread-safe
- Secure
- Easier debugging
- Safe sharing between threads

---

# Why Private?

---

## ❓ Question
**Why do we use the `private` keyword?**

The `private` keyword restricts access to class members.

Only the same class can directly access private variables and methods.

### Advantages

- Prevents accidental modification.
- Protects sensitive data.
- Enables validation.
- Supports encapsulation.

### Example

```java
class Person {

    private int age;

    public void setAge(int age) {

        if (age >= 0) {
            this.age = age;
        }
    }

    public int getAge() {
        return age;
    }
}
```

Without private:

```java
person.age = -10;
```

With private:

```java
person.setAge(-10);
```

Validation prevents invalid data.

---

# Validation

---

## ❓ Question
**Why is Validation important in Encapsulation?**

Validation ensures that only **correct and meaningful data** is stored in an object.

Instead of allowing invalid values, setters verify the input.

### Example

```java
class Employee {

    private double salary;

    public void setSalary(double salary) {

        if (salary > 0) {
            this.salary = salary;
        } else {
            System.out.println("Invalid Salary");
        }
    }

    public double getSalary() {
        return salary;
    }
}

public class Main {

    public static void main(String[] args) {

        Employee emp = new Employee();

        emp.setSalary(-5000);

        System.out.println(emp.getSalary());
    }
}
```

Output

```
Invalid Salary
0.0
```

### Common Validation Examples

- Age cannot be negative.
- Salary cannot be negative.
- Email should contain '@'.
- Password length must be at least 8 characters.
- Marks should be between 0 and 100.

---

# Real Banking Example

---

## ❓ Question
**How is Encapsulation used in Banking Applications?**

Banking applications never allow users to directly modify their account balance.

Instead, all operations are performed through methods like deposit and withdraw.

### Example

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }

    public void withdraw(double amount) {

        if (amount > 0 && amount <= balance) {
            balance -= amount;
        } else {
            System.out.println("Insufficient Balance");
        }
    }

    public double getBalance() {
        return balance;
    }
}

public class Main {

    public static void main(String[] args) {

        BankAccount account = new BankAccount();

        account.deposit(10000);

        account.withdraw(2500);

        System.out.println(account.getBalance());
    }
}
```

Output

```
7500.0
```

Benefits

- Prevents negative balance.
- Ensures secure transactions.
- Centralized validation.
- Easy auditing.

---

# Spring Boot DTO Example

---

## ❓ Question
**How is Encapsulation used in Spring Boot DTOs?**

DTO (Data Transfer Object) classes encapsulate request and response data.

Private fields are accessed through getters and setters.

### Example DTO

```java
public class EmployeeDTO {

    private Long id;
    private String name;
    private String email;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

Controller Example

```java
@RestController
@RequestMapping("/employees")
public class EmployeeController {

    @PostMapping
    public String save(@RequestBody EmployeeDTO dto) {

        return "Employee Saved : " + dto.getName();
    }
}
```

### Why DTOs use Encapsulation

- Prevent direct field modification.
- Easy validation using Bean Validation.
- Secure data transfer.
- Clean architecture.

---

# Best Practices

---

## ❓ Question
**What are the Best Practices for Encapsulation?**

Follow these practices to write secure and maintainable Java code.

### Best Practices

- Make instance variables `private`.
- Expose only required getters and setters.
- Validate input in setter methods.
- Prefer immutable objects whenever possible.
- Never expose mutable internal objects directly.
- Use constructors to initialize mandatory fields.
- Use DTOs to transfer data between layers.
- Keep business logic inside the class.
- Follow the Principle of Least Privilege.
- Use Bean Validation (`@NotNull`, `@Size`, `@Email`) in Spring Boot DTOs.

### Good Example

```java
public class User {

    private String email;

    public void setEmail(String email) {

        if (email != null && email.contains("@")) {
            this.email = email;
        }
    }

    public String getEmail() {
        return email;
    }
}
```

### Bad Example

```java
public class User {

    public String email;
}
```

Anyone can modify the field directly, leading to invalid or insecure data.

---

## ✅ Summary

| Concept | Purpose |
|----------|----------|
| Encapsulation | Wrap data and methods into one unit |
| Data Hiding | Hide variables using `private` |
| Information Hiding | Hide implementation details |
| Getter | Read private data |
| Setter | Modify private data with validation |
| Immutable Object | Object whose state never changes |
| Private | Restricts direct access |
| Validation | Prevents invalid data |
| Banking Example | Secure account operations |
| Spring Boot DTO | Safe data transfer between layers |
| Best Practices | Write secure and maintainable code |




# 40 Java Encapsulation Interview Questions with Answers

---

## 1. What is Encapsulation?

Encapsulation is the process of wrapping data (variables) and methods (functions) into a single unit (class) while restricting direct access to the data using access modifiers.

---

## 2. Why is Encapsulation important?

Encapsulation protects data from unauthorized access, improves security, enables validation, reduces coupling, and makes code easier to maintain.

---

## 3. How is Encapsulation achieved in Java?

By:
- Declaring fields as `private`
- Providing controlled access using `public` getter and setter methods

---

## 4. What is the difference between Encapsulation and Data Hiding?

**Encapsulation** is wrapping data and methods together.

**Data Hiding** is restricting direct access to data using access modifiers.

Data hiding is one part of encapsulation.

---

## 5. What is the difference between Encapsulation and Abstraction?

| Encapsulation | Abstraction |
|---------------|-------------|
| Hides data | Hides implementation |
| Achieved using private fields | Achieved using abstract classes and interfaces |
| Focuses on security | Focuses on simplicity |

---

## 6. Which access modifier is mostly used for Encapsulation?

`private`

Because private members cannot be accessed directly outside the class.

---

## 7. Why are fields generally declared private?

To:
- Prevent unauthorized modification
- Allow validation
- Protect object integrity
- Improve maintainability

---

## 8. What is Data Hiding?

Data hiding is restricting direct access to variables by declaring them private.

---

## 9. What is Information Hiding?

Information hiding means hiding implementation details and exposing only necessary functionality.

---

## 10. What is a Getter?

A getter is a public method used to read the value of a private field.

Example:

```java
public String getName() {
    return name;
}
```

---

## 11. What is a Setter?

A setter is a public method used to update a private field.

Example:

```java
public void setName(String name) {
    this.name = name;
}
```

---

## 12. Why should setters perform validation?

To prevent invalid data from entering an object.

Example:

```java
if(age >= 18)
```

instead of directly assigning the value.

---

## 13. Can we have a Getter without a Setter?

Yes.

This creates a read-only property.

Example:

```java
private final int id;

public int getId() {
    return id;
}
```

---

## 14. Can we have a Setter without a Getter?

Yes.

Useful for write-only fields like passwords.

```java
public void setPassword(String password) {
    this.password = password;
}
```

---

## 15. What happens if fields are public?

Anyone can modify them directly.

```java
employee.salary = -10000;
```

No validation is possible.

---

## 16. Why is direct access to variables discouraged?

Because:
- Invalid data may enter
- Security is reduced
- Business rules are bypassed

---

## 17. Can Encapsulation improve security?

Yes.

Sensitive information remains protected and only controlled methods can modify it.

---

## 18. Does Encapsulation improve maintainability?

Yes.

Implementation changes usually don't affect client code.

---

## 19. Can private methods be inherited?

No.

Private methods belong only to the declaring class.

---

## 20. Can private variables be inherited?

Yes.

They exist in child objects but cannot be accessed directly.

Access is through inherited methods.

---

## 21. Can a constructor access private fields?

Yes.

Constructors belong to the same class.

---

## 22. Can static methods access private variables?

Yes.

If the variable is static.

Otherwise an object is required.

---

## 23. Can another object of the same class access private fields?

Yes.

Private access is class-based, not object-based.

Example:

```java
class Employee {

    private int id;

    void copy(Employee e){
        this.id = e.id;
    }
}
```

---

## 24. Does Encapsulation affect performance?

No significant impact.

Getter and setter methods are usually optimized by the JVM.

---

## 25. What is an Immutable Object?

An object whose state cannot change after creation.

Example:
- String
- LocalDate
- LocalDateTime

---

## 26. How do immutable objects support Encapsulation?

They prevent accidental modification and make objects thread-safe.

---

## 27. Why is String immutable?

For:
- Security
- Thread safety
- String pool optimization
- Hashcode caching

---

## 28. Can immutable classes have setters?

No.

Providing setters would allow modification.

---

## 29. What are the characteristics of an immutable class?

- final class
- private final fields
- Constructor initialization
- No setters
- Defensive copies for mutable objects

---

## 30. What is defensive copying?

Returning a copy instead of the original mutable object.

Example:

```java
return new Date(date.getTime());
```

---

## 31. Why should mutable objects not be exposed directly?

External code can modify them unexpectedly.

---

## 32. How is Encapsulation used in Spring Boot DTOs?

DTO fields are private and accessed through getters and setters.

Frameworks like Jackson use these methods during serialization and deserialization.

---

## 33. How does Bean Validation support Encapsulation?

Validation annotations ensure only valid data enters the object.

Example:

```java
@NotBlank
@Email
@Size(min = 8)
```

---

## 34. Can Lombok support Encapsulation?

Yes.

Annotations like:

```java
@Getter
@Setter
```

generate getter and setter methods automatically.

---

## 35. What is the advantage of using Lombok with Encapsulation?

- Less boilerplate code
- Cleaner classes
- Easier maintenance

---

## 36. Is Encapsulation related to Loose Coupling?

Yes.

Since implementation is hidden, changes inside a class usually don't affect other classes.

---

## 37. Give a real-world example of Encapsulation.

A bank account.

Users cannot directly modify the balance.

They must use:

- deposit()
- withdraw()
- getBalance()

---

## 38. What are common mistakes in Encapsulation?

- Public fields
- Missing validation
- Returning mutable objects directly
- Providing unnecessary setters
- Breaking immutability

---

## 39. What are Encapsulation best practices?

- Make fields private
- Validate input
- Keep business logic inside the class
- Prefer immutable objects
- Expose only required methods
- Follow least privilege principle

---

## 40. Interview Scenario

### Question

You have the following class:

```java
class Employee {

    public int salary;
}
```

What's wrong with it?

### Answer

Problems:

- No encapsulation
- Anyone can modify salary
- Invalid values are possible
- No validation
- Business rules cannot be enforced

Better implementation:

```java
class Employee {

    private int salary;

    public int getSalary() {
        return salary;
    }

    public void setSalary(int salary) {

        if (salary >= 0) {
            this.salary = salary;
        }
    }
}
```

This implementation:
- Protects the data
- Allows validation
- Prevents invalid values
- Follows encapsulation principles
