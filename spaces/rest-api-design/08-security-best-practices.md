# 8. Security Best Practices

Security in REST APIs is critical to protect data, users, and infrastructure. A secure API must guard against unauthorized access, data leaks, and common attack vectors.

## Goals

- Ensure authentication (who you are)
- Enforce authorization (what you can do)
- Protect data in transit and at rest
- Prevent abuse and attacks
- Maintain auditability

---

## Authentication

Verify the identity of the client.

Recommended methods:

1. OAuth2 (preferred for public APIs)
   - Industry standard
   - Supports scopes and delegation
2. JWT (JSON Web Token)
   - `Authorization: Bearer <token>`

Benefits:
- Stateless, scalable, self-contained claims

Best practices:
- Use short-lived tokens and refresh tokens
- Validate signature and expiration
- Never trust client-side data blindly

---

## Authorization

Control access to resources.

Models:
- RBAC (Role-Based Access Control)
- ABAC (Attribute-Based Access Control)

Example:

```
GET /admin/users
```

Only accessible by admin role.

Best practices:
- Enforce authorization at every endpoint
- Apply least-privilege principle
- Do NOT rely on frontend for security

---

## Transport Security (HTTPS)

Always use HTTPS (TLS).

Do not send sensitive data over HTTP. Enforce:
- TLS 1.2+
- HSTS headers

Prevent downgrade attacks.

---

## Input Validation & Sanitization

Never trust incoming data. Validate:
- Required fields
- Data types
- Length limits
- Formats (email, UUID)

This prevents injection attacks (SQL/NoSQL), XSS, and other input-based attacks.

---

## Rate Limiting & Throttling

Protect against abuse and DoS attacks.

Use `429 Too Many Requests` and provide headers:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 10
Retry-After: 60
```

---

## CORS (Cross-Origin Resource Sharing)

Control which domains may access your API.

Example header:

```
Access-Control-Allow-Origin: https://example.com
```

Best practices:
- Avoid `*` in production
- Restrict allowed methods and headers

---

## Data Protection

Sensitive data:
- Never expose passwords, tokens, or internal-only IDs

Encryption:
- Encrypt data at rest
- Hash passwords using bcrypt or argon2

---

## Security Headers

Add protective HTTP headers:

```
Strict-Transport-Security: max-age=31536000
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'
```

---

## Logging & Monitoring

Log security-relevant events:
- Login attempts
- Failed authentication
- Permission denials

Include `X-Request-ID` and user identifiers (when available). Monitor and alert on suspicious patterns.

---

## Token Handling

Best practices:
- Store tokens securely (HTTP-only cookies or secure storage)
- Never expose tokens in URLs
- Rotate keys regularly

---

## API Keys

If used for simple integrations:
- Scope and limit usage
- Rotate keys regularly

---

## Prevent Common Attacks

1. Injection attacks
   - Use parameterized queries and validate inputs
2. Broken authentication
   - Strong token validation and secure session handling
3. Sensitive data exposure
   - Encrypt and mask logs
4. Mass assignment
   - Explicitly map allowed fields
5. Replay attacks
   - Use timestamps, nonces, and idempotency keys

---

## Idempotency & Replay Protection

Use idempotency keys for non-idempotent operations (e.g., payments):

```
Idempotency-Key: abc-123
```

This prevents duplicate processing on retries.

---

## File Upload Security

- Validate file types and size
- Scan for malware
- Store outside public directories

---

## Dependency & Supply Chain Security

- Keep dependencies updated
- Scan for vulnerabilities (Snyk, Dependabot)

---

## Zero Trust Approach

Verify every request — do not assume internal traffic is safe.

---

## Security Testing

- Penetration testing
- Automated security scans
- Fuzz testing

---

## Common Mistakes

- No authentication
- Hardcoded secrets
- Using HTTP instead of HTTPS
- No rate limiting
- Exposing internal errors
- Weak password hashing
- Overly permissive CORS

---

## Security Checklist

- HTTPS enforced
- Authentication implemented (JWT/OAuth2)
- Authorization enforced (RBAC/ABAC)
- Input validation in place
- Rate limiting enabled
- CORS configured securely
- Sensitive data protected
- Security headers added
- Logging & monitoring active
- Dependencies updated

---

## Example Secure Request

```
GET /users/123
Authorization: Bearer <token>
X-Request-ID: abc-123
```

---

## Summary

| Area | Key Practice |
|---|---|
| Auth | OAuth2 / JWT |
| Transport | HTTPS only |
| Validation | Strict input validation |
| Protection | Rate limiting + headers |
| Data | Encrypt + avoid exposure |
| Monitoring | Log security events |

---

## AI Agents

Secure REST API by:

- Using JWT/OAuth2 authentication
- Enforcing HTTPS
- Validating all inputs
- Implementing RBAC authorization
- Adding rate limiting
- Using security headers
- Avoiding sensitive data exposure
- Logging security events
