# 📦 Denormalization in RDBMS (System Design)

## 📖 What is Denormalization?

**Denormalization** is the process of **intentionally introducing redundancy** into a normalized database design to **optimize performance** and **reduce complex joins**.

It’s the opposite of normalization and is often used in **system design** when performance, read efficiency, or query simplicity is prioritized.

---

## 🧠 Why Use Denormalization?

- Improve **read performance** (fewer joins)
- Reduce **query complexity**
- Optimize for **reporting** and **analytics**
- Increase **availability** in distributed systems
- Reduce **foreign key constraints** in massive datasets

---

## ⚡ Example: From Normalized to Denormalized

### 🎯 Normalized Structure (3NF)

1. **Customer Table**

| CustomerID | Name  |
|------------|-------|
| 1          | Alice |

2. **Order Table**

| OrderID | CustomerID | OrderDate   |
|---------|------------|-------------|
| 101     | 1          | 2023-08-01  |

3. **Product Table**

| ProductID | ProductName |
|-----------|-------------|
| P1        | iPhone      |

4. **OrderProduct Table**

| OrderID | ProductID |
|---------|-----------|
| 101     | P1        |

🔁 Querying requires **multiple joins**.

---

### 🎯 Denormalized Structure

| OrderID | CustomerName | OrderDate   | ProductName |
|---------|---------------|-------------|--------------|
| 101     | Alice         | 2023-08-01  | iPhone       |

✅ Fewer joins, better read performance, but **data duplication**.


## ✅ Benefits of Denormalization

| Benefit                    | Description                                      |
|----------------------------|--------------------------------------------------|
| 🔍 Faster Reads            | Reduces join operations                          |
| 🧾 Simpler Queries         | Easier to write and maintain                     |
| 📊 Better for Reporting    | Pre-joined or aggregated data                    |
| 🧩 Caching Friendly        | Suits key-value or document cache mechanisms     |

---

## ❌ Drawbacks of Denormalization

| Drawback                   | Description                                      |
|----------------------------|--------------------------------------------------|
| 📦 Data Redundancy         | Same data stored in multiple places              |
| 🔄 Update Anomalies        | Difficult to maintain consistency on updates     |
| 🐢 Slower Writes           | More data to write and maintain                  |
| 🛠️ Complex Maintenance     | Harder to handle inserts/deletes in bulk         |

---

## 🔄 Normalization vs Denormalization

| Feature              | Normalization                        | Denormalization                      |
|----------------------|--------------------------------------|--------------------------------------|
| Purpose              | Reduce redundancy, ensure integrity  | Improve performance, reduce joins    |
| Storage              | Efficient                           | Redundant                            |
| Read Performance     | Slower due to joins                 | Faster                               |
| Write Performance    | Faster                              | Slower                               |
| Complexity           | More normalized schema               | Flatter, easier for querying         |
| Use Case             | OLTP systems                        | OLAP/reporting systems               |

---

## 🧠 When to Denormalize?

- When **read performance** is critical
- For **reporting or analytics** workloads
- When operating at **large scale** (e.g., web-scale)
- In **NoSQL or document-oriented** systems
- When data changes are **infrequent**

---

## 💡Summary

> Use denormalization **selectively**. Combine normalized design for integrity and denormalized views or replicas for performance.
