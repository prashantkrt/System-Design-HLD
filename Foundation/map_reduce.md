
# Map–Reduce — Guide + Real-World Examples

**Author:** Generated for Prashant  
**Date:** 2025-12-11

---

## What is Map–Reduce? (Simple definition)

**Map–Reduce** is a programming model for processing large datasets in parallel across a distributed cluster. It breaks a job into two primary phases:

- **Map**: processes input data and emits intermediate key-value pairs.  
- **Reduce**: aggregates intermediate values for each key to produce final results.

There is also a **Shuffle** phase between Map and Reduce that groups values by key and moves them to the right reducer (handled by the framework).

---

## Why Map–Reduce?

- Scales horizontally across many machines  
- Processes petabytes of data reliably  
- Built-in fault tolerance (re-execute failed tasks)  
- Simple model for parallel aggregation (counting, summing, grouping)

Common frameworks: **Hadoop MapReduce**, **Apache Spark (map/reduce-like APIs)**, **Google's original MapReduce system**.

---

## Map–Reduce Flow (high level)

1. Input data is split into chunks (input splits).  
2. **Mapper** runs on each split, producing `(key, value)` pairs.  
3. Engine shuffles and sorts intermediate pairs by key and sends them to reducers.  
4. **Reducer** processes all values for a key and outputs final `(key, result)` pairs.  
5. Results are written to distributed storage.

---

## Core Concepts

- **Stateless mappers**: each map task processes its input independently.  
- **Combiners** (optional): local reducers that run after map to reduce network I/O by partially aggregating values on the mapper node.  
- **Partitioner**: decides which reducer gets which key (commonly hash(key) % numReducers).  
- **Fault tolerance**: failed map/reduce tasks are retried automatically.

---

## Classic Example Patterns

### 1. Word Count (distributed)
Count how many times each word appears in a huge corpus.

**Map**: for each word emit `(word, 1)`  
**Shuffle**: group counts per word  
**Reduce**: sum counts for each word

**Pseudo-Python Map**
```python
# mapper
for line in sys.stdin:
    for word in line.strip().split():
        print(f"{word}\t1")
```

**Pseudo-Python Reduce**
```python
# reducer
current_word = None
current_count = 0
for line in sys.stdin:
    word, count = line.strip().split('\t')
    count = int(count)
    if word == current_word:
        current_count += count
    else:
        if current_word:
            print(f"{current_word}\t{current_count}")
        current_word = word
        current_count = count
# last key
if current_word:
    print(f"{current_word}\t{current_count}")
```

---

### 2. Trending Hashtags (Twitter)
Find top hashtags in millions of tweets.

**Map**: emit `(hashtag, 1)` for each hashtag found in a tweet  
**Combine**: sum counts on map node per hashtag (reduces traffic)  
**Reduce**: sum all counts and optionally run a final sort/top-k

---

### 3. Most Watched Movies (Analytics)
Compute total views per movie.

**Map**: read logs `movieId,userId,timestamp` → emit `(movieId, 1)`  
**Reduce**: sum values for each movieId → total view count

---

### 4. Sales Aggregation (eCommerce)
Total revenue per product.

**Map**: read orders `orderId,productId,qty,price` → emit `(productId, qty * price)`  
**Reduce**: sum values per productId → total revenue

---

### 5. Unique Visitors per Page (distinct counting)
Count unique users per page (requires deduplication or HyperLogLog for approximate counts).

**Map**: emit `(pageId, userId)`  
**Reduce**: collect unique userIds and count them (or use approximate algorithms)

---

### 6. PageRank Iteration (Graph processing)
Each Map–Reduce iteration spreads PageRank contributions along outgoing links; reducer aggregates contributions and recomputes new PageRank. Repeat multiple iterations (or use graph engines like Giraph/Spark GraphX).

---

### 7. Fraud Detection (financial logs)
Compute user-level aggregates (sum of amounts, frequency) and flag anomalies.

**Map**: emit `(userId, transactionData)`  
**Reduce**: aggregate and compute risk score per user

---

## Implementation Notes & Best Practices

- **Use combiners** for associative/commutative operations (sum, count) to reduce network I/O.  
- **Choose an appropriate number of reducers** — too few causes bottlenecks; too many increases overhead.  
- **Write idempotent reducers** — reruns should not corrupt output.  
- **Keep map/reduce functions stateless** and avoid large per-task memory usage.  
- **Use compression** for map output to reduce shuffle size.  
- **Skew handling**: hot keys (very frequent keys) can overload a reducer; use techniques like key-splitting or custom partitioners.

---

## Hadoop MapReduce Example (Java — high level)

**Mapper**
```java
public class TokenizerMapper extends Mapper<LongWritable, Text, Text, IntWritable> {
  private final static IntWritable one = new IntWritable(1);
  private Text word = new Text();

  public void map(LongWritable key, Text value, Context context) {
    String line = value.toString();
    for (String token : line.split("\\s+")) {
      word.set(token);
      context.write(word, one);
    }
  }
}
```

**Reducer**
```java
public class IntSumReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
  public void reduce(Text key, Iterable<IntWritable> values, Context context) {
    int sum = 0;
    for (IntWritable val : values) sum += val.get();
    context.write(key, new IntWritable(sum));
  }
}
```

---

## Advanced Topics

- **Map-side joins**: join a large dataset with a small lookup table by loading the small table into memory on mappers.  
- **Reduce-side joins**: co-partition data by join key and join in reducer (costly).  
- **Secondary sorting**: sort by composite keys using custom partitioner and comparator.  
- **Iterative processing**: MapReduce is batch-oriented; use Spark or Flink for iterative/streaming workloads.  
- **Streaming / Real-time**: for low-latency streaming, prefer systems like Kafka Streams, Flink, or Spark Streaming.

---

## Common Interview Questions & Answers (short)

**Q: What is the shuffle phase?**  
A: The shuffle groups mapper outputs by key, sorts them, and transfers groups to reducers.

**Q: Why use combiners?**  
A: To reduce intermediate data transferred during shuffle by pre-aggregating on mapper nodes.

**Q: How to handle skewed keys?**  
A: Use techniques like salting (append random prefix), custom partitioner, or splitting heavy keys across reducers.

**Q: MapReduce vs Spark?**  
A: MapReduce is batch, disk-based between stages; Spark uses in-memory RDDs/DAG which makes iterative and interactive analytics much faster.

---

## Wrap-up — One-liners you can use

- **Map**: "Process and emit key-value pairs for local data."  
- **Shuffle**: "Group by key and move to reducers."  
- **Reduce**: "Aggregate grouped values into final output."  
- **MapReduce**: "Parallelize big-data aggregation by mapping and reducing."

---

## Further Reading

- Google MapReduce paper (2004)  
- Hadoop MapReduce documentation  
- Apache Spark (for faster, in-memory alternatives)  
- Research on combiners, partitioners, and skew-mitigation techniques

---

## End of document
