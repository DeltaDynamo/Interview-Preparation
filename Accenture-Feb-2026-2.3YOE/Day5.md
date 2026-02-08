# 🟦 DAY 5 — Concurrency + Validation (Interview-Grade)

---

## 🌅 Morning — Core Java Concurrency

---

## 1️⃣ Thread vs Process (How interviewers expect it)

### Process

* Independent execution unit
* Own memory space
* Heavyweight
* IPC needed for communication

### Thread

* Lightweight execution unit
* Shares process memory (heap)
* Faster context switching

📌 Interview line

> Threads share memory, which gives performance but introduces concurrency bugs.

---

## 2️⃣ Why concurrency is hard

Because:

* Shared mutable state
* Non-deterministic execution
* Bugs are timing-dependent

📌

> Most concurrency bugs cannot be reproduced reliably.

---

## 3️⃣ `synchronized` (What it actually does)

`synchronized` ensures:

1. **Mutual exclusion** — only one thread enters
2. **Visibility** — changes are visible to other threads

Example:

```java
synchronized void increment() {
    count++;
}
```

Internally:

* Uses object-level or class-level monitor lock
* Thread acquires lock before execution
* Releases after block/method ends

---

## 4️⃣ Where `synchronized` is used

* Critical sections
* Shared counters
* Lazy initialization

📌 Avoid synchronizing large blocks → hurts performance.

---

## 5️⃣ `volatile` (Conceptual, not low-level)

`volatile` guarantees:

* **Visibility**, not atomicity

Example:

```java
volatile boolean running = true;
```

* Writes go directly to main memory
* Reads always see latest value

❌ `volatile` does NOT make compound operations thread-safe.

---

## 6️⃣ `synchronized` vs `volatile`

| Aspect           | synchronized | volatile |
| ---------------- | ------------ | -------- |
| Mutual exclusion | ✅            | ❌        |
| Visibility       | ✅            | ✅        |
| Atomicity        | ✅            | ❌        |
| Performance cost | Higher       | Lower    |

📌

> Use `volatile` for flags, `synchronized` for state mutation.

---

## 7️⃣ Common concurrency bugs (Must-know)

### 1. Race condition

Multiple threads modify shared data concurrently.

### 2. Visibility issue

One thread doesn’t see updates by another.

### 3. Deadlock

Two threads waiting on each other forever.

### 4. Starvation

Thread never gets CPU or lock.

📌 Interview gold line

> Race conditions come from unsynchronized access to shared mutable state.

---

---

## 🌆 Evening — Spring Validation + Exception Handling

---

## 8️⃣ Why validation matters in backend

* First line of defense
* Prevents bad data
* Reduces downstream failures

📌

> Validation belongs at the API boundary.

---

## 9️⃣ `@Valid` in Spring Boot

Used to trigger validation on request objects.

```java
@PostMapping("/users")
public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO dto) {
    ...
}
```

* Uses Bean Validation (JSR-380)
* Automatically throws exception on failure

---

## 🔟 Common Validation Annotations

| Annotation      | Use                  |
| --------------- | -------------------- |
| `@NotNull`      | Must not be null     |
| `@NotBlank`     | Not null + not empty |
| `@Size`         | Length constraints   |
| `@Email`        | Email format         |
| `@Min` / `@Max` | Numeric bounds       |
| `@Pattern`      | Regex                |

📌

> Prefer `@NotBlank` over `@NotNull` for strings.

---

## 1️⃣1️⃣ What happens when validation fails?

Spring throws:

```
MethodArgumentNotValidException
```

Handled via:

```java
@ControllerAdvice
```

---

## 1️⃣2️⃣ `@ControllerAdvice` (Why interviewers love it)

Centralized exception handling across controllers.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<?> handleValidation(...) {
        ...
    }
}
```

Benefits:

* Clean controllers
* Consistent error responses
* Separation of concerns

---

## 🔁 Revision — Explain Race Condition (Clear Example)

```java
class Counter {
    int count = 0;

    void increment() {
        count++;
    }
}
```

Two threads execute `increment()` simultaneously:

1. Both read `count = 0`
2. Both increment to `1`
3. Both write `1`

Expected: `2`
Actual: `1`

📌 Fix:

* Use `synchronized`
* Or `AtomicInteger`

---

# 🎯 DAY 5 — INTERVIEW Q&A (WITH ANSWERS)

---

### Q1️⃣ Why threads are faster than processes?

**Answer:**
They share memory and have lower context-switching overhead.

---

### Q2️⃣ Does `volatile` make code thread-safe?

**Answer:**
No. It guarantees visibility, not atomicity.

---

### Q3️⃣ Difference between race condition and deadlock?

**Answer:**
Race condition causes incorrect results; deadlock causes threads to block forever.

---

### Q4️⃣ Why not synchronize everything?

**Answer:**
It degrades performance and reduces concurrency.

---

### Q5️⃣ Why validation should not be in service layer?

**Answer:**
Because invalid requests should be rejected early at the boundary.

---

### Q6️⃣ How does Spring know validation failed?

**Answer:**
Bean Validation throws an exception which Spring intercepts before controller logic.

---

### Q7️⃣ How do you return clean validation errors?

**Answer:**
Using `@ControllerAdvice` and mapping field errors to response DTO.

---

### Q8️⃣ Can validation annotations be used on entities?

**Answer:**
Yes, but DTOs are preferred to avoid coupling persistence with API validation.

---