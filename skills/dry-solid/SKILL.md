---
name: dry-solid
description: Enforce DRY and SOLID when writing or refactoring PHP code. Trigger whenever the AI is about to duplicate a method, copy-paste logic, create similar classes, or write new PHP code. Also trigger on requests involving clean code, refactoring for reuse, or applying SOLID principles in PHP.
license: MIT
metadata:
  author: chuck
---

# DRY + SOLID for PHP

These rules are mandatory for all PHP code generation, editing, and refactoring.

## 1. Zero Tolerance for Duplication (DRY)

**Never copy-paste an existing method, function, or non-trivial block of logic.**

When you detect that you are about to duplicate code:

1. **Stop**.
2. Extract the common logic into a single reusable unit.
3. Update every call site to use the new single source of truth.
4. Delete the duplicated code.

### Preferred extraction order in PHP

1. **Private / protected method** on the same class (simplest and most common)
2. **Trait** (when the same behavior is needed across unrelated classes)
3. **Dedicated service / action / helper class** (when the logic has its own responsibility)
4. **Interface + implementation** (only when polymorphism or multiple variants are truly needed)

### PHP-specific duplication smells

- Identical or nearly identical methods in controllers, models, or services
- Repeated validation, mapping (`toArray`, DTOs), formatting, or calculation logic
- Copy-pasted query scopes, filters, or authorization checks
- Similar `boot()`, constructors, or setup code across classes
- Duplicated try/catch + logging blocks

Even 5+ lines that appear more than once should usually be extracted.

## 2. SOLID Principles in PHP (Mandatory)

### S — Single Responsibility Principle
- A class should have only one reason to change.
- Controllers should orchestrate, not contain business rules.
- Prefer small focused classes: Actions, Services, Value Objects, DTOs, Query Objects.
- Methods should ideally stay under 20–30 lines.

### O — Open/Closed Principle
- Prefer adding new classes over modifying existing ones with growing `if/else` or `match` statements.
- Use Strategy, Decorator, or Factory Method when behavior needs to vary.
- Laravel example: replace long conditionals with Strategy classes or pipeline/middleware.

### L — Liskov Substitution Principle
- Subtypes must be substitutable for their base types.
- Avoid empty method overrides or throwing `BadMethodCallException` in subclasses.
- Prefer composition + interfaces over deep inheritance.

### I — Interface Segregation Principle
- Prefer small, focused interfaces over fat ones.
- Do not force a class to implement methods it does not need.
- Example: split a large `RepositoryInterface` into smaller read/write or specific query interfaces when appropriate.

### D — Dependency Inversion Principle
- Depend on abstractions (interfaces), not concrete classes.
- Prefer constructor injection.
- Avoid `new` of non-trivial collaborators inside business logic.
- In Laravel/Symfony, resolve services from the container instead of hard-coding concrete classes.

**Good PHP example (DIP):**
```php
class OrderService
{
    public function __construct(
        private readonly OrderRepositoryInterface $orders,
        private readonly PaymentGatewayInterface $payments,
    ) {}
}
```

## 3. PHP Practical Rules

- Before writing a new method, search the current class, traits, and nearby classes for similar logic.
- Prefer **constructor property promotion** + `readonly` (PHP 8.1+) for injected dependencies.
- Prefer **composition** over inheritance. Traits are acceptable for horizontal reuse but should stay small and focused.
- Keep methods pure when possible.
- Name extracted methods after *what* they do (`calculateTotal()`, `normalizeEmail()`), not *how*.
- Prefer early returns over deep nesting.
- Use `match` expressions instead of long switch/if chains when mapping values.
- When extracting, keep type declarations (parameter + return types) strict.

## 4. Refactoring Trigger Checklist (PHP)

Act immediately when any of these are true:

- [ ] I am about to copy an existing method
- [ ] Two methods differ only in a small detail
- [ ] A controller/model/service is growing large and handling multiple concerns
- [ ] I am adding another `if` / `match` arm for a new variant
- [ ] A concrete class is being instantiated with `new` inside business logic instead of injected
- [ ] An interface has methods that some implementers leave empty or throw
- [ ] The same validation or transformation logic appears in multiple Form Requests / controllers

If any box is checked → extract and apply SOLID before continuing.

## 5. Output Expectations

When you perform an extraction or SOLID refactor:

1. Show the new reusable method, trait, or class first.
2. Show the updated call sites.
3. Briefly state which SOLID principle(s) improved and why the change reduces future maintenance cost.

Never leave duplicated logic “for now”. Fix it in the same change.

## 6. Quick PHP Idioms That Support These Rules

| Goal                        | Preferred PHP approach                     |
|----------------------------|--------------------------------------------|
| Shared behavior            | Private method → Trait → Service           |
| Varying algorithms         | Strategy interface + implementations       |
| Cross-cutting concerns     | Decorator or Middleware                    |
| Object creation            | Factory Method or container binding        |
| Immutable data             | `readonly` class / properties (PHP 8.2+)   |
| Multiple related variants  | Enum (PHP 8.1+) + match or Strategy        |
