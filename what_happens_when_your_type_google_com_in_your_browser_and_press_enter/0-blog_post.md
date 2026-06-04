# What Happens When You Type https://www.google.com

---

## README.md

```markdown
# What Happens When You Type https://www.google.com

## Description
A blog post explaining the complete flow of what happens
when you type https://www.google.com in your browser and press Enter.

## Blog Post URL
[Insert your Medium/LinkedIn URL here]

## Author
[Abdel Mourid]
```

---

## Blog Post

---

# 🌐 What Happens When You Type `https://www.google.com` and Press Enter?

*A deep dive into the infrastructure behind every web request.*

---

## 1. 🔍 DNS Request – Translating the Domain Name

When you type `https://www.google.com`, your browser doesn't understand domain names — it needs an **IP address**.

**The DNS resolution process:**
1. Browser checks its **local cache**
2. OS checks the **hosts file** (`/etc/hosts`)
3. Query sent to **Recursive DNS Resolver** (your ISP or 8.8.8.8)
4. Resolver asks **Root Name Server** → `.com TLD server` → **Google's Authoritative Name Server**
5. Returns IP address (e.g., `142.250.74.46`)

```
Browser → Local Cache → OS Cache → ISP Resolver → Root NS → .com TLD → google.com NS → IP
```

---

## 2. 🔗 TCP/IP – Establishing the Connection

With the IP address known, your browser initiates a **TCP connection** using the **3-Way Handshake**:

```
Client  ──── SYN ────────► Server
Client  ◄─── SYN-ACK ───── Server
Client  ──── ACK ────────► Server
✅ Connection Established
```

- **IP** handles routing packets across networks
- **TCP** ensures reliable, ordered delivery
- Connection is made on **port 443** (HTTPS)

---

## 3. 🔥 Firewall – First Line of Defense

Before reaching Google's servers, your request passes through **firewalls**:

| Firewall Level | Purpose |
|----------------|---------|
| Your router/OS | Blocks unauthorized outgoing traffic |
| Google's network firewall | Filters malicious requests, DDoS protection |
| Application firewall (WAF) | Blocks SQL injection, XSS, etc. |

The firewall inspects:
- Source/destination IP
- Port numbers
- Packet content (deep inspection)

---

## 4. 🔐 HTTPS/SSL – Securing the Connection

Since we're using **HTTPS**, a **TLS Handshake** occurs:

```
1. Client Hello     → Browser sends supported TLS versions & cipher suites
2. Server Hello     → Server chooses cipher, sends SSL Certificate
3. Certificate      → Browser verifies cert with Certificate Authority (CA)
4. Key Exchange     → Symmetric session key is generated
5. Encrypted        → All data is now encrypted 🔒
```

**Why it matters:**
- Encrypts data in transit
- Verifies you're talking to the **real** Google
- Prevents man-in-the-middle attacks

---

## 5. ⚖️ Load Balancer – Distributing the Traffic

Google receives **billions** of requests daily. A **Load Balancer** distributes traffic across multiple servers:

```
                    ┌─► Web Server 1
Request ──► LB ────┼─► Web Server 2
                    └─► Web Server 3
```

**Load balancing algorithms:**
- **Round Robin** – requests distributed evenly
- **Least Connections** – sent to least busy server
- **IP Hash** – same user → same server

This ensures **high availability** and **no single point of failure**.

---

## 6. 🖥️ Web Server – Handling the HTTP Request

The request reaches a **Web Server** (Google uses its own servers, similar to **Nginx** or **Apache**):

**Web server responsibilities:**
- Receives HTTP/HTTPS request
- Serves **static content** (HTML, CSS, images) directly
- Forwards dynamic requests to the **Application Server**
- Handles SSL termination

```
GET / HTTP/1.1
Host: www.google.com
Accept: text/html
```

---

## 7. ⚙️ Application Server – Processing the Logic

For dynamic content, the web server forwards to the **Application Server**:

- Executes **business logic**
- Processes your search query
- Calls **microservices** (spell check, autocomplete, ranking algorithms)
- Communicates with the **database**
- Returns processed data to web server

```
Web Server ──► App Server ──► [Business Logic] ──► Database
                          ◄── [Processed Result] ◄──
```

---

## 8. 🗄️ Database – Retrieving the Data

The Application Server queries Google's **distributed databases**:

- **Google uses:** Bigtable, Spanner, and custom distributed systems
- Searches indexed web pages
- Retrieves relevant results based on your query
- Returns data to Application Server

```
App Server ──► Database Query ──► Index Lookup ──► Results
```

**Database types used:**
| Type | Purpose |
|------|---------|
| Relational (SQL) | Structured user data |
| NoSQL | Web index, large-scale data |
| Cache (Redis/Memcached) | Fast repeated queries |

---

## 🔄 Complete Flow Summary

```
You type https://www.google.com
         │
         ▼
    DNS Resolution → IP: 142.250.74.46
         │
         ▼
    TCP 3-Way Handshake (Port 443)
         │
         ▼
    Firewall Check ✅
         │
         ▼
    TLS/SSL Handshake 🔒
         │
         ▼
    Load Balancer ⚖️
         │
         ▼
    Web Server (Nginx/Apache)
         │
         ▼
    Application Server (Business Logic)
         │
         ▼
    Database Query 🗄️
         │
         ▼
    Response travels back same path
         │
         ▼
    Browser renders Google's homepage 🎉
```

---

## 🎯 Conclusion

In less than **500 milliseconds**, your simple request travels through:
- DNS resolution
- TCP/IP networking
- Firewall security
- SSL encryption
- Load balancing
- Web & application servers
- Database lookups

...and returns a fully rendered page to your browser.

Understanding this flow is **fundamental** for any Full-Stack Engineer, SRE, or DevOps professional.

---

*Written by [Abdel Mourid]*

---