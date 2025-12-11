
# NoSQL Databases in System Design — Complete Guide

**Author:** Prashant Kumar Tiwary
**Date:** 2025-12-11  

---

# 📌 What is NoSQL?

**NoSQL (Not Only SQL)** refers to non-relational databases built for:
- High scalability  
- Distributed architecture  
- Flexible schema  
- High availability  
- Handling massive datasets  

Used by systems like Netflix, Uber, Amazon, Facebook, and more.

---

# 🎯 Why NoSQL Exists?

Traditional SQL databases struggle when:
- Data becomes huge (TB–PB scale)
- Traffic reaches millions of reads/writes per second
- Schema changes frequently
- Distributed, globally replicated systems are required

NoSQL was designed to solve these problems.

---

# 🧠 Types of NoSQL Databases

There are **4 major types**, each optimized for specific use cases.

---

# 1️⃣ Key–Value Databases

### ✔ Description
Store data as **key → value** pairs.  
Fastest and simplest NoSQL type.

### ✔ Examples
- Redis  
- DynamoDB  
- Riak  

### ✔ Use Cases
- Caching  
- User sessions  
- Shopping carts  
- Leaderboards  
- Real-time counters  

### ✔ Example
```
Key: "user:1001"
Value: { "name": "Prashant", "role": "Developer" }
```

---

# 2️⃣ Document Databases

### ✔ Description
Store documents in JSON/BSON format.  
Schema is flexible and nested.

### ✔ Examples
- MongoDB  
- CouchDB  
- Firestore  

### ✔ Use Cases
- User profiles  
- Product catalogs  
- Blogs, comments  
- Microservices  

### ✔ Example Document
```json
{
  "userId": 101,
  "name": "Prashant",
  "skills": ["Java", "Spring"],
  "address": {
    "city": "Delhi",
    "pin": 110001
  }
}
```

---

# 3️⃣ Column-Family Databases

### ✔ Description
Based on Google Bigtable architecture.  
Optimized for large-scale writes.

### ✔ Examples
- Apache Cassandra  
- HBase  

### ✔ Use Cases
- IoT time-series  
- Large analytics systems  
- Event logs  
- Messaging  

### ✔ Example Layout
```
RowKey: user123
Columns:
  login_time: 2025-06-10
  last_action: "viewed_product"
```

---

# 4️⃣ Graph Databases

### ✔ Description
Store data as **nodes** and **edges**.  
Best for highly connected data.

### ✔ Examples
- Neo4j  
- Amazon Neptune  

### ✔ Use Cases
- Social networks  
- Fraud detection  
- Recommendation systems  
- Knowledge graphs  

### ✔ Example
```
(Prashant) -- follows --> (Tech Channel)
```

---

# ⭐ SQL vs NoSQL — Quick Comparison

| Feature | SQL | NoSQL |
|---------|------|--------|
| Schema | Fixed | Flexible |
| Relations | Supports JOINs | No JOINs, embedded data |
| Scalability | Vertical | Horizontal |
| Availability | Medium | High |
| Consistency | Strong | Usually eventual |
| Model | Tables | JSON, Key-value, Graph, Columns |

---

# 🔥 CAP Theorem for NoSQL

In distributed systems, you only fully get **two of the three**:

| Letter | Meaning |
|--------|----------|
| **C** | Consistency |
| **A** | Availability |
| **P** | Partition Tolerance |

### NoSQL usually chooses:
- **AP (Available + Partition tolerant)** → Cassandra, DynamoDB  
- **CP (Consistent + Partition tolerant)** → MongoDB (replica sets), HBase  

---

# 🏗 Where NoSQL Fits in System Design

### ✔ Choose NoSQL when:
- You expect billions of records  
- You need high availability  
- Schema evolves frequently  
- System must scale horizontally  
- JOINs are not required  
- Data is semi-structured  

---

# 📝 Real System Examples

### 1. Netflix
- Uses Cassandra  
- Handles millions of writes per second  

### 2. Uber
- MongoDB for trip and map-related data  

### 3. Amazon
- DynamoDB for shopping cart & session storage  

---

# 🧩 Example Architecture — E-Commerce System

| Service | Database | Why |
|---------|----------|------|
| Product Catalog | MongoDB | Flexible document schema |
| Cart Service | Redis | Fast read/write |
| Recommendation Engine | Neo4j | Relationships |
| Order History | Cassandra | Time-series, heavy writes |
| Search | ElasticSearch | Full-text queries |

---

# 🛠 Example Data Modeling (MongoDB)

### Product Document
```json
{
  "productId": 501,
  "name": "Laptop",
  "price": 55000,
  "attributes": {
    "ram": "16GB",
    "processor": "M4 Pro"
  },
  "reviews": [
    {"rating": 5, "comment": "Excellent"},
    {"rating": 4, "comment": "Good"}
  ]
}
```

---

# 🧨 When NOT to Use NoSQL

Avoid NoSQL when:
- You need strong ACID transactions  
- Complex relational JOINs are needed  
- Data relationships are complex (unless using graph DB)  
- Strict consistency is mandatory (e.g., banking systems)  

---

# 🎯 Quick One-liners (Interview-Ready)

- **“NoSQL is chosen for scale, availability, and flexibility.”**  
- **“Key–value stores = fastest NoSQL.”**  
- **“Document DBs fit microservices perfectly.”**  
- **“Cassandra is ideal for heavy write, distributed workloads.”**  
- **“Graph DBs are unbeatable for relationship-heavy data.”**

---

# ✔ Final Summary

| Type | Best For |
|------|-----------|
| Key–Value | Fast caching, sessions |
| Document | Flexible data, microservices |
| Column | High write throughput |
| Graph | Connected data |

NoSQL plays a huge role in modern system design and is essential knowledge for building scalable systems.

---

## End of Document

If you'd like, I can also generate:  
- PDF version  
- Visual diagrams (architecture + comparisons)  
- Interview Q&A on NoSQL  

