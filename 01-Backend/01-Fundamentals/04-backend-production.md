# Backend Production Foundation

## 1. Timeouts

A **timeout** defines how long the application waits before giving up.

- Prevents requests from hanging forever.
- Important for DB queries, HTTP calls, and external APIs.

**Interview:**

> A timeout prevents a request from waiting indefinitely for a dependency.

---

## 2. Retries

A **retry** means trying a failed operation again.

- Use a limited number of retries.
- Usually use retries for **temporary failures**.
- Use **backoff** between retries.
- Don't blindly retry every operation, especially writes.
---

## 3. Graceful Shutdown

Graceful shutdown means **stopping the application safely**.

- Stop accepting new requests.
- Finish current requests.
- Close DB/network connections.
- Then shut down.

**Interview:**

> Graceful shutdown allows the application to finish ongoing work and close resources safely before stopping.

---

## 4. Load Balancing

Load balancing **distributes traffic across multiple servers**.

    Client
       ↓
    Load Balancer
      ↙   ↓   ↘
    Server Server Server

Benefits:

- Better performance
- High availability
- Prevents one server from becoming a bottleneck

---

## 5. Health Checks

A health check tells us whether the application is **alive and healthy**.

Example:

    GET /health

A load balancer or Kubernetes can use it to detect unhealthy instances.

**Interview:**

> Health checks help detect unhealthy servers and prevent traffic from being sent to them.

---

## 6. Readiness Checks

Readiness asks:

> **"Can this application receive traffic?"**

Health/Liveness asks:

> **"Is this application alive?"**

Example:

    GET /readiness

A server may be alive but **not ready** because it is still starting or a required dependency is unavailable.

**Remember:**

    Liveness → Is it alive?
    Readiness → Can it receive traffic?

---

## 7. Config and Secrets

Do **not hard-code** production configuration or secrets in source code.

### Config

Examples:

    PORT
    DATABASE_URL
    REDIS_URL
    API_URL
    NODE_ENV

### Secrets

Examples:

    DATABASE_PASSWORD
    API_KEY
    API_SECRET
    API_TOKEN

Use:

- Environment variables
- Secret management tools

---

## 8. Logging

Logging records important application events.

Useful for:

- Debugging
- Monitoring
- Auditing
- Finding errors

Example:

    logger.info("Server started");
    logger.warn("Server is slow");
    logger.error("Database connection failed");

**Remember:**

    INFO  → Normal events
    WARN  → Potential problem
    ERROR → Failure

---

## 9. Backward-Compatible Deployments

The new version should continue working with **old clients and data** during deployment.

Why?

Because old and new versions may temporarily run together.

Example:

    Old Frontend ──┐
                   ├──→ New Backend
    New Frontend ──┘

### Database Migration

Use **Expand → Migrate → Contract**:

1. Add new column/structure.
2. Deploy code supporting old + new.
3. Migrate data.
4. Update clients.
5. Remove old column later.

This allows old and new versions to work safely together.

---

## 10. Monitoring

Monitoring means **tracking the health and performance of the application**.

Monitor things like:

- CPU
- Memory
- Request latency
- Error rate
- Traffic
- Database performance

**Simple difference:**

    Logging   → What happened?
    Monitoring → How is the system performing?