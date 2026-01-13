
# CDN (Content Delivery Network) in System Design – Notes

## 1. What is a CDN?
A **CDN (Content Delivery Network)** is a globally distributed network of servers (called **edge servers**) that deliver content to users from the nearest geographical location.

**Goal:**
- Reduce latency
- Improve performance
- Protect origin servers
- Scale globally

---

## 2. Is CDN a Cache?
**Short answer:**  
CDN uses caching, but CDN itself is **not just a cache**.

- **Cache** → a technique to store data temporarily for fast access
- **CDN** → a distributed system that uses caching + networking + security

> Cache is a mechanism. CDN is a system that heavily relies on caching.

---

## 3. Why CDN is Needed

Without CDN:
- All users hit a central server
- High latency for distant users
- Heavy load on origin server

With CDN:
- Requests served from nearest edge server
- Low latency
- Reduced backend traffic
- Better user experience

---

## 4. CDN Request Flow (Step-by-Step)

1. User requests a resource (e.g. image, JS file)
2. DNS routes the request to the nearest CDN edge server
3. CDN checks cache:
   - Cache Hit → return content immediately
   - Cache Miss → fetch from origin server
4. CDN stores the response
5. Future requests are served from cache

---

## 5. What Does CDN Cache?

### Best Candidates for CDN Caching
- Images
- Videos
- CSS files
- JavaScript files
- Fonts
- Static HTML pages

### Sometimes Cached
- Public GET APIs
- Read-only endpoints

### Usually Not Cached
- Personalized user data
- Authenticated responses
- Highly dynamic content

---

## 6. CDN Cache Expiration (TTL)

CDNs respect HTTP cache headers.

Example:
```
Cache-Control: max-age=3600
```

- Content cached for 1 hour
- After expiration, CDN refreshes content from origin

TTL helps:
- Prevent stale data
- Control cache freshness

---

## 7. CDN vs Application Cache (Redis, In-Memory)

| Feature | CDN | Application Cache |
|------|-----|------------------|
| Location | Global (Edge servers) | Backend |
| Data Type | Mostly static | Mostly dynamic |
| Latency | Extremely low | Low |
| User-Specific Data | No | Yes |
| Scale | Internet-level | Service-level |

---

## 8. Additional Capabilities of CDN

Beyond caching, CDN provides:
- Load balancing
- Geo-based routing
- DDoS protection
- SSL termination
- Compression and optimization

---

## 9. When to Use CDN

Use CDN when:
- You have global users
- Application serves static or media-heavy content
- You want lower latency
- You need protection from traffic spikes

Examples:
- Video streaming platforms
- E-commerce websites
- News portals
- Social media applications

---

## 10. Interview-Ready Explanation

> A CDN is a globally distributed network of edge servers that delivers content closer to users.  
> It uses caching to store static and cacheable content, reducing latency and offloading traffic from origin servers.  
> While caching is a core component, CDN also provides routing, security, and performance optimization.

---

## 11. Key Takeaways

- CDN improves speed and scalability
- CDN uses caching but is more than cache
- Best suited for static and public content
- Reduces load on backend services

---

End of Notes
