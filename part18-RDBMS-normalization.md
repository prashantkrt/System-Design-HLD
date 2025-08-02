# 🧮 Normalization in RDBMS

## 📘 What is Normalization?

**Normalization** is a process in relational database design used to:
- Minimize redundancy (duplicate data)
- Ensure data integrity
- Organize data efficiently across multiple tables

It involves decomposing larger tables into smaller, related tables and defining relationships between them.

---

## 🧠 Why Normalize?

- Eliminate data anomalies (insertion, update, deletion)
- Reduce duplication
- Improve consistency and maintainability
- Make data logically structured

---

## 🔢 Normal Forms (NF)

Normalization is done through **normal forms** — each form has rules and builds upon the previous one.

---

### ✅ 1NF (First Normal Form)

**Rule**: 
- Atomic (indivisible) values
- No repeating groups or arrays

**Example: Not in 1NF**

| StudentID | Name     | Courses         |
|-----------|----------|-----------------|
| 1         | Alice    | Math, Science   |

**Convert to 1NF**

| StudentID | Name   | Course   |
|-----------|--------|----------|
| 1         | Alice  | Math     |
| 1         | Alice  | Science  |

✅ Each field contains atomic values.

---

### ✅ 2NF (Second Normal Form)

**Rule**:
- Must be in **1NF**
- No **partial dependency** (i.e., no non-prime attribute depends on part of a composite key)

**Example: Not in 2NF**

| StudentID | Course   | Instructor   |
|-----------|----------|--------------|
| 1         | Math     | Mr. John     |
| 1         | Science  | Ms. Jane     |

Assume: `(StudentID, Course)` is the primary key, but `Instructor` depends only on `Course`.

**Convert to 2NF**

1. **StudentCourse Table**

| StudentID | Course   |
|-----------|----------|
| 1         | Math     |
| 1         | Science  |

2. **CourseInstructor Table**

| Course   | Instructor   |
|----------|--------------|
| Math     | Mr. John     |
| Science  | Ms. Jane     |

✅ No partial dependency.

---

### ✅ 3NF (Third Normal Form)

**Rule**:
- Must be in **2NF**
- No **transitive dependency** (non-prime attribute depends on another non-prime attribute)

**Example: Not in 3NF**

| EmployeeID | Name   | DeptID | DeptName   |
|------------|--------|--------|------------|
| 101        | Raj    | D1     | HR         |

Here, `DeptName` depends on `DeptID`, not on `EmployeeID`.

**Convert to 3NF**

1. **Employee Table**

| EmployeeID | Name   | DeptID |
|------------|--------|--------|
| 101        | Raj    | D1     |

2. **Department Table**

| DeptID | DeptName |
|--------|----------|
| D1     | HR       |

✅ Transitive dependency removed.

---

### 🛡️ BCNF (Boyce-Codd Normal Form)

**Rule**:
- Must be in **3NF**
- Every determinant must be a candidate key

**Example: Not in BCNF**

| StudentID | Course   | Instructor   |
|-----------|----------|--------------|
| 1         | Math     | Mr. John     |

Suppose:
- A course is taught by only one instructor
- But instructor can teach multiple courses

Then `Course → Instructor` violates BCNF (Course is not a candidate key, but determines Instructor)

**Convert to BCNF**

1. **StudentCourse Table**

| StudentID | Course   |
|-----------|----------|
| 1         | Math     |

2. **CourseInstructor Table**

| Course   | Instructor |
|----------|------------|
| Math     | Mr. John   |

✅ All determinants are candidate keys.

---

## 📝 Summary Table

| Normal Form | Requirement                                                  |
|-------------|--------------------------------------------------------------|
| 1NF         | Atomic values, no repeating groups                           |
| 2NF         | 1NF + no partial dependency                                  |
| 3NF         | 2NF + no transitive dependency                               |
| BCNF        | 3NF + every determinant is a candidate key                   |

---

## 🚦 When to Stop Normalizing?

- Up to **3NF** is often sufficient for most applications.
- Beyond that (BCNF, 4NF, 5NF) may be needed for complex data models.
- Over-normalization can hurt performance—use wisely!

---

## 🧠 Tip

> Normalize for **clarity and correctness**, but **denormalize for performance** in high-read scenarios (e.g., analytics, reporting).

