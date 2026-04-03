# 12. Audit Checklist

The audit checklist is used to systematically evaluate the quality, consistency, security, and performance of a REST API. Apply during code reviews, architecture reviews, API governance, or production readiness checks.

## Goals

- Ensure consistency across APIs
- Detect design flaws early
- Enforce best practices
- Improve maintainability and reliability
- Reduce security and performance risks

---

## 1. API Design

### Resource Design
- Uses nouns (not verbs) in endpoints
- Uses plural resource names (`/users`, `/orders`)
- Uses consistent naming (kebab-case)
- URLs are simple and predictable
- Nesting depth ≤ 2–3 levels

### HTTP Semantics
- Correct HTTP methods used (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`)
- No misuse of `POST` for reads
- `PUT` used for full updates only
- `PATCH` used for partial updates

### Versioning
- Versioning strategy defined (`/v1/...`)
- No breaking changes without version bump
- Old versions supported during migration
- Deprecation strategy defined

---

## 2. Request Validation

- All inputs validated (body, query, path)
- Required fields enforced
- Data types validated
- Input length limits defined
- Invalid requests return `400`/`422`

---

## 3. Response Standards

- Consistent response structure (`data`, `meta`, `errors`)
- JSON format used consistently
- Field naming uses `camelCase`
- Pagination metadata included where applicable
- No unnecessary data returned

---

## 4. Error Handling

- Standard error format used (`code`, `message`, `details`)
- Proper HTTP status codes returned
- Validation errors include field details
- No sensitive/internal data exposed
- Error codes are consistent and meaningful

---

## 5. Performance & Scalability

- Pagination implemented for list endpoints
- Filtering and sorting supported
- Field selection available (optional)
- Caching headers used (`Cache-Control`, `ETag`)
- Compression enabled (`gzip`/`brotli`)
- No large payload responses
- Database queries optimized (no N+1)

---

## 6. Security

- HTTPS enforced
- Authentication implemented (JWT/OAuth2)
- Authorization enforced (RBAC/ABAC)
- Input validation prevents injection
- Rate limiting configured
- CORS configured securely
- Sensitive data not exposed
- Security headers present

---

## 7. Idempotency & Reliability

- Idempotency implemented for critical `POST` operations
- Idempotency keys supported
- Safe retry strategy defined
- Timeouts configured
- Duplicate requests handled
- Data consistency ensured (transactions/locking)

---

## 8. Observability & Monitoring

- Request ID included (`X-Request-ID`)
- Structured logging implemented
- Metrics collected (latency, errors, throughput)
- Distributed tracing enabled
- Health endpoints available (`/health`)
- Alerts configured

---

## 9. Documentation

- OpenAPI / Swagger specification available
- Endpoints documented with examples
- Error responses documented
- Authentication documented
- Versioning documented

---

## 10. Consistency

- Naming consistent across API
- Response formats consistent
- Error formats consistent
- Query parameter patterns consistent

---

## 11. Testing

- Unit tests implemented
- Integration tests implemented
- Error scenarios tested
- Load/performance tests conducted
- Security tests performed

---

## 12. Deployment Readiness

- Environment configuration externalized
- Secrets managed securely
- Rate limits configured
- Monitoring dashboards ready
- Rollback strategy defined

---

## Audit Scoring (Optional)

Assign a score to evaluate API quality:

| Score | Quality |
|---:|---|
| 90–100% | Production-ready |
| 70–89% | Needs improvement |
| <70% | High risk |

---

## Common Audit Failures

- No pagination on large endpoints
- Inconsistent error formats
- Missing authentication
- Returning HTTP 200 for errors
- No versioning strategy
- No observability (logs/metrics)
- Exposing sensitive data

---

## Example Audit Flow

1. Review endpoint design (`/users` vs `/getUsers`)
2. Validate request/response formats
3. Check error handling consistency
4. Verify security (auth, HTTPS, validation)
5. Evaluate performance (pagination, caching)
6. Confirm observability (logs, metrics)

---

## Summary

| Area | What to Check |
|---|---|
| Design | Naming, structure, HTTP methods |
| Validation | Input correctness |
| Responses | Format & consistency |
| Errors | Structured handling |
| Performance | Pagination, caching |
| Security | Auth, HTTPS, validation |
| Reliability | Idempotency, retries |
| Observability | Logs, metrics, tracing |

---

## AI Agents

Audit REST APIs for:

- Proper resource naming and HTTP methods
- Consistent request/response structure
- Structured error handling
- Pagination, filtering, performance optimizations
- Security (JWT, HTTPS, validation)
- Idempotency and retry safety
- Observability (logs, metrics, tracing)
