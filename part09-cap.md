# 📘 CAP Theorem (Brewer’s Theorem)

The **CAP Theorem** describes the fundamental trade-offs in distributed systems involving:

- **C**onsistency  
- **A**vailability  
- **P**artition Tolerance

---

## 🔺 What Is CAP Theorem?

> In a distributed system, it is **impossible to simultaneously guarantee all three**:  
> - **Consistency (C)**  
> - **Availability (A)**  
> - **Partition Tolerance (P)**  

You can only **choose two at most** in a real-world system.

---

## 🧠 Definitions

### 1. **Consistency**
- Every read reflects the **most recent write**.
- No stale data.

### 2. **Availability**
- System always responds to requests (non-error responses).
- Might return outdated data to stay available.

### 3. **Partition Tolerance**
- System continues to work even if **communication is lost between nodes**.

---

## ⚖️ Trade-offs

| Type | Guarantees                   | Sacrifices   |
|------|------------------------------|--------------|
| **CP**  | Consistency + Partition       | Availability |
| **AP**  | Availability + Partition       | Consistency  |
| **CA**  | Consistency + Availability     | Partition Tolerance *(not feasible in real distributed systems)* |

---
---

### ✅ CP (Consistency + Partition Tolerance)
- Guarantees consistency and handles network splits.
- Sacrifices availability: may **reject requests** during partition to maintain data correctness.

**Example**: Banking systems prefer to **reject** inconsistent transactions.

---

### ✅ AP (Availability + Partition Tolerance)
- System is always responsive and tolerant of network issues.
- Sacrifices consistency: **some reads may be stale** or conflicting.

**Example**: Social media or cached APIs — always respond, even if not perfectly up-to-date.

---

### 🚫 CA (Consistency + Availability) — **Not Feasible in Practice**
- In theory, you could have consistency and availability if **no partition ever occurs**.
- But in real distributed systems, **partitions are inevitable** (e.g., node crashes, network loss).

> To remain consistent and available during a partition, nodes would have to **stay in sync without communicating**, which is **impossible**.

So, **in the presence of partitions**, you **must choose between consistency and availability**.

---

## 🛠️ Real-World Examples

| System        | Type | Notes |
|---------------|------|-------|
| MongoDB       | AP   | Always available, may serve stale data |
| HBase         | CP   | Consistent, but might reject requests |
| Traditional SQL (single-node) | CA   | Not partition tolerant |

---

## 📌 Note
> In real-world distributed systems, **partition tolerance is a must**, so the real choice is between **consistency and availability**.

