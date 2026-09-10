# OpenAPI Documentation & Contracts

Make the API self-documenting so the eight laws become an enforceable, machine-readable contract.

## Why OpenAPI

The eight laws make an API predictable for humans. OpenAPI makes that predictability usable by tools:

- Interactive docs (Swagger UI, Redoc, Stoplight)
- Client SDK generation (TypeScript, PHP, etc.)
- Request/response validation
- Mock servers
- Breaking-change detection in CI

The OpenAPI document is the executable version of the design decisions already made (kebab-case paths, camelCase JSON, consistent errors, etc.).

## Source of Truth

Prefer **code → OpenAPI** (code-first) in Laravel:

1. Controllers, Form Requests, and API Resources remain the implementation.
2. PHP attributes (or annotations) on those classes supply the missing documentation details.
3. A generator produces the OpenAPI document from the real code + attributes.

This keeps the contract in sync with reality and avoids drift between a hand-written spec and the running API.

## Required Attributes / Annotations

Every public endpoint should be described so a generator can produce a complete contract. At minimum document:

- Operation summary / description
- HTTP method and path (already visible from routes, but confirm)
- Path parameters (camelCase, e.g. `{userId}`)
- Query parameters (filters, sort, pagination)
- Request body schema (from Form Request or explicit schema)
- Success response schema (from API Resource) and status code (`200`, `201`, `204`, …)
- Error responses (`401`, `403`, `404`, `422`, `429`, …) using the shared error envelope
- Authentication / security requirements

Use PHP 8 attributes when the chosen generator supports them. Fall back to docblock annotations only when necessary.

### What the attributes must capture

Because the skill standardizes these conventions, the generated document must reflect them:

| Convention              | How it appears in OpenAPI                          |
|-------------------------|----------------------------------------------------|
| URL paths               | kebab-case (`/api/user-profiles`)                  |
| Route parameters        | camelCase (`userId`, `orderId`)                    |
| JSON keys               | camelCase properties in schemas                    |
| Error shape             | Reusable schema matching the standard error envelope |
| Pagination              | Consistent `meta` object on collection responses   |
| Dates                   | `format: date-time` (ISO 8601)                     |
| Status codes            | Explicitly listed for success and error cases      |

## Laravel Package Guidance

Recommended (attribute-friendly, modern):

- **dedoc/scramble** — excellent zero-to-low config generation from routes, Form Requests, and API Resources; strong PHP attribute support.

Also viable:

- **knuckleswtf/scribe** — very capable, good strategy/response documentation
- **darkaonline/l5-swagger** + swagger-php — classic annotation style

Choose one generator and stay consistent. Configure it once so camelCase JSON, the shared error schema, and pagination meta are emitted automatically wherever possible.

## Practical Rules

- Add or update attributes whenever you add or change an endpoint.
- Reuse schema components for the standard error envelope and pagination meta.
- Version the OpenAPI document in parallel with the API (`/api/v1`, `/api/v2`).
- Fail CI if the generated document is out of date or if breaking changes appear without a version bump.
- Never hand-edit the generated OpenAPI file as the long-term source of truth — regenerate it from code + attributes.

## Checklist for Each Endpoint

- [ ] PHP attribute(s) or annotations present
- [ ] Summary and description written
- [ ] Path and query parameters documented (camelCase names)
- [ ] Request body schema correct
- [ ] Success response schema + status code correct
- [ ] Error responses (especially `422` validation and the shared error shape) documented
- [ ] Security requirements declared when needed
- [ ] Generated OpenAPI output reviewed for accuracy

The goal is simple: a consumer (or a code generator) should be able to understand and integrate with the API from the OpenAPI document alone, without reverse-engineering controllers.
