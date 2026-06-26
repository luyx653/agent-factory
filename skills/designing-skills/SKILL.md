---
name: designing-skills
description: Use when defining or changing a skill's trigger conditions, output shape, behavioral claims, bundled resources, or validation criteria before editing SKILL.md
---

# Designing Skills

## Overview

A skill design defines when the skill should load, what it should produce, and which behavioral claims Reviewer can test. Do not use agent-only fields such as persona or tool constraints for skill design.

## Design Checklist

**Scope:**
- One-sentence summary of what this skill helps an agent do.
- Explicit non-goals: what this skill must not cover.
- Handoff boundary: which follow-up skill implements or validates the design.

**Trigger conditions:**
- Specific task types, symptoms, or artifacts that should trigger the skill.
- Upstream state required before the skill applies.
- Similar tasks that should not trigger this skill.

**Description field:**
- Start with `Use when...`.
- Describe triggering conditions only, not the workflow.
- Include concrete keywords an agent would search for.
- Keep it short enough to scan in the skill list.

**Output shape:**
- State whether the skill produces a decision, plan, file edit, verdict, checklist, or workflow.
- Name required sections or formats when consistency matters.
- Identify bundled resources only when they are needed.

**Behavioral claims:**
- Write observable `must` and `must not` claims.
- Include negative constraints for common shortcuts.
- Make each claim testable without interpreting intent.

**Reviewer checks:**
- List pressure scenarios that should pass.
- Include at least one non-trigger scenario.
- Define what counts as PASS or FAIL for each behavioral claim.

## Skill Spec Format

````markdown
## Skill Design Spec

**Scope**
- This skill does:
- This skill does not:

**Trigger Conditions**
- Use when:
- Do not use when:

**Description**
```yaml
description: Use when ...
```

**Output Shape**
- Required sections:
- Expected final artifact:

**Behavioral Claims**
- Must:
- Must not:

**Resources**
- Inline content:
- References/scripts/assets:

**Reviewer Checks**
- Scenario 1:
- Scenario 2:
````

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Adding persona to a skill | Put persona in agent definitions only |
| Adding tool constraints to a skill | Put tool allowlists in agents; skills define procedure |
| Vague trigger conditions | Name concrete task types, symptoms, and non-triggers |
| Description summarizes workflow | Description says when to load; body says what to do |
| Untestable claims | Rewrite as observable behavior Reviewer can assert |
| Too much context inline | Keep core workflow inline; move heavy reference to supporting files |

## Handoff

After designing a skill, pass the spec to Creator. Creator uses `agent-factory:writing-skills` to edit `SKILL.md`. Reviewer uses `agent-factory:evaluating-agent-behavior` to validate the completed skill against the behavioral claims.