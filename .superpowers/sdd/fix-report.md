# Cross-Reference Fix Report

## Files Changed

### 1. `skills/executing-plans/SKILL.md`
- Replaced "Superpowers works much better…" with "agent-factory works much better…" and removed broken `../using-superpowers/references/` path
- Replaced `superpowers:subagent-driven-development` with `agent-factory:subagent-driven-development` in the note
- Removed Step 3 reference to dropped `superpowers:finishing-a-development-branch`; replaced with prose about verifying tests and presenting options
- Removed Integration section entries for `superpowers:using-git-worktrees` and `superpowers:finishing-a-development-branch`; kept only `agent-factory:writing-plans`

### 2. `skills/subagent-driven-development/SKILL.md`
- Removed two flowchart nodes referencing `../requesting-code-review/code-reviewer.md` and `superpowers:finishing-a-development-branch`; replaced with generic prose nodes
- Removed broken path `../requesting-code-review/code-reviewer.md` from Prompt Templates section; replaced with prose description of final reviewer dispatch
- Removed Integration section entries for `superpowers:using-git-worktrees`, `superpowers:requesting-code-review`, and `superpowers:finishing-a-development-branch`
- Updated remaining `superpowers:` prefixes in Integration to `agent-factory:`

### 3. `skills/writing-skills/SKILL.md`
- Replaced line 12 cross-platform links to deleted `../using-superpowers/references/` files with: "**Personal skills live in your Claude Code skills directory.** See the Claude Code documentation for the path."
- Updated `superpowers:test-driven-development` references (lines 18, 283, 393) to `agent-factory:test-driven-development`
- Updated `superpowers:systematic-debugging` reference (line 284) to `agent-factory:systematic-debugging`

### 4. `skills/writing-plans/SKILL.md`
- Removed `superpowers:using-git-worktrees` reference; replaced with generic prose
- Updated `superpowers:subagent-driven-development` and `superpowers:executing-plans` references to `agent-factory:` prefix (3 occurrences)

### 5. `skills/systematic-debugging/SKILL.md`
- Updated `superpowers:test-driven-development` to `agent-factory:test-driven-development`
- Updated `superpowers:verification-before-completion` to `agent-factory:verification-before-completion`

### 6. `skills/writing-skills/testing-skills-with-subagents.md`
- Updated `superpowers:test-driven-development` to `agent-factory:test-driven-development`

## Grep Verification Output

```
$ grep -r "using-superpowers" skills/ --include="*.md"
(no output) — CLEAN

$ grep -r "requesting-code-review" skills/ --include="*.md"
(no output) — CLEAN

$ grep -r "using-git-worktrees" skills/ --include="*.md"
(no output) — CLEAN

$ grep -r "finishing-a-development-branch" skills/ --include="*.md"
(no output) — CLEAN
```

All four checks pass.
