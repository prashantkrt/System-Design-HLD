
# Cache in System Design – Complete Notes

## 1. What is Cache?
Cache is a fast, temporary storage layer that stores frequently accessed or recently used data so that future requests can be served faster.

**Primary goals:**
- Reduce latency
- Reduce load on databases and downstream services
- Improve scalability and user experience

> Cache improves performance, not correctness.

---

## 2. Why Cache is Important in System Design

Without cache:
- Every request hits the database
- High latency
- Database becomes a bottleneck
- Poor scalability

With cache:
- Faster responses (milliseconds)
- Reduced database load
- Better throughput
- Cost-efficient scaling

---

## 3. Where Cache Fits in Architecture

Typical request flow:

Client → Application → Cache → Database

Steps:
1. Application checks cache
2. If data exists (cache hit), return immediately
3. If not (cache miss), fetch from database
4. Store result in cache
5. Return response to client

---

## 4. Cache Hit vs Cache Miss

### Cache Hit
- Requested data is found in cache
- Very fast response
- Ideal scenario

### Cache Miss
- Data not found in cache
- Application queries database
- Cache is updated with fresh data

**Goal:** Maintain a high cache hit ratio

---

## 5. Common Cache Use Cases

### 5.1 Read-Heavy Data
Examples:
- User profiles
- Product catalogs
- Configuration data

Reason:
- Data changes rarely
- Read frequency is very high

---

### 5.2 Expensive Computations
Examples:
- Search results
- Recommendation engines
- Aggregated analytics

Strategy:
- Cache computation results instead of recomputing

---

### 5.3 External API Responses
Examples:
- Payment status checks
- Weather APIs
- Third-party integrations

Benefits:
- Lower latency
- Reduced cost
- Avoid rate limits

---

### 5.4 Session Management
Examples:
- Logged-in user sessions
- Token or session metadata

Reason:
- Short-lived data
- Very fast access required

---

### 5.5 Rate Limiting
Example:
- Limit user to N requests per minute

How cache helps:
- Stores counters
- Atomic increment operations
- Extremely fast access

---

## 6. Cache Strategies

### 6.1 Cache-Aside (Lazy Loading)
Flow:
1. Application checks cache
2. On miss, fetch data from database
3. Store data in cache
4. Return response

Pros:
- Simple
- Flexible
- Most commonly used

Cons:
- Risk of stale data

---

### 6.2 Write-Through Cache
Flow:
- Data is written to cache and database at the same time

Pros:
- Cache always has fresh data

Cons:
- Slower write operations

---

### 6.3 Write-Behind (Write-Back) Cache
Flow:
- Data written to cache first
- Database updated asynchronously

Pros:
- Very fast writes

Cons:
- Risk of data loss if cache fails

---

## 7. Cache Expiration (TTL)

TTL (Time To Live):
- Defines how long data stays in cache
- After TTL expires, data is automatically removed

Benefits:
- Prevents stale data
- Controls memory usage

---

## 8. Common Cache Problems (Important for Interviews)

### 8.1 Cache Stampede
Problem:
- Cache entry expires
- Many requests hit database simultaneously

Solutions:
- Locking mechanisms
- Early cache refresh
- Request coalescing

---

### 8.2 Stale Data
Problem:
- Database updated
- Cache still holds old value

Solutions:
- Cache eviction on update
- Versioning keys
- Proper TTL configuration

---

### 8.3 Cache Penetration
Problem:
- Repeated requests for non-existing data
- Always hits database

Solutions:
- Cache null or empty responses
- Use bloom filters

---

## 9. Popular Caching Tools

- Redis
- Memcached
- In-memory caches (Caffeine, Ehcache)

---

## 10. When NOT to Use Cache

- Highly dynamic data
- Strong consistency requirements
- Very small systems where cache adds complexity

---

## 11. Key Takeaways

- Database is the source of truth
- Cache is a performance optimization layer
- Always design cache with eviction and expiration strategies
- High cache hit ratio is critical for scalability

---

End of Notes
