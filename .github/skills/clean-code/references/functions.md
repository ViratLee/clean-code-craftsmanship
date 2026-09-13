# Functions

## Small, then smaller
Target under 20 lines; 2–4 lines is the ideal. Indent depth of 1 or 2, never 3.
Blocks inside `if`, `else`, `while` should be one line long — and that line is
usually a function call with a descriptive name.

## Do one thing
A function does one thing if you cannot extract another function from it with a
name that is not merely a restatement of its own name.

Test: can you describe it in a `TO` paragraph without using "and"?
> TO RenderPageWithSetupsAndTeardowns, we check whether the page is a test page and
> if so include setups and teardowns. **and** — this function does two things.

## Single level of abstraction
Every statement in a function must live at the same conceptual altitude. Mixing
`getHtml()` (high) with `.append("\n")` (low) is a smell.

**Stepdown Rule**: the file reads top-down as a narrative. Each function is followed
by those at the next level down.

## Argument count

| Count | Name | Verdict |
|---|---|---|
| 0 | niladic | Ideal |
| 1 | monadic | Fine |
| 2 | dyadic | Acceptable with natural ordering (`Point(x, y)`) |
| 3 | triadic | Avoid |
| 4+ | polyadic | Requires an argument object or list |

**Never pass a boolean flag.** `render(true)` proves the function does two things.
Split it into `renderForSuite()` and `renderForSingleTest()`.

When arguments cluster, they are hiding a concept:
```java
Circle makeCircle(double x, double y, double radius);   // triadic
Circle makeCircle(Point center, double radius);         // dyadic + a new concept
```

## No side effects
A side effect is a lie: the name promises one thing and the body secretly does another.

```java
public boolean checkPassword(String user, String password) {
    // ...validates...
    Session.initialize();   // LIE. Now you cannot check a password without
                            // destroying the current session.
}
```
Rename to `checkPasswordAndInitializeSession` — or better, split it.

## Command Query Separation
A function either **does** something or **answers** something. Never both.

```java
// bad: does the reader know if it set, or if it tested for existence?
if (set("username", "unclebob")) { ... }

// good
if (attributeExists("username")) {
    setAttribute("username", "unclebob");
}
```

## Prefer exceptions to error codes
Error codes force the caller to handle the error immediately, producing deeply
nested `if` chains. Worse, an error-code enum becomes a dependency magnet — every
class that uses it recompiles when it changes.

```java
// bad
if (deletePage(page) == E_OK) {
    if (registry.deleteReference(page.name) == E_OK) { ... }
}

// good
try {
    deletePage(page);
    registry.deleteReference(page.name);
} catch (Exception e) {
    logger.log(e.getMessage());
}
```

Extract the `try`/`catch` bodies into their own functions — error handling is one
thing, so a function that handles errors should do nothing else.

Prefer **unchecked** exceptions. Checked exceptions violate the Open/Closed Principle:
a `throws` clause added at the bottom of the call stack forces edits all the way up.

## Never return null, never pass null
Returning `null` pushes the burden of a missing check onto every caller; a single
omission becomes an NPE at runtime.

- Return an **empty collection** instead of `null`.
- Return a **Special Case Object** (Null Object pattern) instead of `null`.
- For a genuinely optional single value, use `Optional<T>`.
- Passing `null` as an argument is worse — there is no good defense. Forbid it by
  convention and let it throw.

## DRY
Duplication is the root of most evil in software. Every duplicated block is a place
where a future fix will be applied in three of four locations.
