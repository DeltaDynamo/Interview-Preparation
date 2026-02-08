## 🟦 DAY 1 — Core Java OOP (DEEP)

### 1️⃣ Runtime Polymorphism (This is BIG)

#### Clear concept

Runtime polymorphism means:

> **The method that gets executed is decided at runtime based on the actual object, not the reference type.**

```java
Parent p = new Child();
p.show();   // Child's show() is called
```

Why?

* Method calls are resolved using **dynamic dispatch**
* JVM looks at the **object on heap**, not reference on stack

---

#### What happens internally (interview gold)

* JVM creates a **virtual method table (v-table)**
* At runtime:

  1. Reference points to object
  2. JVM checks object’s class
  3. Calls overridden method from child

---

#### What can and cannot be polymorphic

✅ Instance methods
❌ Static methods (method hiding)
❌ Private methods
❌ Variables (only reference-based)

---

#### Interview questions

**Q1. Why static methods are not polymorphic?**
Because static methods are:

* Resolved at **compile time**
* Belong to class, not object
* No v-table entry

---

**Q2. Can constructor be overridden?**
❌ No
Constructors initialize objects — they are not inherited.

---

**Q3. Real project usage?**

* Strategy pattern
* Payment methods
* Notification channels

👉 You must give **your own example** here.

---

### 2️⃣ Abstraction vs Encapsulation (They love this)

#### Encapsulation (HOW)

> **Bundling data + methods and hiding internal state**

```java
class Account {
    private double balance;

    public void deposit(double amount) {
        if (amount > 0) balance += amount;
    }
}
```

Why?

* Protect invariants
* Prevent invalid state
* Change internals without breaking users

---

#### Abstraction (WHAT)

> **Exposing only essential behavior, hiding implementation**

```java
interface PaymentService {
    void pay(double amount);
}
```

Why?

* Reduces cognitive load
* Enables loose coupling
* Supports polymorphism

---

#### Interview comparison (say this clearly)

| Encapsulation                 | Abstraction                              |
| ----------------------------- | ---------------------------------------- |
| HOW it works                  | WHAT it does                             |
| Data hiding                   | Design hiding                            |
| Achieved via access modifiers | Achieved via interfaces/abstract classes |

---

#### Interview trap

**Q: Is encapsulation possible without abstraction?**
✅ Yes (POJO with private fields)

**Q: Is abstraction possible without encapsulation?**
❌ In practice, abstraction relies on encapsulation

---

### 3️⃣ Interface vs Abstract Class (VERY common)

#### Interface

* Represents **capability**
* Multiple inheritance supported
* No state (except constants)

```java
interface Flyable {
    void fly();
}
```

---

#### Abstract class

* Represents **“is-a” relationship**
* Can have state
* Partial implementation

```java
abstract class Vehicle {
    int speed;
    abstract void move();
}
```

---

#### When to use what (KEY ANSWER)

Use **interface** when:

* Behavior may be implemented by unrelated classes
* Multiple inheritance is needed

Use **abstract class** when:

* Classes share state + behavior
* You want default implementation

---

#### Interview question

**Q: Why Spring prefers interfaces?**

* Loose coupling
* Easier mocking
* Enables proxying (AOP)

---

### 4️⃣ Composition vs Inheritance (MUST answer well)

#### Inheritance

```java
class Car extends Engine { }
```

Problems:

* Tight coupling
* Fragile base class
* Hard to change

---

#### Composition

```java
class Car {
    private Engine engine;
}
```

Benefits:

* Flexible
* Replace behavior at runtime
* Follows **“favor composition over inheritance”**

---

#### Interview killer question

**Q: Why composition is preferred over inheritance?**
Say this:

> Inheritance couples child to parent’s internal behavior. Composition allows behavior change without breaking hierarchy.

---

### 5️⃣ How this shows in Spring Boot

* Interfaces → Service contracts
* Implementations → Swappable logic
* Dependency Injection → Polymorphism in action

Example:

```java
@Autowired
PaymentService paymentService;
```

Spring decides **which implementation at runtime**.

---

### 🔥 Rapid-Fire Interview Questions (Practice)

Answer these **out loud**:

1. How does JVM decide which method to call at runtime?
2. Why can’t static methods be overridden?
3. Difference between abstraction and encapsulation with example
4. Interface vs abstract class in real project
5. Where did you use composition in your code?

---

### 🧠 Homework (Very important)

Before Day 2:

* Prepare **ONE real project example** where:

  * Polymorphism was used
  * Inheritance caused pain
  * Composition fixed it

---

# Questions & Answers:

Got it. I’ll give you **model interview answers** — the kind a **solid 3–5 YOE backend engineer** would give.
Read them, then practice saying them **in your own words** (do NOT memorize verbatim).

I’ll keep them **clear, structured, and defensible under follow-ups**.

---

# ✅ DAY 1 — IDEAL INTERVIEW ANSWERS

---

## 🔹 Q1️⃣ Runtime Polymorphism

**Answer:**

Runtime polymorphism means that **the method which gets executed is decided at runtime based on the actual object, not the reference type**.

In Java, this happens through **dynamic method dispatch**.
At runtime, the JVM looks at the object stored in the heap, checks its class, and then calls the overridden method from that class.

So even if the reference is of the parent type, the JVM invokes the child’s method implementation.

---

## 🔹 Q2️⃣ Static Method Call

```java
Parent p = new Child();
p.staticMethod();
```

**Answer:**

The **Parent’s static method** gets called.

This is **not polymorphism**, because static methods are:

* Resolved at **compile time**
* Bound to the **reference type**, not the object

Static methods belong to the class, not the instance, so method overriding does not apply here. This is called **method hiding**, not overriding.

---

## 🔹 Q3️⃣ Abstraction vs Encapsulation (with project examples)

**Answer:**

**Encapsulation** is about **hiding internal state and controlling access**.
Example from my project:
I had a `User` entity where fields like password and status were private, and updates were only allowed through methods that enforced validation. This ensured the object never entered an invalid state.

**Abstraction** is about **exposing only what the client needs to know**.
Example from my project:
We exposed a `PaymentService` interface with a `pay()` method, hiding whether the payment was done via Razorpay or Stripe. Clients only depended on the abstraction, not the implementation.

---

## 🔹 Q4️⃣ Why Constructors Can’t Be Overridden

**Answer:**

Constructors are used to **initialize objects**, not to define behavior.
Overriding applies to inherited behavior, but constructors are **not inherited** — they belong to the class itself.

Also, method overriding requires the same method signature and return type, but constructors don’t have return types and are tightly coupled to class creation, so polymorphism does not apply to them conceptually.

---

## 🔹 Q5️⃣ Interface vs Abstract Class (Project-based)

**Answer:**

I used **interfaces** when:

* Multiple unrelated classes needed to implement the same behavior
* I wanted loose coupling and easy mocking

For example, different notification channels (Email, SMS, Push) implemented a common `NotificationService` interface.

I used **abstract classes** when:

* There was shared state or common base logic
* Classes had a strong “is-a” relationship

For example, a base `AbstractReportGenerator` had common formatting logic, while subclasses handled specific report types.

---

## 🔹 Q6️⃣ Composition vs Inheritance

**Answer:**

Composition is preferred because **inheritance tightly couples the child to the parent’s internal behavior**, making changes risky.

In one case, we extended a base class for logging behavior. Later, changes in the parent class unintentionally affected multiple child classes.

We fixed this by using composition — injecting a `Logger` component instead. This made behavior swappable and reduced side effects.

---

## 🔹 Q7️⃣ Why Spring Exists

**Answer:**

Spring exists to solve the problem of **tight coupling and manual dependency management** in plain Java applications.

Without Spring, objects create their own dependencies, which makes the code hard to test, hard to change, and violates SOLID principles.

Spring centralizes object creation, wiring, and lifecycle management, enabling loose coupling, better testability, and scalable design.

---

## 🔹 Q8️⃣ IoC and DI (Junior-friendly)

**Answer:**

**Inversion of Control** means the application does not control object creation — the framework does.

**Dependency Injection** is how Spring achieves IoC.
Instead of creating dependencies using `new`, Spring injects required objects from the outside, usually through constructors.

This allows us to change implementations without changing business logic.

---

## 🔹 Q9️⃣ Constructor Injection vs Field Injection

**Answer:**

Constructor injection is preferred because:

1. It makes dependencies explicit and enforces mandatory dependencies
2. It allows immutable objects and easier unit testing

Field injection hides dependencies and makes testing harder because you can’t easily instantiate the class without Spring.

---

## 🔹 Q🔟 Spring vs Spring Boot

**Answer:**

Spring Boot removes:

* Manual configuration
* XML-heavy setup
* External server configuration

It provides auto-configuration, embedded servers, and sensible defaults so developers can focus on business logic instead of infrastructure setup.

---

## 🔹 Q1️⃣1️⃣ Bean Injection Decision

```java
@Autowired
PaymentService paymentService;
```

**Answer:**

Spring decides the implementation based on:

* Type matching
* Bean definitions in the context

If there are multiple implementations, Spring throws an ambiguity error unless:

* One is marked `@Primary`
* Or `@Qualifier` is used to specify which one to inject

---

## 🔹 Q1️⃣2️⃣ How Spring Uses OOP Concepts

**Answer:**

Spring uses:

* **Interfaces** to define contracts (abstraction)
* **Polymorphism** to inject different implementations at runtime
* **Composition** by assembling objects instead of using inheritance

Example:
A service depends on an interface, and Spring injects a concrete implementation based on configuration — enabling runtime flexibility.

---

## 🔹 Q1️⃣3️⃣ OOP Violation (Behavioral)

**Answer:**

In one project, we initially put too much business logic inside entity classes, violating the Single Responsibility Principle.

This made entities hard to maintain and test.
We later refactored the logic into service classes, keeping entities focused only on data representation.

---

## 🧠 Final Interview Tip (Important)

If you say **clear, calm answers like these**, interviewers usually:

* Stop digging
* Move faster
* Assume competence

---

## 👉 Next step

Say:

* **“Ask me Day 2 interview questions”**
  or
* **“Grill me deeper on any weak area”**

We’ll keep leveling this up until it feels automatic.
