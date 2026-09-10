---
name: design-patterns-php
description: Apply, explain, implement, and refactor with classic GoF design patterns in PHP (based on Refactoring.Guru). Use when the user asks about design patterns, pattern selection, PHP implementations, code structure for creational/structural/behavioral patterns, or improving OOP design with patterns. Triggers include factory method, singleton, strategy, observer, decorator, adapter, and similar pattern names or "which pattern for X".
license: MIT
metadata:
  author: chuck
---

# Design Patterns in PHP

Provide accurate, practical guidance on the 23 classic Gang of Four design patterns for PHP, drawn from Refactoring.Guru material and its official PHP examples.

## When to Use This Skill

- User requests explanation, implementation, or comparison of a design pattern in PHP.
- User describes a design problem (object creation, interface mismatch, behavior variation, etc.) and needs a recommended pattern.
- User wants to refactor existing PHP code toward a pattern or avoid anti-patterns.
- User asks for PHP-specific idioms, modern PHP 8+ adaptations, or real-world usage notes.

Do **not** invent new patterns or mix unrelated modern techniques unless the user explicitly asks. Stick to the classic catalog.

## Core Principles

1. Prefer composition over inheritance where the pattern allows it.
2. Favor interfaces and type declarations (PHP 7.3+ style used in the source examples).
3. Keep examples self-contained and focused on the pattern’s structure.
4. Always state **Intent**, **Problem it solves**, **Structure roles**, **Applicability**, **Pros/Cons**, and **Relations** to other patterns.
5. Warn about overuse (especially Singleton) and note PHP language quirks (private constructors, cloning, serialization, late static binding, etc.).

## Workflow

1. Identify the category (Creational / Structural / Behavioral) and the specific pattern.
2. Load the matching reference file from `references/` for detailed notes if needed.
3. Explain intent and problem in plain language.
4. Show a clean PHP 8+ implementation (interfaces + classes, type hints, minimal comments).
5. Provide a short real-world PHP scenario (Laravel/Symfony style is fine when relevant).
6. List pros, cons, and common pitfalls.
7. Mention related patterns and when to prefer one over another.
8. If the user supplies code, refactor it toward the pattern or diagnose misapplication.

## PHP Conventions for Examples

- Use PSR-12 style.
- Prefer interfaces over abstract classes unless the pattern requires shared state/behavior.
- Explicit return and parameter types.
- Namespace only when illustrating larger structure; otherwise keep single-file examples.
- Avoid frameworks unless the user requests them.
- For Singleton: private `__construct()`, private `__clone()`, private `__wakeup()`, and a static `getInstance()`.
- Prefer dependency injection over global access when discussing alternatives to Singleton.

## Catalog Overview

### Creational
- Abstract Factory — families of related objects
- Builder — step-by-step complex construction
- Factory Method — subclass decides concrete product
- Prototype — clone existing objects
- Singleton — single shared instance

### Structural
- Adapter — incompatible interfaces
- Bridge — separate abstraction from implementation
- Composite — tree structures treated uniformly
- Decorator — dynamic behavior attachment
- Facade — simplified interface to a subsystem
- Flyweight — share fine-grained state
- Proxy — control access to another object

### Behavioral
- Chain of Responsibility — pass request along handlers
- Command — encapsulate request as object
- Iterator — traverse without exposing structure
- Mediator — centralize complex communications
- Memento — capture/restore object state
- Observer — publish-subscribe notifications
- State — behavior changes with internal state
- Strategy — interchangeable algorithms
- Template Method — skeleton algorithm with overridable steps
- Visitor — separate operations from object structure

## Reference Files

- `references/catalog.md` — short intent + applicability for every pattern
- `references/creational.md` — detailed notes + PHP tips for creational patterns
- `references/structural.md` — detailed notes + PHP tips for structural patterns
- `references/behavioral.md` — detailed notes + PHP tips for behavioral patterns
- `references/php-idioms.md` — language-specific gotchas, modern PHP adaptations, testing notes

Load only the files required for the current question.

## Output Style

- Be concise and structured.
- Lead with the recommended pattern when the user describes a problem.
- Show runnable PHP code in a single block when illustrating.
- Always note complexity vs. benefit so the user can decide whether the pattern is warranted.
- Credit the conceptual source implicitly by staying faithful to Refactoring.Guru terminology and structure.

## Anti-Patterns to Flag

- God objects / overusing Singleton for convenience
- Premature abstraction (pattern for its own sake)
- Leaking concrete types instead of depending on interfaces
- Breaking the Open/Closed Principle when a simple strategy or factory would suffice
