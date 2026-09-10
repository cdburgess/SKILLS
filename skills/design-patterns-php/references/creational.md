# Creational Patterns — Detailed Notes (PHP)

## Abstract Factory

**Roles**  
- AbstractFactory (interface)  
- ConcreteFactory (one per family)  
- AbstractProductA / AbstractProductB  
- ConcreteProductA1, A2, …  

**PHP tips**  
- Declare factory methods with return type of the abstract product interface.  
- Prefer constructor injection of the factory rather than static access.  
- In frameworks, often realized via service containers that resolve families.

**When to prefer over Factory Method**  
When you need to create multiple related products that must stay consistent.

## Builder

**Roles**  
- Builder (interface with build steps)  
- ConcreteBuilder  
- Director (optional — orchestrates the steps)  
- Product  

**PHP tips**  
- Fluent interface (`return $this`) is common and idiomatic.  
- Director can be omitted if the client is simple; keep it when construction sequences are complex or reusable.  
- Use for configuration objects, query builders, form builders, etc.

## Factory Method

**Roles**  
- Creator (declares factory method)  
- ConcreteCreator  
- Product (interface)  
- ConcreteProduct  

**PHP tips**  
- Factory method can be abstract or provide a default.  
- Useful inside frameworks where subclasses customize component creation (e.g. form fields, middleware).  
- Avoid “Simple Factory” static methods if the goal is open/closed extensibility; use true Factory Method.

## Prototype

**Roles**  
- Prototype (interface with `clone()`)  
- ConcretePrototype  
- Client  

**PHP tips**  
- PHP’s native `clone` keyword + `__clone()` magic method.  
- Deep vs shallow copy: implement `__clone()` carefully for nested objects.  
- Register prototypes in a registry when many variants exist.  
- Useful for configuration templates, game entities, document prototypes.

## Singleton

**Roles**  
- Singleton (private constructor, static getInstance)

**PHP implementation skeleton**
```php
final class Singleton
{
    private static ?self $instance = null;

    private function __construct() {}
    private function __clone() {}
    public function __wakeup(): void
    {
        throw new \Exception("Cannot unserialize singleton");
    }

    public static function getInstance(): self
    {
        return self::$instance ??= new self();
    }
}
```

**Critical notes**  
- Violates Single Responsibility.  
- Hard to unit-test without seams or containers.  
- Prefer dependency injection / service container in modern PHP applications.  
- If used, make the class `final` and seal the magic methods.  
- Multithreading is rarely an issue in typical PHP-FPM / CLI, but still protect with the static check.

**Relations**  
Facade and many factories are often implemented as Singletons; consider whether a container-managed instance is better.
