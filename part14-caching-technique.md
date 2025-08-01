## 📂 Caching Techniques in System Design

### ✅ What Are Caching Techniques?

Caching techniques define **how and when** data is stored and retrieved from cache to optimize performance, reduce latency, and offload backend systems.

---

### 🔹 Types of Caching Techniques

#### 1. **Cache-Aside (Lazy Loading)**
- Application checks cache first.
- If not present (cache miss), loads from DB and populates cache.
- Most common pattern.

**Pros:** Simple, flexible.  
**Cons:** Cache miss penalty on first access.

---

#### 2. **Read-Through Cache**
- Cache is responsible for loading data from the source.
- Application always interacts with cache.

**Pros:** Centralized logic.  
**Cons:** Higher complexity; less control in app layer.

---

#### 3. **Write-Through Cache**
- Write operations go to both cache and database.
- Ensures consistency.

**Pros:** Data always in sync.  
**Cons:** Slower writes.

---

#### 4. **Write-Behind (Write-Back) Cache**
- Writes go to cache first.
- Later asynchronously written to DB.

**Pros:** Fast writes.  
**Cons:** Risk of data loss on cache failure.

---

#### 5. **Refresh-Ahead Cache**
- Proactively refreshes data before it expires.
- Avoids cache stampede.

**Pros:** Reduced latency on expiration.  
**Cons:** Might refresh unneeded data.

---

#### 6. **Distributed Cache**
- Cache is shared across multiple instances.
- Ensures consistency in microservices or scaled apps.

**Tools:** Redis Cluster, Hazelcast, Apache Ignite

---

#### 7. **Local In-Memory Cache**
- Cache is stored per application instance.
- Fast access but no sharing.

**Tools:** Caffeine, Guava

---

### 📊 Strategy Comparison Table

| Strategy              | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| **Read-Through**      | Cache is checked first; if miss, data is fetched from DB and added to cache |
| **Write-Through**     | Writes go to both cache and database simultaneously                         |
| **Write-Behind**      | Writes go to cache first and then asynchronously to DB                      |
| **Cache-Aside (Lazy)**| App manually loads data into cache when needed                              |
| **Refresh Ahead**     | Periodically refreshes data in cache before it expires                      |
| **Distributed Cache** | Shared cache across nodes for horizontal scalability                        |
| **Local Cache**       | Fast in-memory cache within the same instance (non-shared)                  |

---

### 🔎 When to Use What?

| Technique         | Best For                                         |
|------------------|--------------------------------------------------|
| Cache-Aside      | General-purpose, flexible caching                |
| Read-Through     | Centralized data loading                         |
| Write-Through    | Write-heavy apps needing consistency             |
| Write-Behind     | Low-latency writes, analytics workloads          |
| Refresh-Ahead    | Preventing stampede on expiration                |
| Distributed Cache| Large-scale, multi-node or microservice systems  |
| Local Cache      | Low-latency access within a single app instance  |
