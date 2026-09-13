# Formatting, Comments, and Code Smells

## Vertical formatting

- **File size**: 200 lines typical, 500 lines maximum. Small files are easier to
  understand than large ones — this is not merely aesthetic.
- **Newspaper metaphor**: the top of the file is the headline — high-level concepts
  in broad strokes. Detail increases as you scroll down.
- **Vertical openness**: blank lines separate distinct concepts. Package declaration,
  imports, and each function are separated by a blank line.
- **Vertical density**: lines that are tightly related sit together with no blank
  line between them. Do not let comments break up a dense block.
- **Vertical distance**: concepts that are closely related should be vertically
  close. Never force the reader to jump between files or scroll across a class.
- **Declarations**: local variables declared immediately before first use. Instance
  variables at the top of the class. Loop control variables inside the loop statement.
- **Dependent functions**: caller above callee, so the code flows downward.

## Horizontal formatting

- Line width 100–120 characters. Never require horizontal scrolling.
- **Do not align declarations or assignments vertically.** Alignment emphasizes the
  wrong thing — the reader's eye follows the column of names or values instead of the
  type/name pairing. And if the list is long enough to need alignment, the list is the
  problem, not the formatting.
- Use spaces to associate weakly and omit them to associate strongly:
  `b*b - 4*a*c` reads correctly; `b * b - 4 * a * c` does not.
- Never break indentation, even for one-line `if` bodies or short `while` loops.

## Team rules
A team agrees one formatting style and encodes it in the IDE formatter. A codebase
with several individual styles is harder to read than any single style, including a
bad one.

## Comments

Comments are, at best, a necessary evil. They compensate for our failure to express
ourselves in code. Every comment is a small defeat.

**Do not comment bad code — rewrite it.**

Comments lie. Code moves, comments do not. An inaccurate comment is worse than no
comment: it is misinformation the reader will believe.

### Acceptable comments
- Legal headers required by the organization
- Explanation of intent behind a non-obvious decision ("we sort here because the
  downstream API requires ordering it does not document")
- Warning of consequences ("this test takes 8 minutes — don't run it casually")
- `TODO` with a tracked owner
- Amplification of the importance of something that looks trivial
- Public API Javadoc

### Bad comments
- Redundant: `// the day of the month` above `private int dayOfMonth;`
- Mandated: Javadoc on every private method because a rule says so
- Journal comments: a changelog at the top of the file. Git exists.
- Noise: `/** Default constructor */`
- Position markers: `//////////// ACTIONS ////////////`
- Closing brace comments: `} // end while` — the function is too long instead
- Attributions: `/* Added by Rick */` — Git exists
- **Commented-out code**: delete it. Everyone else assumes it is important and leaves
  it there forever. Git remembers it.
- HTML in comments, non-local information, too much information

## Smell catalogue

### Environment
- **E1** Build requires more than one step. `git clone && ./build` must be enough.
- **E2** Tests require more than one command to run.

### Functions
- **F1** Too many arguments
- **F2** Output arguments (a function that mutates an argument instead of `this`)
- **F3** Flag arguments
- **F4** Dead function — nobody calls it. Delete it.

### General
- **G5** Duplication — the single most important smell. Every time you see it, an
  abstraction is missing.
- **G6** Code at the wrong level of abstraction — a low-level detail exposed on a
  high-level interface
- **G8** Too much information — a well-defined module has a tiny interface
- **G9** Dead code — unreachable branches, `catch` blocks that never fire
- **G10** Vertical separation — variables declared far from use
- **G11** Inconsistency — same concept named differently in two places
- **G12** Clutter — empty constructors, unused variables, meaningless comments
- **G14** Feature envy — a method more interested in another class's data than its own
- **G15** Selector arguments — see F3
- **G16** Obscured intent — dense, compressed, "clever" expressions
- **G19** Use explanatory variables — break a calculation into named intermediates
- **G20** Function names should say what they do
- **G23** Prefer polymorphism to `if`/`else` and `switch` chains
- **G25** Replace magic numbers with named constants
- **G28** Encapsulate conditionals — `if (shouldBeDeleted(timer))` not a raw boolean expr
- **G29** Avoid negative conditionals — `if (buffer.shouldCompact())` beats `if (!buffer.shouldNotCompact())`
- **G30** Functions should do one thing
- **G31** Hidden temporal couplings — if calls must happen in order, make the order
  structurally enforced by passing each result into the next call
- **G34** Functions should descend only one level of abstraction
- **G36** Avoid transitive navigation (Law of Demeter)

### Names
- **N1** Choose descriptive names
- **N2** Choose names at the appropriate level of abstraction
- **N4** Unambiguous names
- **N5** Long names for long scopes
- **N6** Avoid encodings
- **N7** Names should describe side effects — `createOrReturnOos()` not `getOos()`

### Tests
- **T1** Insufficient tests — test everything that could possibly break
- **T2** Use a coverage tool
- **T3** Don't skip trivial tests
- **T5** Test boundary conditions
- **T6** Exhaustively test near bugs — bugs cluster
- **T9** Tests should be fast
