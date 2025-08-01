## 🗂️ Caching in System Design - Notes

### ✅ What is Caching?

**Caching** is a technique used to **store frequently accessed data in a temporary storage (cache)** to improve performance and reduce load on the primary data source.

---

### 🌟 Why Use Caching?

| Benefit                | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| ⚡ Faster Response Time | Serves data from memory or fast storage                                     |
| 📊 Reduced Load        | Minimizes traffic to databases or external services                          |
| ✅ Increased Throughput | Handles more requests efficiently                                            |
| 🌐 Better Scalability   | Supports higher concurrency levels with less backend pressure                |

---

### 🔹 Common Caching Strategies

| Strategy              | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| **Read-Through**      | Cache is checked first; if miss, data is fetched from DB and added to cache |
| **Write-Through**     | Writes go to both cache and database simultaneously                         |
| **Write-Behind**      | Writes go to cache first and then asynchronously to DB                      |
| **Cache-Aside (Lazy)**| App manually loads data into cache when needed                              |
| **Refresh Ahead**     | Periodically refreshes data in cache before it expires                      |

---

### 🔍 Cache Eviction Policies

| Policy   | Description                                                   |
|----------|---------------------------------------------------------------|
| **LRU**  | Least Recently Used - removes the least recently accessed     |
| **LFU**  | Least Frequently Used - removes least accessed items          |
| **FIFO** | First-In First-Out - evicts oldest entries first              |
| **TTL**  | Time to Live - automatically expires items after time limit   |

---

### 🔹 Cache Types

| Type              | Example Tools           | Use Case                                      |
|-------------------|--------------------------|-----------------------------------------------|
| **In-Memory**     | Redis, Memcached         | High-speed reads, small data, session caching |
| **Distributed**   | Redis Cluster, Hazelcast | Scalable shared cache across services         |
| **Local**         | Guava, Caffeine          | Per-instance lightweight caching              |

---

### 📈 Where to Apply Caching?

- ✅ Database Query Results  
- ✅ API Responses  
- ✅ Static Assets (Images, JS, CSS)  
- ✅ Session Data / Authentication Tokens  

---

### ⚠️ Common Pitfalls

- ❌ Stale Data (ensure proper TTL or invalidation)  
- ❌ Cache Inconsistency (especially in write-behind)  
- ❌ Over-caching (can cause memory bloat)  
- ❌ Cache Stampede (many requests for same data on cache miss)  

---

### 📉 Cache Stampede Mitigation

- Use **locking** or **request coalescing**  
- Apply **randomized TTLs**  
- Use **background refreshes**  

---

### 🛠️ Tools and Libraries

| Tool        | Description                          |
|-------------|--------------------------------------|
| Redis       | Popular in-memory key-value store    |
| Memcached   | Lightweight distributed cache        |
| Caffeine    | High-performance Java local cache    |
| Spring Cache| Abstraction for cache management     |

---

> Caching is a **powerful tool** to improve system performance, but it must be **used carefully** to avoid data inconsistency and memory issues.
