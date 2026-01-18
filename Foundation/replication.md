
# Replication

## 1. What is Replication?
Replication is the process of maintaining multiple copies of the same data across different servers (nodes) to improve availability, fault tolerance, and performance.

### Key Goals
- High Availability
- Fault Tolerance
- Low Latency Reads
- Data Durability

---

## 2. Why Replication is Needed (With Example)

### Example: E-commerce Application
- Users browse products (read-heavy)
- Orders are placed (write operations)

If there is only one database:
- DB crash → complete downtime
- High traffic → slow responses

With replication:
- Reads go to replicas
- Writes go to leader
- If one node fails, others continue serving

---

## 3. Types of Replication

### 3.1 Leader–Follower (Master–Slave) Replication

#### How it works
- One leader handles all writes
- Followers replicate data from leader
- Reads are served from followers

#### Example
```
User places order → Leader DB
User views product → Follower DB
```

#### Pros
- Simple to implement
- Strong consistency for writes

#### Cons
- Leader is single point of failure (for writes)

#### Used in
- MySQL
- PostgreSQL
- Most microservice architectures

---

### 3.2 Multi-Leader (Multi-Master) Replication

#### How it works
- Multiple leaders accept writes
- Data is synced across leaders

#### Example
- User updates profile in India
- Same user updates profile in US
- System must resolve conflicts

#### Conflict Resolution Strategies
- Last Write Wins (LWW)
- Version vectors
- Custom business rules

#### Pros
- High write availability
- Suitable for geo-distributed systems

#### Cons
- Complex conflict handling

---

### 3.3 Leaderless Replication

#### How it works
- No fixed leader
- Any node can accept read/write
- System uses quorum mechanism

#### Example
- Write to 2 out of 3 nodes = success
- Read from 2 out of 3 nodes

#### Pros
- No single point of failure
- Highly available

#### Cons
- Eventual consistency
- Complex design

#### Used in
- Cassandra
- DynamoDB

---

## 4. Replication Strategies

### 4.1 Synchronous Replication

#### Definition
Write is confirmed only after all replicas acknowledge.

#### Example
```
Bank transaction:
Leader → Replica 1 → Replica 2 → Success
```

#### Pros
- Strong consistency
- No data loss

#### Cons
- High latency

#### Use cases
- Banking
- Financial systems

---

### 4.2 Asynchronous Replication

#### Definition
Leader responds immediately; replicas update later.

#### Example
```
Instagram post:
Write → Leader → Success (replicas sync later)
```

#### Pros
- Fast
- Scalable

#### Cons
- Temporary inconsistency

#### Use cases
- Social media
- E-commerce

---

## 5. Replication Lag

### What is Replication Lag?
Delay between leader write and follower update.

### Problems
- Stale reads
- Inconsistent user experience

### Solutions
- Read-your-writes consistency
- Read from leader for critical data
- Monitoring replication lag

---

## 6. Replication vs Sharding

| Replication | Sharding |
|------------|---------|
| Copies same data | Splits data |
| Improves availability | Improves scalability |
| Same dataset everywhere | Different data per shard |

### Real-world systems use both together.

---

## 7. CAP Theorem and Replication

CAP = Consistency, Availability, Partition Tolerance

- CP systems: Synchronous replication
- AP systems: Asynchronous / leaderless replication

---

## 8. Failure Scenarios

### Leader Failure
- Promote follower to leader
- Requires leader election

### Network Partition
- Split-brain problem in multi-leader
- Solved using quorum and consensus

---

## 9. Object-Oriented Design (OOD) Example

### Replication Interface
```java
public interface ReplicationStrategy {
    void write(Data data);
    Data read(String key);
}
```

### Leader-Follower Implementation
```java
public class LeaderFollowerReplication implements ReplicationStrategy {
    public void write(Data data) {
        writeToLeader(data);
        replicateToFollowers(data);
    }

    public Data read(String key) {
        return readFromFollower(key);
    }
}
```

---

## 10. Interview Tips

### One-liners
- Replication improves availability and fault tolerance.
- Leader–Follower is simplest and most common.
- Asynchronous replication favors availability over consistency.

### Common Interview Questions
- Replication vs Sharding?
- How do you handle replication lag?
- What happens if leader fails?

---

## 11. Summary
Replication is a core concept in system design that ensures systems remain available, scalable, and resilient under failures. Choosing the right replication strategy depends on consistency, latency, and business requirements.


