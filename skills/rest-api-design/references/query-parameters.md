# Query Parameters

The path identifies the resource. Query parameters refine the result set.

## Appropriate Uses

- Filtering: `?status=active&role=admin`
- Sorting: `?sort=-created_at` (prefix `-` for descending is a common convention)
- Pagination: `?page=2&per_page=25` or cursor-based equivalents
- Field selection / sparse fieldsets when useful
- Search: `?q=search+term`

Example:

```
GET /api/products?category=books&status=in_stock&sort=-price&page=2&per_page=20
```

## Anti-patterns

- Putting filters into the path until the URL becomes unreadable
- Using query parameters to express actions (`?action=delete`, `?method=update`)
- Mixing path-based and query-based filtering inconsistently across endpoints

## Pagination Envelope (suggested)

```json
{
  "data": [ ... ],
  "meta": {
    "current_page": 2,
    "per_page": 25,
    "total": 137,
    "last_page": 6
  }
}
```

Keep the pagination shape identical on every collection endpoint.
