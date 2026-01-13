# Cache Eviction Techniques 

## 1. What is Cache Eviction?

**Cache eviction** means:

> 👉 *Removing old or less useful data from cache to make space for new data.*

### Simple analogy 🧠
Think of your **mobile phone storage**:
- Storage is limited
- When full, you delete old photos/videos
- New data can now be saved

That deletion = **Cache Eviction**

---

## 2. Why Do We Need Cache Eviction?

Cache is always **limited in size**.

Without eviction:
- Cache becomes full ❌
- New data cannot be stored ❌
- Performance degrades ❌

With eviction:
- Cache stays fresh ✅
- Frequently used data remains ✅
- System stays fast & stable ✅

---

## 3. When Does Cache Eviction Happen?

Eviction happens when:
- Cache is **full**
- New data needs to be added
- TTL (time-to-live) expires

---

## 4. Most Important Cache Eviction Techniques

---

## 4.1 LRU (Least Recently Used)

### Meaning
> Remove the data that was **used longest time ago**.

### Example

Cache size = 3

Access order:
```
A → B → C
```
Cache = `[A, B, C]`

Now access:
```
D
```
👉 **A** is evicted (not used recently)

Cache becomes:
```
[B, C, D]
```

### Where Used
- Redis (default)
- Databases
- Web applications

### Pros
- Works well for real-world traffic

### Cons
- Slight overhead to track usage

---

## 4.2 LFU (Least Frequently Used) 

### Meaning
> Remove the data that is **used least number of times**.

### Example

| Key | Usage Count |
|----|------------|
| A | 10 |
| B | 3 |
| C | 1 |

👉 **C** is evicted

### Where Used
- Analytics systems
- Recommendation engines

### Pros
- Keeps popular data longer

### Cons
- Old popular data may stay forever

---

## 4.3 FIFO (First In First Out)

### Meaning
> Remove the data that was **inserted first**.

### Example

Inserted order:
```
A → B → C
```

New entry:
```
D
```
👉 **A** is evicted

### Pros
- Very simple

### Cons
- Ignores usage pattern ❌

---

## 4.4 Random Eviction

### Meaning
> Remove **any random item** from cache.

### Example

Cache = `[A, B, C]`

Evicted randomly → maybe **B**

### Pros
- Very fast

### Cons
- Poor cache hit ratio

---

## 4.5 TTL-based Eviction (Time To Live) ⏰

### Meaning
> Data is removed **after a fixed time**.

### Example

```
userProfile → TTL = 10 minutes
```
After 10 minutes → auto removed

### Where Used
- Session cache
- OTP cache
- Temporary data

### Pros
- Prevents stale data

### Cons
- Data may expire even if useful

---

## 4.6 MRU (Most Recently Used)

### Meaning
> Remove the data that was **used most recently**.

### Rare use case
- Sequential data access patterns

### Example

Recently used = **C** → evicted

---

## 5. Comparison Table (Very Important for Interviews)

| Strategy | Eviction Based On | Use Case |
|-------|-----------------|---------|
| LRU | Least recent use | Web apps, APIs |
| LFU | Least frequent use | Analytics |
| FIFO | Oldest entry | Simple systems |
| TTL | Time expiry | Sessions, OTP |
| Random | Random | Low memory systems |
| MRU | Most recent use | Special workloads |

---

## 6. Which Eviction Strategy Should I Choose?

### General Rule

> ✅ **LRU + TTL** is best for 90% of systems

### Examples
- User data → **LRU**
- OTP / token → **TTL**
- Trending data → **LFU**

---

## 7. Cache Eviction in Real Systems

### Redis
- LRU
- LFU
- TTL-based

### CDN
- TTL-based eviction

### Database Cache
- LRU + LFU mix

---

## 8. Common Interview Question

**Q: What happens when cache is full?**

**Answer**:
> Cache eviction policy decides which data to remove so new data can be stored.

---

## 9. One-Line Summary 🧠

> **Cache eviction ensures cache stays useful, fresh, and efficient by removing less valuable data.**
