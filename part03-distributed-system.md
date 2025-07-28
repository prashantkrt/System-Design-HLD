# 🌐 Distributed Systems

A **Distributed System** is a collection of **independent computers (nodes)** that appear to the user as a **single coherent system**. These machines communicate over a network and coordinate to perform tasks.

A **distributed system** is a collection of multiple individual systems connected through
a network that share resources, communicate and coordinate to achieve common goals.

---

## 📌 Key Characteristics

- **Multiple independent nodes** work together
- **No shared memory** — communication happens via **network protocols**
- Offers **fault tolerance** and **scalability**
- **Concurrency**: many processes can run in parallel
- Appears as **a single system** to the end-user

## 🛡️ Data Replication & Fault Tolerance

- **Replication** is the process of copying data across multiple nodes to ensure **fault tolerance** and **high availability**.
- Most distributed systems (like Cassandra, Kafka, MongoDB, HDFS) use a **Replication Factor (RF)** to define how many copies of the data exist.

### 🧮 What is Replication Factor?

- **Replication Factor (RF)** = Number of nodes holding a copy of the same data.
- Example: If RF = 3, then the same data is stored on 3 different nodes.

### 📉 What Happens if a Node Fails?

- With RF > 1, the system can continue serving data even if one or more nodes go down.
- Example:
  - **RF = 3**
  - One node crashes ✅ → **No data loss**, system keeps working
  - Two nodes crash ⚠️ → **Still safe**, but critical
  - All replicas crash ❌ → **Data loss**
---

### 🔹 Simple Explanation of the Distributed System Diagram

- **Node 1, Node 2, Node 3**  
  Individual machines (servers) in a distributed cluster. Each node can:
  - Store data  
  - Run services  
  - Handle requests  

- **<----> (Arrows between nodes)**  
  Nodes are connected directly (peer-to-peer), meaning they can **talk to each other directly**.

- **Network Communication Layer**  
  The backbone that allows nodes to:
  - 🚀 Send messages (like data updates)  
  - 🔁 Sync replicated data  
  - 🧭 Coordinate (e.g., leader election, consensus)

  ---

### 💡 Extra Insights:

- All 3 nodes are part of the same **distributed cluster**.
- If **Node 2 fails**, the other nodes continue to function.
- Thanks to **replication factor**, data from Node 2 can be recovered or **redistributed** to ensure availability and no data loss.

```
      +-------------+          +-------------+          +-------------+
      |   Node 1    | <------> |   Node 2    | <------> |   Node 3    |
      +-------------+          +-------------+          +-------------+
             \                       |                        /
              \                      |                       /
               \                     |                      /
                \                    |                     /
                 \                   |                    /
                  +---------------------------------------+
                  |           Network Communication       |
                  +---------------------------------------+
```


---

## 🧪 Real-World Examples

| System / Tool       | Description                                               |
|---------------------|-----------------------------------------------------------|
| **Google Search**   | Distributed web crawlers and index servers                |
| **Hadoop**          | Distributed file system (HDFS) + MapReduce compute engine |
| **Apache Kafka**    | Distributed event streaming platform                      |
| **Netflix Backend** | Built on microservices distributed across regions         |
| **Amazon S3**       | Distributed object storage system                         |
| **DNS**             | Global system for resolving domain names                  |
| **Blockchain**      | Peer-to-peer distributed ledger system                    |

---

## 🧩 Types of Distributed Systems

- **Client-Server**: Centralized servers serve multiple clients (e.g., web server)
- **Peer-to-Peer (P2P)**: All nodes are equal (e.g., BitTorrent, blockchain). There is no central server. Every device (called a peer) in the system can talk directly to each other, share data, and do the same work.
- **Three-tier architecture**: UI ↔ Application logic ↔ Data storage
- **Distributed Databases**: Multiple database nodes act as one (e.g., Cassandra, MongoDB cluster)
  - **Distributed Database**:
    Data is sharded (split) or replicated across multiple servers.
    These servers work together as one system
  - **MongoDB Cluster**:  
    - Primary node: handles all writes
    - Secondary nodes: replicate data from primary
    
  - **Cassandra**:
    - Fully peer-to-peer (no primary)
    - All nodes are equal and data is distributed using partitioning + replication
    - When you query Cassandra, it will automatically fetch data from the right node  
    - These servers work together as one system.  

  - **Regular (Centralized) Database**:
    - All data lives on one server.
    - If that server goes down → ❌ Data is unavailable.   

---

## ✅ Advantages

- High **scalability** (add more nodes to grow)
- Improved **fault tolerance** and **redundancy**
- **Better resource utilization** across servers
- Supports **geographical distribution** for global users
- Enables **parallel processing** and faster computations
- Low Latency
- No single point of failure 
- Scalable 

---

## ❌ Challenges

- **Network latency** and communication failures
- **Data consistency** (especially with replication)
- **Synchronization** and coordination between nodes
- **Debugging and monitoring** are more complex
- Requires careful **security** across network boundaries
- complex
- management required like load balancers 
- diffucult ot secure  
- Messages going from node-1 to node-2 may be lost

---

## 🔁 Relationship to Microservices

- Microservices **run on** distributed systems
- Distributed systems are the **foundation** for cloud-native and microservices-based apps
- But **not all Distributed Systems** use **Microservices** (e.g., monolithic systems distributed over multiple servers). ex: monolitic deployed the same application on multiple servers so this kind of application is logically monolithic, but physically distributed
- **Microservices are built on the principles of Distributed Systems.**
- Every Microservices architecture is a **type of Distributed System**.


---

> 💡 A well-designed distributed system balances **performance**, **reliability**, and **scalability**, but it requires careful engineering and DevOps discipline.



