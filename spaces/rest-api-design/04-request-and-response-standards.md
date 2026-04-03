# 4. Request & Response Standards

Consistent request and response formats are critical for developer experience, maintainability, and automation. A well-designed API should be predictable, self-descriptive, and standardized across all endpoints.

## Request Standards

### 1. Content Type & Headers

Always explicitly define the format of the request and response.

**Standard headers:**

```
Content-Type: application/json
Accept: application/json
```

Why: Ensures client and server agree on format and avoids parsing ambiguity.

---

### 2. Request Body Structure

Use JSON as the default format.

**Example:**

```http
POST /users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Rules:**

- Use camelCase for JSON fields
- Keep structure flat unless nesting is meaningful
- Avoid unnecessary wrappers

---

### 3. Validation

- Validate all inputs on the server
- Reject invalid requests with proper error responses

Examples of validation failures:

- Missing required fields
- Invalid formats (email, UUID)
- Business rule violations

---

### 4. Idempotency (for POST when needed)

For critical operations (payments, orders) include an idempotency key:

```
POST /payments
Idempotency-Key: abc-123
```

Prevents duplicate processing on retries.

---

### 5. Query Parameters

Used for filtering, sorting, and pagination.

**Example:**

```
GET /users?status=active&sort=createdAt&page=1&limit=20
```

---

### 6. Path Parameters

Used to identify a specific resource:

```
GET /users/{id}
```

---

## Response Standards

### 1. Consistent Response Structure

Use a predictable wrapper:

```json
{
  "data": {},
  "meta": {},
  "errors": []
}
```

Benefits: standard parsing, easier client integration, supports extensibility.

---

### 2. Data Field

Contains the actual response payload.

**Example:**

```json
{
  "data": {
    "id": "123",
    "name": "John Doe"
  }
}
```

---

### 3. Meta Field

Contains additional information about the response.

**Example:**

```json
{
  "meta": {
    "requestId": "abc-123",
    "timestamp": "2026-01-01T12:00:00Z"
  }
}
```

Common meta fields: pagination info, request ID, processing time.

---

### 4. Errors Field

- Use when something goes wrong
- Should be empty or omitted on success
- Follow a consistent structure

Example error format:

```json
{
  "errors": [
    {
      "code": "VALIDATION_ERROR",
      "message": "Invalid input",
      "details": [
        { "field": "email", "issue": "Invalid format" }
      ]
    }
  ]
}
```

---

### 5. HTTP Status Codes

Use standard HTTP codes correctly:

| Code | Meaning |
|---:|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Validation Error |
| 500 | Internal Server Error |

---

### 6. Location Header (for POST)

When creating a resource, return `201 Created` with a `Location` header:

```
HTTP/1.1 201 Created
Location: /users/123
```

---

### 7. Empty Responses

For delete/update operations use `204 No Content` and return no body.

---

### 8. Pagination Response Format

```json
{
  "data": [],
  "meta": {
    "page": 2,
    "limit": 50,
    "total": 120
  }
}
```

---

### 9. Partial Responses (Field Selection)

Support field selection to reduce payload size:

```
GET /users?fields=id,name
```

---

### 10. Consistent Naming

- Use camelCase for JSON fields
- Avoid mixing camelCase and snake_case

---

## Advanced Practices

### 1. HATEOAS (Optional)

Include links in responses:

```json
{
  "data": { "id": "123" },
  "links": {
    "self": "/users/123",
    "orders": "/users/123/orders"
  }
}
```

### 2. Content Negotiation

Support multiple formats when needed using the `Accept` header (e.g., `application/json`, `application/xml`).

### 3. Compression

Enable gzip or brotli and honor `Accept-Encoding`.

```
Accept-Encoding: gzip
```

---

## Common Mistakes

- Inconsistent response formats
- Missing or incorrect HTTP status codes
- Returning raw database objects
- Mixing camelCase and snake_case
- Returning errors with HTTP 200
- No validation

---

## Summary

| Area         | Rule                                |
| ------------ | ----------------------------------- |
| Headers      | Always define Content-Type & Accept |
| Request body | JSON, camelCase                     |
| Response     | data/meta/errors structure          |
| Status codes | Use correctly                       |
| Errors       | Structured and meaningful           |
| Pagination   | Always include meta                 |
| Consistency  | Same format everywhere              |

---

## AI Agents

Design API request/response with:

- JSON format (camelCase)
- Consistent response wrapper (data/meta/errors)
- Proper HTTP status codes
- Structured error responses
- Pagination metadata
- Validation and clear error messages