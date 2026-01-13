# Caching Strategies 

## 1. What is Caching?

**Caching** means:

> 👉 *Storing frequently used data in fast storage (memory) so future requests are served faster.*

### Simple analogy 🍽️
Think of a restaurant kitchen:
- Popular dishes are pre-prepared
- Customers get food faster

Pre-prepared food = **Cache**

---

## 2. Why Do We Need Caching Strategies?

Caching is not just storing data — **how you store and update it matters**.

Without strategy:
- Data becomes stale ❌
- Cache inconsistency ❌
- Bugs and wrong responses ❌

With strategy:
- Fast response ✅
- Correct data ✅
- Scalable systems ✅

---

## 3. Types of Caching Strategies (Most Important)

---

## 3.1 Cache-Aside (Lazy Loading) ⭐⭐⭐⭐

### How it works
1. App checks cache
2. If data exists → return it
3. If not → fetch from DB
4. Store in cache

```
Client → App → Cache ❌
              ↓
            Database
              ↓
            Cache ✅
```

### Example
```java
User user = cache.get(id);
if(user == null) {
  user = db.getUser(id);
  cache.put(id, user);
}
```

### Pros
- Simple
- Most commonly used

### Cons
- Cache miss penalty

### Where Used
- Web apps
- Microservices

---

## 3.2 Read-Through Cache

### How it works
- App talks only to cache
- Cache fetches from DB automatically

```
Client → Cache → DB
```

### Pros
- Cleaner application code

### Cons
- Cache becomes complex

### Where Used
- Managed caches

---

## 3.3 Write-Through Cache

### How it works
1. App writes to cache
2. Cache writes to DB synchronously

```
App → Cache → DB
```

### Pros
- Cache always consistent

### Cons
- Write latency increases

### Use Case
- Financial data

---

## 3.4 Write-Behind (Write-Back) Cache

### How it works
1. App writes to cache
2. Cache writes to DB **asynchronously**

```
App → Cache → (async) DB
```

### Pros
- Very fast writes

### Cons
- Risk of data loss

### Use Case
- Analytics
- Logging systems

---

## 3.5 Cache-Through

### Meaning
- Combines **read-through + write-through**

### Pros
- Strong consistency

### Cons
- Slower writes

---

## 3.6 TTL-Based Caching ⏰

### How it works
- Data expires after fixed time

```
productList → TTL = 5 minutes
```

### Pros
- Avoids stale data

### Cons
- Useful data may expire

---

## 3.7 Refresh-Ahead Cache

### How it works
- Cache refreshes data **before TTL expires**

### Pros
- No cache miss

### Cons
- More load on DB

---

## 4. Comparison Table (Interview Favorite)

| Strategy | Read Speed | Write Speed | Consistency |
|-------|-----------|------------|------------|
| Cache-Aside | Fast | Normal | Eventual |
| Read-Through | Fast | Normal | Strong |
| Write-Through | Normal | Slow | Strong |
| Write-Behind | Fast | Fast | Weak |
| TTL-based | Fast | Normal | Eventual |

---

## 5. Where to Use Which Strategy?

### Rule of Thumb

- User profiles → **Cache-Aside + TTL**
- Sessions / OTP → **TTL-based**
- Financial data → **Write-Through**
- Logs / Metrics → **Write-Behind**

---

## 6. Cache Consistency Problem

### Problem
- DB updated
- Cache still has old data

### Solutions
- Cache eviction on update
- Short TTL
- Write-through strategy

---

## 7. Caching in Real Systems

### Redis
- Cache-aside
- TTL
- LRU eviction

### CDN
- Read-through
- TTL-based

### Microservices
- Local + Distributed cache

---

## 8. Interview One-Liners 🧠

- Cache-aside is most popular
- TTL avoids stale data
- Write-through ensures consistency
- Write-behind improves performance

---

## 9. One-Line Summary

> **Caching strategies define how data flows between cache, application, and database to achieve speed and consistency.**


