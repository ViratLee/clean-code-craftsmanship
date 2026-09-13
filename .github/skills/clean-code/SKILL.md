---
name: clean-code
description: Refactor, review, and write code following Clean Code and Software Craftsmanship principles (Robert C. Martin). Use this skill whenever asked to refactor, clean up, review, or improve existing code; when asked "is this code good?" or "how would you improve this?"; or when code is described as messy, hard to read, hard to test, or hard to maintain. Also use when the request mentions code smells, technical debt, legacy code, SOLID, SRP, DRY, Law of Demeter, TDD, long functions, god classes, deep nesting, magic numbers, or naming conventions, even without the words "clean code". Apply proactively when writing new production code another developer will maintain. Do NOT use for throwaway scripts, one-off data exploration, prototypes stated to be disposable, or pure performance optimization where readability is explicitly traded away.
license: See LICENSE-NOTICE.md in this directory.
---

# Clean Code Craftsmanship

Writing clean code is a professional obligation, not a luxury. The only way to go
fast is to keep the code clean at all times.

## Decide the mode first

| User intent | Mode | What to do |
|---|---|---|
| "refactor / clean this up" | **Refactor** | Follow the Successive Refinement Cycle below, output the 3-part format |
| "review this code" | **Review** | List smells by severity, do not rewrite unless asked |
| "write X for me" | **Author** | Write it clean the first time; apply the checklist before returning |
| "why is this bad?" | **Explain** | Name the specific rule violated, show the minimal fix |

In Copilot code review, default to **Review** mode: comment on smells at the lines
where they occur and do not rewrite whole files in a review comment.

## The Successive Refinement Cycle (Refactor mode)

1. **Confirm test coverage.** If there are no tests covering the code, say so and
   write characterization tests FIRST. Refactoring without tests is not refactoring,
   it is rewriting and hoping.
2. **Rough draft is allowed.** Getting it to work comes first. The initial version
   may be clumsy and duplicated — that is expected, not a failure.
3. **Refactor mercilessly.** Make tiny, behavior-preserving changes one at a time.
   Never mix a behavior change into a refactoring step.
4. **Boy Scout Rule.** Leave the code cleaner than you found it, every time.

## Test-Driven Development (Author mode)

Apply the three laws in strict order when the user is doing TDD or asks for tests first:

1. Write no production code until a failing unit test exists.
2. Write no more of a test than is sufficient to fail (not compiling counts as failing).
3. Write no more production code than is sufficient to pass the current failing test.

## Pre-return checklist

Run this against every piece of code before returning it. If any item fails, fix it
or explicitly justify the exception to the user.

- [ ] Every name reveals intent without needing a comment
- [ ] No function longer than ~20 lines; indent depth ≤ 2
- [ ] Every function does one thing at one level of abstraction
- [ ] Arguments ≤ 2; zero boolean flag arguments
- [ ] No function both mutates state and returns a value (Command Query Separation)
- [ ] Errors thrown as exceptions, never returned as codes
- [ ] No `null` returned, no `null` passed
- [ ] No train wrecks (`a.getB().getC().getD()`)
- [ ] Each class has exactly one reason to change
- [ ] Construction is separated from use
- [ ] Tests are Fast, Independent, Repeatable, Self-Validating, Timely
- [ ] No commented-out code, no obsolete comments

## Output format (Refactor and Explain modes)

Always produce these three sections in this order:

### 1. Original — smells identified
Quote the offending code, then list each smell with its rule name.

### 2. Refactored
The clean version. Complete and compilable, not a fragment.

### 3. Refactoring steps
Numbered, one step per transformation, each justified by the rule that demanded it.
The reader should be able to replay the steps by hand.

**Worked example:**

Original:
```java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65)) {
```

Refactored:
```java
if (employee.isEligibleForFullBenefits()) {
```

Step: the comment existed only to explain the conditional — a sign the code failed to
express itself. Extracting the predicate into an intention-revealing method on
`Employee` makes the comment redundant and moves the rule to the class that owns the data.

## Reference files

Read the relevant file from this skill's directory when the task touches that area.
Do not read all of them.

| File | Read when |
|---|---|
| `references/naming.md` | Renaming, reviewing identifiers, arguing about a name |
| `references/functions.md` | Functions too long, too many args, nested, side effects |
| `references/objects-and-classes.md` | Class design, SRP, cohesion, Law of Demeter, DI, architecture |
| `references/formatting-and-smells.md` | File layout, vertical/horizontal formatting, comment smells, the full smell catalogue |

Test-specific guidance (F.I.R.S.T., TDD discipline, test naming) is not yet covered by
a reference file. The smell catalogue in `references/formatting-and-smells.md` has the
T1–T9 test smells; apply the checklist above for everything else and say when a
question goes beyond what these files cover.

## Non-negotiables

- Never add a comment to compensate for unclear code — fix the code instead.
- Never change behavior and structure in the same step.
- If the user's existing style contradicts a rule here, follow the codebase's
  convention and mention the conflict once. Consistency beats purity.
