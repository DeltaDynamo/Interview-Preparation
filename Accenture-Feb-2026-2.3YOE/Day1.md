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