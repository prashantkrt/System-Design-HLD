# 📘 Functional & Non-Functional Requirements  

*A clean, reusable reference for System Design*

## 1. What Are Requirements?
Functional Requirements (FR) → What the system should do  
Non-Functional Requirements (NFR) → How the system should behave

---

# ✅ 2. Functional Requirements (FR)

Functional requirements describe specific behaviors, features, and functionality.

## 2.1 Characteristics
- Define inputs → processes → outputs  
- Describe user interactions  
- Must be testable  

## 2.2 Examples

### Example: Authentication
- User Login  
- Password Reset  
- User Registration  

### Example: E-commerce
- Add to Cart  
- Checkout  
- Order Tracking  

### Example: Microservices
- Inventory service updates stock  
- Order-service publishes Kafka event  
- Aggregator returns combined response  

---

# ⚙️ 3. Non-Functional Requirements (NFR)

NFRs describe quality attributes like performance, scalability, availability, etc.

## Categories & Examples

### Performance
- API response < 200 ms  
- Handle 3000 RPS  

### Scalability
- Horizontal scaling  
- Auto-scaling on CPU > 70%  

### Availability
- 99.99% uptime  
- Zero-downtime deployments  

### Consistency
- Strong consistency → latest write  
- Eventual consistency → sync later  

### Security
- HTTPS only  
- bcrypt password hashing  

### Observability
- Metrics exposure  
- Distributed tracing  

---

# Summary Table
| Type | Description | Example |
|------|-------------|---------|
| Functional | What system does | Place an order |
| Non-Functional | How system behaves | Handle 10k orders/min |

---

# Combined Example: User Login

## Functional
- Login with email/password  
- OTP for new device  

## Non-Functional
- Response <150 ms  
- Handle 2000 logins/min  
- bcrypt hashing  
- 99.9% availability  
