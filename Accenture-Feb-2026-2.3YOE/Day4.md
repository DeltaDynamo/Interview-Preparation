# 🟦 DAY 4 — Exceptions + REST Design (Interview-Ready)

---

## 🌅 Morning — Core Java Exceptions (How professionals think)

---

## 1️⃣ Exception Hierarchy (Mental Model)

```
Throwable
 ├── Error
 │    ├── OutOfMemoryError
 │    └── StackOverflowError
 └── Exception
      ├── Checked Exceptions
      │    ├── IOException
      │    └── SQLException
      └── RuntimeException (Unchecked)
           ├── NullPointerException
           ├── IllegalArgumentException
           └── IndexOutOfBoundsException
```

### Key understanding

* **Errors** → JVM level, don’t catch
* **Exceptions** → application-level issues

📌 Interview line

> Errors indicate system failure, exceptions indicate recoverable application conditions.

---

## 2️⃣ Checked vs Unchecked Exceptions (REAL reasoning)

### Checked Exceptions

* Enforced by compiler
* Caller must handle or declare

Example:

```java
FileInputStream fis = new FileInputStream("a.txt");
```

**Used when:**

* Caller can reasonably recover
* External systems involved (IO, network)

---

### Unchecked Exceptions

* Extend `RuntimeException`
* Not enforced by compiler

Example:

```java
user.getName(); // NPE
```

**Used when:**

* Programming mistakes
* Invalid method arguments
* Business rule violations

📌 **Modern backend rule**

> Business exceptions → unchecked
> Technical exceptions → checked or wrapped

---

## 3️⃣ Why checked exceptions are controversial

Problems:

* Pollute method signatures
* Encourage catch-and-ignore
* Hard to evolve APIs

📌 Senior-level line

> Checked exceptions don’t scale well in layered architectures.

---

## 4️⃣ Custom Exceptions (Clean way)

### Bad way

```java
throw new Exception("User not found");
```

### Correct way

```java
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(String msg) {
        super(msg);
    }
}
```

**Why custom exceptions matter**

* Semantic meaning
* Better error mapping (REST)
* Cleaner handling

---

## 5️⃣ Exception handling best practices

✅ Catch specific exceptions
❌ Don’t swallow exceptions
❌ Don’t use exceptions for flow control
✅ Wrap low-level exceptions at boundaries

📌

> Catch exceptions where you can add value, not where they occur.

---

---

## 🌆 Evening — REST Design + Spring (What interviewers test)

---

## 6️⃣ REST Principles (Simple, not academic)

REST is about **resources**, not actions.

Example:

```
/users
/users/{id}
```

Key ideas:

* Stateless
* Resource-based URLs
* HTTP methods define action
* Representation via JSON/XML

📌

> REST uses HTTP semantics correctly instead of reinventing protocols.

---

## 7️⃣ HTTP Methods (Real usage)

| Method | Usage           |
| ------ | --------------- |
| GET    | Read resource   |
| POST   | Create resource |
| PUT    | Full update     |
| PATCH  | Partial update  |
| DELETE | Remove resource |

❗ **GET must be idempotent**
❗ **PUT is idempotent, POST is not**

---

## 8️⃣ HTTP Status Codes (Very Important)

### 2xx — Success

* 200 OK
* 201 Created
* 204 No Content

### 4xx — Client error

* 400 Bad Request (validation)
* 401 Unauthorized
* 403 Forbidden
* 404 Not Found
* 409 Conflict

### 5xx — Server error

* 500 Internal Server Error
* 503 Service Unavailable

📌 Interview favorite

> 400 vs 422?
> 400 = malformed request
> 422 = semantically invalid

---

## 9️⃣ DTO vs Entity (Critical separation)

### Entity

* Represents DB table
* Managed by JPA
* Contains persistence annotations

### DTO

* Represents API contract
* Used for request/response
* No JPA annotations

📌 Why separation matters

* Security (don’t expose internals)
* API stability
* Decoupling persistence from API

📌 Interview one-liner

> Entity is for database, DTO is for communication.

---

## 🔁 Revision — Design an API + Error Response

---

### Example API

**GET /users/{id}**

#### Success Response

```json
{
  "id": 10,
  "name": "Anubhav",
  "email": "a@example.com"
}
```

#### Error Response (404)

```json
{
  "timestamp": "2026-02-06T18:30:00",
  "status": 404,
  "error": "USER_NOT_FOUND",
  "message": "User with id 10 not found",
  "path": "/users/10"
}
```

📌 Good error response should include:

* HTTP status
* Error code
* Human-readable message
* Traceable info (path, timestamp)

---

# 🎯 DAY 4 — INTERVIEW Q&A (WITH ANSWERS)

---

### Q1️⃣ Why unchecked exceptions are preferred for business logic?

**Answer:**
Because they don’t clutter method signatures and naturally propagate to global handlers.

---

### Q2️⃣ When should you use checked exceptions?

**Answer:**
When the caller can reasonably recover, usually with IO or external systems.

---

### Q3️⃣ Why not expose JPA entities directly in REST APIs?

**Answer:**
It tightly couples API with DB schema and can leak sensitive or internal fields.

---

### Q4️⃣ Difference between PUT and PATCH?

**Answer:**
PUT replaces the entire resource, PATCH updates only specific fields.

---

### Q5️⃣ How do you handle exceptions globally in Spring Boot?

**Answer:**
Using `@ControllerAdvice` with `@ExceptionHandler`.

---

### Q6️⃣ What status code for validation failure?

**Answer:**
400 Bad Request (or 422 Unprocessable Entity).

---

### Q7️⃣ Why REST APIs should be stateless?

**Answer:**
Improves scalability and simplifies server-side design.

---

### Q8️⃣ What’s wrong with returning 200 for errors?

**Answer:**
Breaks HTTP semantics and makes client-side handling unreliable.

---