# PHP-Specific Idioms & Gotchas

## Language Features That Affect Patterns

- **Interfaces + type declarations** are first-class. Always declare return types on factory methods, strategy methods, etc.
- **`clone` and `__clone()`** — essential for Prototype. Remember shallow copy by default.
- **Private constructor + static** — classic Singleton. Also seal `__clone` and `__wakeup`.
- **Late static binding (`static::`)** — useful in Factory Method hierarchies.
- **Traits** — can implement shared behavior but do not replace proper composition for most patterns.
- **Anonymous classes** — handy for one-off adapters or simple strategies in tests.
- **Readonly properties / classes (PHP 8.1/8.2)** — excellent for immutable Value Objects, Mementos, Commands, Flyweights.
- **Enums (PHP 8.1)** — sometimes replace simple State or Strategy variants.

## Testing Considerations

- Prefer constructor injection so tests can supply mocks/fakes.
- Singleton is hostile to unit testing; extract an interface or use a container.
- For Observer/Mediator, assert that the correct notifications were sent.
- Command pattern pairs well with test doubles for the Receiver.

## Modern PHP Recommendations

- Prefer a DI container (Laravel, Symfony, PHP-DI, etc.) over manual Singleton or Service Locator.
- Middleware pipelines are Chain of Responsibility + Decorator hybrids (PSR-15).
- Event systems implement Observer (PSR-14).
- Message buses implement Command.
- Avoid “Simple Factory” static methods when the Open/Closed Principle matters; use proper Factory Method or Abstract Factory.

## Naming Conventions (Refactoring.Guru style)

- Interfaces often named after the role: `Product`, `Factory`, `Strategy`, `Handler`, `Subject`.
- Concrete classes include the variation: `RoadLogistics`, `EmailNotification`, `JsonRenderer`.
- Keep example code in one file for clarity when teaching; split into proper files/namespaces in real projects.

## Common Pitfalls in PHP

1. Forgetting to prevent cloning/serialization of Singletons.
2. Using inheritance for Strategy or Decorator when composition is clearer.
3. Making Flyweight extrinsic state part of the shared object.
4. Exposing Memento internals.
5. Creating deep inheritance trees instead of stacking Decorators.
6. Treating every conditional as a candidate for State/Strategy — sometimes a simple match expression is enough.

## When Not to Use a Pattern

- The problem is simple and the pattern adds more classes than value.
- The language or framework already provides the abstraction (Iterator, events, middleware).
- You are solving a one-off case that will never vary.

Always weigh complexity against the flexibility gained.
