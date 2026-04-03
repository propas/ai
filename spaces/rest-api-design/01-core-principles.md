# 1. Core Principles

## Statelessness

Each request from a client must contain all the information needed to process it.

- The server does **NOT** store client session state
- Every request is independent
- Authentication (e.g., JWT) must be sent with each request

**Example:**

```http
GET /users/123
Authorization: Bearer <token>
```

**Bad:**

- Server remembers logged-in user without token
- Hidden session state

**Benefits:**

- Easier scaling (horizontal)
- Better reliability
- Simpler server design

---

## Resource-Oriented Design

APIs should expose resources (nouns), not actions (verbs).

- Resources = entities (users, orders, products)
- Operations are defined by HTTP methods, not URL names

**Example:**

```http
GET /users
POST /orders
GET /users/123/orders
```

**Bad:**

```http
POST /createUser
GET /getOrders
```

**Benefits:**

- Predictable API structure
- Easier to understand and document
- Aligns with REST philosophy

---

## Uniform Interface

A consistent and standardized way to interact with resources.

Includes:

- Standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`)
- Consistent URL patterns
- Standard response formats
- Use of HTTP status codes

**Example:**

```http
GET /users/123    → returns user
DELETE /users/123 → deletes user
```

**Benefits:**

- Reduces learning curve
- Enables tooling (Swagger, SDKs)
- Improves maintainability

---

## Client-Server Separation

Client and server are independent systems.

- Client handles UI/UX
- Server handles data & business logic
- They communicate via API only

This means:

- You can change frontend without breaking backend
- You can scale backend independently

**Example:**

Mobile app, web app, and CLI all use the same API

**Benefits:**

- Flexibility
- Independent development
- Better scalability

---

## Cacheable Responses

Responses should define whether they can be cached.

- Use HTTP caching headers
- Clients and intermediaries (CDNs) can reuse responses

**Example:**

```http
Cache-Control: max-age=3600
ETag: "abc123"
```

**Benefits:**

- Reduced server load
- Faster response times
- Improved user experience

> **Important:** Only cache safe/idempotent responses (typically `GET`)

---

## Layered Architecture

The system can be composed of multiple layers.

```
Client → API Gateway → Service → Database
```

The client does not know if it connects directly or through intermediaries.

Layers may include:

- Load balancers
- API gateways
- Caching layers
- Security layers

**Benefits:**

- Better scalability
- Improved security
- Easier system evolution

---

## Summary

| Principle         | Key Idea               |
| ----------------- | ---------------------- |
| Statelessness     | No server-side session |
| Resource-oriented | Use nouns, not verbs   |
| Uniform interface | Consistent API rules   |
| Client-server     | Separation of concerns |
| Cacheable         | Improve performance    |
| Layered           | Modular architecture   |

