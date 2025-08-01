## 📌 Load Balancer - System Design Notes

### ✅ What is a Load Balancer?

A **load balancer** is a component that **distributes incoming network traffic across multiple servers (backend instances)** to ensure:

- No single server is overwhelmed  
- High availability and reliability  
- Better performance and scalability  

---

### 🎯 Why Use Load Balancing?

| Benefit/Role                | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| ✅ Scalability              | Distributes load as traffic increases                                       |
| ✅ High Availability        | Redirects traffic if a server goes down                                     |
| ✅ Fault Tolerance          | Ensures continuity by detecting and bypassing failed instances              |
| ✅ Better Performance       | Routes to least-loaded or geographically closest server                     |
| ❤️ Health Checks            | Continuously monitors backend server health and removes unhealthy instances |
| 🔐 SSL Termination          | Offloads SSL decryption from backend servers, reducing their load           |
| 🔄 Session Persistence      | Maintains client session on same backend (sticky sessions)                  |
| 🧭 Smart Routing            | Routes based on URL paths, cookies, headers, or custom rules                |
| 📊 Load Monitoring          | Tracks traffic metrics to adjust balancing strategy dynamically             |
| 🌐 Protocol Support         | Supports HTTP, HTTPS, TCP, UDP, WebSockets, etc.                            |
| ⏱️ Timeout & Retry Handling | Retries failed requests or times out bad responses gracefully               |
| ☁️ Multi-Region Support     | Balances load across geographically distributed data centers                |

---

### ⚙️ Types of Load Balancing Algorithms

| Algorithm          | Description                                                 |
|--------------------|-------------------------------------------------------------|
| 🔁 Round Robin     | Rotates requests across servers sequentially                |
| 🎯 Least Connections | Sends request to server with fewest active connections     |
| 🧠 IP Hash          | Chooses server based on client IP address hash             |
| 📊 Weighted RR      | Like Round Robin but considers server capacity (weights)   |
| 📈 Resource-based   | Uses metrics (CPU, memory) to route requests               |
| ⏱️ Least Response Time | Routes traffic to the server with the fastest average response time       |

---

### 🌐 Types of Load Balancers

| Type              | Works On              | Example Tools/Services                      |
|-------------------|-----------------------|---------------------------------------------|
| 🔧 Hardware LB     | Physical devices       | F5 Networks, Cisco ACE                      |
| 💻 Software LB     | Software-only solution | HAProxy, NGINX, Envoy                       |
| ☁️ Cloud-based LB  | Managed by cloud       | AWS ELB, Azure LB, GCP Load Balancer        |


