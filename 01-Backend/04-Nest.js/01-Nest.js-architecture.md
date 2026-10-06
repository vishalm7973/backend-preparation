# Express.js & NestJS

## 1. What is Express.js / NestJS?

**Node.js** → Runtime that runs JavaScript on the server.

**Express.js / NestJS** → Frameworks that make it easier to build backend APIs.

---

## 2. Express.js vs NestJS

### Express.js
- Lightweight
- Flexible
- Less structure
- Developer decides the architecture

### NestJS
- More structured
- Modules, Controllers, Services
- Dependency Injection
- Guards, Pipes, Interceptors

**Remember:**

    Express → Freedom
    NestJS  → Structure

---

## 3. Why NestJS if Node.js already exists?

**Node.js** is a runtime that runs JavaScript on the server.

**NestJS** is a backend framework built on Node.js that provides a structured architecture for building large applications.

It provides:
- Modules
- Controllers
- Providers
- Dependency Injection
- Guards
- Pipes
- Interceptors

**Remember:**

    Node.js  → Runtime
    NestJS  → Backend framework + Structure

NestJS uses **Express or Fastify** underneath for HTTP handling.

---

## 4. Validation in NestJS

NestJS commonly uses:

**DTO + ValidationPipe + class-validator**

The DTO defines validation rules.

ValidationPipe checks the incoming data before it reaches the controller.

Invalid data is rejected.

**Flow:**

    Request
       ↓
    ValidationPipe
       ↓
    Controller

---

## 5. Exception Filters

Exception Filters handle errors and customize the response sent to the client.

Common exception filters:

- `NotFoundExceptionFilter` → Handles 404 errors
- `BadRequestExceptionFilter` → Handles 400 errors
- `UnauthorizedExceptionFilter` → Handles 401 errors
- `ForbiddenExceptionFilter` → Handles 403 errors
- `ConflictExceptionFilter` → Handles 409 errors
- `HttpExceptionFilter` → Handles general HTTP exceptions

They can be used for:
- Consistent error responses
- Handling specific exceptions
- Centralized error logging

**Remember:**

> Exception Filter = Handle and format errors

---

# NestJS Lifecycle

    Client
      ↓
    Middleware
      ↓
    Guard
      ↓
    Interceptor (before)
      ↓
    Pipe
      ↓
    Controller
      ↓
    Service
      ↓
    Database
      ↓
    Interceptor (after)
      ↓
    Response
      ↓
    Client

---

## 1. Modules

A **Module** groups related functionality together.

Example:

    UsersModule
      ├── UsersController
      └── UsersService

**Remember:**

> Module = Organize related code

---

## 2. Controllers

A **Controller** handles incoming HTTP requests.

Example:

    GET /users
        ↓
    UsersController

It receives the request and calls the required service.

**Remember:**

> Controller = Handle request

---

## 3. Providers

A **Provider** is a class managed by NestJS.

Services are the most common type of provider.

They usually contain business logic.

**Remember:**

> Provider = Reusable class managed by NestJS

---

## 4. Dependency Injection

**Dependency Injection (DI)** means a class receives the dependencies it needs instead of creating them itself.

### Without DI

    class UsersController {
      usersService = new UsersService();
    }

### With DI

    class UsersController {
      constructor(private usersService: UsersService) {}
    }

NestJS provides `UsersService` automatically.

**Remember:**

> DI = Give the class what it needs

---

## 5. Middleware

Middleware runs **before the request reaches the controller**.

Common uses:

- Logging
- Request preprocessing
- Adding data to the request

Example:

    function logger(req, res, next) {
      console.log(req.method, req.url);
      next();
    }

**Remember:**

> Middleware = Process the request before the controller

---

## 6. Guards

A **Guard** decides whether a request is allowed to continue.

Common uses:

- Authentication
- Authorization
- Roles
- Permissions

**Remember:**

> Guard = Allow or deny request

---

## 7. Pipes

A **Pipe** validates or transforms incoming data.

Example:

    "123" → 123

Common uses:

- Validate DTO
- Convert string to number

**Remember:**

> Pipe = Validate + Transform

---

## 8. Interceptors

An **Interceptor** runs logic before and after a controller method.

Common uses:

- Logging
- Execution time
- Response transformation
- Caching

**Remember:**

> Interceptor = Before + After