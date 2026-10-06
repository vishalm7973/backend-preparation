# Backend Architecture

## 1. Client–Server Architecture

This describes **how the client and server communicate**.

    Client
      ↓ Request
    Server
      ↓
    Process Request
      ↓ Response
    Client

- Client sends a request.
- Server processes it.
- Server may use DB, cache, or other services.
- Server sends a response.

**Interview:**

> Client-server architecture means the client sends requests to the server, and the server processes them and returns responses.

---

# 2. Application Architecture

This describes **how the backend application is structured and deployed**.

## 2.1 Monolith

The entire application is **one deployable unit**.

    Backend Application
    ├── Users
    ├── Orders
    ├── Payments
    └── Products

### Pros

- Simple to develop
- Easy to test and deploy
- Simple transactions
- Less infrastructure

### Cons

- Can become tightly coupled
- Large codebase becomes difficult to maintain
- Cannot scale individual features independently
- Small changes may require full deployment

---

## 2.2 Modular Monolith

Still **one application**, but divided into clear modules.

    One Application
    ├── User Module
    ├── Order Module
    ├── Payment Module
    └── Notification Module

- Each module has its own business logic.
- Modules have clear boundaries.
- Easier to maintain than a large monolith.
- Can be a good step toward microservices.

**Remember:**

> Monolith = One application
>
> Modular Monolith = One application + clear modules

---

## 2.3 Microservices

Application is divided into **independently deployable services**.

    API Gateway
    ├── User Service → User DB
    ├── Order Service → Order DB
    └── Payment Service → Payment DB

Each service can:

- Deploy independently
- Scale independently
- Own its data
- Be maintained by different teams

### When to use?

Use microservices when there are:

- Clear business boundaries
- Need for independent scaling
- Need for independent deployment
- Multiple teams/services

### Problems

- Network failures
- Distributed transactions
- More infrastructure
- More monitoring
- More operational complexity

**Interview:**

> Microservices provide independent deployment and scaling, but introduce distributed-system complexity.

---

# 3. Code Architecture

This describes **how code inside the backend is organized**.

## 3.1 Layered Architecture

Common structure:

    Client
      ↓
    Controller
      ↓
    Service
      ↓
    Repository
      ↓
    Database

### Controller

Handles HTTP-related work:

- Routes
- Params
- Query
- Body
- HTTP response

**No business logic.**

### Service

Handles:

- Business logic
- Use cases
- Calling repositories
- Calling other services

### Repository

Handles:

- Database queries
- CRUD operations
- Data access

**Simple rule:**

> Controller = HTTP
>
> Service = Business Logic
>
> Repository = Database

---

# 4. Design Principles

These are **principles used to write better code**.

## 4.1 Dependency Injection (DI)

DI means a class **receives its dependencies instead of creating them itself**.

### Bad

    class UserService {
      private repository = new UserRepository();
    }

### Good

    class UserService {
      constructor(private repository: UserRepository) {}
    }

### Benefits

- Less coupling
- Easier testing
- Easy to replace dependencies
- Better maintainability

**Remember:**

> IoC = Principle
>
> DI = Technique
>
> DI Container = Connects dependencies automatically

---

## 4.2 SOLID Principles

SOLID helps make code:

- Maintainable
- Testable
- Flexible
- Reusable
- Less tightly coupled

### S — Single Responsibility

A class should have **one main responsibility**.

    UserService
    PaymentService
    EmailService

Instead of one huge `GodService`.

### O — Open/Closed

> Open for extension, closed for modification.

Example:

Add PayPal without changing existing Stripe logic.

### L — Liskov Substitution

Child classes should work wherever the parent is expected.

Example:

If `Bird` has `fly()`, making `Penguin` extend `Bird` can break the design.

### I — Interface Segregation

Prefer **small, focused interfaces** instead of one large interface.

### D — Dependency Inversion

High-level code should depend on **abstractions**, not specific implementations.

    OrderService
         ↓
    PaymentProvider
         ↓
    Stripe / PayPal

---

## 4.3 DRY — Don't Repeat Yourself

DRY means **avoid repeating the same logic or knowledge in multiple places**.

Instead of writing the same code again and again, create a reusable function, method, service, or utility.

### Benefits

- Less duplicate code
- Easier maintenance
- Fix bugs in one place
- Easier to reuse logic

**Interview:**

> DRY means avoiding duplication by keeping common logic in one reusable place.

---

# 5. Domain Design

This describes **how we model complex business logic**.

## 5.1 DDD — Domain-Driven Design

DDD means designing software around the **business domain, concepts, and rules**.

Instead of starting with:

> "What database tables do we need?"

First ask:

> "What business problem are we solving?"

### Why use DDD?

- Handle complex business logic
- Define clear business boundaries
- Use common business terminology
- Make large systems easier to maintain

### Important Concepts

| Concept | Simple Meaning |
|---|---|
| Domain | Business area |
| Entity | Object with identity |
| Value Object | Value matters, not identity |
| Aggregate | Group of related objects |
| Aggregate Root | Main entry point of aggregate |
| Bounded Context | Clear business boundary |
| Ubiquitous Language | Shared language between business and developers |

---

# 6. Stateless vs Stateful

This describes **whether the application depends on state from previous requests**.

## Stateless

Server does **not store client state locally**.

    Request 1 → Server A
    Request 2 → Server B
    Request 3 → Server C

Any server can handle the request.

- Usually sends authentication information with each request.
- JWT is one common approach.
- Shared session storage like Redis can also be used.

### Why useful?

Easy horizontal scaling.

---

## Stateful

Server depends on information from previous requests.

Example:

    Login → Server A stores session

    Next Request → Server B
                   ↓
              Doesn't know session

### Solutions

- Sticky sessions
- Shared session store like Redis

---

# 7. How Everything Fits Together

This is the **most important mental model**.

    BACKEND SYSTEM
      │
      ├── Client–Server
      │      └── Client ↔ Backend
      │
      ├── Application Architecture
      │      ├── Monolith
      │      ├── Modular Monolith
      │      └── Microservices
      │
      ├── Code Architecture
      │      └── Layered Architecture
      │             ├── Controller
      │             ├── Service
      │             └── Repository
      │
      ├── Design Principles
      │      ├── Dependency Injection
      │      ├── SOLID
      │      └── DRY
      │
      └── Domain Design
             └── DDD
