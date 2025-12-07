
# 📘 System Design Calculation Notes  
*Complete, in‑depth notes for Latency, Throughput, Availability, Capacity Planning, SLA, Replication, Consistency & more.*

---

# 🧩 1. **Latency**

## ✔ What is Latency?
Latency is the **time taken** for a single request to travel through a system end‑to‑end.

Formula:
```
Latency = Time when response is received – Time when request was sent
```

## ✔ Types of Latency
### 1. **Network Latency**
Time taken for packets to travel over the network.

Example:  
Client → Server RTT = 40 ms  
Server processing = 10 ms  
```
Total Latency = 40 + 10 = 50 ms
```

### 2. **Disk Latency**
Time taken by storage systems to read/write data.

Example:  
SSD read latency = 0.1 ms  
HDD read latency = 4 ms

### 3. **Service Latency**
Time service takes to compute a result.

---

## ✔ Percentile Latency (P50, P90, P95, P99)
Used to measure **worst-case** performance.

Definition:
- **P50** = 50% of requests complete within this time  
- **P95** = 95% of requests complete within this time  
- **P99** = 99% of requests complete within this time (tail latency)

Example:  
If P99 latency = **800 ms**, that means:  
> For every 100 requests, 1 will take **800 ms or more**.

---

## ✔ Tail Latency in Distributed Systems
If 1 service has 1% probability of being slow and you call **100 services**,  
Probability at least 1 is slow:

```
P = 1 - (0.99)^100 ≈ 63%
```

This is why microservices sometimes feel slow → more calls, more tail latency.

---

# ⚡ 2. **Throughput**

## ✔ What is Throughput?
How many operations your system can perform per second.

Formula:
```
Throughput = Total requests processed / Total time
```

Example:
```
6000 requests processed in 10 seconds  
Throughput = 6000 / 10 = 600 RPS
```

---

## ✔ Throughput vs Latency
- **Latency** = Time per request  
- **Throughput** = Requests per second  

Relationship:
```
Throughput = 1 / Latency   (only in perfect parallel systems)
```

Example:
```
Latency = 50 ms = 0.05 sec  
Max Throughput = 1 / 0.05 = 20 requests/sec  
```

---

# 🏗 3. **Availability**

## ✔ What is Availability?
Percentage of time the system is up.

Formula:
```
Availability = Uptime / (Uptime + Downtime)
```

## ✔ Common Availability Levels
| Availability | Downtime / Year |
|--------------|-----------------|
| 99% | 3.65 days |
| 99.9% | 8.76 hours |
| 99.99% | 52.6 minutes |
| 99.999% | 5.26 minutes |

---

## ✔ Combined Availability in Distributed Systems

### **Case 1: Services in Series (AND dependency)**  
Total Availability = product of individual availabilities.

Example:
```
Service A: 99.9%  
Service B: 99.9%

Total = 0.999 × 0.999 = 0.998001 = 99.8001%
```

Series reduces availability.

---

### **Case 2: Services in Parallel (OR dependency)**
System works if ANY replica works.

Formula:
```
Total Availability = 1 – (1 – A)^N
```

Example:
```
Two replicas, each 99%

Total = 1 – (0.01)^2 = 0.9999 = 99.99%
```

Parallel increases availability.

---

# 📥 4. **Durability**

Durability ensures data is **never lost** after being written.

Example:  
RDBMS ensures commit logs are written to disk before confirming success.

## ✔ Durability via Replication
If each server has a failure probability `P`:

```
Data loss probability = P^N
```

Example (3 replicas):
```
P = 0.01  
P^3 = 0.000001 (0.0001%)
```

---

# 📦 5. **Capacity Planning**

## ✔ Steps

### Step 1 — Estimate QPS (Queries Per Second)
Example:
```
100M users  
10% DAU  
Each makes 5 requests/day  

DAU = 10M  
Total requests/day = 10M × 5 = 50M  
QPS = 50M / 86400 ≈ 578 QPS
```

Peak traffic = 3× = **1734 QPS**

---

### Step 2 — Calculate Storage Needs
Example:
```
Each record = 1 KB  
500M records/day  

Storage/day = 500M KB = 500 GB/day  
Storage/year ≈ 500 × 365 = 182 TB
```

---

### Step 3 — Bandwidth Calculation
Formula:
```
Bandwidth = QPS × Payload Size
```

Example:
```
Response size = 2 KB  
QPS = 1000  

Bandwidth = 1000 × 2 KB = 2000 KB/sec = 2 MB/sec
```

---

# 🔁 6. **Replication Lag**

If leader and follower database differ in update timestamp:

```
Replication Lag = follower_timestamp – leader_timestamp
```

Example:
Leader updated at: 10:00:00  
Follower updated at: 10:00:02  
Lag = **2 sec**

---

# 🔒 7. **Consistency Models**

## ✔ Strong Consistency
Reads always return latest write.

Example:
Bank balance must always be correct.

## ✔ Eventual Consistency
Systems sync after some delay.

Example:
Instagram likes update after 1–2 seconds.

## ✔ Read-After-Write Consistency
User immediately sees their own writes.

---

# 📡 8. **Caching Calculations**

## ✔ Cache Hit Ratio
```
Hit Ratio = Cache Hits / Total Requests
```

Example:
```
Hits = 800  
Misses = 200  
Hit Ratio = 800 / 1000 = 80%
```

---

## ✔ Effective Latency with Cache
```
Effective Latency = hit_ratio × cache_latency + miss_ratio × db_latency
```

Example:
```
Cache latency = 5 ms  
DB latency = 50 ms  
Hit ratio = 80%

= 0.8 × 5 + 0.2 × 50 = 4 + 10 = 14 ms
```

---

# 📨 9. **Queue (Kafka / RabbitMQ) Throughput**

Throughput depends on:
- Batch size  
- Message size  
- I/O speed  

Example:
```
Message size = 1 KB  
100,000 messages/sec  
Kafka throughput = 100 MB/sec
```

---

# 🏎 10. **SLA, SLO, SLI**

## ✔ SLA (Service Level Agreement)
Contractual guarantee.

Example:
```
SLA: 99.9% uptime
```

## ✔ SLO (Service Level Objective)
Internal target.

Example:
```
SLO: P99 latency < 200 ms
```

## ✔ SLI (Service Level Indicator)
Actual measured value.

Example:
```
SLI: last week P99 latency = 180 ms
```

---

# 🧮 11. **Little’s Law**

```
L = λ × W
```

Where:  
L = number of items in system  
λ = arrival rate (req/sec)  
W = response time

Example:
```
Arrival rate = 100 RPS  
Latency = 0.2 sec  

L = 100 × 0.2 = 20 items in system
```

---

# 🔚 End of Notes
These notes contain all major calculations used in system design interviews and real production architecture.

