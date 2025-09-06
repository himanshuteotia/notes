Edge servers are the **physical servers in a CDN** that sit closer to end users geographically and handle delivering cached content. They’re the “edge” of the network — between the user and the origin data center.

---

### Why they exist

If every request had to go to the **origin server** (say, your S3 bucket in Virginia), someone in Dubai or India would have high latency and poor performance. Edge servers solve this by acting as **local caches**.

---

### How they work

1. **First request (cache miss)**

   * User in Dubai requests `cdn.myapp.com/profile-images/user123.png`.
   * Request is routed to the nearest edge server (say, AWS CloudFront PoP in Dubai).
   * Edge doesn’t have the file → it fetches from origin (S3), caches it locally, then serves the user.

2. **Subsequent requests (cache hit)**

   * Another user in Dubai requests the same image.
   * Edge already has it in cache → serves instantly, no round-trip to origin.

3. **Cache expiration**

   * Each cached object has a TTL (from `Cache-Control` headers).
   * After TTL, edge either refreshes from origin or continues to serve until revalidated.

---

### What runs on edge servers

* **Cache storage**: images, videos, CSS, JS, etc.
* **TLS termination**: handle HTTPS so origin doesn’t get overloaded.
* **Routing/Load balancing**: decide best path for requests.
* **Security filters**: block malicious traffic (DDoS, WAF rules).
* **Edge compute**: modern CDNs (Cloudflare Workers, AWS Lambda\@Edge, Fastly Compute\@Edge) let you run logic right at the edge (auth checks, redirects, AB testing).

---

### Analogy

Think of the **origin server** as a central factory.
**Edge servers** are warehouses spread across cities.
Customers (users) don’t wait for shipping from the factory every time; they pick it up from the nearest warehouse.

---

👉 In short:
Edge servers = distributed CDN nodes that **cache, secure, and sometimes compute** content close to users, reducing latency and origin load.
