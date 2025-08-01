# 🕰️ Lamport Logical Clock

The **Lamport Logical Clock** is a concept used in distributed systems to order events **without relying on synchronized physical clocks**.

Proposed by **Leslie Lamport**, it helps maintain a **partial ordering of events** across distributed processes.

---

## 🔍 Why Do We Need It?

In distributed systems:
- There is **no global clock**.
- Network delays can cause messages to **arrive out of order**.
- We still need to **determine the correct sequence of events**.

Lamport Logical Clock provides a **logical timestamp** to events to help us order them correctly.

---

## ⚙️ How It Works

Each process maintains a **local counter** (logical clock).

### 🧾 Rules:

1. **Before any event (send, internal action)**:  
   → Increment the local clock.  
   `L = L + 1`

2. **When sending a message**:  
   → Increment the clock, then send it along with the message.  
   `Send(msg, timestamp=L)`

3. **When receiving a message with timestamp T**:  
   → Update the local clock:  
   `L = max(L, T) + 1`

---

## 📘 Example

Imagine two processes: **P1** and **P2**

| Event               | P1 Clock | P2 Clock |
|---------------------|----------|----------|
| Start               | 1        | 1        |
| P1 sends msg        | 2        |          |
| P2 receives msg     |          | max(1,2)+1 = 3 |
| P2 does internal op |          | 4        |

> After receiving a message, the receiver's clock must always be **ahead of the sender’s**.

## 📘 Real-World Example

Let’s say we have two backend services:

- **P1** → User Service  
- **P2** → Notification Service  

Each starts with a Lamport clock of 1.

---

### 💬 Events Step-by-Step:

| Step | Description                                  | P1 Clock | P2 Clock | Notes                                       |
|------|----------------------------------------------|----------|----------|---------------------------------------------|
| 1    | P1 starts and performs an action             | 1 → 2    | 1        | P1 increments before the action             |
| 2    | P1 sends message to P2 (with timestamp 2)    | 2        |          | Message contains clock = 2                  |
| 3    | P2 receives message from P1                  | 2        | 1 → 3    | P2 sets clock to `max(1,2) + 1 = 3`         |
| 4    | P2 does its own internal operation           | 2        | 3 → 4    | Increments clock for internal event         |

---

### 🧠 What’s Going On?

- **P1** performs a user-related action and sends a message to P2 with timestamp 2.
- **P2** receives the message, compares it to its current clock, and updates it to `max(1, 2) + 1 = 3`.
- **P2** then performs an internal task (like sending a notification), increasing its clock to **4**.
- This maintains the **correct logical order** across both services.
---

## ✅ Benefits

- Simple to implement  
- Provides a **causal ordering**: if event A → B, then timestamp(A) < timestamp(B)

---

## ❌ Limitations

- Only provides **partial ordering**  
- Cannot detect **concurrent events** (i.e., events that are independent)

---

## 🔁 Related Concept

- **Vector Clock**: More advanced; can detect concurrent events as well as causal ones.



