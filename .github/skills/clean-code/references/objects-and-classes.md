# Objects, Data Structures, Classes and Systems

## The data/object anti-symmetry

|  | Objects | Data Structures |
|---|---|---|
| Expose | Behavior (functions) | Data (fields) |
| Hide | Data | Nothing |
| Easy to add | New **types** without touching existing functions | New **functions** without touching existing types |
| Hard to add | New functions (every subclass must change) | New types (every function must change) |

Choose deliberately. Procedural code (data structures + free functions) is the right
answer when you expect new operations. OO is right when you expect new variants.

**Hybrids are the worst of both.** A class with public getters/setters for every field
plus business methods is a data structure pretending to be an object. Pick a side.

## Law of Demeter
A method `f` of class `C` may only call methods of:
- `C` itself
- objects `f` created
- objects passed as arguments to `f`
- objects held in instance variables of `C`

It may **not** call methods on objects returned by any of the above.

```java
// train wreck — knows about three levels of structure it does not own
String outDir = ctxt.getOptions().getScratchDir().getAbsolutePath();

// tell, don't ask
BufferedOutputStream bos = ctxt.createScratchFileStream(fileName);
```

Note: if `ctxt`, `Options`, and `ScratchDir` are plain **data structures** with public
fields, Demeter does not apply — data structures have no behavior to hide.

## Single Responsibility Principle
A class has one, and only one, reason to change.

Practical test: write the class's responsibility in ~25 words without using "and",
"or", "if", or "but". If you cannot, split it.

If a class name contains `Manager`, `Processor`, `Super`, or `Data`, it is probably
doing too much.

Prefer many small, single-purpose classes over a few large ones. A system with many
small classes has no more moving parts than a system with a few large ones — but the
developer only has to understand the ones relevant to the task.

## Cohesion
A class is cohesive when its methods manipulate most of its instance variables.
Maximal cohesion means every method touches every field.

**Falling cohesion is a signal to split.** When a few methods use only a subset of
fields, extract that subset plus those methods into a new class. This usually
happens naturally as you break long functions into smaller ones and promote their
locals to fields.

## Isolate from change (DIP)
Depend on abstractions, not concretions.

```java
// coupled to a concrete, untestable API
public class Portfolio {
    private TokyoStockExchange exchange = new TokyoStockExchange();
}

// depends on an interface — now testable with a fake
public class Portfolio {
    private StockExchange exchange;
    public Portfolio(StockExchange exchange) { this.exchange = exchange; }
}
```

The test for good design: can you test it without a network, a database, or a clock?

## Separate construction from use
Object construction and wiring is a concern of its own. Do not scatter it through
runtime logic.

**Anti-pattern — lazy initialization inline:**
```java
public Service getService() {
    if (service == null)
        service = new MyServiceImpl(...);   // hard-coded dependency, untestable,
    return service;                          // SRP violated, not thread-safe
}
```

Preferred approaches:
- **`main` separation**: build the whole object graph in `main`, hand the finished
  graph to the application, which never knows how anything was constructed.
- **Abstract Factory**: when the application must control *when* an object is
  created but not *how*.
- **Dependency Injection**: the container wires it. In Spring Boot this is
  constructor injection — prefer it over field `@Autowired`, because a constructor
  makes the dependency mandatory, final, and visible to a plain unit test.

## Growing systems
Software systems cannot be designed correctly up front. They grow. Clean separation
of concerns at every level is what makes incremental growth possible — architecture
that must be right on day one is architecture that will be wrong on day two.

Standards and frameworks are useful only when they demonstrably earn their keep.
Do not adopt an abstraction before the pain it solves exists.
