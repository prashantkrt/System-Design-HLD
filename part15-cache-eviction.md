# Cache Eviction Techniques

Cache eviction techniques determine which data to remove when a cache reaches capacity. Below are the key strategies, including all specified methods.

## 1. Least Recently Used (LRU)
- **Description**: Removes the least recently accessed item.
- **Mechanism**: Tracks access order using a doubly linked list and hash map.
- **Use Case**: Workloads with temporal locality (recently used data likely reused).
- **Pros**: Prioritizes frequently used data.
- **Cons**: High overhead for maintaining order.

## 2. Least Frequently Used (LFU)
- **Description**: Evicts the item with the lowest access frequency.
- **Mechanism**: Maintains a frequency counter for each item.
- **Use Case**: Caches where frequency predicts future use.
- **Pros**: Effective for stable access patterns.
- **Cons**: Memory overhead for counters; struggles with recency shifts.

## 3. Most Recently Used (MRU)
- **Description**: Evicts the most recently accessed item.
- **Mechanism**: Tracks the latest access, removing it first.
- **Use Case**: Niche scenarios where least recent data is likely reused (e.g., cyclic access patterns).
- **Pros**: Simple for specific workloads.
- **Cons**: Poor for typical temporal locality.

## 4. Last-In, First-Out (LIFO)
- **Description**: Removes the most recently added item (stack-like).
- **Mechanism**: Uses a stack to track insertion order.
- **Use Case**: Rare in caching; suited for stack-based applications.
- **Pros**: Simple implementation.
- **Cons**: Ignores access patterns; often inefficient.

## 5. First-In, First-Out (FIFO)
- **Description**: Removes the oldest item added to the cache.
- **Mechanism**: Uses a queue to track insertion order.
- **Use Case**: Simple caches with uniform access patterns.
- **Pros**: Low overhead; easy to implement.
- **Cons**: Ignores access frequency or recency.

## 6. Random Replacement (RR)
- **Description**: Evicts a randomly selected item.
- **Mechanism**: Uses a random number generator.
- **Use Case**: Low-complexity systems with unpredictable access.
- **Pros**: Minimal overhead; simple.
- **Cons**: Poor performance for non-random workloads.

## 7. Time-To-Live (TTL)
- **Description**: Removes items after a predefined expiration time.
- **Mechanism**: Assigns a timestamp or expiration to each item.
- **Use Case**: Caches with time-sensitive data (e.g., session tokens).
- **Pros**: Predictable eviction; prevents stale data.
- **Cons**: Requires time tracking; may evict useful data prematurely.

## 8. Adaptive Replacement Cache (ARC)

- **Description**: Smartly chooses between recent and frequent data.
- **Simple Idea**: ARC combines the **best of LRU and LFU**. It keeps two types of items:
  - Recently used
  - Frequently used

- **Mechanism**:
  - Maintains **two lists**:
    - One for items used **recently** (like LRU)
    - One for items used **frequently** (like LFU)
  - ARC automatically adjusts how much space to give each list based on the usage pattern.

- **Use Case**: When your data sometimes changes frequently and sometimes stays stable.
- **Pros**: Adapts smartly to different workloads.
- **Cons**: More complex to implement and manage.


## 9. Clock (Second-Chance FIFO)

- **Description**: A faster and simpler version of LRU that gives every item a second chance before removing it.
- **Simple Idea**:
  - Items are arranged in a **circle** (like a clock).
  - Each item has a **"used recently" flag** (called a reference bit).
  - When it’s time to evict:
    - If the item’s flag is **0** (not used recently), remove it.
    - If the flag is **1**, reset it to **0** and move to the next item (give it a second chance).

- **Mechanism**:
  - Works like a clock hand moving around.
  - Keeps checking items until it finds one that wasn’t used recently.

- **Use Case**: Good for systems that need LRU-like behavior but with lower cost.
- **Pros**: Less memory and processing than true LRU.
- **Cons**: Not as accurate as full LRU, but close enough for many cases.