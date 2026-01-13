# Load Balancer in System Design 

## 1. What is a Load Balancer?

A **Load Balancer (LB)** is a component that **distributes incoming traffic across multiple servers** so that:
- No single server is overloaded
- System remains **fast, scalable, and highly available**

### Simple analogy 🏥
Think of a hospital reception desk:
- Patients come in
- Receptionist sends them to **Doctor A, B, or C** based on availability
- No doctor gets overloaded

The receptionist = **Load Balancer**

---

## 2. Why Do We Need a Load Balancer?

Without a Load Balancer:
- One server handles everything ❌
- If it crashes → **entire system goes down** ❌
- Scaling is hard ❌

With a Load Balancer:
- Traffic is shared ✅
- Servers can be added/removed easily ✅
- High availability & fault tolerance ✅

### Key Benefits
- **Scalability** – handle more users
- **High Availability** – no single point of failure
- **Performance** – faster response times
- **Reliability** – unhealthy servers removed automatically

---

## 3. Basic Architecture

```
Client
   |
   v
Load Balancer
  /   \
Server1  Server2  Server3
```

The client **never directly talks to backend servers**.

---

## 4. Types of Load Balancers (by Layer)

### 4.1 Layer 4 Load Balancer (Transport Layer)

- Works at **TCP/UDP level**
- Does NOT inspect request content
- Faster, but less intelligent

**Examples**:
- TCP traffic
- Database connections

**Pros**:
- Very fast
- Low latency

**Cons**:
- No routing based on URL/header

---

### 4.2 Layer 7 Load Balancer (Application Layer)

- Works at **HTTP/HTTPS level**
- Understands request content

Can route based on:
- URL (`/api/users`)
- Headers
- Cookies
- Hostname

**Pros**:
- Smart routing
- Ideal for microservices

**Cons**:
- Slightly slower than L4

---

## 5. Load Balancing Algorithms

### 5.1 Round Robin

```
Request1 → Server1
Request2 → Server2
Request3 → Server3
Request4 → Server1
```

- Simple
- Equal distribution

❌ Not good if servers have different capacities

---

### 5.2 Weighted Round Robin

- Powerful servers get **more traffic**

Example:
```
Server1 (weight 3)
Server2 (weight 1)
```

Server1 gets 3x requests

---

### 5.3 Least Connections

- Request goes to server with **fewest active connections**

✅ Good for long-running requests

---

### 5.4 IP Hash

- Client IP → same backend server

Used for:
- Session stickiness

❌ Poor load distribution sometimes

---

### 5.5 Random

- Random server selection

Used rarely

---

## 6. Health Checks

Load Balancer continuously checks server health:

```
GET /health
```

If server fails:
- Marked **unhealthy**
- Traffic stops going to it

### Types of Health Checks
- HTTP status check
- TCP connection check
- Custom logic

---

## 7. Session Persistence (Sticky Sessions)

Sometimes users must stay on the same server.

### Why?
- In-memory session data
- Legacy applications

### Techniques
- Cookies
- IP hash

❌ Avoid in microservices → use Redis instead

---

## 8. Load Balancer Placement

### 8.1 Public Load Balancer

- Faces internet
- Handles user traffic

### 8.2 Internal Load Balancer

- Between services
- Used in microservices

```
Internet → LB → API Gateway → Services
```

---

## 9. Load Balancer vs API Gateway

| Feature | Load Balancer | API Gateway |
|------|--------------|------------|
| Traffic distribution | ✅ | ❌ |
| Authentication | ❌ | ✅ |
| Rate limiting | ❌ | ✅ |
| Protocol translation | ❌ | ✅ |

They often **work together**.

---

## 10. Common Load Balancer Tools

### Software
- NGINX
- HAProxy
- Envoy

### Cloud Managed
- AWS ALB / NLB
- GCP Load Balancer
- Azure Load Balancer

---

## 11. Real-World Use Cases

### 11.1 E-commerce Website

- High traffic during sales
- LB distributes traffic to product, payment services

### 11.2 Microservices Architecture

- Each service has multiple instances
- LB balances traffic between them

### 11.3 Database Read Scaling

- Read replicas
- LB routes read queries

---

## 12. Single Point of Failure Problem

❌ One Load Balancer is dangerous

### Solution
- Multiple LBs
- DNS-based load balancing
- Cloud-managed LBs

---

## 13. Interview Tips 🚀

Always mention:
- Load Balancer
- Health checks
- Horizontal scaling
- Failover

---

## 14. One-Line Summary

> **A Load Balancer is the backbone of scalable, reliable, and high-performance distributed systems.**
