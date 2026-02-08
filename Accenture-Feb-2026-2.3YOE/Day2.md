# 🟦 DAY 2 — Core Java Memory + Spring Boot Startup

---

## 🌅 Morning — Core Java Memory (VERY IMPORTANT)

---

## 1️⃣ Stack vs Heap (Most misunderstood topic)

### Clear explanation

* **Stack** → stores *method calls & local variables*
* **Heap** → stores *objects*

```java
User u = new User();
```

What happens:

* `u` (reference) → stored in **stack**
* `new User()` object → stored in **heap**

---

### Key properties

| Stack                          | Heap                  |
| ------------------------------ | --------------------- |
| Thread-specific                | Shared across threads |
| Fast access                    | Slower than stack     |
| Stores references & primitives | Stores objects        |
| Auto cleaned                   | GC managed            |

---

### Interview trap

❌ “Objects are stored in stack”
❌ “Reference is in heap”

---

### Interview question

**Q: Why is stack memory faster than heap?**
Because stack allocation is sequential and managed by CPU, not GC.

---

## 2️⃣ Object Lifecycle (Interview favorite)

### Object lifecycle steps

1. Class loading
2. Memory allocation
3. Constructor execution
4. Object usage
5. Eligible for GC
6. Garbage collected

---

### When object becomes eligible for GC

* Reference set to `null`
* Goes out of scope
* Reassigned to another object

```java
obj = null;
```

---

### Interview question

**Q: Can we force GC?**
❌ No
`System.gc()` is just a request.

---

## 3️⃣ Pass by Value (CONFUSION AREA)

### Golden rule

> **Java is always pass-by-value.**

---

### Why confusion happens

Because **value of reference** is passed.

```java
void change(User u) {
    u = new User(); // changes local copy only
}
```

---

### Object modification works

```java
void update(User u) {
    u.name = "New";
}
```

Why?

* Reference value copied
* Object still same

---

### Interview killer question

**Q: Why Java didn’t choose pass-by-reference?**

* Safety
* Simpler memory model
* Prevents accidental corruption

---

## 4️⃣ Immutability (VERY IMPORTANT)

### What is immutability?

Once created, **state cannot change**.

Examples:

* `String`
* `Integer`

---

### How to make a class immutable

1. Class `final`
2. Fields `private final`
3. No setters
4. Defensive copies

```java
final class User {
    private final String name;
}
```

---

### Why immutability matters

* Thread-safe
* Cache-friendly
* Predictable behavior

---

### Interview question

**Q: Why String is immutable?**

* Security
* HashMap key safety
* String pool optimization

---

## 🌆 Evening — Spring Boot Startup

---

## 5️⃣ `@SpringBootApplication` (VERY common)

```java
@SpringBootApplication
public class App {}
```

### What’s inside it?

It combines:

1. `@Configuration`
2. `@EnableAutoConfiguration`
3. `@ComponentScan`

---

### Interview question

**Q: Why is @ComponentScan important?**
It tells Spring where to look for beans.

---

## 6️⃣ Spring Boot Startup Flow (High Level)

### Step-by-step

1. JVM starts
2. `main()` runs
3. `SpringApplication.run()`
4. ApplicationContext created
5. Auto-configuration kicks in
6. Beans created
7. App ready

---

### Important internal ideas

* Classpath scanning
* Conditional configuration
* Embedded server startup

---

### Interview question

**Q: When are beans created?**
During context initialization.

---

## 7️⃣ Auto-Configuration (Core Boot idea)

### What is auto-configuration?

> Spring Boot configures beans automatically based on classpath and properties.

---

### How it works (high-level)

* `spring.factories`
* `@ConditionalOnClass`
* `@ConditionalOnMissingBean`

Example:

* If `spring-web` present → DispatcherServlet configured

---

### Interview trap

❌ “Spring Boot magic”
Say **conditions + classpath**.

---

## 🔁 Revision — JVM Memory (Explain like this)

Example:

```java
public void createUser() {
    User u = new User("A");
}
```

Explain:

* `u` on stack
* User object on heap
* Method exits → stack cleared
* Object eligible for GC

---

## 🔥 Rapid-Fire Interview Questions (Day 2)

Answer confidently:

1. Difference between stack & heap?
2. When is object eligible for GC?
3. Is Java pass by reference?
4. Why String immutable?
5. What does @SpringBootApplication contain?
6. How does auto-configuration work?

---

## 🧠 Mental Model (Important)

> JVM manages memory.
> Spring manages objects.

Say this — it lands very well.

---

# ✅ DAY 2 — INTERVIEW QUESTIONS WITH ANSWERS

(Core Java Memory + Spring Boot Startup)

---

## 🔹 Section 1: Core Java Memory

---

### Q1️⃣ Explain stack vs heap using a real Java example.

**Answer:**

The **stack** stores method calls and local variables, while the **heap** stores objects.

```java
User u = new User();
```

Here:

* The reference variable `u` is stored in the **stack**
* The `User` object created using `new` is stored in the **heap**

Each thread has its own stack, but the heap is shared across threads.

---

### Q2️⃣ Walk through memory allocation step-by-step.

```java
public void createUser() {
    User u = new User("A");
}
```

**Answer:**

1. Method `createUser()` is pushed onto the **stack**
2. Reference variable `u` is created in the stack
3. `new User("A")` creates an object in the **heap**
4. Constructor initializes the object
5. Method completes → stack frame is removed
6. Object becomes **eligible for GC** if no references exist

Actual memory is freed only when the garbage collector runs.

---

### Q3️⃣ What makes an object eligible for GC?

**Answer:**

An object becomes eligible for garbage collection when it is **no longer reachable**.

Common scenarios:

1. Reference is set to `null`
2. Reference goes out of scope
3. Reference is reassigned to another object

GC eligibility does **not** guarantee immediate collection.

---

### Q4️⃣ Can we force garbage collection?

**Answer:**

No, we cannot force GC in Java.

Calling `System.gc()` only **requests** the JVM to run GC.
The JVM decides whether or not to honor the request.

---

### Q5️⃣ Is Java pass-by-value or pass-by-reference?

**Answer:**

Java is **always pass-by-value**.

For objects, the **value of the reference** is passed.

```java
void change(User u) {
    u = new User(); // does not affect caller
}
```

The reference copy changes locally, but the original reference remains unchanged.

---

### Q6️⃣ Why does object modification reflect outside the method, but reassignment doesn’t?

**Answer:**

Because:

* The reference value is copied
* Both references point to the same object initially

Modifying the object changes shared state.
Reassigning the reference only changes the local copy.

---

### Q7️⃣ What is immutability? Why is it preferred in multi-threaded apps?

**Answer:**

Immutability means an object’s state **cannot be changed after creation**.

Immutable objects are preferred because:

* They are inherently thread-safe
* No synchronization needed
* Safer for caching and sharing across threads

---

### Q8️⃣ Why is `String` immutable?

**Answer:**

`String` is immutable for:

1. **Security** (used in class loading, DB URLs)
2. **Performance** (string pool reuse)
3. **Thread safety**
4. **HashMap key safety**

---

## 🔹 Section 2: Spring Boot Startup

---

### Q9️⃣ What is `@SpringBootApplication`?

**Answer:**

`@SpringBootApplication` is a convenience annotation that combines:

* `@Configuration`
* `@EnableAutoConfiguration`
* `@ComponentScan`

It marks the main configuration class of a Spring Boot application.

---

### Q🔟 Explain Spring Boot startup flow at a high level.

**Answer:**

1. JVM starts
2. `main()` method runs
3. `SpringApplication.run()` is called
4. ApplicationContext is created
5. Auto-configuration is applied
6. Beans are created
7. Embedded server starts
8. Application becomes ready

---

### Q1️⃣1️⃣ What is auto-configuration in Spring Boot?

**Answer:**

Auto-configuration means Spring Boot automatically configures beans based on:

* Classpath dependencies
* Application properties
* Conditional annotations

For example, if `spring-web` is present, Spring Boot configures `DispatcherServlet`.

---

### Q1️⃣2️⃣ How does Spring Boot know what to auto-configure?

**Answer:**

Spring Boot uses:

* `spring.factories`
* Conditional annotations like `@ConditionalOnClass`
* Absence of user-defined beans

This ensures defaults are applied only when needed.

---

### Q1️⃣3️⃣ When are Spring beans created?

**Answer:**

Spring beans are created during **ApplicationContext initialization**, before the application starts serving requests.

---

## 🧠 Final Mental Model (Very Important)

> **JVM manages memory.
> Spring Boot manages objects.**

If this clicks in your head, Day 2 is done.

---