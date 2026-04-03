# 6. Versioning Strategy

Versioning allows you to evolve your API without breaking existing clients. A clear strategy ensures backward compatibility, smooth upgrades, and long-term stability.

## Why Versioning Matters

APIs change over time:

- New fields are added
- Existing fields change meaning
- Endpoints are redesigned
- Business logic evolves

Without versioning, clients can break unexpectedly and deployments become risky.

Versioning enables safe evolution, multiple client support, and controlled deprecation.

---

## When to Version

Introduce a new version when making breaking changes, such as:

- Removing fields
- Renaming fields
- Changing response structure
- Changing semantics/behavior
- Removing endpoints

Non-breaking changes (no version bump required):

- Adding optional fields
- Adding new endpoints
- Adding query parameters

---

## Versioning Strategies

### 1) URI Versioning (Recommended ✅)

Include the version in the URL path:

```
/v1/users
/v2/users
```

Pros:

- Simple and explicit
- Easy to debug
- Works well with caching/CDNs
- Widely adopted

Cons:

- URL changes between versions

### 2) Header Versioning

Pass the version via a custom header:

```
GET /users
API-Version: 1
```

Pros:

- Clean URLs
- Separation of concerns

Cons:

- Harder to test/debug
- Less visible in logs and tools

### 3) Accept Header (Content Negotiation)

Specify version in the media type:

```
Accept: application/vnd.myapi.v1+json
```

Pros:

- REST-pure approach
- Flexible

Cons:

- Complex
- Harder for clients/tools

---

## Recommended Approach

Use URI versioning for most APIs, e.g. `/api/v1/users`.

Why: it's developer-friendly, works well with tooling (OpenAPI, proxies), and is clear and explicit.

---

## Versioning Best Practices

1. Never break existing clients

- Old versions must continue to work
- Changes should be backward compatible when possible

2. Support Multiple Versions

- Serve `/v1/users` and `/v2/users` during migration to allow gradual client adoption

3. Deprecation Strategy

- Announce deprecation early
- Provide migration guides and examples
- Set a clear sunset timeline

Example headers:

```
Deprecation: true
Sunset: Wed, 01 Jan 2027 00:00:00 GMT
```

4. Keep Versions Stable

- Avoid frequent version bumps; only version for breaking changes

5. Avoid Over-Versioning

- Do not version every small change; prefer additive changes where possible

6. Maintain Compatibility

- Prefer additive changes (adding fields) over renames or removals

7. Version at the API Boundary

- Version externally (API surface) and keep internal services version-free when practical

---

## Versioning vs Compatibility

| Change | Version Required |
|---|:---:|
| Add field | No |
| Remove field | Yes |
| Rename field | Yes |
| Add endpoint | No |
| Change response structure | Yes |

---

## URL Design Recommendation

Use a consistent URL prefix and versioning pattern:

```
/api/v1/users
/api/v1/orders
```

Consistent prefix components:

- `/api` → logical grouping
- `/v1` → version

---

## Migration Strategy

- Release `/v2`
- Keep `/v1` active during migration
- Notify clients and provide migration guidance
- Monitor usage and deprecate `/v1` after the sunset date

---

## Testing & Documentation

- Maintain separate OpenAPI / Swagger specs per version
- Document differences and migration examples clearly
- Provide sample requests/responses for each version
- Run integration tests against multiple versions during CI

---

## Example Evolution

v1 response:

```json
{
  "id": "123",
  "name": "John"
}
```

v2 (breaking change):

```json
{
  "id": "123",
  "fullName": "John Doe"
}
```

---

## Common Mistakes

- No versioning at all
- Breaking changes without notice
- Removing endpoints abruptly
- Versioning too often
- Mixing versions in the same endpoint

---

## Common Pattern

```
/api/v1/...
/api/v2/...
```

---

## Summary

| Rule | Description |
|---|---|
| Version on breaking change | Protect clients by versioning only when necessary |
| Prefer URI versioning | Simplicity and tooling compatibility |
| Support multiple versions | Smooth migration for clients |
| Deprecate gradually | Avoid disruption with clear timelines |
| Keep backward compatibility | Prioritize stability |

---

## AI Agents

Design REST API versioning:

- Use URI versioning (`/api/v1`)
- Only version breaking changes
- Maintain backward compatibility
- Support multiple versions simultaneously
- Add deprecation and sunset headers

