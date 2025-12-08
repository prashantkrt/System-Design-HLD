# Fault Tolerance in System Design

## 1. What is Fault Tolerance?
Fault tolerance means a system continues to operate correctly even if some components fail.

---

## 2. Why Fault Tolerance Matters
- Prevent downtime  
- Protect revenue  
- Maintain user trust  
- Avoid cascading failures  
- Improve system reliability  

---

## 3. Fault Tolerant Techniques

### 3.1 Replication
Keeping multiple copies of services or data.

**Example — Web Servers**
```
      Load Balancer
       /     |     \
    App1   App2   App3
```

If App2 crashes → LB only sends traffic to App1 & App3.

---

### 3.2 Redundancy
Extra backup components.

**Example**  
Kafka keeps replica logs across multiple brokers.

---

### 3.3 Load Balancing
Distributes traffic and removes unhealthy servers.

---

### 3.4 Graceful Degradation
System still works with reduced functionality.

---

### 3.5 Retry Logic
Automatically retry failed operations.

---

### 3.6 Circuit Breaker
Stops calling a failing service to avoid cascading failures.

---

### 3.7 Failover
Automatic switch to backup when primary fails.

---

### 3.8 Sharding / Partitioning
Splitting data to isolate failures.

---

### 3.9 Rate Limiting
Protects systems from overload.

---

### 3.10 Auto-Healing
Kubernetes kills and restarts unhealthy pods.

---

## 4. Summary Table
| Technique | Purpose | Example |
|----------|---------|---------|
| Replication | Multiple copies | App servers, DB replicas |
| Redundancy | Backup | Kafka |
| Load balancing | Route traffic | Nginx, ALB |
| Graceful degradation | Partial features | Netflix fallback |
| Retry | Temporary failure handling | API retry |
| Circuit breaker | Prevent cascade failures | Hystrix |
| Failover | Backup switch | RDS |
| Sharding | Isolation | User partitions |
| Rate limiting | Overload protection | 429 |
| Auto healing | Self recovery | K8s |

---

## 5. Complete Example — E-commerce Checkout
Payment service fails → but system still works due to:
- Retry  
- Circuit breaker  
- Graceful degradation  
- Failover  
- LB routing  
- Auto-healing  

---

## 6. Real-World Example — Netflix
Uses region replication, circuit breakers, caching, and Chaos Monkey.

