# 5. Error Handling

Error handling in REST APIs should be consistent, predictable, and informative. A well-designed error system helps clients understand what went wrong and how to fix it.

## Goals of Good Error Handling

- Provide clear and actionable messages
- Use correct HTTP status codes
- Be consistent across all endpoints
- Avoid leaking sensitive/internal details
- Support debugging and observability

---

## Standard Error Response Format

Use a consistent structure for all errors:

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User not found",
    "details": []
  }
}
```

### Fields Explained

- `code`: Machine-readable error identifier
- `message`: Human-readable explanation
- `details`: Optional structured info (validation errors, etc.)

---

## HTTP Status Codes

Always use standard HTTP status codes correctly.

### Client Errors (4xx)

| Code | Meaning | When to Use |
|---:|---|---|
| 400 | Bad Request | Invalid syntax / malformed request |
| 401 | Unauthorized | Missing/invalid authentication |
| 403 | Forbidden | Authenticated but not allowed |
| 404 | Not Found | Resource does not exist |
| 409 | Conflict | Resource state conflict |
| 422 | Validation Error | Semantic validation failure |

### Server Errors (5xx)

| Code | Meaning |
|---:|---|
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

---

## Validation Errors

Validation errors should be detailed and structured.

**Example:**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": [
      { "field": "email", "issue": "Invalid format" },
      { "field": "password", "issue": "Too short" }
    ]
  }
}
```

**Best practices:**

- Include field-level errors
- Avoid vague messages like "Invalid request"

---

## Business Logic Errors

Used when request is valid but cannot be processed.

**Example:**

```json
{
  "error": {
    "code": "INSUFFICIENT_FUNDS",
    "message": "Account balance is too low"
  }
}
```

**Typical codes:** `409 Conflict` or `422 Unprocessable Entity`.

---

## Authentication & Authorization Errors

**401 Unauthorized**

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Missing or invalid token"
  }
}
```

**403 Forbidden**

```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "You do not have access to this resource"
  }
}
```

---

## Not Found Errors

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 123 does not exist"
  }
}
```

---

## Rate Limiting Errors

**429 Too Many Requests**

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests, please try again later"
  }
}
```

Include header:

```
Retry-After: 60
```

---

## Correlation / Request ID

Always include a request identifier (e.g., `X-Request-ID: abc-123`) and include it in error responses:

```json
{
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "Unexpected error occurred",
    "requestId": "abc-123"
  }
}
```

This helps debugging across systems.

---

## Logging Strategy

Log internally:

- Full error stack trace
- Request context
- User ID (if available)

Do NOT expose in responses:

- Stack traces
- Internal class names
- Raw database errors

---

## Error Code Design

Use stable, machine-readable codes.

**Good examples:**

- `USER_NOT_FOUND`
- `INVALID_EMAIL`
- `PAYMENT_FAILED`

**Bad examples:**

- "Something went wrong"
- "Error123"

**Rules:**

- Uppercase
- Snake case
- Stable over time

---

## Global Error Handling

Implement a centralized error handler (middleware / exception mapper) to ensure consistent responses, avoid duplicated logic, and keep controllers/services clean.

---

## Fail Fast Principle

- Validate early
- Reject invalid input immediately

Benefits: saves resources and gives clear feedback to clients.

---

## Common Mistakes

- Returning HTTP 200 with error message
- Inconsistent error formats
- Missing error codes
- Exposing internal stack traces
- Using vague messages
- No validation

---

## Recommended Error Flow

1. Validate request → if invalid return `400`/`422`
2. Process request → if business issue return `409`/`422`
3. If unexpected → return `500`
4. Log internally with request ID

---

## Summary

| Area         | Rule                      |
| ------------ | ------------------------- |
| Format       | Consistent JSON structure |
| Status codes | Use correct HTTP codes    |
| Messages     | Clear and actionable      |
| Codes        | Machine-readable          |
| Security     | Hide internal details     |
| Logging      | Full internal logs        |
| Debugging    | Include request ID        |

---

## AI Agents

Implement REST API error handling with:

- Consistent error format (`code`, `message`, `details`)
- Proper HTTP status codes (4xx, 5xx)
- Validation error details per field
- No internal stack traces in responses
- Include `requestId` for debugging
- Centralized exception handling
