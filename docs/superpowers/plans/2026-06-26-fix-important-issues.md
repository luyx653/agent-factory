# Fix Important Issues Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use agent-factory:subagent-driven-development (recommended) or agent-factory:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Fix the 4 Important-severity issues found in the agent-factory plugin code review.

**Architecture:** Four targeted file edits — two agent definition files and two skill files. No new files created. Each task is independent and commits separately.

**Tech Stack:** Markdown, Bash (verification only)

## Global Constraints

- All SKILL.md descriptions must start with `Use when` — never summarize workflow
- Agent tool allowlists must list tools explicitly, not rely on behavioral "MUST NOT" alone
- Verdict format in agent files must exactly match the format in the referenced skill
- Commit after each task completes

---

### Task 1: Add tool allowlist to genius.md

**Files:**
- Modify: `agents/genius.md`

**Interfaces:**
- Consumes: nothing
- Produces: genius agent definition with explicit tool allowlist in addition to behavioral constraints

- [ ] **Step 1: Verify current state (no tool allowlist)**

```bash
grep -n "Allowed tools" agents/genius.md
```

Expected: no output (allowlist absent).

- [ ] **Step 2: Add tool allowlist after the REQUIRED SKILL line**

In `agents/genius.md`, insert the following line immediately after the `**REQUIRED SKILL:**` line:

```
**REQUIRED SKILL:** Use `agent-factory:designing-agents` for every design task.

**Allowed tools:** Read, Glob, Grep, WebSearch — no file write tools.
```

Full resulting file content:

```markdown
---
name: genius
description: Design architect for agent-factory — use when defining scope, persona, tool constraints, or architecture for a new agent or skill, before any implementation files are written
---

You are the Genius — a design architect specializing in agent and skill authoring. You produce specifications; you never implement them.

Your job: analyze the request, explore existing patterns, and produce a clear specification that Creator can implement without making design decisions.

**REQUIRED SKILL:** Use `agent-factory:designing-agents` for every design task.

**Allowed tools:** Read, Glob, Grep, WebSearch — no file write tools.

**Your outputs always include:**
- Scope: what this agent/skill does and explicitly does NOT do
- Persona (for agents): name, purpose, voice
- Tool constraints: which tools are allowed and why
- Success criteria: behavioral claims Reviewer will verify

**You MUST NOT:**
- Create or modify any files
- Skip the designing-agents skill to "just design it quickly"
- Produce vague scope definitions
```

- [ ] **Step 3: Verify allowlist is present**

```bash
grep -n "Allowed tools" agents/genius.md
```

Expected: one line containing `Read, Glob, Grep, WebSearch`.

- [ ] **Step 4: Commit**

```bash
git add agents/genius.md
git commit -m "fix: add explicit tool allowlist to genius agent"
```

---

### Task 2: Fix reviewer.md — tool allowlist and verdict format

**Files:**
- Modify: `agents/reviewer.md`

**Interfaces:**
- Consumes: nothing
- Produces: reviewer agent definition with explicit tool allowlist and verdict format aligned to `evaluating-agent-behavior/SKILL.md`

- [ ] **Step 1: Verify current state (no allowlist, single-line verdict)**

```bash
grep -n "Allowed tools" agents/reviewer.md
grep -n "Rationalization" agents/reviewer.md
```

Expected: both return no output (allowlist absent, multi-line format absent).

- [ ] **Step 2: Rewrite reviewer.md with both fixes**

Full resulting file content:

```markdown
---
name: reviewer
description: Validation specialist for agent-factory — use when verifying a completed skill or agent definition passes behavioral pressure tests before deployment
---

You are the Reviewer — a critical validator who verifies agents and skills behave as their specifications claim, under pressure.

**REQUIRED SKILL:** Use `agent-factory:evaluating-agent-behavior` for every validation task.

**Allowed tools:** Read, Glob, Grep, Agent — no file write tools.

**Your process:**
1. Read the completed skill/agent definition
2. Extract every behavioral claim from the spec
3. Design pressure scenarios for each claim
4. Run each scenario as a fresh subagent
5. Report using this verdict format:

```
PASS — all N behavioral claims held under pressure

FAIL — [exact claim violated]
  Rationalization used: "[verbatim text from agent output]"
  Scenario: [which pressure scenario triggered it]
```

**You MUST NOT:**
- Create or modify any files
- Report PASS without running actual pressure scenarios
- Skip testing because "the skill looks well-written"
- Suggest edits — report verdict only; Creator handles fixes
```

- [ ] **Step 3: Verify both fixes are present**

```bash
grep -n "Allowed tools" agents/reviewer.md
grep -n "Rationalization" agents/reviewer.md
```

Expected: first returns one line with `Read, Glob, Grep, Agent`; second returns one line with `Rationalization used:`.

- [ ] **Step 4: Commit**

```bash
git add agents/reviewer.md
git commit -m "fix: add tool allowlist and align verdict format in reviewer agent"
```

---

### Task 3: Add Designing Skills section to designing-agents/SKILL.md

**Files:**
- Modify: `skills/designing-agents/SKILL.md`

**Interfaces:**
- Consumes: nothing
- Produces: skill with a "Designing Skills" section that covers skill-specific design elements (no persona, no tool constraints — only trigger conditions, output shape, success criteria)

- [ ] **Step 1: Verify current state (no Designing Skills section)**

```bash
grep -n "Designing Skills" skills/designing-agents/SKILL.md
```

Expected: no output.

- [ ] **Step 2: Append Designing Skills section to end of file**

Add the following content at the end of `skills/designing-agents/SKILL.md`:

```markdown

---

## Designing Skills

A skill has different design elements than an agent. No persona, no tool constraints — only trigger conditions, output shape, and behavioral claims.

**Trigger conditions:**
- When exactly does this skill fire? (specific task types, not vague intent)
- What upstream state must exist before this skill is relevant?
- List 2-4 concrete triggering scenarios

**Description format:**
- Must start with `Use when` followed by the specific trigger condition
- Never describe what the skill does — only when it applies
- Max 1024 characters total

**Output shape:**
- What does the skill produce? (a plan, a decision, a code file, a verdict)
- What format? (markdown section, structured table, specific code block)
- Who consumes the output and what do they do with it?

**Success criteria:**
- Observable: "Produces X when Y" / "Always invokes Z before W"
- Falsifiable: write each criterion so Reviewer can design a scenario that would produce a FAIL
- Avoid vague criteria like "produces high-quality output"

## Skill File Structure

```markdown
---
name: skill-name
description: Use when [specific triggering condition — not what it does]
---

# [Skill Title]

## Overview

[One paragraph: the problem this skill solves and when it applies]

## [Process / Checklist / Steps]

[The actual workflow content]

## [Output Format] (if applicable)

[Expected shape of the skill's output]
```
```

- [ ] **Step 3: Verify section was added**

```bash
grep -n "Designing Skills" skills/designing-agents/SKILL.md
grep -n "Trigger conditions" skills/designing-agents/SKILL.md
```

Expected: both return one matching line each.

- [ ] **Step 4: Commit**

```bash
git add skills/designing-agents/SKILL.md
git commit -m "fix: add Designing Skills section to designing-agents skill"
```

---

### Task 4: Verify brainstorming/SKILL.md description compliance

**Files:**
- Verify: `skills/brainstorming/SKILL.md`
- Modify only if description does not start with `Use when`

**Interfaces:**
- Consumes: nothing
- Produces: confirmed-compliant brainstorming skill description

- [ ] **Step 1: Check current description**

```bash
head -5 skills/brainstorming/SKILL.md
```

Read the `description:` field on line 3.

- [ ] **Step 2: Evaluate compliance**

If description starts with `Use when` → **no change needed**, skip to Step 4.

If description starts with anything else (e.g., `You MUST use this...`) → proceed to Step 3.

- [ ] **Step 3: Fix description (only if needed)**

Replace the `description:` line in the frontmatter with:

```
description: Use when beginning any creative or feature work — before writing code, building components, or modifying behavior
```

Verify:

```bash
head -5 skills/brainstorming/SKILL.md
```

Expected: line 3 starts with `description: Use when`.

- [ ] **Step 4: Commit (always — either "verified OK" or "fixed")**

If no change was needed:

```bash
git commit --allow-empty -m "chore: verify brainstorming description is compliant (no change needed)"
```

If a fix was applied:

```bash
git add skills/brainstorming/SKILL.md
git commit -m "fix: update brainstorming description to start with 'Use when'"
```

---

## Final Verification

- [ ] `agents/genius.md` contains `Allowed tools: Read, Glob, Grep, WebSearch`
- [ ] `agents/reviewer.md` contains `Allowed tools: Read, Glob, Grep, Agent`
- [ ] `agents/reviewer.md` verdict format contains `Rationalization used:` and `Scenario:`
- [ ] `skills/designing-agents/SKILL.md` contains a `## Designing Skills` section
- [ ] `skills/brainstorming/SKILL.md` description starts with `Use when`
- [ ] Update `fix-list.md` — check off all 4 Important items

```bash
grep "Allowed tools" agents/genius.md agents/reviewer.md
grep "Rationalization" agents/reviewer.md
grep "Designing Skills" skills/designing-agents/SKILL.md
head -4 skills/brainstorming/SKILL.md
```
