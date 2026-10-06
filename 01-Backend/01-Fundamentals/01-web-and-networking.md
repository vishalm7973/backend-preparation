# What Happens When You Enter a URL in a Browser?

When you enter a URL such as:

```text
https://example.com/products
```

the browser goes through several steps before showing the webpage.

The simplified flow is:

```text
User enters URL
       ↓
Browser checks cache
       ↓
DNS finds IP address
       ↓
TCP / QUIC connection
       ↓
TLS handshake (HTTPS)
       ↓
HTTP Request
       ↓
CDN / WAF / Reverse Proxy / Load Balancer
       ↓
Backend Server
       ↓
Cache (Redis) / Database
       ↓
HTTP Response
       ↓
Browser
       ↓
HTML + CSS + JavaScript
       ↓
DOM + CSSOM
       ↓
Layout → Paint
       ↓
User sees the webpage
```

---

# Step 1: Browser Reads the URL

Suppose the user enters:

```text
https://www.example.com/products
```

A URL has different parts:

```text
https://www.example.com/products
  ↓           ↓             ↓
Protocol    Domain        Path
```

### Important parts

- `https` → Protocol
- `www.example.com` → Domain name
- `/products` → Path

Default ports:

```text
HTTP  → 80
HTTPS → 443
```

---

# Step 2: Browser Checks Cache

Before making a network request, the browser checks whether it already has the required resources cached.

For example:

```text
Browser Cache
     ↓
HTML
CSS
JavaScript
Images
Fonts
```

If the resource is available and can be reused, the browser may avoid downloading it again.

This makes the website load faster.

---

# Step 3: DNS Finds the IP Address

Computers communicate with servers using **IP addresses**, but humans use domain names.

For example:

```text
example.com
     ↓
DNS
     ↓
93.184.216.34
```

DNS stands for:

> **Domain Name System**

You can think of DNS like a phone book.

It converts:

```text
Domain Name → IP Address
```

For example:

```text
google.com → IP address
```

The browser and operating system may already have the DNS result cached.

If it is not cached, the system performs DNS resolution through DNS servers.

### Simplified DNS flow

```text
Browser  →  OS DNS Cache  →  DNS Resolver  →  Root DNS  →  TLD DNS (.com)  →  Authoritative DNS  →  IP Address
```

The actual process can be optimized using caching, so not every request goes through all these steps.

---

# Step 4: Establish a Network Connection

After getting the server's IP address, the browser needs to establish a connection.

The protocol depends on the HTTP version.

```text
HTTP/1.1 → TCP
HTTP/2   → TCP
HTTP/3   → QUIC over UDP
```

---

## TCP 3-Way Handshake

For HTTP/1.1 and HTTP/2, TCP establishes a connection using a 3-way handshake.

```text
Client                    Server
  |                         |
  | -------- SYN ---------> |
  |                         |
  | <------ SYN-ACK ------- |
  |                         |
  | -------- ACK ---------> |
  |                         |
  |    Connection Ready     |
```

### What happens?

1. Client sends `SYN`
2. Server responds with `SYN-ACK`
3. Client sends `ACK`

Now the TCP connection is established.

---

# Step 5: TLS Makes HTTPS Secure

If the URL uses:

```text
https://
```

TLS is used to secure the connection.

TLS stands for:

> **Transport Layer Security**

TLS provides:

- Encryption
- Data integrity
- Server authentication

### Simplified flow

```text
Browser  →  TLS Handshake  →  Server Certificate  →  Certificate Validation  →  Key Exchange  →  Secure Connection
```

The server provides a certificate containing information about its identity.

The browser verifies the certificate and establishes cryptographic keys for the secure connection.

After that, HTTP data is sent through the encrypted TLS connection.

SSL (Secure Sockets Layer) was the older protocol.
TLS (Transport Layer Security) is its newer, improved successor.

> **Interview answer:**
>
> TLS secures HTTPS communication by providing encryption, integrity, and authentication.

---

# Step 6: Browser Sends an HTTP Request

Once the connection is ready, the browser sends an HTTP request.

Example:

```http
GET /products HTTP/1.1
Host: example.com
User-Agent: Chrome
Accept: text/html
Cookie: session_id=abc123
```

An HTTP request can contain:

- HTTP method
- URL/path
- Headers
- Cookies
- Body

---

## Common HTTP Methods

| Method | Purpose |
|---|---|
| GET | Fetch/query data |
| POST | Create a new resource / send data |
| PUT | Replace/update a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |
| QUERY | Send a query in the request body and retrieve the result |

---

# Step 7: Request Reaches the Server Infrastructure

The request may pass through several infrastructure components before reaching the backend application.

A typical production architecture might look like:

```text
User  →  CDN  →  WAF  →  Load Balancer  →  Reverse Proxy  →  Backend Server  →  Redis  →  Database
```

Not every application has every layer.

---

# CDN

CDN stands for:

> **Content Delivery Network**

A CDN is a network of servers distributed across different geographic locations.

Its main purpose is to deliver content from a location closer to the user.

For example:

```text
User in India
      ↓
CDN Edge Server in India
      ↓
Cached image / CSS / JS
```

Instead of:

```text
User in India
      ↓
Origin Server in USA
```

the CDN may serve the resource from a nearby edge location.

---

## What Does a CDN Usually Cache?

A CDN commonly caches:

- Images
- CSS
- JavaScript
- Fonts
- Videos
- Other static files
- Some cacheable API responses

### CDN Cache Hit

If the CDN already has the requested resource:

```text
User  →  CDN  →  Cached Resource
```

The request may stop at the CDN.

This is called a:

> **Cache Hit**

---

## CDN Cache Miss

If the CDN does not have the resource:

```text
User  →  CDN  →  Origin Server  →  Response  →  CDN  →  User
```

This is called a:

> **Cache Miss**

The CDN can then cache the response for future requests if the caching rules allow it.

---

## Interview Answer: CDN

> A CDN is a distributed network of edge servers that caches and delivers content closer to users, reducing latency and decreasing the load on the origin server.

---

# CDN vs WAF vs Firewall vs Load Balancer

These components have different responsibilities.

| Component | Simple Meaning | Main Job |
|---|---|---|
| **CDN** | Nearby content store | Caches and delivers content closer to users |
| **WAF** | Web security guard | Inspects HTTP requests and blocks web attacks |
| **Firewall** | Network gate | Controls network traffic based on IP, port, protocol, etc. |
| **Load Balancer** | Traffic distributor | Sends requests to healthy backend servers |
| **Reverse Proxy** | Middleman | Receives requests and forwards them to backend services |

---

# WAF

WAF stands for:

> **Web Application Firewall**

A WAF protects web applications by inspecting HTTP/HTTPS requests.

It can help detect and block attacks such as:

- SQL Injection
- Cross-Site Scripting (XSS)
- Malicious HTTP requests
- Some automated/bot traffic

Example:

```text
User
 ↓
WAF
 ↓
Is request safe?
 ↓
Yes → Backend
No  → Block
```

---

# Firewall

A traditional network firewall mainly controls network traffic.

For example:

```text
Allow:
Port 443 → HTTPS

Block:
Unexpected ports
Unauthorized network traffic
```

A simple way to remember:

```text
Firewall → Network-level protection

WAF → Web/HTTP-level protection
```

---

# Load Balancer

Suppose you have multiple backend servers:

```text
                 ┌── Server 1
                 │
User → Load Balancer ├── Server 2
                 │
                 └── Server 3
```

The load balancer distributes incoming requests among healthy servers.

This helps with:

- Scalability
- High availability
- Fault tolerance
- Distributing traffic

If one server becomes unhealthy, the load balancer can stop sending traffic to it.

---

# Reverse Proxy

A reverse proxy sits between the client and backend servers.

```text
Client  →  Reverse Proxy  →  Backend
```

Examples of reverse proxy software include:

- Nginx
- HAProxy
- Envoy

A reverse proxy can handle things such as:

- Routing
- TLS termination
- Load balancing
- Compression
- Request forwarding
- Security rules

---

# Step 8: Request Reaches the Backend

After passing through the infrastructure, the request reaches the backend application.

For example:

```text
GET /api/products
```

The backend could be built using any technology.

For a Node.js backend:

```text
HTTP Request
     ↓
Node.js / NestJS
     ↓
Authentication
     ↓
Validation
     ↓
Business Logic
     ↓
Cache / Database
```

---

# Step 9: Backend Authenticates the User

If the API requires authentication, the backend checks credentials or tokens.

For example:

```text
Authorization: Bearer <access_token>
```

The backend may verify:

- JWT
- Session
- API key
- OAuth token
- Cookies

If authentication fails:

```text
401 Unauthorized
```

If the user is authenticated but does not have permission:

```text
403 Forbidden
```

---

# Step 10: Backend Validates the Request

The backend should validate incoming data before processing it.

Example:

```json
{
  "productId": 123,
  "quantity": 2
}
```

The backend checks:

```text
Is productId valid?
Is quantity a number?
Is quantity greater than 0?
Does the product exist?
Does the user have permission?
```

If validation fails, the backend returns an appropriate error response.

---

# Step 11: Backend Runs Business Logic

After authentication and validation, the backend executes the required business logic.

For example:

```text
GET /api/products/123
```

Backend:

```text
Receive Request
      ↓
Authenticate User
      ↓
Validate Request
      ↓
Check Cache
      ↓
If not found → Query Database
      ↓
Process Data
      ↓
Return Response
```

---

# Step 12: Backend Checks Redis Cache

The backend may use Redis to avoid querying the database every time.

Example:

```text
Backend  →  Redis  →  Is Data Available?
```

### Cache Hit

```text
Backend  →  Redis  →  Data Found  →  Return Data
```

### Cache Miss

```text
Backend  →  Redis  →  Data Not Found  →  Database  →  Get Data  →  Store in Redis  →  Return Data
```

Redis is useful for frequently accessed data because it is much faster than repeatedly querying the database for the same data.

---

# Step 13: Backend Queries the Database

If the required data is not available in the cache, the backend may query the database.

For example:

```sql
SELECT *
FROM products
WHERE id = 123;
```

The database could be:

- PostgreSQL
- MySQL
- MongoDB
- SQL Server
- etc.

The database returns the requested data to the backend.

---

# Step 14: Backend Sends an HTTP Response

After processing the request, the backend sends a response.

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

Response body:

```json
{
  "id": 123,
  "name": "Laptop",
  "price": 75000
}
```

---

# Common HTTP Status Codes

| Status Code | Meaning |
|---|---|
| **200** | OK / Success |
| **201** | Resource Created |
| **204** | Success with No Content |
| **301** | Permanent Redirect |
| **302** | Temporary Redirect |
| **400** | Bad Request |
| **401** | Unauthorized / Authentication required |
| **403** | Forbidden |
| **404** | Resource Not Found |
| **500** | Internal Server Error |
| **502** | Bad Gateway |
| **503** | Service Unavailable |

---

# Step 15: Response Reaches the Browser

The response travels back toward the browser.

Simplified:

```text
Database  →  Backend  →    Load Balancer /      →       CDN         →  Internet  →  Browser
                           Reverse Proxy           (if applicable)
```

The browser receives the response.

For a webpage, the response may contain HTML.

For an API request, it is commonly JSON.

---

# Step 16: Browser Renders the Page

Now the browser needs to turn the received resources into something visible.

For example:

```text
HTML → DOM ──┐
             ├→ Render Tree → Layout → Paint → Display
CSS  → CSSOM ┘
```

---

# DOM

DOM stands for:

> **Document Object Model**

The browser converts HTML into a tree-like structure.

Example:

```html
<body>
  <h1>Hello</h1>
  <p>Welcome</p>
</body>
```

becomes approximately:

```text
Document
   ↓
 body
 ├── h1
 │    └── "Hello"
 │
 └── p
      └── "Welcome"
```

JavaScript can manipulate this DOM.

---

# CSSOM

CSS is parsed into a structure called the:

> **CSS Object Model (CSSOM)**

It represents the styles that should be applied to the document.

---

# Render Tree

The browser combines information from:

```text
DOM  +  CSSOM  →  Render Tree
```

The render tree contains the information needed to display visible elements.

---

# Layout

During layout, the browser calculates:

- Width
- Height
- Position
- Size
- Relationship between elements

For example:

```text
Where should this button appear?
How wide should it be?
How tall should it be?
```

---

# Paint

After layout, the browser paints pixels onto the screen.

For example:

```text
Layout  →  Paint  →  Pixels  →  Screen
```

---

# JavaScript Execution

JavaScript can then:

- Change the DOM
- Handle user interactions
- Make API requests
- Update application state
- Modify styles
- Render dynamic content

For example, a React application may make an API request:

```text
React App
   ↓
GET /api/products
   ↓
Backend
   ↓
JSON Response
   ↓
React State
   ↓
UI Update
```

---

# React / Next.js Rendering

Modern frontend applications can use different rendering approaches.

## CSR — Client-Side Rendering

The browser downloads JavaScript and renders much of the UI on the client.

```text
Server  →  HTML + JavaScript  →  Browser  →  JavaScript Runs  →  UI Rendered
```

---

## SSR — Server-Side Rendering

The server generates HTML for a request and sends it to the browser.

```text
Browser  →  Server  →  HTML Generated  →  Browser  →  Display HTML
```

JavaScript can then make the page interactive.

---

## SSG — Static Site Generation

The HTML is generated ahead of time, usually during the build process.

```text
Build Time
    ↓
Generate HTML
    ↓
Deploy
    ↓
User requests page
    ↓
Serve existing HTML
```

This can work very well with CDNs because static files can often be cached at the edge.

---

# Hydration

In frameworks such as React/Next.js, hydration means attaching JavaScript behavior to server-rendered HTML.

Simplified:

```text
Server-rendered HTML
        ↓
Browser displays HTML
        ↓
JavaScript loads
        ↓
React hydrates
        ↓
Page becomes interactive
```

---

# Where Does Caching Happen?

Caching can happen at multiple levels.

```text
1. Browser Cache
       ↓
2. DNS Cache
       ↓
3. CDN Cache
       ↓
4. Application Cache
   (Redis / Memcached)
       ↓
5. Database / OS Caches
```

Each cache serves a different purpose.

---

# Browser Cache

Stores resources on the user's device.

Example:

```text
CSS
JavaScript
Images
Fonts
```

This avoids downloading the same resources repeatedly.

---

# DNS Cache

DNS results can be cached by:

```text
Browser
OS
Router
DNS Resolver
```

This avoids performing DNS resolution every time.

---

# CDN Cache

CDNs store content at edge locations.

```text
User
 ↓
Nearby CDN
 ↓
Cached Content
```

This reduces latency and origin-server load.

---

# Redis Cache

Redis is commonly used by backend applications to cache frequently accessed application data.

```text
Backend
   ↓
Redis
   ↓
Database
```

This can reduce database load and improve response time.

---

# Database Cache

Databases and operating systems can also cache frequently accessed data internally.

The application does not always need to manage these caches directly.

---

# HTTP/1.1 vs HTTP/2 vs HTTP/3

## HTTP/1.1

```text
HTTP/1.1
   ↓
TCP
```

HTTP/1.1 has limited request concurrency and can suffer from head-of-line blocking at the connection/request level.

Multiple TCP connections may be used by browsers to improve concurrency.

---

## HTTP/2

```text
HTTP/2
   ↓
TCP
   ↓
TLS (for HTTPS in browsers)
```

HTTP/2 introduces:

- Multiplexing
- Streams
- Header compression
- More efficient use of a connection

Multiple HTTP requests can share a single TCP connection.

```text
One TCP Connection
       ↓
 ┌─────┼─────┐
 ↓     ↓     ↓
Req1  Req2  Req3
```

---

## HTTP/3

```text
HTTP/3
   ↓
QUIC (Quick UDP Internet Connections)
   ↓
UDP (User Datagram Protocol)
```

HTTP/3 uses QUIC instead of TCP.

QUIC provides:

- Multiplexed streams
- Faster connection establishment
- TLS 1.3 integration
- Better behavior when packets are lost on one stream

A major advantage is that packet loss affecting one stream does not block unrelated streams in the same way TCP-level head-of-line blocking can.

---

# HTTP Version Comparison

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| Transport | TCP | TCP | QUIC / UDP |
| Multiplexing | Limited | Yes | Yes |
| Header Compression | No | HPACK | QPACK |
| TLS | HTTPS uses TLS | HTTPS uses TLS | Built into QUIC |
| Connection Setup | TCP + TLS | TCP + TLS | QUIC + TLS |
| Better handling of packet loss | No | Limited by TCP | Better per-stream behavior |

---

# Simple Interview Answer

If an interviewer asks:

> **"What happens when you enter a URL in the browser?"**

You can answer:

> When I enter a URL, the browser first parses the URL and checks its cache. Then DNS resolves the domain name to an IP address. The browser establishes a TCP connection for HTTP/1.1 or HTTP/2, or a QUIC connection for HTTP/3. For HTTPS, TLS secures the connection. The browser then sends an HTTP request, which may pass through a CDN, WAF, reverse proxy, and load balancer before reaching the backend. The backend authenticates and validates the request, executes business logic, and may use Redis and a database such as PostgreSQL or MongoDB. It then sends an HTTP response back to the browser. Finally, the browser parses the HTML and CSS, executes JavaScript, performs layout and painting, and displays the page.