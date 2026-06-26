---
name: writing-agents
description: Use when creating or editing agent .md definitions, agent personas, tool allowlists, model tiers, dispatch triggers, or agent behavior constraints
---

# Writing Agents

## Overview

Writing agents turns a Genius specification into an executable role definition. An agent file must make dispatch conditions, persona, tool access, required skills, outputs, model tier, and hard boundaries explicit enough that Reviewer can test every behavioral claim.

**REQUIRED INPUT:** Start from a Genius specification. If scope, tool access, required skills, or success criteria are unclear, stop and return to Genius.

## Agent File Contract

Every `agents/*.md` file needs:

| Section | Required content |
|---------|------------------|
| Frontmatter | `name` and `description` only |
| Persona | Who the agent is, its role, and decision style |
| Required skills | Skills the agent must load before doing its work |
| Allowed tools | Explicit allowlist and write/read boundary |
| Model tier | Recommended model strength and escalation condition |
| Outputs | Concrete deliverables or verdict shape |
| Process | Ordered steps when sequence matters |
| MUST NOT | Hard prohibitions that define role boundaries |

## Frontmatter

Use exactly two fields:

```markdown
---
name: agent-name
description: Use when [specific dispatch trigger, not a role summary]
---
```

Rules:
- `name` uses lowercase letters, numbers, and hyphens.
- `description` starts with `Use when`.
- Describe when to dispatch the agent, not what the agent does.
- Do not include workflow summaries in the description.

## Body Pattern

```markdown
You are the [Name] - [one sentence persona and role].

Your job: [specific responsibility and handoff boundary].

**REQUIRED SKILL:** Use `agent-factory:skill-name` for [task type].

**Allowed tools:** [explicit tool list] - [write/read boundary].

**Recommended model:** [strongest|medium|medium-or-strong] - [when to escalate].

**Your outputs:** [concrete deliverables or verdict format].

**Your process:**
1. [Only include if order matters]

**You MUST NOT:**
- [Hard boundary]
- [Known rationalization to block]
```

## Tool Boundaries

Tool access is a behavioral claim, so write it directly:

| Agent type | Typical tools |
|------------|---------------|
| Designer | `Read, Glob, Grep, WebSearch` - no file write tools |
| Implementer | `Read, Write, Edit, Bash, Glob, Grep` - full file access within task scope |
| Reviewer | `Read, Glob, Grep, Bash, Agent` - no file write tools |

Do not rely on `MUST NOT create files` alone. Pair behavioral constraints with an explicit tool allowlist.

## Model Tiers

State the default model tier where the host supports routing:

| Agent role | Default tier | Escalate when |
|------------|--------------|---------------|
| Design | strongest | Always for new scope or architecture |
| Implementation | medium | Spec is ambiguous or cross-cutting |
| Validation | medium-or-strong | Behavior boundaries, pressure testing, or release gates are involved |

## Behavioral Claims

Write each claim so Reviewer can test it:
- "Always uses X before Y"
- "Never modifies files"
- "Returns PASS/FAIL in this format"
- "Escalates to Genius when the spec is unclear"

Avoid vague claims like "be careful", "ensure quality", or "produce good output".

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Description starts with role label | Start with the dispatch condition |
| Tool access is implied | Add an `Allowed tools` line |
| Agent can both design and implement | Split responsibilities or add a hard handoff |
| Model choice is undocumented | Add `Recommended model` with escalation condition |
| Output shape is vague | Specify exact sections, file paths, or verdict format |
| MUST NOT list is generic | Name concrete forbidden actions and rationalizations |
