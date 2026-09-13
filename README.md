# clean code craftsmanship
code review, refactor base on code quality baseline, follows Clean Code conventions. 

## Verifying the skill works

Validation tools only confirm the file is well-formed. The test that actually
matters is whether Copilot loads and applies the skill.

1. Open the repository in VS Code.
2. Switch Copilot Chat to **agent mode**.
3. Open a file that contains a real code smell — a long function, a boolean flag
   argument, a train wreck call chain.
4. Send: `Review this function for code smells.`

**Loaded correctly:** the response names specific rules and smell identifiers
(for example G5 Duplication, F3 Flag arguments) and follows the three-part output
format defined in the skill — smells identified, refactored version, refactoring
steps.

**Not loaded:** the response gives generic readability advice with no rule names
and no defined structure.

If the skill does not trigger, check that your VS Code version supports agent
skills, and that the file path is exactly `.github/skills/clean-code/SKILL.md`
with `SKILL.md` in uppercase.
