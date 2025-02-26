### **Proxy vs Reverse Proxy**

Both **proxy** and **reverse proxy** serve as intermediaries between clients and servers but have different purposes.

---

## **1. Proxy (Forward Proxy)**
A **proxy server** sits between the client (e.g., browser) and the internet. It forwards client requests to the target server and then returns the response back to the client.

### **Use Cases of Proxy:**
✅ **Anonymity & Security:** Hides the client’s IP address.  
✅ **Content Filtering:** Schools & organizations use proxies to restrict access.  
✅ **Caching:** Stores frequently accessed web pages to improve load time.  
✅ **Geo-Unblocking:** Bypass regional restrictions (e.g., accessing Netflix US from India).

### **Example of a Forward Proxy:**
A user wants to visit `example.com`, but instead of making a direct request, they go through a proxy server.

```
Client → Proxy Server → example.com
```

---

## **2. Reverse Proxy**
A **reverse proxy** sits in front of a server (or multiple servers) and handles requests from clients on behalf of the backend server(s).

### **Use Cases of Reverse Proxy:**
✅ **Load Balancing:** Distributes traffic across multiple backend servers.  
✅ **SSL Termination:** Manages HTTPS encryption and decryption.  
✅ **Security:** Hides internal server details and prevents direct exposure.  
✅ **Caching & Compression:** Reduces server load and improves performance.  

### **Example of a Reverse Proxy:**
When a user requests `example.com`, the request first goes through a reverse proxy, which forwards it to an appropriate backend server.

```
Client → Reverse Proxy → Backend Server(s)
```

---

## **Key Differences:**

| Feature        | Proxy (Forward Proxy) | Reverse Proxy |
|--------------|------------------|------------------|
| **Position** | Sits between client & internet | Sits between client & backend server |
| **Client Identity** | Hides client’s identity from the server | Hides server details from the client |
| **Use Case** | Privacy, security, and content filtering | Load balancing, security, caching |
| **Example** | VPN, web proxies (e.g., HideMyAss) | Nginx, HAProxy, AWS ALB, Cloudflare |

---

## **Examples in Real World:**
- **Proxy:** A VPN service like NordVPN acts as a forward proxy, allowing users to access blocked content.
- **Reverse Proxy:** Cloudflare acts as a reverse proxy, protecting websites from DDoS attacks and improving performance.