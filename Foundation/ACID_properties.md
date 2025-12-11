
# ACID Properties — Clear Guide with Examples

**Author:** Prashant Kumar Tiwary
**Date:** 2025-12-11

---

## Overview

**ACID** is an acronym that describes four key properties of reliable database transactions:

- **A**: **Atomicity**  
- **C**: **Consistency**  
- **I**: **Isolation**  
- **D**: **Durability**

Together, they ensure that database transactions are processed reliably, even in the presence of errors, crashes, or concurrent activity.

---

## 1 — Atomicity

### Definition
**Atomicity** means a transaction is an indivisible unit of work: either **all** of its operations complete successfully, or **none** of them do. There is no partial completion visible to other transactions or the system.

### Example (bank transfer)
Suppose we transfer \$100 from Alice to Bob. A transaction might contain two operations:

1. `UPDATE accounts SET balance = balance - 100 WHERE id = 'Alice';`  
2. `UPDATE accounts SET balance = balance + 100 WHERE id = 'Bob';`

Atomicity ensures:
- If both updates succeed → the transaction commits.  
- If any update fails → the transaction is rolled back and neither account is changed.

### SQL pseudo-code
```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 'Alice';
UPDATE accounts SET balance = balance + 100 WHERE id = 'Bob';
-- on success:
COMMIT;
-- if failure:
ROLLBACK;
```

### How databases implement it
- Use of write-ahead logs (WAL), undo logs, or redologs  
- Transaction manager ensures either commit or rollback

---

## 2 — Consistency

### Definition
**Consistency** means that a transaction brings the database from one valid state to another valid state, **preserving all defined rules, constraints, and invariants** (like foreign keys, unique constraints, checks, triggers).

### Example
If there's a constraint that `accounts.balance >= 0`, the transfer transaction must not violate it. If debit would make `Alice` negative and that breaks a constraint, the transaction must fail and roll back.

### Important notes
- Consistency is partly the database's job (constraints, triggers) and partly the application's responsibility (business rules).
- ACID consistency ≠ Distributed systems *consistency* in CAP theorem; ACID consistency refers to integrity constraints.

---

## 3 — Isolation

### Definition
**Isolation** controls how and when the intermediate (uncommitted) state of a transaction is visible to other concurrent transactions. Proper isolation prevents concurrency anomalies.

### Common concurrency anomalies
- **Dirty read**: Transaction A reads uncommitted changes from Transaction B. If B rolls back, A saw invalid data.  
- **Non-repeatable read**: Transaction A reads the same row twice and gets different values because Transaction B modified and committed between reads.  
- **Phantom read**: Transaction A re-executes a query and finds newly inserted rows by Transaction B (matching the query predicate).

### Isolation levels (SQL standard)
1. **Read Uncommitted** — lowest isolation; dirty reads possible.  
2. **Read Committed** — prevents dirty reads; non-repeatable & phantom reads possible.  
3. **Repeatable Read** — prevents dirty & non-repeatable reads; phantom reads may still occur (behavior varies by DB).  
4. **Serializable** — highest isolation; transactions behave as if executed serially; prevents all three anomalies.

### Behavior by DB (notes)
- PostgreSQL: `REPEATABLE READ` (since PG 9.1) provides snapshot isolation which avoids many anomalies and `SERIALIZABLE` provides true serializability using predicate locking/SSI.  
- MySQL/InnoDB: `REPEATABLE READ` uses next-key locking and often prevents phantoms for many workloads; `SERIALIZABLE` enforces stricter behavior.

### Example: Non-repeatable read
Transaction T1:
```sql
BEGIN;
SELECT balance FROM accounts WHERE id = 'Alice'; -- returns 1000
-- Meanwhile, T2 updates Alice's balance and commits
SELECT balance FROM accounts WHERE id = 'Alice'; -- might return 900 (non-repeatable read)
COMMIT;
```

Under `REPEATABLE READ` (or `SERIALIZABLE`), T1 would see the same value on repeated reads (unless it deliberately refreshed with a new snapshot).

### Setting isolation level (examples)
PostgreSQL:
```sql
-- session level
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- per transaction
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

MySQL:
```sql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION;
```

---

## 4 — Durability

### Definition
**Durability** guarantees that once a transaction has committed, its changes will survive permanently — even in the case of system crashes, power failures, or OS errors.

### How durability is achieved
- Committing writes changes to durable storage (disk) or a write-ahead log before acknowledging success.  
- On crash, recovery uses these logs to replay or persist committed transactions.

### Example
After `COMMIT`, if a server power-cycles, the committed transfer still reflects in the accounts table when the database restarts.

---

## Putting ACID Together — full transaction life-cycle

1. `BEGIN` → start a transaction.  
2. Perform multiple read/write operations (atomic unit).  
3. If all operations succeed, `COMMIT` → changes become durable and visible based on isolation rules.  
4. If any operation fails, `ROLLBACK` → all changes undone (atomicity).  
5. Throughout, database constraints ensure **consistency**, and the isolation level controls concurrency behavior.

---

## Practical Examples & SQL

### Bank transfer (complete)
```sql
-- PostgreSQL example
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
UPDATE accounts SET balance = balance - 100 WHERE id = 'Alice';
UPDATE accounts SET balance = balance + 100 WHERE id = 'Bob';
COMMIT;
```

### Enforcing consistency with constraints
```sql
CREATE TABLE accounts (
  id TEXT PRIMARY KEY,
  balance NUMERIC CHECK (balance >= 0)
);
```
If the first `UPDATE` would violate `balance >= 0`, the second update won't run because the transaction can be rolled back.

---

## Common misconceptions

- **ACID means no concurrency** — Wrong. ACID allows concurrency but controls the visibility and effects via isolation and locking/snapshot mechanisms.  
- **ACID guarantees perfect performance** — No. Higher isolation (SERIALIZABLE) may reduce concurrency and throughput. It's a trade-off.  
- **ACID consistency = CAP consistency** — These are different concepts. ACID consistency refers to integrity constraints; CAP consistency is about distributed systems guarantees.

---

## Testing ACID in practice (simple tests)

### Dirty read test (read uncommitted)
1. In session A:
```sql
BEGIN;
UPDATE accounts SET balance = balance - 50 WHERE id = 'Alice';
-- do NOT COMMIT yet
```
2. In session B (set READ UNCOMMITTED if DB supports):
```sql
SELECT balance FROM accounts WHERE id = 'Alice'; -- might see the uncommitted value
```
3. If A rolls back, B would have read an invalid value (dirty read).

### Non-repeatable read test
1. Session A: `BEGIN; SELECT balance FROM accounts WHERE id='Alice';` (1000)
2. Session B: `BEGIN; UPDATE accounts SET balance = 900 WHERE id = 'Alice'; COMMIT;`
3. Session A: `SELECT balance FROM accounts WHERE id='Alice';` (maybe 900 under Read Committed)

---

## ACID vs BASE (brief)

- **ACID**: strict transactional guarantees — good for OLTP, banking, finance.  
- **BASE**: Basically Available, Soft state, Eventually consistent — often used in distributed systems/NoSQL, prioritizing availability and partition tolerance.

---

## Quick Reference Table

| Property | Guarantees | Typical DB support |
|----------|------------|--------------------|
| Atomicity | All-or-nothing transactions | All RDBMS |
| Consistency | Preserves constraints and invariants | All RDBMS |
| Isolation | Controls visibility of concurrent work | All RDBMS; levels vary |
| Durability | Committed data survives crashes | All RDBMS (if configured) |

---

## Tips for Developers

- Use transactions for multi-step updates that must be atomic.  
- Keep transactions short: long transactions increase contention and lock duration.  
- Choose the isolation level that fits your correctness/performance needs; test with your workload.  
- Use constraints and foreign keys to enforce **consistency** as much as possible in the DB, not just in app logic.  
- Monitor write-ahead log (WAL) or redo log performance for durable-heavy workloads.

---

## Further reading / pointers (suggested)
- PostgreSQL documentation: Transactions and Isolation Levels  
- MySQL/InnoDB: Transaction isolation and locking  
- Papers on Serializable Snapshot Isolation (SSI)

---

### End of document
