# 🧭 Mapper Layer — Notes

## 📘 1. Definition

The **Mapper Layer** (also called **Converter Layer**) is responsible for transforming data between different object types — typically between:

> **Entity ↔ DTO (Data Transfer Object)**

It isolates conversion logic from your business logic, keeping the code **clean**, **reusable**, and **maintainable**.

---

## 🧩 2. Why Mapper Layer is Needed

**Problem Without Mapper → Solution With Mapper**

- ❌ Controllers or services become messy with repeated `.setXYZ()` code  
  ✅ Mapping is centralized inside one class

- ❌ Risk of missing or inconsistent field mappings  
  ✅ Single, well-tested mapping logic ensures consistency

- ❌ Tight coupling between Entity and DTO  
  ✅ Mapper decouples layers for flexibility

- ❌ Difficult maintenance if Entity or DTO changes  
  ✅ Only Mapper code needs updates

---

## ⚙️ 3. Responsibilities of Mapper

A Mapper should:

- Convert **incoming DTO → Entity** for database persistence
- Convert **Entity → Response DTO** for API output
- Handle **partial updates** (in `updateEntity()` methods)
- Compute **derived fields** (like `fullName`, `age`)
- Keep **Entity & DTO layers synchronized** but **independent**

---

## 🧱 4. Typical Methods

| Method | Purpose |
|--------|----------|
| `toEntity(CreateDTO dto)` | Converts DTO to Entity (used in POST/create). |
| `toResponse(Entity entity)` | Converts Entity to DTO (used in GET responses). |
| `updateEntity(UpdateDTO dto, Entity entity)` | Updates existing entity fields selectively (used in PUT/PATCH). |
| `calculateAge(LocalDate dob)` | Helper logic for computed fields like age. |

---

## 🧰 5. Implementation Options

### 1. **Manual Mapping**
- **Example:** Custom class with `.set()` calls (like `StudentMapper`)
- **Pros:** Full control
- **Cons:** More boilerplate
- **Use for:** Small or simple applications

---

### 2. **ModelMapper Library**
- **Example:** `modelMapper.map(source, destination)`
- **Pros:** Fast to write
- **Cons:** Less control
- **Use for:** Medium projects with many DTOs

---

### 3. **MapStruct (Recommended for Large Projects)**
- **Type:** Compile-time generated mappers
- **Pros:** High performance, clean, and type-safe
- **Use for:** Enterprise or production-grade applications

---

## 🧠 6. Best Practices

✅ Keep Mapper classes **stateless** → annotate with `@Component`  
✅ Never inject **repositories or business logic** inside mappers  
✅ Keep only **conversion logic**, nothing else  
✅ Write **unit tests** to verify mapping accuracy  
✅ Always **recompute derived fields** (like `age`, `fullName`) inside the mapper

---

## 📄 7. Example (Summary)

```java
@Component
public class StudentMapper {

    public Student toEntity(StudentCreateRequest dto) {
        // Convert DTO → Entity
    }

    public StudentResponse toResponse(Student entity) {
        // Convert Entity → DTO
    }

    public void updateEntity(StudentUpdateRequest dto, Student entity) {
        // Update selective fields
    }
}
