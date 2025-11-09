# 🧱 Data Access Object (DAO) — Explained

## 🔍 What is DAO?

**DAO (Data Access Object)** is a **design pattern** used to separate **database access logic** from the **business logic** of an application.

👉 In simple terms:

> **DAO = A layer responsible only for talking to the database (CRUD operations)** — nothing else.

---

## 🧩 DAO in Spring Boot

In **Spring Boot** (using **JPA** or **JDBC**), the **DAO layer** is typically implemented using **Repository interfaces**, for example:

```java
@Repository
public interface StudentRepository extends JpaRepository<StudentEntity, Long> {
    // custom query methods if needed
}
```

This repository acts as your DAO layer — it handles:

Querying the database

Saving, updating, deleting data

Abstracting away all SQL code

----
### ⚙️ Why DAO Layer is Used

Here’s a clear breakdown of **why** the DAO layer is important in a Spring Boot project:

---

#### 🧠 **1. Separation of Concerns**
Keeps database logic **separate** from business logic, leading to **cleaner**, more **modular** code.

---

#### 🧩 **2. Reusability**
DAO methods like `saveStudent()` or `findAllStudents()` can be **reused** across multiple services or components — avoiding code duplication.

---

#### 🔄 **3. Maintainability**
You can **easily switch or modify** database logic (for example, changing from **MySQL → MongoDB**) without breaking the rest of the application.

---

#### 🧰 **4. Testability**
DAO classes can be **mocked** during unit testing, making it easier to test **service logic** independently.

---

#### 🚀 **5. Abstraction**
DAO hides the **complex SQL or ORM details** — higher layers (like Service) just call simple, meaningful methods such as `findAll()` or `save()`.

---

> ✅ **In short:** The DAO layer makes your app **modular**, **testable**, and **database-independent**.

---
## ✅ Example Architecture with DAO Layer

The **DAO pattern** in Spring Boot helps maintain a clean separation between layers.

Controller → Service → DAO (Repository) → Database

---

### 🧩 Example Implementation

#### **DAO Layer (Repository)**

```java
@Repository
public interface StudentRepository extends JpaRepository<StudentEntity, Long> {}
```
Service Layer
```java
@Service
public class StudentService {
private final StudentRepository studentRepository;

    public StudentService(StudentRepository studentRepository) {
        this.studentRepository = studentRepository;
    }

    public List<StudentEntity> getAllStudents() {
        return studentRepository.findAll();  // delegate to DAO layer
    }
}
```
## 🌟 Advantages of Using DAO in Spring Boot

---

### ✅ **Cleaner Architecture**
Each layer does one specific job — making the code easier to **read**, **understand**, and **maintain**.

---

### 🔁 **Reduces Code Duplication**
All database access logic is **centralized** inside the DAO layer, ensuring **reusable** and **consistent** operations.

---

### 🧪 **Easier Testing**
DAO components can be **mocked** using `@MockBean` during **unit testing**, isolating service logic from actual database operations.

---

### 🗄️ **Database Independence**
Switching databases (e.g., **H2 → MySQL → PostgreSQL**) requires **minimal or no changes** in the upper layers.

---

### ♻️ **Improved Reusability**
The same DAO class can be **reused** by multiple **service** or **controller** classes across the application.

---

### 🔒 **Encapsulation**
Upper layers (**Controller**, **Service**) don’t need to know **how** data is fetched or saved — they just rely on **simple method calls**.

---
These DTOs are pure data carriers — no JPA annotations, no business logic.

Validation annotations only exist on request DTOs (for input validation).

Response DTO is output-only, used for mapping entities → clean API responses.

You’ll typically map these using ModelMapper or MapStruct.