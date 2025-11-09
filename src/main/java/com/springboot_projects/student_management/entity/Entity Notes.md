# 🧱 Entity Layer — Spring Boot + JPA

This section explains the **Entity Layer** in a Spring Boot project using **JPA (Java Persistence API)** — including the role of `Entity`, `BaseEntity`, and how they map Java objects to database tables.

---

## 🧩 What is an Entity?

In Spring Boot and JPA, an **Entity** is a **Java class that represents a table in a database**.  
Each **object (instance)** of the entity corresponds to a **row (record)** in that table, and each **field** in the class represents a **column** in the table.

> 💡 In short:  
> **Entity = Table**, **Object = Row**, **Field = Column**

---

⚙️ Key Annotations Explained

| Annotation                                            | Purpose                                                                                          |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `@Entity`                                             | Marks the class as a JPA entity → JPA treats it as a database table.                             |
| `@Table(name = "students")`                           | (Optional) Specifies the exact name of the table. If omitted, the class name is used by default. |
| `@Id`                                                 | Marks the **primary key** field of the entity.                                                   |
| `@GeneratedValue(strategy = GenerationType.IDENTITY)` | Lets the database auto-generate primary key values.                                              |
| `@Column`                                             | Customizes column properties (e.g., nullable, unique, length).                                   |
| `@Enumerated(EnumType.STRING)`                        | Stores enums (like Gender) as strings instead of integers.                                       |
| `@ManyToOne`, `@OneToMany`, etc.                      | Define relationships between entities (for future use).                                          |

🧩 How It Works

    - When you mark a class with @Entity, JPA maps it to a database table.

    - When you save an object (e.g., studentRepository.save(student)), JPA converts it into an SQL INSERT.

    - When you fetch it, JPA converts SQL SELECT results back into Java objects.

🧾 Summary Table

| Concept             | Java Side           | Database Side      |
| ------------------- | ------------------- | ------------------ |
| Entity Class        | Represents a table  | Table              |
| Entity Object       | Represents a row    | Record             |
| Entity Field        | Represents a column | Column             |
| Primary Key (`@Id`) | Unique identifier   | Primary key column |

🧩 In Short

Entity is the backbone of Spring Data JPA —
It connects your Java objects to database tables,
allowing you to work with data without writing SQL manually.

---

# 🧱 BaseEntity — Common Auditing Superclass

---

## 🧩 Overview

`BaseEntity` is an abstract parent class used to hold common fields and logic shared by all entities in a **Spring Boot + JPA** project.  
It helps eliminate duplicate code and automatically manages audit timestamps like when a record is created or updated.

---

## 🧠 Why Use BaseEntity?

✅ Avoids repetitive fields (like `createdAt`, `updatedAt`) in every entity.  
✅ Automatically sets timestamps during create/update operations.  
✅ Keeps entity layer clean, modular, and consistent.  
✅ Provides a foundation for adding future audit fields (e.g., `createdBy`, `updatedBy`).

---


# 🧩 Gender Enum — Controlled Gender Values


## 🧠 What is an Enum?

An **enum (short for *enumeration*)** in Java is a **special data type** used to define a **fixed set of constants** — predefined values that don’t change.  
Enums make your code **type-safe**, **readable**, and prevent invalid data entry.

> 💡 Example:  
> Instead of allowing strings like `"male"`, `"M"`, `"unknown"`,  
> you restrict possible values to only `MALE`, `FEMALE`, and `OTHER`.

## 🧱 File: `Gender.java`

---
# 🧱 StudentEntity — JPA Entity Class

---

## 🧩 Definition

`StudentEntity` is a **JPA entity class** that represents a single record in the **`students`** table of the database.  
It defines the **structure, data types, and constraints** of a student record in the system.

---

## 🧠 Purpose

- Maps **Java objects** to **database rows** using JPA (Java Persistence API).
- Defines the **data model** for students.
- Used by the **Repository Layer** to perform **CRUD (Create, Read, Update, Delete)** operations automatically.

---

## ⚙️ Key Annotations Explained

| Annotation | Description |
|-------------|--------------|
| `@Entity` | Declares the class as a JPA entity — it represents a database table. |
| `@Table(name = "students")` | Customizes the table name in the database. |
| `@Id` | Marks the **primary key** field. |
| `@GeneratedValue(strategy = GenerationType.IDENTITY)` | Lets the database auto-generate the ID sequentially. |
| `@Column` | Specifies column-level settings (nullable, unique, length, etc.). |
| `@Enumerated(EnumType.STRING)` | Saves Enum values as readable text (e.g., `"MALE"`) instead of numeric codes. |

---

## 🧾 Typical Fields

| Field | Type | Description |
|--------|------|-------------|
| `id` | `Long` | Unique identifier for each student (Primary Key). |
| `firstName` | `String` | Student’s first name. |
| `lastName` | `String` | Student’s last name. |
| `email` | `String` | Unique email ID for the student. |
| `gender` | `Gender` (Enum) | Represents gender (`MALE`, `FEMALE`, `OTHER`). |
| `dateOfBirth` | `LocalDate` | Student’s date of birth. |
| `address` | `String` | Optional — represents student’s address or location. |

---

## ⚡ Key Points to Remember

- ✅ Every entity **must have a primary key** annotated with `@Id`.
- 🔄 Use `@GeneratedValue` to **auto-generate IDs** safely.
- 🧩 Entities **must have a no-argument constructor** (required by JPA).
- 💡 Use **wrapper classes** (`Long`, `Integer`) instead of primitives to handle nulls safely.
- 🚫 Avoid placing **business logic** inside entities — keep them as data models.
- 🧱 Always use `EnumType.STRING` for enums — prevents future schema breakage.
- 🕒 Common fields like `createdAt` and `updatedAt` can be inherited from a shared **`BaseEntity`**.

---

## 🧭 Entity Role in Project Architecture

| Layer | Responsibility | Used By | Connected To |
|--------|----------------|----------|--------------|
| **Entity Layer** | Represents database tables | Repository Layer | Database via JPA & Hibernate ORM |

---

## 💬 Interview Notes

**Q:** What is an Entity in Spring Boot?  
**A:** An Entity is a lightweight Java class annotated with `@Entity` that maps directly to a database table using JPA. Each object of that class represents a single row in the table.

---

**Q:** Why use `@GeneratedValue(strategy = GenerationType.IDENTITY)`?  
**A:** It tells JPA to let the **database auto-generate the primary key** value (commonly used in databases like MySQL and H2).

---

📘 **In Short:**  
`StudentEntity` is the **bridge** between Java code and the database —  
it defines how student data is structured, stored, and retrieved automatically through JPA.


