# Resource Naming

## Core Rule

URLs identify resources. HTTP methods describe operations.

## Collections and Singles

Always use plural nouns for collections:

```
GET    /api/users
POST   /api/users
GET    /api/users/{userId}
PATCH  /api/users/{userId}
DELETE /api/users/{userId}
```

Avoid mixing singular and plural conventions inside the same API.

## Predictability

Use these defaults consistently:

- Prefer plural collection names (`orders`, `products`, `user-profiles`)
- URL paths: kebab-case for multi-word resources (`/api/user-profiles`, `/api/order-items`)
- Route parameters: camelCase (`{userId}`, `{orderId}`, `{userProfileId}`) to match controller method variables
- Keep identifiers stable

A developer who has seen two endpoints should be able to guess the third.

## Nesting

Nest only when the child has a strong, meaningful relationship to the parent and the nesting stays shallow (generally ≤ 2 levels):

```
GET  /api/users/{userId}/orders
POST /api/users/{userId}/orders
```

If a resource has an independent identity, prefer a top-level endpoint:

```
GET /api/orders/{orderId}
```

Deep nesting such as `/api/users/{userId}/orders/{orderId}/items/{itemId}/products/{productId}` becomes unmaintainable.

## Anti-patterns

- Verb-based paths: `/api/getUsers`, `/api/createOrder`, `/api/deleteProduct`
- Inconsistent synonyms: `user` in one place, `customer` in another, `account` in a third
- Action parameters in the path or query string that replace HTTP methods
