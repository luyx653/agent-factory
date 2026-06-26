# agent-factory Plugin Design

**Date:** 2026-06-26  
**Status:** Approved

## Overview

A new standalone Claude Code plugin ported selectively from superpowers, specialized for authoring agents, skills, and AI workflow definitions. Primary use case is a creation tool (writing/modifying `.md`, `SKILL.md`, `plugin.json` files), not runtime orchestration.

Skills auto-sense when to trigger via description matching — no explicit slash commands required.

---

## Architecture

```
agent-factory plugin
│
├── Session-start hook
│     └── Injects using-agent-factory skill into every conversation
│
├── Skills (auto-sense layer)
│     ├── 10 ported from superpowers (minimal adaptation)
│     └── 3 new domain-specific skills
│
└── Agents (role execution layer)
      ├── genius.md   — Design: scope, persona, system prompt structure
      ├── creator.md  — Implementation: write/edit skill and agent files
      └── reviewer.md — Validation: behavior testing, quality gates
```

Skills hold workflow logic. Agents hold role constraints and tool boundaries. No overlap.

---

## Agent Roles

### Genius (设计师)
- **Purpose:** All "from nothing" design decisions
- **Tool access:** Read, Glob, Grep, WebSearch — no file writes
- **Primary skills:** `brainstorming`, `designing-agents`, `writing-plans`
- **Triggers:** Designing a new agent/skill, architectural decisions, choosing between approaches

### Creator (工匠)
- **Purpose:** All file creation and modification
- **Tool access:** Full (Read, Write, Edit, Bash, Glob, Grep)
- **Primary skills:** `writing-agents`, `writing-skills`, `executing-plans`, `test-driven-development`, `subagent-driven-development`
- **Triggers:** Implementing a design spec, writing SKILL.md/agent .md files, executing an implementation plan

### Reviewer (验收员)
- **Purpose:** All validation, testing, quality review
- **Tool access:** Read, Glob, Grep, Bash (read-only) — no file creation or modification
- **Primary skills:** `evaluating-agent-behavior`, `verification-before-completion`, `systematic-debugging`
- **Triggers:** Post-implementation skill validation, agent behavior diverges from spec, pre-deployment quality gate

### Role Boundaries

| Action | Genius | Creator | Reviewer |
|--------|--------|---------|----------|
| Read files | ✓ | ✓ | ✓ |
| Create / modify files | ✗ | ✓ | ✗ |
| Run shell commands | ✗ | ✓ | ✓ (read-only) |
| Design decisions | ✓ | ✗ | ✗ |
| Output test verdicts | ✗ | ✗ | ✓ |

Only one agent active at a time. Genius output is Creator input. Reviewer triggers only after Creator completes.

---

## Skill Set

### Ported from superpowers (10)

| Skill | Adaptation |
|-------|-----------|
| `using-superpowers` | **Rewritten** as `using-agent-factory` — new entry point describing three-agent dispatch logic |
| `brainstorming` | No changes |
| `writing-plans` | No changes |
| `writing-skills` | No changes |
| `test-driven-development` | No changes |
| `systematic-debugging` | No changes |
| `subagent-driven-development` | No changes |
| `dispatching-parallel-agents` | No changes |
| `verification-before-completion` | No changes |
| `executing-plans` | No changes |

### New (3)

**`designing-agents`** — Genius primary  
- Agent persona design, scope definition, tool boundary decisions  
- System prompt structure templates  
- Triggers when user wants to define a new agent

**writing-agents** — Creator primary  
- Agent .md authoring, persona implementation, tool allowlists, model tiers, output contracts  
- Triggers when writing or editing agent definitions

**evaluating-agent-behavior** — Reviewer primary  
- Pressure test scenario design methodology  
- Behavioral assertions for agent outputs  
- Triggers at post-implementation validation stage or when agent behavior diverges from spec

### Dropped from superpowers (4)

`finishing-a-development-branch`, `receiving-code-review`, `requesting-code-review`, `using-git-worktrees` — generic dev workflow, not specific to agent/skill authoring domain.

---

## Invocation Pattern

```
User conversation
  → skill description matches current intent
  → skill decides: handle directly OR delegate to specialist agent
  → agent takes over scoped task, result returns to main conversation
```

Typical full flow (writing a new skill):
```
User: "Write me a designing-agents skill"
  → brainstorming triggers → Genius designs spec
  → writing-plans triggers
  → writing-skills triggers → Creator implements SKILL.md
  → verification-before-completion triggers → Reviewer runs pressure tests
  → tests pass → Creator commits
```

---

## File Structure

```
agent-factory/
├── .claude-plugin/
│   └── plugin.json              # Plugin metadata (name: agent-factory)
├── hooks/
│   ├── hooks.json               # SessionStart hook config
│   ├── session-start            # Injects using-agent-factory skill content
│   └── run-hook.cmd             # Windows/Unix polyglot hook runner
├── agents/
│   ├── genius.md                # Designer persona + read-only tool constraints
│   ├── creator.md               # Craftsman persona + full tool access
│   └── reviewer.md              # Validator persona + read-only constraints
├── package.json                 # Package metadata
└── skills/
    ├── using-agent-factory/
    │   └── SKILL.md             # Entry point (rewritten from using-superpowers)
    ├── brainstorming/           # Ported (with scripts/)
    ├── writing-plans/           # Ported
    ├── writing-skills/          # Ported (with supporting files)
    ├── writing-agents/          # New
    ├── test-driven-development/ # Ported
    ├── systematic-debugging/    # Ported (with supporting files)
    ├── subagent-driven-development/ # Ported (with scripts/)
    ├── dispatching-parallel-agents/ # Ported
    ├── verification-before-completion/ # Ported
    ├── executing-plans/         # Ported
    ├── designing-agents/
    │   └── SKILL.md             # New
    └── evaluating-agent-behavior/
        └── SKILL.md             # New
```

---

## Files to Port from superpowers

| File | Source | Changes |
|------|--------|---------|
| `.claude-plugin/plugin.json` | superpowers | name, description, keywords |
| `hooks/hooks.json` | superpowers | None |
| `hooks/session-start` | superpowers | Line 11, 27: `using-superpowers` → `using-agent-factory` |
| `hooks/run-hook.cmd` | superpowers | None |
| `package.json` | superpowers | name, description, keywords; remove opencode/pi fields |

## Skills Directory Cleanup

| Action | Target |
|--------|--------|
| Delete | `skills/finishing-a-development-branch/` |
| Delete | `skills/receiving-code-review/` |
| Delete | `skills/requesting-code-review/` |
| Delete | `skills/using-git-worktrees/` |
| Rename + rewrite | `skills/using-superpowers/` → `skills/using-agent-factory/` |
