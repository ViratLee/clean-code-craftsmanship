# Agent skills

Skills in this directory are loaded automatically by GitHub Copilot when relevant to
the task. They also work with Claude Code and other tools that implement the
Agent Skills specification.

## Contents

| Skill | Purpose |
|---|---|
| `clean-code` | Refactoring, code review, and authoring against Clean Code principles |

## Where skills are picked up

| Location | Scope |
|---|---|
| `.github/skills/` | This repository, for everyone who clones it |
| `.claude/skills/` | Also read by Copilot; use if the repo already targets Claude Code |
| `~/.copilot/skills/` | Your machine only, across all projects |

## Verifying a skill loads

Requires GitHub CLI 2.90.0 or later.

```bash
gh skill publish --dry-run     # validate against the Agent Skills specification
gh skill publish --fix         # auto-fix metadata issues without publishing
```

In Copilot CLI:

```
/skills reload
/skills info clean-code
```

In VS Code agent mode, open a file with a real smell and ask for a review. If the
skill loaded, the response names specific rules rather than giving generic advice.

## Surfaces

Skills work with Copilot cloud agent, Copilot code review, Copilot CLI, the GitHub
Copilot app, and agent mode in VS Code. They do not apply to inline code completion.
The baseline rules in `.github/copilot-instructions.md` cover that gap.
