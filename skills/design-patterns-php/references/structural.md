# Structural Patterns — Detailed Notes (PHP)

## Adapter

**Roles**  
- Target (interface expected by client)  
- Adaptee (existing class with incompatible interface)  
- Adapter (implements Target, wraps Adaptee)  

**PHP tips**  
- Object Adapter (composition) is preferred over Class Adapter (inheritance) in PHP.  
- Common for third-party libraries, legacy code, or different HTTP clients / storage drivers.  
- Can also adapt functions via a thin class wrapper.

## Bridge

**Roles**  
- Abstraction  
- RefinedAbstraction  
- Implementor (interface)  
- ConcreteImplementor  

**PHP tips**  
- Keep the Abstraction hierarchy independent of the Implementor hierarchy.  
- Useful for cross-platform UI, different rendering engines, or storage backends that evolve separately.  
- Often confused with Adapter; Bridge is designed up-front for independent variation.

## Composite

**Roles**  
- Component (common interface)  
- Leaf  
- Composite (holds children, implements Component)  

**PHP tips**  
- Implement recursive operations (e.g. `render()`, `getPrice()`, `execute()`).  
- PHP arrays or SplObjectStorage work well for children.  
- Classic for DOM-like structures, file systems, menus, organization charts, form field groups.

## Decorator

**Roles**  
- Component (interface)  
- ConcreteComponent  
- Decorator (implements Component, holds a Component)  
- ConcreteDecorators  

**PHP tips**  
- Decorators usually forward calls and add behavior before/after.  
- Stackable at runtime — very powerful for middleware, logging, caching, validation layers.  
- Prefer over deep inheritance trees.  
- PSR-15 middleware is a real-world Decorator/Chain hybrid.

## Facade

**Roles**  
- Facade (simple interface)  
- Subsystem classes (complex)  

**PHP tips**  
- Facade does not prevent direct access to the subsystem; it merely simplifies the common path.  
- Often a good candidate for a service in a DI container.  
- Useful for complex libraries (image processing, payment gateways, email systems).

## Flyweight

**Roles**  
- Flyweight (shares intrinsic state)  
- ConcreteFlyweight  
- FlyweightFactory (caches/reuses)  
- Client (supplies extrinsic state)  

**PHP tips**  
- Intrinsic state is immutable and shared; extrinsic is passed in.  
- Useful for text rendering (characters), game entities with many similar objects, icon caches.  
- In PHP, watch memory carefully; the pattern shines when object count is high and intrinsic data is large.

## Proxy

**Roles**  
- Subject (interface)  
- RealSubject  
- Proxy (implements Subject, controls access to RealSubject)  

**Variants**  
- Virtual (lazy init)  
- Protection (access control)  
- Remote (network)  
- Smart reference (logging, reference counting, caching)

**PHP tips**  
- Lazy-loading proxies are common for Doctrine entities or expensive services.  
- Can implement the same interface and be injected transparently.  
- Combine with `__call` for dynamic proxies if needed, but prefer explicit interfaces.
