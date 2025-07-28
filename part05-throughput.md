# 🚀 Throughput in System Design

## 📘 What is Throughput?

> **Throughput** refers to the number of requests or transactions a system can process per unit of time. It is volume of work or the information flowing through a system

It measures the **capacity** of the system.

- Usually measured as:
  - ✅ **Requests per second (RPS)**
  - ✅ **Transactions per second (TPS)**
  - ✅ **Messages per second**

---

## 🧠 Simple Analogy

> Imagine a toll booth on a highway.

- **Latency** = Time it takes one car to pass through.
- **Throughput** = Number of cars passing per minute.

Even if latency is low, **bad throughput** means fewer cars can pass per minute.

---

## 🏗️ Throughput vs Latency

| Metric     | Meaning                                     | Unit                     |
|------------|---------------------------------------------|--------------------------|
| Latency    | Time to handle a single request             | Milliseconds (ms)        |
| Throughput | Number of requests handled per second       | Requests per second (RPS)|

---

## 📈 What Affects Throughput?

- 🔄 **Concurrency level** (threads, async, non-blocking I/O)
- 🧮 **CPU/memory** capacity
- 🛢️ **Database performance**
- 📡 **Network bandwidth**
- 🚪 **Queue sizes** and processing speed
- ⏱️ **Rate limits / throttling**

---

## 📊 How to Measure It?

 ### You can visualize throughput as: 
 - req/sec – very common
 - req/min – easier to understand over longer timeframes
 - req/hour – useful for business metrics (e.g., hourly traffic patterns)

Use tools like:

- **Apache JMeter** Load testing APIs, DB, etc.
- **Gatling**
- **Locust**
- **wrk** HTTP benchmarking. Simple and blazing fast for benchmarking your Java HTTP services (Spring Boot endpoints, REST APIs, etc.)
- **Prometheus + Grafana dashboards** Real-time metrics
- **APM Tools** (Datadog, Elastic APM, New Relic) Deep performance insights

---

## 💡 Tips to Improve Throughput

- Use **asynchronous/non-blocking I/O**
- Add **caching** (Redis, Memcached)
- Optimize **DB queries & indexes**
- Introduce **load balancers**
- **Horizontal scaling** (add more servers)
- **vertical scaling** for quick performance gains (more CPU/RAM)

---

## 🔚 Summary

- Throughput tells how **much** your system can handle.
- Focus on **throughput** when you want to scale out.
- Focus on **latency** when users are waiting.

> 📌 High throughput + low latency = Great performance!





