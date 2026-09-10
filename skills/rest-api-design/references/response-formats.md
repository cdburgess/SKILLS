# Response Formats

Consistency across endpoints is more valuable than any single clever format.

## Rules

- Use JSON as the primary representation.
- JSON keys use camelCase (e.g. `firstName`, `createdAt`) — aligns with JavaScript/TypeScript frontends and is the default produced by Laravel API Resources.
- Use ISO 8601 for all dates and times (`2026-09-10T13:30:00Z`).
- Keep collection and single-resource envelopes predictable.
- Apply the same pagination, error, and metadata shapes everywhere.

## Suggested Envelopes

Single resource:

```json
{
  "data": {
    "id": "123",
    "firstName": "Jane",
    "lastName": "Doe",
    "createdAt": "2026-09-10T13:30:00Z"
  }
}
```

Collection:

```json
{
  "data": [
    { "id": "123", "firstName": "Jane", "lastName": "Doe" },
    { "id": "456", "firstName": "John", "lastName": "Doe" }
  ],
  "meta": { ... }
}
```

Once a developer understands one endpoint they should be able to make safe assumptions about the rest of the API.
