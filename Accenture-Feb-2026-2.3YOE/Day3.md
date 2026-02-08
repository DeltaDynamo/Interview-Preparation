# 🟦 DAY 3 — Collections + Dependency Injection (Interview-Grade)

---

## 🌅 Morning — Core Java Collections (DEEP, NOT NOISY)

---

## 1️⃣ `HashMap` Internals (Must-Know)

### How HashMap actually works

At a high level:

* HashMap = **Array of buckets**
* Each bucket = **Linked list → Tree (Red-Black Tree since Java 8)**

### Put operation (step-by-step)

1. `key.hashCode()` is called
2. Hash is spread using bit manipulation
3. Index is calculated:

   ```java
   index = (n - 1) & hash
   ```
4. If bucket empty → insert
5. If collision:

   * Traverse linked list
   * If same key (`equals`) → replace value
   * Else add new node
6. If bucket size > 8 → convert to Red-Black Tree
7. If load factor exceeded (default `0.75`) → **rehash**

### Why `equals()` and `hashCode()` both matter

| Method       | Purpose             |
| ------------ | ------------------- |
| `hashCode()` | Finds the bucket    |
| `equals()`   | Finds the exact key |

❗ **Same hashCode but equals=false** → collision, both keys stored
❗ **equals=true but hashCode different** → contract violation (bug)

---

## 2️⃣ Why HashMap performance degrades

### Normal case

* Time complexity: **O(1)**

### Worst case

* Before Java 8: **O(n)** (linked list)
* After Java 8: **O(log n)** (tree)

### Reasons for degradation

* Poor hash function
* Too many collisions
* Mutable keys
* Large initial data without resizing properly

📌 **Interview gold line**

> HashMap performance degrades not because of size, but because of *collision distribution*.

---

## 3️⃣ `ArrayList` vs `LinkedList` (Beyond basics)

| Operation      | ArrayList      | LinkedList            |
| -------------- | -------------- | --------------------- |
| get(i)         | O(1)           | O(n)                  |
| add(end)       | Amortized O(1) | O(1)                  |
| add(middle)    | O(n)           | O(n)                  |
| Memory         | Compact        | Heavy (node pointers) |
| Cache friendly | Yes            | No                    |

📌 **Real-world truth**

* 90% of the time → `ArrayList`
* `LinkedList` rarely justified unless:

  * Frequent head insertions
  * Iterator-based traversal

---

## 4️⃣ `ConcurrentHashMap` Basics

### Why not synchronized HashMap?

Problems:

* Entire map locked
* Poor scalability

### ConcurrentHashMap approach

* No global lock
* Uses:

  * CAS (Compare-And-Swap)
  * Fine-grained locking on bins
* Read operations are **lock-free**

📌 **Key interview point**

> ConcurrentHashMap allows concurrent reads and writes without blocking the entire map.

---

## 5️⃣ `equals()` & `hashCode()` — Real Bug Scenario

### Common mistake

```java
class User {
    String id;
}
```

Used as HashMap key → **BUG**

### Correct approach

```java
@Override
public boolean equals(Object o) { ... }

@Override
public int hashCode() { ... }
```

📌 If key fields are mutable → **DON’T use as HashMap key**

---

---

## 🌆 Evening — Spring Dependency Injection (Real Issues)

---

## 6️⃣ Constructor vs Field Injection (Interview Favorite)

### Field Injection

```java
@Autowired
Service service;
```

❌ Problems:

* Hidden dependencies
* Cannot create immutable beans
* Hard to test
* Fails at runtime, not compile time

---

### Constructor Injection (Preferred)

```java
@Service
class OrderService {
    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

✅ Benefits:

* Explicit dependencies
* Immutable objects
* Easier unit testing
* Better design

📌 **Interview one-liner**

> Constructor injection makes invalid states unrepresentable.

---

## 7️⃣ Bean Scopes (Practical)

| Scope               | Meaning                    |
| ------------------- | -------------------------- |
| singleton (default) | One instance per container |
| prototype           | New instance every time    |
| request             | Per HTTP request           |
| session             | Per user session           |

📌 **Important**

* Injecting prototype into singleton → prototype loses meaning
* Needs `ObjectFactory` / `Provider`

---

## 8️⃣ Circular Dependencies (Classic Trap)

### Example

```java
A → depends on B  
B → depends on A
```

### Field injection

* Spring can resolve using proxies

### Constructor injection

* ❌ Fails at startup

📌 **Why constructor injection exposes circular deps**

> Because object creation cannot complete without dependencies.

📌 **Fixes**

* Refactor design
* Introduce mediator
* Use events
* Use `@Lazy` (last resort)

---

---

## 🔁 Revision Summary (Say This Confidently)

* HashMap performance depends on **hash distribution**
* `equals()` without `hashCode()` is a logical bug
* `ArrayList` is default choice in real systems
* `ConcurrentHashMap` avoids global locking
* Constructor injection improves design and testability
* Circular dependencies often indicate **design smell**

---

# 🎯 DAY 3 — INTERVIEW Q&A (WITH ANSWERS)

---

### Q1️⃣ Why HashMap uses both equals() and hashCode()?

**Answer:**
hashCode() identifies the bucket, equals() identifies the exact key within the bucket.

---

### Q2️⃣ What happens if hashCode is same but equals is false?

**Answer:**
Collision occurs; both entries are stored in the same bucket.

---

### Q3️⃣ Why ArrayList is preferred over LinkedList in practice?

**Answer:**
Better cache locality, lower memory overhead, and faster random access.

---

### Q4️⃣ How does ConcurrentHashMap achieve concurrency?

**Answer:**
By avoiding global locks and using CAS with fine-grained locking at bucket level.

---

### Q5️⃣ Why constructor injection is better than field injection?

**Answer:**
It enforces immutability, exposes dependencies clearly, improves testability, and fails fast.

---

### Q6️⃣ Why circular dependency is a design problem?

**Answer:**
It indicates tight coupling and makes object lifecycle complex and fragile.

---

### Q7️⃣ Can Spring resolve circular dependency with constructor injection?

**Answer:**
No. Constructor injection fails fast, which is actually desirable.

---