# Chapter 19: OOP in Spring Boot

---

## ❓ Question
**What is IoC (Inversion of Control)?**

IoC is a design principle where the **control of object creation and dependency management is inverted** — instead of your code creating its own dependencies with `new`, an external container (the Spring `ApplicationContext`) creates and manages them for you.

```java
// WITHOUT IoC — class controls its own dependency creation
class OrderService {
    private PaymentGateway gateway = new StripeGateway(); // tightly coupled
}

// WITH IoC — Spring container controls creation and hands the dependency to the class
@Service
class OrderService {
    private final PaymentGateway gateway;

    @Autowired
    OrderService(PaymentGateway gateway) { // Spring supplies this
        this.gateway = gateway;
    }
}
```

---

## ❓ Question
**What is DI (Dependency Injection)?**

DI is the **mechanism** that implements IoC — dependencies are "injected" into a class from outside, typically via constructor, field, or setter injection.

```java
@Service
class NotificationService {
    // Constructor injection (recommended) — dependency is mandatory & final
    private final EmailSender emailSender;

    @Autowired
    NotificationService(EmailSender emailSender) {
        this.emailSender = emailSender;
    }
}

@Service
class ReportService {
    @Autowired // Field injection — simpler syntax, but harder to test/less explicit
    private PdfGenerator pdfGenerator;
}
```

---

## ❓ Question
**What is the Spring Bean Lifecycle?**

```
1. Instantiation        → Spring creates the object (via reflection, calling the constructor)
2. Populate Properties   → dependencies injected (@Autowired fields/setters)
3. @PostConstruct         → custom initialization logic runs
4. Bean Ready              → available in the ApplicationContext for use
5. @PreDestroy               → cleanup logic runs before context shutdown
6. Bean Destroyed             → removed from the container
```

```java
@Component
class CacheWarmer {
    @PostConstruct
    void init() { System.out.println("Cache warmed up"); }

    @PreDestroy
    void cleanup() { System.out.println("Cache cleared"); }
}
```

---

## ❓ Question
**What is `@Component`, and how do `@Service`/`@Repository`/`@Controller` relate to it?**

`@Component` is the generic stereotype annotation telling Spring "manage this class as a bean." `@Service`, `@Repository`, and `@Controller` are all **specializations** of `@Component` — functionally identical for bean registration, but semantically indicating each class's architectural role.

```java
@Component  // generic — any Spring-managed class
class UtilityHelper { }

@Service    // business/service layer
class OrderService { }

@Repository // data access layer — also enables exception translation for persistence errors
class OrderRepository { }

@Controller // web layer — handles HTTP requests
class OrderController { }
```

---

## ❓ Question
**What does `@Autowired` do, and what is a Proxy in this context?**

`@Autowired` tells Spring to **automatically inject** a matching bean. For certain features (like `@Transactional` or AOP), Spring wraps your actual bean in a **dynamic proxy** — a runtime-generated subclass or JDK interface implementation that intercepts method calls to add extra behavior before delegating to your real object.

```java
@Service
class PaymentService {
    @Transactional // Spring wraps this bean in a proxy that manages the transaction boundary
    void processPayment() {
        // actual business logic — the proxy handles commit/rollback around this call
    }
}
```

```
Client calls paymentService.processPayment()
        │
        ▼
   [Proxy Object] ── begins transaction
        │
        ▼
  [Real PaymentService] ── executes actual method body
        │
        ▼
   [Proxy Object] ── commits or rolls back transaction
```

---

## ❓ Question
**What is AOP (Aspect-Oriented Programming) in Spring?**

AOP lets you define **cross-cutting concerns** (logging, security, transactions, caching) separately from your core business logic, applying them declaratively via annotations without cluttering every method with repetitive boilerplate.

```java
@Aspect
@Component
class LoggingAspect {
    @Before("execution(* com.app.service.*.*(..))") // runs before any service method
    void logBefore(JoinPoint jp) {
        System.out.println("Calling: " + jp.getSignature().getName());
    }
}

@Service
class OrderService {
    void placeOrder() {
        // no logging code needed here — the aspect handles it automatically
    }
}
```

AOP is implemented internally using the same proxy mechanism — the aspect's logic is woven in via a proxy that intercepts method calls.

---

# 🎯 40 Interview Questions — OOP in Spring Boot

## ❓ Question
**1. (TCS) What is the core benefit of IoC in application design?**

It decouples classes from the responsibility of creating their own dependencies, making the system more modular, testable, and easier to reconfigure without changing business logic code.

---

## ❓ Question
**2. (Infosys) What are the three main types of Dependency Injection in Spring?**

Constructor injection, setter injection, and field injection.

---

## ❓ Question
**3. (Wipro) Why is constructor injection generally preferred over field injection?**

It makes dependencies explicit and immutable (`final` fields), ensures the object is never in an incomplete state, and makes unit testing straightforward without needing a Spring context or reflection-based mocking.

---

## ❓ Question
**4. (Amazon) What is the default scope of a Spring Bean?**

Singleton — exactly one shared instance per bean definition within the ApplicationContext.

---

## ❓ Question
**5. (Google) How would you make a Spring Bean created fresh on every request?**

Annotate it with `@Scope("prototype")`.

---

## ❓ Question
**6. (Accenture) What does `@Repository` add beyond what `@Component` provides?**

Automatic translation of persistence-related exceptions (like JDBC `SQLException`) into Spring's unified `DataAccessException` hierarchy.

---

## ❓ Question
**7. (Cognizant) What happens if two beans of the same type exist and you use `@Autowired` without further qualification?**

Spring throws a `NoUniqueBeanDefinitionException` — you must disambiguate using `@Qualifier` or `@Primary`.

---

## ❓ Question
**8. (Capgemini) What does `@Primary` do?**

Marks a bean as the default choice when multiple candidates of the same type exist and no explicit qualifier is given.

---

## ❓ Question
**9. (Deloitte) When does `@PostConstruct` run relative to dependency injection?**

After the bean is instantiated and all its dependencies have been injected, but before the bean is made available for use elsewhere in the application.

---

## ❓ Question
**10. (Oracle) What's the difference between BeanFactory and ApplicationContext?**

`BeanFactory` is the basic IoC container interface with lazy initialization by default. `ApplicationContext` extends it with additional enterprise features (event handling, AOP integration, eager singleton initialization by default, internationalization support).

---

## ❓ Question
**11. (Microsoft) Why can't `@Transactional` work correctly on a private method?**

Because Spring's proxy-based AOP intercepts calls at the public method level; private methods can't be proxied/intercepted, so the transactional behavior is silently skipped.

---

## ❓ Question
**12. (IBM) What's the difference between JDK dynamic proxies and CGLIB proxies in Spring AOP?**

JDK dynamic proxies require the target class to implement an interface (the proxy implements that interface). CGLIB proxies work by subclassing the actual concrete class at runtime, used when no interface is present.

---

## ❓ Question
**13. (HCL) Why does calling a `@Transactional` method from within the same class (self-invocation) not trigger the proxy behavior?**

Because the call bypasses the proxy entirely — it's a direct internal method call (`this.method()`), not going through the external proxy object that intercepts calls from outside the class.

---

## ❓ Question
**14. (Mindtree) What is the purpose of `@Qualifier`?**

To explicitly specify which bean implementation to inject when multiple candidates of the same type are available, resolving ambiguity that `@Autowired` alone can't handle.

---

## ❓ Question
**15. (LTI) How does Spring's component scanning discover beans automatically?**

Via `@ComponentScan` (often implied by `@SpringBootApplication`), which scans specified packages for classes annotated with `@Component` and its specializations, registering them as bean definitions.

---

## ❓ Question
**16. (Amazon) What's the difference between `@Bean` and `@Component`?**

`@Component` is applied directly on a class you own, letting component scanning discover it automatically. `@Bean` is applied on a method inside a `@Configuration` class, explicitly defining and returning a bean instance — often used for third-party classes you can't annotate directly.

---

## ❓ Question
**17. (Flipkart) Why might field injection make unit testing harder compared to constructor injection?**

Field-injected dependencies can only be set via reflection or a Spring test context, since there's no public constructor/setter accepting them — constructor injection allows simply passing mocks directly when instantiating the class under test.

---

## ❓ Question
**18. (Paytm) What does `@Lazy` do on a bean definition?**

Delays bean creation until it's actually first needed/requested, rather than eagerly during application context startup — useful for beans that are expensive to create but rarely used.

---

## ❓ Question
**19. (Zoho) How does Spring resolve circular dependencies between two beans?**

For singleton beans using setter/field injection, Spring can resolve certain circular dependencies via early proxy references during bean creation; constructor-based circular dependencies are **not** resolvable and throw a `BeanCurrentlyInCreationException`.

---

## ❓ Question
**20. (Freshworks) What annotation combination effectively creates a Spring Boot application's entry point?**

`@SpringBootApplication`, which itself bundles `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`.

---

## ❓ Question
**21. (Adobe) How does `@Value` relate to dependency injection concepts?**

It injects externalized configuration property values (from `application.properties`/`.yml`, environment variables) into bean fields, extending the DI concept beyond just object dependencies to simple configuration values too.

---

## ❓ Question
**22. (SAP) What's a real-world reason to use `@Scope("prototype")` instead of the default singleton?**

For stateful beans that must maintain per-request or per-use isolated state (e.g., a multi-step wizard/form object) that shouldn't be shared across concurrent users.

---

## ❓ Question
**23. (JPMorgan) How does Spring's proxy mechanism enable declarative security (`@PreAuthorize`)?**

The proxy intercepts the annotated method call, checks the security expression against the current authentication context **before** delegating to the actual method body, throwing an access-denied exception if the check fails.

---

## ❓ Question
**24. (Goldman Sachs) Why is it considered good practice to depend on interfaces rather than concrete `@Service` classes when injecting dependencies?**

It follows the Dependency Inversion Principle, keeps classes loosely coupled, and makes it trivial to swap implementations (e.g., for testing) without touching the dependent class's code.

---

## ❓ Question
**25. (Morgan Stanley) What is the purpose of `@Configuration` classes in Spring?**

To define one or more `@Bean` methods explicitly, acting as a Java-based (rather than XML-based) source of bean definitions for the ApplicationContext.

---

## ❓ Question
**26. (Infosys) How does AOP's "pointcut" expression determine where advice is applied?**

A pointcut expression (e.g., `execution(* com.app.service.*.*(..))`) defines a pattern matching specific methods/classes/packages, and the associated advice logic (`@Before`, `@After`, `@Around`, etc.) is woven in only at those matched join points.

---

## ❓ Question
**27. (Wipro) What's the difference between `@Before`, `@After`, and `@Around` advice in Spring AOP?**

`@Before` runs prior to the target method. `@After` runs after it completes (regardless of outcome). `@Around` wraps the entire call, giving full control to execute logic both before and after, and even to skip or modify the actual method invocation.

---

## ❓ Question
**28. (TCS) Can constructor injection be used with optional dependencies?**

Yes, using `Optional<T>` as the constructor parameter type, or by providing multiple constructors with `@Autowired(required = false)` — though it's often cleaner to design mandatory dependencies as always-required via constructor injection.

---

## ❓ Question
**29. (Capgemini) How does Spring's IoC container achieve loose coupling between the `@Controller`, `@Service`, and `@Repository` layers?**

Each layer depends on the layer below via injected interfaces/abstractions rather than direct instantiation, letting each layer be developed, tested, and swapped independently.

---

## ❓ Question
**30. (Cognizant) What happens internally when Spring creates a singleton bean with circular field-injected dependencies (A needs B, B needs A)?**

Spring creates an early, not-fully-initialized reference for the first bean, injects it into the second, then completes the first bean's initialization — a somewhat fragile mechanism that's generally discouraged in favor of redesigning to avoid circular dependencies altogether.

---

## ❓ Question
**31. (Amazon) Why does Spring Boot favor "convention over configuration" for OOP design in typical applications?**

It reduces boilerplate configuration by providing sensible defaults (auto-configuration, stereotype annotations, component scanning) so developers can focus on business logic rather than wiring plumbing manually.

---

## ❓ Question
**32. (Google) How does `@Transactional`'s propagation behavior relate to proxy-based method interception?**

Propagation settings (like `REQUIRED`, `REQUIRES_NEW`) determine how the proxy manages transaction boundaries when a `@Transactional` method calls another `@Transactional` method — but again, only effective when the call passes through the external proxy, not via internal self-invocation.

---

## ❓ Question
**33. (Oracle) What's the benefit of Spring's event-driven `ApplicationEventPublisher`/`@EventListener` model, tying back to the Observer pattern?**

It decouples the event publisher from event consumers entirely — publishers don't need any reference to listeners, supporting extensible, loosely-coupled cross-cutting workflows (e.g., sending a welcome email after user registration) without tangling that logic into the core registration service.

---

## ❓ Question
**34. (Microsoft) How does Spring Boot support polymorphism through its strategy-style bean injection with `List<InterfaceType>`?**

Spring can inject **all** beans implementing a given interface as a `List<InterfaceType>`, letting application code iterate/dispatch across all available implementations dynamically — a direct OOP polymorphism pattern powered by the container.

---

## ❓ Question
**35. (Deloitte) Why might overusing `@Autowired` field injection be considered to violate encapsulation principles?**

It requires non-`final`, often package-private-or-public-accessible fields solely for reflection-based injection, exposing internal structure and preventing the class from guaranteeing its own invariants at construction time (unlike constructor injection).

---

## ❓ Question
**36. (Accenture) How does dependency injection in Spring embody the Dependency Inversion Principle at the framework level?**

High-level business classes depend on injected abstractions (interfaces) rather than instantiating low-level implementations directly, and the container (not the classes themselves) decides which concrete implementation satisfies that abstraction — exactly DIP's core idea, automated.

---

## ❓ Question
**37. (IBM) What's a practical example of the Decorator pattern appearing via Spring AOP proxies in a real application?**

A `@Cacheable`-annotated service method gets wrapped by a caching proxy that checks the cache first and only delegates to the real method on a cache miss — extending behavior without modifying the original method's code, textbook Decorator semantics.

---

## ❓ Question
**38. (HCL) Why does Spring Boot testing often use `@MockBean` instead of manually constructing mocks?**

`@MockBean` replaces the real bean in the Spring ApplicationContext with a Mockito mock automatically, letting integration-style tests exercise the full DI-wired application context while substituting specific dependencies (like external API clients) with controlled test doubles.

---

## ❓ Question
**39. (Mindtree) How does the Bean lifecycle's `@PreDestroy` phase relate to proper resource cleanup in OOP design?**

It provides a guaranteed hook for releasing resources (closing connections, shutting down thread pools) tied to a bean's lifecycle, similar in spirit to a well-designed class's cleanup responsibilities, but managed centrally by the container rather than manually by each caller.

---

## ❓ Question
**40. (LTI) What's the overall architectural benefit of applying strong OOP principles (SOLID, DI, encapsulation) throughout a Spring Boot application?**

It results in loosely coupled, independently testable layers that can evolve, be mocked, and be maintained by large teams without cascading changes — directly leveraging Spring's container to enforce and simplify good object-oriented design at scale.

---
