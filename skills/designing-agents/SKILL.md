---
name: designing-agents
description: Use when defining or changing a agent's purpose, persona, tool constraints, or success criteria before any implementation files are written
---

# Designing Agents

## Overview

An agent definition needs four elements before Creator touches a file: **scope**, **persona**, **tool constraints**, **success criteria**. Missing any one produces an agent that drifts under pressure.

Use `agent-factory:designing-skills` instead when designing a skill's trigger conditions, output shape, bundled resources, or behavioral claims.

## Design Checklist

**Scope:**
- One-sentence summary of what this agent does
- Trigger list: specific scenarios that dispatch this agent
- Hard limits: what it must never do (explicit, not implied)

**Persona:**
- Name and role label
- Voice/attitude (meticulous, creative, critical)
- Decision-making style (ask vs act, conservative vs bold)

**Tool constraints:**
- List allowed tools explicitly - start restrictive, add only what's needed
- State file-write permission clearly: read-only vs full access
- One-line rationale for any non-obvious restriction

**Success criteria:**
- Observable outputs: "produces X", "modifies Y"
- Behavioral constraints: "never does Z under any pressure"
- Written so Reviewer can assert them without ambiguity

## Agent File Structure

```markdown
---
name: agent-name
description: Use when [specific triggering conditions - not what it does]
---

[Persona statement: 1-2 sentences, who this is and primary job]

**REQUIRED SKILL:** Use `plugin:relevant-skill` for [specific task type]

**Your outputs:** [list of deliverables]

**You MUST NOT:** [hard constraints - specific, not generic]
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Overlapping scope with another agent | Draw explicit boundary line between them |
| Tool list too broad | Start with read-only; add write only when design requires it |
| Missing "MUST NOT" constraints | Every agent needs explicit prohibitions or it will improvise |
| Vague success criteria | Write it as Reviewer will test it: observable, falsifiable |
| Description summarizes what the agent does | Description must say WHEN to dispatch it, not what it does |
| Designing a skill with this checklist | Use `agent-factory:designing-skills` for skill trigger, output, resource, and behavior design |
