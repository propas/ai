# 2. Resource Design & Naming Conventions

Designing resources correctly is one of the most important parts of REST API design. It directly impacts usability, consistency, and long-term maintainability.

## What is a Resource?

A resource represents a real-world entity or concept, for example:

- users
- orders
- products
- payments

Each resource is identified by a URI (Uniform Resource Identifier).

## Use Nouns, Not Verbs

REST APIs should describe what, not what to do. Operations are handled by HTTP methods, not URL names.

**Good:**

```http
GET /users
POST /orders
DELETE /users/123
```

**Bad:**

```http
GET /getUsers
POST /createOrder
DELETE /deleteUser
```

**Why:**

- Cleaner and predictable API
- Aligns with REST semantics

---

## Use Plural Resource Names

Always use plural nouns for collections.

**Good:**

```http
GET /users
GET /orders
GET /products
```

**Bad:**

```http
GET /user
GET /order
```

**Why:**

- Consistent mental model: `/users` represents a collection, `/users/{id}` a single item

---

## Use Consistent Naming Style

Choose one style and stick to it.

- Recommended: `kebab-case` for URLs (e.g., `user-profiles`)
- Alternative: `snake_case` (less common in URLs)

**Example:**

```http
GET /user-profiles
GET /order-items
```

**Bad:**

```http
GET /UserProfiles
GET /orderItems
```

**Rule:**

- No camelCase in URLs
- No spaces
- No special characters

---

## Keep URLs Simple and Predictable

URLs should be easy to read and guess.

**Good:**

```http
GET /users/123
GET /users/123/orders
```

**Bad:**

```http
GET /usersById/123
GET /fetchUserOrders/123
```

**Rule:**

- If you need to explain the URL, it's too complex

---

## Use Resource Hierarchies (Nesting)

Use nesting to represent relationships.

**Example:**

```http
GET /users/123/orders
GET /orders/456/items
```

**Meaning:**

- Orders belong to a user
- Items belong to an order

---

## Limit Nesting Depth

Avoid deeply nested URLs.

**Good:**

```http
GET /users/123/orders
```

**Bad:**

```http
GET /users/123/orders/456/items/789/details
```

**Rule:**

- Max depth: 2–3 levels
- Beyond that → use query parameters

---

## Use Query Parameters for Filtering

Do NOT encode filters in the path.

**Good:**

```http
GET /orders?status=completed
GET /users?role=admin
```

**Bad:**

```http
GET /orders/completed
GET /getUsersByRole/admin
```

---

## Use Identifiers Properly

- Use unique identifiers in paths
- Prefer UUIDs or stable IDs

**Example:**

```http
GET /users/123
GET /orders/550e8400-e29b-41d4-a716-446655440000
```

---

## Use Sub-Resources for Relationships

When a resource is tightly coupled to another resource:

**Example:**

```http
GET /users/123/orders
POST /users/123/orders
```

---

## Avoid Actions in URLs

Actions should not be part of resource paths.

**Bad:**

```http
POST /orders/123/cancel
POST /users/123/activate
```

**Better:**

```http
PATCH /orders/123
{
  "status": "cancelled"
}
```

**Exception:**

- If action is not CRUD (e.g., `/auth/login`), using an action endpoint can be acceptable

---

## Consistent Naming Across API

If you use `/users` don’t also use `/customers` for the same concept.

**Rule:**

- One concept = one name

---

## Use Standard Resource Patterns

| Operation | Endpoint |
|---|---|
| List | GET /users |
| Get one | GET /users/{id} |
| Create | POST /users |
| Update | PUT/PATCH /users/{id} |
| Delete | DELETE /users/{id} |

---

## Use Hyphenated Multi-Word Resources

**Good:**

```http
GET /user-settings
GET /payment-methods
```

**Bad:**

```http
GET /userSettings
GET /paymentMethods
```

---

## Summary

| Rule             | Description            |
| ---------------- | ---------------------- |
| Use nouns        | Resources, not actions |
| Use plural       | `/users`, not `/user`  |
| Keep it simple   | Predictable URLs       |
| Limit nesting    | Max 2–3 levels         |
| Use query params | Filtering & sorting    |
| Be consistent    | Same naming everywhere |

---

## AI Agents

Design REST endpoints using:

- plural resource names
- no verbs in URLs
- kebab-case naming
- shallow nesting (max 2 levels)
- query params for filtering/sorting
- standard CRUD patterns