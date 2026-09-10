# Design Patterns Catalog (PHP)

Short reference of Intent + primary Applicability. Use the category files for deeper structure, pros/cons, and PHP notes.

## Creational

**Abstract Factory**  
Intent: Produce families of related objects without specifying concrete classes.  
Use when: Multiple product families must stay consistent (e.g. UI themes, DB drivers).

**Builder**  
Intent: Construct complex objects step by step; same construction process can yield different representations.  
Use when: Object has many optional parts or construction is multi-stage.

**Factory Method**  
Intent: Interface for creating objects in a superclass; subclasses decide the concrete type.  
Use when: A class cannot anticipate the exact class of objects it must create, or you want to delegate creation to subclasses.

**Prototype**  
Intent: Copy existing objects without depending on their concrete classes.  
Use when: Creating an object is expensive or the system should be independent of how products are created/composed.

**Singleton**  
Intent: Ensure a class has only one instance and provide a global access point.  
Use when: Exactly one instance must coordinate actions across the system (prefer DI containers in modern PHP).

## Structural

**Adapter**  
Intent: Convert the interface of a class into another interface clients expect.  
Use when: You need to reuse an existing class whose interface does not match what you need.

**Bridge**  
Intent: Decouple an abstraction from its implementation so the two can vary independently.  
Use when: You want to avoid a permanent binding between abstraction and implementation, or both hierarchies should be extensible.

**Composite**  
Intent: Compose objects into tree structures and treat individual objects and compositions uniformly.  
Use when: Clients should ignore the difference between compositions and individual objects.

**Decorator**  
Intent: Attach additional responsibilities to an object dynamically.  
Use when: You need flexible, runtime extension of behavior without subclass explosion.

**Facade**  
Intent: Provide a simplified interface to a complex subsystem.  
Use when: You want to hide complexity of a library/framework or reduce coupling to many classes.

**Flyweight**  
Intent: Share fine-grained objects to support large numbers of them efficiently.  
Use when: An application uses a large number of objects that can share intrinsic state.

**Proxy**  
Intent: Provide a surrogate or placeholder for another object to control access.  
Use when: You need lazy loading, access control, logging, or remote representation.

## Behavioral

**Chain of Responsibility**  
Intent: Pass a request along a chain of handlers until one handles it.  
Use when: Multiple objects may handle a request and the handler is not known a priori (e.g. middleware).

**Command**  
Intent: Encapsulate a request as an object, thereby allowing parameterization, queuing, logging, and undo.  
Use when: You need to issue requests without knowing the receiver or support undoable operations.

**Iterator**  
Intent: Provide a way to access elements of an aggregate sequentially without exposing its representation.  
Use when: You need a uniform way to traverse different collection structures.

**Mediator**  
Intent: Define an object that encapsulates how a set of objects interact; promote loose coupling.  
Use when: Communication between objects is complex and you want to centralize control.

**Memento**  
Intent: Capture and externalize an object’s internal state so it can be restored later without violating encapsulation.  
Use when: You need snapshots for undo or state recovery.

**Observer**  
Intent: Define a one-to-many dependency so that when one object changes state, all dependents are notified.  
Use when: An object’s change requires updating multiple others without tight coupling (events, pub-sub).

**State**  
Intent: Allow an object to alter its behavior when its internal state changes; the object will appear to change its class.  
Use when: Behavior depends on state and there are many conditional statements.

**Strategy**  
Intent: Define a family of algorithms, encapsulate each one, and make them interchangeable.  
Use when: You need to switch algorithms at runtime or avoid multiple conditional branches.

**Template Method**  
Intent: Define the skeleton of an algorithm in a method, deferring some steps to subclasses.  
Use when: You want to let subclasses redefine certain steps of an algorithm without changing its structure.

**Visitor**  
Intent: Represent an operation to be performed on elements of an object structure; let you define new operations without changing the classes.  
Use when: You need to perform many unrelated operations on a stable object structure.
