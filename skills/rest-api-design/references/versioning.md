# API Versioning

APIs evolve. Treat breaking changes deliberately.

## When to Version

- Adding an optional field → usually non-breaking
- Changing the meaning of an existing field → breaking
- Removing a field or endpoint → breaking
- Changing required request shape → breaking

## Strategies

The most common and obvious approach is a URL prefix:

```
/api/v1/orders
/api/v2/orders
```

Header-based versioning is also valid; the important rule is **pick one strategy and apply it consistently**.

## Rules

- Never silently introduce breaking changes.
- Give consumers a clear migration path and a deprecation window when practical.
- Non-breaking changes should preserve:
  - Endpoint behavior
  - Required fields
  - Existing response fields
  - Resource semantics
  - Authentication behavior

Existing clients must not become collateral damage.
