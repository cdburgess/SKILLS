# Behavioral Patterns — Detailed Notes (PHP)

## Chain of Responsibility

**Roles**  
- Handler (interface with `setNext` / `handle`)  
- ConcreteHandlers  
- Client  

**PHP tips**  
- Each handler decides to process or pass to the next.  
- PSR-15 Request Handlers / Middleware are the canonical PHP example.  
- Build the chain in configuration or a factory; keep it linear and ordered.

## Command

**Roles**  
- Command (interface with `execute()`)  
- ConcreteCommand  
- Receiver  
- Invoker  
- Client  

**PHP tips**  
- Excellent for undo/redo (store history of commands), job queues, GUI actions, CLI commands.  
- In modern PHP, often mapped to message buses or Symfony Messenger / Laravel Jobs.  
- Keep commands immutable when possible.

## Iterator

**Roles**  
- Iterator (interface: `current`, `next`, `key`, `valid`, `rewind`)  
- Aggregate / Collection  
- ConcreteIterator  

**PHP tips**  
- PHP already provides `Iterator`, `IteratorAggregate`, `Traversable`, and `foreach` support.  
- Prefer implementing `IteratorAggregate` + `getIterator()` for most collections.  
- Use when you need custom traversal (tree walkers, filtered iterators, etc.).

## Mediator

**Roles**  
- Mediator (interface)  
- ConcreteMediator  
- Colleague components  

**PHP tips**  
- Components talk only to the mediator, never to each other.  
- Useful for complex dialogs, chat rooms, UI form coordination, or event buses with restricted visibility.  
- Can evolve into a full event dispatcher; keep the distinction clear if the goal is reduced coupling.

## Memento

**Roles**  
- Originator  
- Memento (opaque snapshot)  
- Caretaker  

**PHP tips**  
- Memento should be immutable and only readable by the Originator.  
- In PHP, a simple value object or serialized state works; avoid exposing internal fields.  
- Classic for undo stacks, transactional state, or game save points. Real-world PHP examples are less common than in other languages.

## Observer

**Roles**  
- Subject / Publisher  
- Observer / Subscriber  
- ConcreteObservers  

**PHP tips**  
- PHP’s `SplSubject` / `SplObserver` exist but most codebases use event systems.  
- Prefer a dedicated Event Dispatcher (PSR-14) in frameworks.  
- Be careful with memory leaks from long-lived observers; provide unsubscribe.

## State

**Roles**  
- Context  
- State (interface)  
- ConcreteStates  

**PHP tips**  
- Context holds a current State object and delegates behavior.  
- States can transition the Context to a new state.  
- Replaces large switch/if chains on status fields (order lifecycle, document workflow, connection states).

## Strategy

**Roles**  
- Strategy (interface)  
- ConcreteStrategies  
- Context  

**PHP tips**  
- Inject the strategy (or set it at runtime).  
- Extremely common: sorting, payment methods, tax calculation, compression algorithms, validation rules.  
- Prefer over inheritance when the variation is algorithmic rather than structural.

## Template Method

**Roles**  
- AbstractClass (template method + hooks)  
- ConcreteClass (implements the primitive operations)

**PHP tips**  
- The template method is usually `final` so the skeleton cannot be changed.  
- Provide both abstract methods (must override) and hook methods (optional override).  
- Frequent in frameworks for lifecycle hooks (controllers, form handlers, test cases).

## Visitor

**Roles**  
- Visitor (interface with `visitConcreteElementA`, etc.)  
- ConcreteVisitors  
- Element (accepts visitor)  
- ConcreteElements  
- ObjectStructure  

**PHP tips**  
- Double-dispatch via `accept(Visitor $v)` calling `$v->visit($this)`.  
- Adds new operations without modifying the element classes — useful when the element hierarchy is stable.  
- Can become cumbersome if the element hierarchy changes often.  
- Less common in everyday PHP than in compilers or document processors, but powerful when needed.
