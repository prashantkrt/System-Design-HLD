
# Database Normalization — Step-by-step Guide with Simple Examples

**Author:** Generated for Prashant  
**Date:** 2025-12-11

---

## Overview — What is Normalization?

**Normalization** is the systematic process of organizing data in a relational database to:
- Reduce redundancy (duplicate data)
- Eliminate anomalies (insert, update, delete problems)
- Improve data consistency and maintainability

**In simple words:** normalization arranges data so each fact is stored exactly once, in the correct place.

---

## Why normalize?
- Avoid inconsistent data after updates  
- Make inserts and deletes safe and logical  
- Improve query clarity and reduce storage waste  
- Make the schema easier to maintain and extend

---

## Common Anomalies (what normalization prevents)

1. **Insert anomaly** — cannot add data without unrelated required info.  
2. **Update anomaly** — the same fact exists in multiple places; updating one but not all causes inconsistency.  
3. **Delete anomaly** — deleting a row unintentionally removes other useful information.

---

## Normal Forms covered in this guide
- **1NF (First Normal Form)** — atomic values, no repeating groups  
- **2NF (Second Normal Form)** — 1NF + no partial dependencies on part of a composite key  
- **3NF (Third Normal Form)** — 2NF + no transitive dependencies  
- **BCNF (Boyce–Codd Normal Form)** — stronger version of 3NF where every determinant is a superkey

Each section below explains the rule, shows a small messy table, demonstrates step-by-step transformation, and provides final SQL to create the normalized tables.

---

# 1NF — First Normal Form

### Rule (short)
1. Every column must hold *atomic* (single) values.  
2. No repeating groups (no `Course1, Course2, ...`).  
3. Each column holds values from the same domain.  
4. Each row must be unique (primary key requirement).

### Example scenario
We store students and their courses and phone numbers in one table (messy):

**Messy table (NOT 1NF)**

| StudentID | StudentName | Courses         | Phones       |
|-----------|-------------|-----------------|--------------|
| 1         | Aman        | Java,Python     | 9876,9123    |
| 2         | Neha        | Java,C++,SQL    | 9987         |
| 3         | Raju        | SQL             | 8877,7766    |

#### What's wrong?
- `Courses` column stores multiple values (multi-valued cell).  
- `Phones` column stores multiple values.  
- Potential repeating groups if we had `Course1, Course2` columns.

### Step-by-step conversion to 1NF

**Step 1 — Choose atomic unit:**
Represent each row as one (Student, Course) pair. Store phones in a separate table.

**Step 2 — Expand Courses into multiple rows:**

| StudentID | StudentName | Course  | Phones       |
|-----------|-------------|---------|--------------|
| 1         | Aman        | Java    | 9876,9123    |
| 1         | Aman        | Python  | 9876,9123    |
| 2         | Neha        | Java    | 9987         |
| 2         | Neha        | C++     | 9987         |
| 2         | Neha        | SQL     | 9987         |
| 3         | Raju        | SQL     | 8877,7766    |

**Step 3 — Pull out phones to a separate table (one phone per row):**

`StudentPhone`:

| StudentID | Phone |
|-----------|-------|
| 1         | 9876  |
| 1         | 9123  |
| 2         | 9987  |
| 3         | 8877  |
| 3         | 7766  |

**Step 4 — Final 1NF tables**

`Student`:
- StudentID (PK), StudentName

`Course`:
- CourseID (PK), CourseName

`Enrollment`:
- StudentID (FK), CourseID (FK) — composite PK (StudentID, CourseID)

`StudentPhone`:
- StudentID (FK), Phone — primary key composite (StudentID, Phone)

### SQL to create 1NF schema

```sql
CREATE TABLE Student (
  StudentID INT PRIMARY KEY,
  StudentName VARCHAR(100) NOT NULL
);

CREATE TABLE Course (
  CourseID INT PRIMARY KEY,
  CourseName VARCHAR(100) NOT NULL
);

CREATE TABLE Enrollment (
  StudentID INT,
  CourseID INT,
  PRIMARY KEY (StudentID, CourseID),
  FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
  FOREIGN KEY (CourseID) REFERENCES Course(CourseID)
);

CREATE TABLE StudentPhone (
  StudentID INT,
  Phone VARCHAR(20),
  PRIMARY KEY (StudentID, Phone),
  FOREIGN KEY (StudentID) REFERENCES Student(StudentID)
);
```

---

# 2NF — Second Normal Form

### Rule (short)
- Table is in **1NF**.  
- **No partial dependency**: no non-key attribute depends on part of a composite primary key.

### When does this matter?
When your table's primary key is **composite** (two or more columns), and a non-key attribute is dependent only on *part* of that key.

### Messy example (NOT 2NF)

Suppose we kept a combined `Enrollment` table like:

| StudentID | CourseID | StudentName | CourseName | Instructor |
|-----------|----------|-------------|------------|------------|
| 1         | 10       | Aman        | Java       | Mr.Roy     |
| 1         | 11       | Aman        | Python     | Mr.Roy     |
| 2         | 10       | Neha        | Java       | Mr.Roy     |

Here, primary key is (StudentID, CourseID). But `StudentName` depends only on `StudentID` (part of key). `CourseName` and `Instructor` depend only on `CourseID`. These are **partial dependencies**, violating 2NF.

### Fix (decompose to remove partial dependencies)

Split into:
- `Student(StudentID, StudentName)`  
- `Course(CourseID, CourseName, Instructor)`  
- `Enrollment(StudentID, CourseID)` — composite PK

### Why this helps
- StudentName stored once; CourseName and Instructor stored once.
- Update anomalies removed: change StudentName in one place.

### SQL (2NF final structure)

Same as 1NF structure above — the key point is moving student/course attributes into their own tables instead of the composite-enrollment table.

---

# 3NF — Third Normal Form

### Rule (short)
- Table is in **2NF**.  
- **No transitive dependency**: non-key attributes must not depend on other non-key attributes.

### Example (NOT 3NF)

`Course` table contains instructor name and instructor email in the same table as:

| CourseID | CourseName | InstructorName | InstructorEmail |
|----------|------------|----------------|-----------------|
| 10       | Java       | Mr.Roy         | roy@example.com |

If `InstructorEmail` depends on `InstructorName` (non-key → non-key), this is a transitive dependency:
CourseID → InstructorName → InstructorEmail

### Fix (move instructor details to separate table)

- `Course(CourseID, CourseName, InstructorID)`  
- `Instructor(InstructorID, InstructorName, InstructorEmail)`

### Why this helps
- Instructor data stored once; updates to instructor email or name are localized.
- Eliminates transitive update anomalies.

### SQL (3NF structure)

```sql
CREATE TABLE Instructor (
  InstructorID INT PRIMARY KEY,
  Name VARCHAR(100),
  Email VARCHAR(200)
);

CREATE TABLE Course (
  CourseID INT PRIMARY KEY,
  CourseName VARCHAR(100),
  InstructorID INT,
  FOREIGN KEY (InstructorID) REFERENCES Instructor(InstructorID)
);
```

---

# BCNF — Boyce–Codd Normal Form

### Rule (short)
For every functional dependency `X → Y`, **X must be a superkey** (i.e., X uniquely identifies rows).

BCNF is stricter than 3NF. It handles special cases where 3NF still leaves anomalies.

### Classic example that violates BCNF

Table: `RoomAssignment(Student, Room, Captain)`

Assume:
- Room → Captain (each room has one captain)
- Captain → Room (each captain is assigned to exactly one room)

But primary key of the table is `(Student, Room)` (students per room), so neither `Room` nor `Captain` is a superkey. The dependency `Room → Captain` violates BCNF (Room isn't a superkey).

### Fix: Decompose into two BCNF tables

`Room(RoomID, Captain)`  
`StudentRoom(Student, RoomID)`

Now every determinant is a key.

---

# Quick Comparison of Normal Forms

| Normal Form | Main rule | Fixes |
|-------------|-----------|-------|
| 1NF | Atomic values, no repeating groups | Multi-valued cells, repeating columns |
| 2NF | 1NF + no partial dependencies | Partial dependency on part of composite key |
| 3NF | 2NF + no transitive dependencies | Non-key → non-key dependencies |
| BCNF | Every determinant is a superkey | Edge cases not handled by 3NF |

---

# Optimization notes & practical tips

- **Normalization vs performance**: Higher normalization reduces redundancy but can increase number of joins. For OLTP systems prefer normalized design. For heavy-read reporting (OLAP), denormalization may be acceptable for performance.
- **Indexes**: Use indexes on FK columns and frequently queried attributes to improve join and lookup performance.
- **Surrogate keys**: Use integer surrogate keys where natural keys are long or composite; keep natural keys as unique constraints if required.
- **Balance**: In real systems, practical trade-offs matter — normalize to a reasonable level (usually 3NF or BCNF) and denormalize only when justified by performance or simplicity.
- **Document choices**: When denormalizing, document why (query patterns, performance tests).

---

# Short cheat-sheet (one-liners)

- 1NF: one value per cell.  
- 2NF: non-key columns must depend on the whole composite key, not part of it.  
- 3NF: non-key columns must depend only on the key (no transitive).  
- BCNF: every determinant must be a key.

---

# Appendix — Full worked example (from messy to 3NF + BCNF)

**Messy initial table (combined):**

| StudentID | StudentName | Courses      | Phones   | CourseInstructor | InstructorEmail     |
|-----------|-------------|--------------|----------|------------------|---------------------|
| 1         | Aman        | Java,Python  | 9876,9123| Mr.Roy           | roy@example.com     |
| 2         | Neha        | Java,C++,SQL | 9987     | Mr.Roy           | roy@example.com     |
| 3         | Raju        | SQL          | 8877,7766| Ms.Patel         | patel@example.com   |

**Steps**
1. Expand multi-valued columns → 1NF (Enrollment rows + StudentPhone table)  
2. Split Student and Course attributes into separate tables → remove partial dependencies → 2NF  
3. Move instructor details to `Instructor` table and reference by ID → remove transitive dependency → 3NF  
4. If any functional dependency exists where non-key determines other attributes, check BCNF and decompose if necessary.

---

## End — Notes for you
- This file is a compact but complete guide to normalization with practical steps.  
- If you'd like, I can also:
  - Create a downloadable **PDF** with diagrams, or  
  - Produce ER diagrams (PNG/SVG) for each step, or  
  - Generate sample data and run example SQL queries (SELECTs, JOINs) and show results.

