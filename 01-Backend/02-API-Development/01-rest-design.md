# REST Design & Resource-Oriented URLs

## 1. What is REST?

REST stands for **Representational State Transfer**.

REST is an architectural style for designing APIs around **resources**.

Examples of resources:

- User
- Product
- Order
- Ticket

REST uses HTTP methods to perform operations.

    GET     /users
    POST    /users
    GET     /users/123
    PATCH   /users/123
    DELETE  /users/123
    QUERY   /users/search

---

## 2. What is a Resource?

A resource is data/entity exposed by an API.

    /users
    /products
    /orders
    /tickets

A specific resource can have an ID:

    /users/123
    /products/456

---

## 3. Resource-Oriented URLs

The URL should represent the **resource**, not the action.

### Bad

    GET  /getUsers
    POST /createUser
    POST /deleteUser

### Good

    GET    /users
    POST   /users
    PATCH  /users/123
    DELETE /users/123

**Remember:**

    URL         → Resource
    HTTP Method → Operation

---

## 4. Why Use Nouns Instead of Verbs?

The URL identifies the resource, while the HTTP method defines the operation.

    GET /users

    /users → Resource
    GET    → Operation

**Interview:**

> REST APIs generally use nouns in URLs because the URL identifies the resource, while the HTTP method defines the operation.

---

## 5. Singular vs Plural URLs

A common convention is to use **plural nouns** for collections.

    /users
    /products
    /orders

For one resource:

    /users/123
    /products/456
    /orders/789

This keeps APIs consistent and predictable.

---

# 6. Path Parameters vs Query Parameters

## Path Parameter

Used to identify a specific resource.

    GET /users/123

Here `123` is the path parameter.

## Query Parameter

Used for:

- Filtering
- Searching
- Sorting
- Pagination

Example:

    GET /users?role=admin&page=2

**Remember:**

    Path   → Which resource?
    Query  → How to filter/search/sort/paginate?

---

# 7. What Makes a REST API Predictable?

A good REST API usually has:

- Consistent URLs
- Standard HTTP methods
- Meaningful status codes
- Consistent request/response format
- Consistent error format
- Clear pagination/filtering

Example:

    GET /users
    GET /users/123

    GET /orders
    GET /orders/123

---

# 8. HTTP Semantics

HTTP semantics define the **meaning and expected behavior of HTTP methods and responses**.

They include:

- HTTP methods
- Status codes
- Headers
- Caching
- Safety
- Idempotency

**Simple meaning:**

> HTTP semantics tell clients and servers how HTTP operations are expected to behave.

---

# 9. HTTP Methods

Common HTTP methods:

    GET
    POST
    PUT
    PATCH
    DELETE
    HEAD
    OPTIONS
    QUERY

### QUERY

`QUERY` is a newer HTTP method intended for **safe, query-style requests where the query can be carried in the request content**.

It is useful when a query is too complex or too large to conveniently represent in a URL.

---

# 10. Idempotent HTTP Methods

An idempotent method has the **same intended effect on server state** when the same request is repeated.

Common idempotent methods:

    GET
    HEAD
    PUT
    DELETE
    OPTIONS
    QUERY

**Example:**

    DELETE /users/123

After the user is deleted, repeating the same DELETE does not create another state change.

**Remember:**

> Idempotent = Repeating the request has the same intended server-state effect.

---

# 11. Why is DELETE Idempotent?

After the resource is deleted, sending the same DELETE again does not cause another intended state change.

The response can be different, but the final server state remains consistent with the deletion.

---

# 12. HTTP Status Codes

Choose the status code based on the operation and result.

### Success

    200 → OK
    201 → Created
    204 → No Content

### Client Errors

    400 → Bad Request
    401 → Unauthorized
    403 → Forbidden
    404 → Not Found
    409 → Conflict
    422 → Unprocessable Content
    429 → Too Many Requests

### Server Errors

    500 → Internal Server Error
    502 → Bad Gateway
    503 → Service Unavailable
    504 → Gateway Timeout

**Interview:**

> I choose status codes based on the semantics of the operation and the reason for success or failure.

---

# 13. Consistent Error Responses

APIs should use a **consistent error format**.

Example:

    {
      "code": "USER_NOT_FOUND",
      "message": "User not found",
      "details": {},
      "requestId": "abc123"
    }

Benefits:

- Easier frontend handling
- Easier debugging
- Easier monitoring
- Consistent API behavior

**Interview:**

> A consistent error format allows clients and services to handle errors in a predictable way.
