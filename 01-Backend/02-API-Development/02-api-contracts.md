# API Contracts, Validation, and Serialization

## 1. What is an API Contract?

An **API contract** defines how the client and server communicate.

It specifies:

- Request fields and types
- Required fields
- Validation rules
- Response structure
- Error format
- Status codes and pagination

**Interview:**

> An API contract defines the agreed request and response structure between a client and server.

---

## 2. What is Validation?

Validation checks whether incoming data is **valid before processing it**.

Example:

    {
      "email": "user@example.com",
      "age": 25
    }

We can check:

- Email is valid
- Age is a number
- Required fields exist
- Values are within allowed limits

Invalid data should be rejected early, usually with **400 or 422**.

**Interview:**

> Validation checks incoming data against the API contract before processing it.

---

## 3. Types of Validation

| Type | Meaning |
|---|---|
| Required | Field must be present |
| Type | Checks data type |
| Format | Checks email, UUID, URL, date, etc. |
| Length | Checks min/max length |
| Range | Checks min/max value |
| Pattern | Checks a specific pattern |
| Structural | Checks object/nested structure |
| Business | Checks business rules |
| Cross-field | Checks multiple fields together |
| Uniqueness | Checks value is not already used |

---

## 4. What is Serialization?

**Serialization** converts application data into a format that can be sent or stored.

Example:

    Object
      ↓ Serialize
    JSON
      ↓
    HTTP Response

Common formats:

- JSON
- XML
- Protocol Buffers
- MessagePack
- YAML

JSON is commonly used in REST APIs.

---

## 5. What is Deserialization?

**Deserialization** converts received data into a form the application can use.

    JSON Request
        ↓
    Deserialize
        ↓
    Application Object
        ↓
    Validate
        ↓
    Process

**Remember:**

    Serialization   → Object → JSON
    Deserialization → JSON → Object

---

# 6. Offset vs Cursor Pagination

Both are used to get data page by page.

## Offset Pagination

Uses `offset` to tell the database **how many records to skip**.

Example:

    GET /users?limit=10&offset=10

Meaning:

    Skip 10
      ↓
    Return next 10
      ↓
    Records 11–20

### Pros

- Simple
- Easy to implement
- Supports direct page navigation

### Cons

- Can become slower with large offsets
- Less stable when data changes frequently

---

## Cursor Pagination

Uses a **cursor to tell the API where to continue**.

Example:

    GET /users?limit=10&after=abc123

The API returns a cursor for the next page.

    First Request
         ↓
    Data + nextCursor
         ↓
    Next Request
         ↓
    Send nextCursor
         ↓
    Next Page

### Benefits

- Better for large datasets
- More stable when data changes
- Avoids large offsets

**Interview:**

> Offset pagination is simpler and supports page-based navigation, while cursor pagination is generally better for large or frequently changing datasets.