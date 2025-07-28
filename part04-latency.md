# 📘 Latency in System Design

## ❓ What is Latency?

Latency is the **time delay** between a user's action and the system's response.

> In system design, **latency** refers to the total time it takes for a request to travel from the client to the server, be processed, and for the response to return back to the client.
>
> ⚡ **Latency = Network delay + Computation delay**
>
> **Even more complete and practical for system design** ⚡ **Total Latency = Network Delay + Processing (Computation) Delay + Queuing Delay + Serialization/Deserialization Delay**
>
>
> ✅ Lower latency means faster responses and better user experience.
---

## 🧠 Why is Latency Important?

- Directly affects **user experience**
- Impacts the **performance** of distributed systems
- Critical in **real-time applications** (e.g., online gaming, stock trading, video conferencing)
- Lower latency = **faster, more responsive system**

---

## 🚦 Types of Latency

| Type                   | Description                                          |
|------------------------|------------------------------------------------------|
| **Network Latency**    | Delay in transmitting data over the network          |
| **Disk Latency**       | Time taken to read/write data from storage           |
| **Application Latency**| Time taken by app logic to process the request       |
| **Queue Latency**      | Time a request waits in a queue before processing    |
| **End-to-End Latency** | Total time from sending a request to receiving reply |

---

## 🔁 Latency vs Throughput

| Concept     | Meaning                                      |
|-------------|----------------------------------------------|
| **Latency** | Time taken to complete a single request      |
| **Throughput** | Number of requests completed per second   |

> **Low latency** is not the same as **high throughput** — a system can respond quickly (low latency) but handle fewer requests per second (low throughput), or vice versa.

---

## 📉 How to Reduce Latency

- ✅ Use **caching** (e.g., Redis, Memcached). It is the process of storing the information for a set of period of time on computer. 
- ✅ Use **Content Delivery Networks (CDNs)** for static content. CDNs are the geographically networks of proxy servers and purpose is to serve the content to the user more quickly. 
- ✅ **Optimize database queries** and indexing
- ✅ **Compress** responses (GZIP)
- ✅ Use **asynchronous processing**
- ✅ Deploy services **closer to the user** (geo-distribution)
- ✅ Upgrade to **faster storage** (e.g., SSDs)

---

## 📦 Example Flow

Total latency is the sum of:
- Network delay (client-server)
- Backend processing time
- DB access latency
- Response transmission

---

## 📏 Tools to Measure Latency

- 🧪 `ping`, `curl`, `traceroute`
- 📊 Application logs & metrics - ELK 
- 📈 Distributed tracing (e.g., OpenTelemetry, Zipkin, Jaeger)
- 🧵 Application Performance Monitoring - APM tools (Grafana, Kibana, New Relic, Datadog)

---

## 📌 Summary

- **Latency** = delay before a response
- Affects **responsiveness** and **user satisfaction**
- Minimize it via **caching, CDNs, code optimization**, and **geo-redundancy**



