# 3. HTTP Methods & Semantics

HTTP methods (also called verbs) define what action is performed on a resource. In REST, the method — not the URL — expresses the operation.

## Overview

| Method | Purpose | Idempotent | Safe |
|---|---|:---:|:---:|
| GET | Retrieve data | ✅ | ✅ |
| POST | Create resource | ❌ | ❌ |
| PUT | Replace resource | ✅ | ❌ |
| PATCH | Partial update | ❌ | ❌ |
| DELETE | Remove resource | ✅ | ❌ |

---

## GET — Retrieve Resource

Used to fetch data from the server.

- Must NOT modify state
- Can be cached
- Can be safely retried

**Example:**

```http
GET /users/123
```

**Response:**

```json
{
  "data": {
    "id": "123",
    "name": "John Doe"
  }
}
```

**Characteristics:**

- Safe (no side effects)
- Idempotent (same result every time)

---

## POST — Create Resource

Used to create a new resource or trigger processing.

- Not idempotent (multiple calls may create duplicates)
- Typically returns `201 Created`

**Example:**

```http
POST /users
Content-Type: application/json

{
  "name": "John Doe"
}
```

**Response (example):**

```
HTTP/1.1 201 Created
Location: /users/123
```

**Use cases:**

- Create new entity
- Trigger async job
- Submit forms

---

## PUT — Replace Resource

Used to fully replace a resource.

- Idempotent (same request → same result)
- Client sends full representation

**Example:**

```http
PUT /users/123
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Behavior:**

- Replaces entire resource
- Missing fields may be removed or reset

⚠️ Important: Do **NOT** use `PUT` for partial updates.

---

## PATCH — Partial Update

Used to modify part of a resource.

- Not necessarily idempotent (depends on implementation)
- Sends only changed fields

**Example:**

```http
PATCH /users/123
Content-Type: application/json

{
  "email": "new@email.com"
}
```

**Benefits:**

- Smaller payloads
- More efficient updates

---

## DELETE — Remove Resource

Used to delete a resource.

- Idempotent (deleting twice has same effect)
- Often returns `204 No Content`

**Example:**

```http
DELETE /users/123
```

**Behavior:**

- Resource removed or marked as deleted
- May be a soft delete internally

---

## Idempotency Explained

An operation is idempotent if repeating it produces the same result.

| Method | Idempotent? | Example |
|---|:---:|---|
| GET | ✅ | Fetch same resource |
| PUT | ✅ | Replace with same data |
| DELETE | ✅ | Delete already-deleted resource |
| POST | ❌ | Creates duplicates |

---

## Safe Methods

Safe methods do not modify server state:

- `GET`
- `HEAD`
- `OPTIONS`

Used for read-only operations and caching.

---

## Common Patterns (CRUD Mapping)

| Operation | Method | Endpoint |
|---|---|---|
| List | GET | /users |
| Get | GET | /users/{id} |
| Create | POST | /users |
| Update | PUT/PATCH | /users/{id} |
| Delete | DELETE | /users/{id} |

---

## Status Code Semantics

| Method | Success Codes |
|---|---|
| GET | 200 |
| POST | 201 |
| PUT | 200 / 204 |
| PATCH | 200 / 204 |
| DELETE | 204 |

---

## Advanced Usage

### 1) POST for Non-CRUD Actions

Sometimes `POST` is used for operations that are not natural CRUD on a resource, for example:

```
POST /auth/login
POST /reports/generate
```

Acceptable when the action cannot be modeled as a resource CRUD operation.

### 2) PUT vs PATCH Decision

- Full update → `PUT`
- Partial update → `PATCH`

### 3) Async Operations

Use `POST` to start async work:

```
POST /exports
→ 202 Accepted
```

Response should include job ID and a status endpoint.

---

## Common Mistakes

- Using `POST` for everything
- Using `GET` to modify data
- Using `PUT` for partial updates
- Ignoring idempotency
- Returning wrong status codes

---

## Summary

- Use GET for reading
- Use POST for creating
- Use PUT for full updates
- Use PATCH for partial updates
- Use DELETE for removal
- Respect idempotency and safety

---

## AI Agents

Use correct HTTP semantics:

- GET = read (safe, idempotent)
- POST = create (non-idempotent)
- PUT = full replace (idempotent)
- PATCH = partial update
- DELETE = remove (idempotent)
- Return correct HTTP status codes