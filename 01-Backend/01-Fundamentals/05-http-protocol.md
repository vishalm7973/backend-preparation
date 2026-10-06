# HTTP & Protocol Basics

## 1. What is a Protocol?

A **protocol** is a set of rules that allows systems to communicate.

It defines how data is:

- Formatted
- Sent
- Received
- Understood

### Common Protocols

- HTTP / HTTPS → Web communication
- TCP → Reliable data transmission
- UDP → Fast data transmission
- SMTP → Sending emails
- FTP → File transfer
- WebSocket → Real-time communication
- SSH → Secure remote access

---

# 2. HTTP Caching

HTTP caching means **storing an HTTP response and reusing it later** instead of requesting it again.

Benefits:

- Faster response
- Less bandwidth
- Less server load

## Cache-Control

Controls how a response can be cached.

### max-age

    Cache-Control: max-age=3600

Response can be reused for **1 hour**.

### no-cache

    Cache-Control: no-cache

Must **validate with the server** before reusing the cached response.

### public

    Cache-Control: public, max-age=3600

Can be cached by shared caches such as **CDNs**.

### private

    Cache-Control: private, max-age=600

Only the user's browser/device can cache it.

---

## ETag

ETag helps check whether a resource has changed.

    Client → "Is this version still valid?"
    Server → 304 Not Modified

If unchanged, the server returns **304** instead of sending the full response again.

---

## Last-Modified

Server sends the time when the resource was last changed.

The client can later send:

    If-Modified-Since

If nothing changed, the server can return:

    304 Not Modified

---

## Interview: What is HTTP Caching?

> HTTP caching stores responses in browsers, proxies, or CDNs and reuses them for later requests. It reduces latency, bandwidth usage, and server load.

---

# 3. Idempotent vs Non-Idempotent

### Idempotent

Repeating the same request has the **same intended effect on the server**.

Useful when requests may be repeated because of:

- Network failures
- Timeouts
- Client retries
- Load balancer retries
- Duplicate requests

### Idempotent HTTP Methods

- GET
- HEAD
- PUT
- DELETE
- OPTIONS

### Non-Idempotent

Repeating the request can create a **new effect each time**.

Example:

    POST /users

Sending it twice may create two users.

**Remember:**

    Idempotent     → Repeat safely
    Non-idempotent → Repeat may create another effect

---

# 4. HTTP Cache vs Redis Cache

### HTTP Cache

Usually caches the **HTTP response**.

Examples:

- Browser cache
- CDN cache
- Proxy cache

### Redis Cache

Usually caches **application/server data**.

Example:

    User data
    Product data
    Session data

**Simple difference:**

    HTTP Cache → Reduces network/server work

    Redis Cache → Reduces backend/database work

---

# 5. Content Negotiation

Content negotiation is how the **client and server decide the response format**.

Examples:

- JSON
- XML
- HTML

The client can use headers such as:

    Accept: application/json

---

# 6. Content-Type

`Content-Type` tells us **what format the request or response body contains**.

Example:

    Content-Type: application/json

Means:

> The body contains JSON data.

**Remember:**

    Accept       → What format the client wants

    Content-Type → What format the body actually is