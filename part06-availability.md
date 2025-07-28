# ✅ Availability in System Design

## 🔍 What is Availability?

**Availability** is the ability of a system or component to be operational and accessible when required. Fault Tolerance directly contributes to High Availability in system design.


> **Formula:**  
> **Availability (%) = (Uptime) / (Uptime + Downtime) × 100**

### 📌 Example:
If your service was up **999 hours** and down **1 hour**: Availability = 999 / (999 + 1) = 99.9%

---

## 📈 Example Uptime Levels

| Availability % | Downtime/Year    |
|----------------|------------------|
| 99% (Two 9s)   | ~3.65 days       |
| 99.9% (Three 9s)| ~8.76 hours      |
| 99.99%         | ~52.6 minutes    |
| 99.999%        | ~5.26 minutes    |

---

## 🧠 How to Improve Availability?

- ✅ **Replication**: Duplicate data or service instances across nodes or zones to improve availability, reliability, and fault tolerance.
- ✅ **Redundancy**: Extra hardware/software components that take over on failure.
- ✅ **Load Balancing**: Distribute traffic across multiple servers.
- ✅ **Failover Mechanisms**: Automatically switch to a backup system.
- ✅ **Health Checks & Monitoring**: Detect failures early (Spring Boot + Actuator, Prometheus).
- ✅ **Graceful Degradation**: Partial functionality when some services are down.
- ✅ **Retry Logic & Circuit Breakers**: Prevent system overload (Resilience4j, Hystrix).
- ✅ **Distributed Systems**: Deploy services across regions/zones.

---

## 🔄 Replication vs Redundancy

| Replication                              | Redundancy                                  |
|------------------------------------------|---------------------------------------------|
| Focuses on **data duplication**          | Focuses on **component duplication**        |
| Used in databases (e.g., MySQL replicas) | Used in systems (e.g., backup servers)      |
| Improves availability & read performance | Improves fault tolerance & failover         |

---

## ☕ Java Developer Perspective

- Use **Spring Boot Actuator** for health monitoring.
- Use **@Retryable** from Spring Retry or **Resilience4j** for circuit breaking and retries.
- Use **Kubernetes liveness/readiness probes** for container health checks.
- Store logs in **ELK stack** for diagnosis.
- Monitor latency/throughput/availability with **Prometheus + Grafana**.
- For cloud, use **multi-zone deployment** in AWS/GCP/Azure for high availability.

---

## 🧪 Tools for Testing Availability

- **Chaos Engineering** (Simian Army, Chaos Monkey for Spring Boot)
- **Load Testing Tools**: Apache JMeter, Gatling
- **Monitoring**: Prometheus, Grafana, New Relic, Datadog

---



## 🧠 Summary

> High availability = Resilience + Monitoring + Redundancy + Smart Design  
> "Design for failure and nothing fails."
> **Redundancy**: Extra hardware/components to take over during failure.
> **Replication**: Duplicate data/services to avoid single points of failure.

