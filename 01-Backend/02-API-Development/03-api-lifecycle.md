# API Lifecycle

API lifecycle covers how an API is **designed, changed, maintained, and eventually replaced** without unnecessarily breaking clients.

---

## 1. API Versioning

API versioning allows us to **change an API without breaking existing clients**.

Example:

    /api/v1/users/123
    /api/v2/users/123

    v1 → Old contract
    v2 → New contract

Existing clients can continue using `v1`, while new clients use `v2`.

### Why do we need it?

One backend may be used by:

- Web application
- Mobile application
- Admin dashboard
- Partner application

Changing the existing API can break these clients.

---

## 2. API Versioning Approaches

### A. URL Versioning — Most Common

    /api/v1/users
    /api/v2/users

Simple and easy to understand.

### B. Query Parameter

    /api/users?version=1
    /api/users?version=2

### C. Header Versioning

    GET /api/users
    Accept: application/vnd.myapp.v2+json

### D. Custom Header

    GET /api/users
    API-Version: 2

---

## 3. Deprecation

Deprecation means an API is **no longer recommended for new development** and may be removed later.

Usually:

    Old API
       ↓
    Mark as deprecated
       ↓
    Give migration time
       ↓
    Remove later

---

## 4. Backward Compatibility

Backward compatibility means **new changes should not break existing clients**.

Example:

    Old Client → New Backend ✓
    New Client → New Backend ✓

API versioning is one way to maintain backward compatibility.

---

## 5. Bulk Operations

Bulk operations allow processing **multiple resources in one API request**.

Instead of:

    POST /users
    POST /users
    POST /users

Use a bulk endpoint:

    POST /users/bulk

Benefits:

- Fewer network requests
- Better performance
- Less network overhead

---

## 6. Partial Update

Partial update means changing **only specific fields** of an existing resource.

`PATCH` is commonly used.

Example:

    PATCH /users/123

    {
      "name": "Vishal"
    }

Only the `name` is updated.

**Remember:**

    PUT   → Replace/update the resource
    PATCH → Update specific fields