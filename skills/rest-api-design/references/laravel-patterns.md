# Laravel Patterns

Laravel Boost already encodes framework best practices. Map the eight REST laws onto Laravel primitives as follows.

## Naming Conventions (defaults)

- URL paths: kebab-case (`/api/user-profiles`, `/api/order-items`)
- Route parameters: camelCase (`{userId}`, `{orderId}`) so they match controller method variables
- JSON keys: camelCase (`firstName`, `createdAt`) — typically emitted by API Resources

## Resources & Controllers

- Prefer plural resource routes: `Route::apiResource('orders', OrderController::class)`
- Use explicit route model binding
- Return API Resources (or JsonResource) configured for camelCase output so the response shape stays consistent

## Validation & Status Codes

- Form Requests for input validation → natural `422` responses
- Use `response()->json($data, $status)` or the Resource’s `withResponse` / `additional` methods to set accurate status codes (`201`, `204`, `409`, etc.)
- Never return a successful status with an error body

## Errors

- Centralize exception rendering (Laravel’s exception handler or a dedicated API exception renderer) so every error follows the same shape
- Map domain exceptions to the correct HTTP status and a stable error `code`

## Query Parameters

- Accept filters, sorts, and pagination via query string
- Use a consistent package or trait (e.g. spatie/laravel-query-builder or a thin internal layer) so every collection endpoint behaves the same way

## Versioning

- Prefer URL versioning under `/api/v1/...` for clarity
- Keep versioned route files or route groups clean and parallel

## Authentication & Rate Limiting

- Sanctum (or Passport) for token auth
- Laravel’s built-in rate limiters for `429` responses
- Always enforce authorization at the resource level (Policies), not only authentication

The framework gives you the tools. The eight laws keep the resulting HTTP surface predictable.
