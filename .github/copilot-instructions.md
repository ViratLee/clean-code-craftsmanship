# Repository instructions

## Code quality baseline

This repository follows Clean Code conventions. Apply these on every change:

- Names reveal intent. If a name needs a comment, the name is wrong.
- Functions stay short, do one thing, and keep all statements at one level of abstraction.
- Two arguments maximum. Never pass a boolean flag to select behavior.
- A function either does something or answers something, never both.
- Signal errors with exceptions, not error codes. Never return or pass `null`.
- Do not comment bad code — rewrite it. Delete commented-out code; Git remembers it.
- Behavior changes and refactoring go in separate commits.

For anything beyond this baseline — a refactoring plan, a code review, a naming
argument, a class-design question, the full smell catalogue — use the `clean-code`
skill in `.github/skills/clean-code/` and read its reference files.

When the existing code in this repository contradicts a rule above, follow the
surrounding convention and raise the conflict once instead of rewriting unrelated code.
