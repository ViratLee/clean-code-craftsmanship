# Meaningful Names

## Reveal intent
A name must answer three questions: why it exists, what it does, how it is used.
If a name needs a comment to be understood, it is the wrong name.

```java
// bad
int d; // elapsed time in days

// good
int elapsedTimeInDays;
```

## Avoid disinformation
- Do not use platform or domain abbreviations that mean something else (`hp`, `aix`, `sco`).
- Do not put `List` in a name unless the thing is genuinely a `List`. Prefer
  `accountGroup`, `accounts`, or `bunchOfAccounts`.
- Beware near-identical names differing by one character in a long prefix.

## Avoid encodings
- No Hungarian notation (`strName`, `iCount`).
- No member prefixes (`m_description`). Modern IDEs make them noise.
- No interface prefix `I`. Prefer `ShapeFactory` as the interface and
  `ShapeFactoryImpl` as the implementation if you must distinguish.

## Scope correspondence
Name length should be proportional to scope size.

| Scope | Style | Example |
|---|---|---|
| 3–5 line loop | Single letter | `i`, `j` |
| Method-local | Short descriptive | `total`, `matched` |
| Class field | Descriptive | `pendingApprovals` |
| Public API / global | Long, searchable | `MAX_RETRY_ATTEMPTS_BEFORE_BACKOFF` |

Single-letter names and raw numeric literals are unsearchable. `MAX_CLASSES_PER_STUDENT`
can be grepped; `7` cannot.

## Domain lexicons
- Use **solution domain** names (CS terms) for technical concepts: `JobQueue`,
  `AccountVisitor`, `ObserverRegistry`. Programmers read these fluently.
- Use **problem domain** names when no technical equivalent exists: `Premium`,
  `Underwriter`, `PolicyRider`. Ask a domain expert what they call it.

## One word per concept
Pick one and stay consistent across the codebase: `fetch` / `retrieve` / `get` — choose
one. Same for `controller` / `manager` / `driver`.

## Class and method naming
- Classes: noun or noun phrase. Never a verb. Avoid `Manager`, `Processor`, `Data`, `Info`.
- Methods: verb or verb phrase. `postPayment`, `deletePage`, `save`.
- Accessors/mutators/predicates: `getName`, `setName`, `isPosted`.
- When a constructor is overloaded, prefer a static factory with a descriptive name:
  `Complex.fromRealNumber(23.0)` beats `new Complex(23.0)`.
