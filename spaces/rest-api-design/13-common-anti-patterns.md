# 13. Common Anti-Patterns

Anti-patterns are frequent design mistakes that lead to poor API usability, performance issues, security risks, and long-term maintenance problems.

Understanding them helps you avoid technical debt early.

---

## Goals

- Identify bad practices early
- Improve API consistency and quality
- Prevent scalability and security issues
- Enable clean, maintainable APIs

---

## 1. RPC-Style APIs (Verbs in URLs)

Using actions instead of resources.

❌ Bad:

```http
POST /createUser
GET /getOrders
DELETE /deleteUser
```

✅ Good:

```http
POST /users
GET /orders
DELETE /users/123
```

Problem: breaks REST principles and leads to inconsistent design.

---

## 2. Misusing HTTP Methods

Using wrong HTTP verbs for operations.

❌ Bad:

```http
GET /deleteUser/123
POST /users/123/update
```

Problems: violates semantics, breaks caching and tooling.

---

## 3. Returning HTTP 200 for Errors

❌ Bad:

```json
{
  "success": false,
  "error": "Something went wrong"
}
```

Problems: clients cannot detect failures via status codes; breaks monitoring and retries.

✅ Use proper status codes (4xx, 5xx).

---

## 4. No Pagination

Returning entire datasets.

❌ Bad:

```
GET /users
→ returns 1,000,000 records
```

Problems: high memory usage, slow responses, system crashes.

---

## 5. Deeply Nested URLs

❌ Bad:

```
/users/1/orders/2/items/3/details
```

Problems: hard to maintain and complex routing.

✅ Keep nesting ≤ 2–3 levels.

---

## 6. Inconsistent Naming

❌ Bad:

```
/users
/customers
/getUserList
```

Problems: confusing API, hard to learn/use.

---

## 7. Overloading Endpoints

Using one endpoint for multiple actions.

❌ Bad:

```
POST /users?action=delete
```

Problems: hard to understand, breaks REST semantics.

---

## 8. No Versioning

❌ Bad: breaking API changes without versioning.

Problems: clients break unexpectedly; no backward compatibility.

---

## 9. Exposing Internal Implementation

❌ Bad:

```json
{
  "dbId": "abc123",
  "internalStatusCode": 42
}
```

Problems: security risk, tight coupling to backend.

---

## 10. Lack of Validation

❌ Bad: accepting any input without checks.

Problems: security vulnerabilities, data corruption.

---

## 11. No Error Standardization

❌ Bad:

```json
{ "error": "error" }
{ "message": "fail" }
{ "msg": "oops" }
```

Problems: hard to parse, inconsistent client handling.

---

## 12. Over-Fetching Data

❌ Bad: returning full profile, settings, history on every request.

Problems: large payloads, performance issues.

---

## 13. Under-Fetching (Chatty APIs)

❌ Bad: multiple requests needed for one screen.

Problems: high latency, increased load.

---

## 14. Ignoring Idempotency

❌ Bad: retrying `POST` creates duplicates.

Problems: duplicate payments/orders.

---

## 15. No Caching

❌ Bad: every request hits the database.

Problems: poor performance, high load.

---

## 16. No Rate Limiting

❌ Bad: unlimited requests allowed.

Problems: abuse, DoS attacks.

---

## 17. Poor Security Practices

❌ Bad: no authentication, using HTTP, exposing sensitive data.

Problems: security breaches and data leaks.

---

## 18. Ignoring Observability

❌ Bad: no logs, metrics, or tracing.

Problems: hard to debug and no production visibility.

---

## 19. Tight Coupling Between Client & API

❌ Bad: breaking changes without fallbacks.

Problems: fragile integrations.

---

## 20. No Documentation

❌ Bad: no OpenAPI/Swagger, no examples.

Problems: poor developer experience.

---

## Anti-Pattern Summary

| Anti-Pattern | Impact |
|---|---|
| RPC-style URLs | Breaks REST design |
| Wrong HTTP methods | Incorrect semantics |
| No pagination | Performance issues |
| No versioning | Breaking clients |
| No validation | Security risks |
| No caching | Slow API |
| No observability | Hard to debug |

---

## How to Avoid Anti-Patterns

- Follow REST standards strictly
- Use consistent naming and structure
- Validate all inputs
- Implement pagination, caching, rate limiting
- Design for scalability and observability
- Document everything

---

## Quick Review Checklist

- Uses resource-based URLs
- Correct HTTP methods used
- Proper status codes returned
- Pagination implemented
- Versioning in place
- Security enforced
- Error format consistent
- Observability enabled

---

## Summary

Avoiding anti-patterns ensures your API is:

- Scalable
- Secure
- Maintainable
- Developer-friendly

---

## AI Agents

Avoid REST API anti-patterns:

- No verbs in URLs (use nouns)
- Use correct HTTP methods
- Do not return 200 for errors
- Implement pagination and caching
- Avoid deep nesting
- Add versioning and documentation
- Enforce security and validation
- Provide consistent error formats
