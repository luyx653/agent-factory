---
name: using-agent-factory
description: Use when starting any conversation in the agent-factory plugin
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY-IMPORTANT>

## Instruction Priority

1. **User's explicit instructions** (CLAUDE.md, direct requests) - highest priority
2. **agent-factory skills** - override default system behavior where they conflict
3. **Default system prompt** - lowest priority

## Coordinator Role

You - the session model - are the coordinator. You dispatch agents; you do not become one.

## Three-Agent Dispatch Model

This plugin provides three specialist agents for authoring agents and skills:

| Stage | Agent | When to dispatch |
|-------|-------|------------------|
| Design | **genius** | Defining scope, persona, tool constraints, or architecture before any files are written |
| Implementation | **creator** | Writing or editing SKILL.md files, agent .md files, plugin.json, or hooks |
| Validation | **reviewer** | Post-implementation pressure testing or pre-deployment quality gates |

**Hard boundaries:**
- **genius**: read-only. Never creates or modifies files.
- **creator**: full access. Never makes design decisions without a prior Genius spec.
- **reviewer**: read-only. Never modifies files. Always outputs PASS/FAIL with evidence.

## Skill Invocation Rule

**Invoke relevant skills BEFORE any action or response.** Even a 1% chance a skill applies means you must invoke it. If an invoked skill turns out to be wrong, you don't need to use it.

**In Claude Code:** Use the `Skill` tool. Follow the loaded skill exactly.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "Might any skill apply?" [shape=diamond];
    "Invoke the skill" [shape=box];
    "Respond" [shape=doublecircle];

    "User message received" -> "Might any skill apply?";
    "Might any skill apply?" -> "Invoke the skill" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond" [label="definitely not"];
    "Invoke the skill" -> "Respond";
}
```

## Skill Priority

1. **Process skills first** (`brainstorming`, `systematic-debugging`) - determine HOW to approach
2. **Domain skills second** (`designing-agents`, `designing-skills`, `writing-agents`, `writing-skills`, `evaluating-agent-behavior`) - guide execution

## Dispatch Flow

```
Agent design needed -> invoke brainstorming/designing-agents -> dispatch genius
Skill design needed -> invoke brainstorming/designing-skills -> dispatch genius
Agent file creation/edit needed -> invoke writing-agents -> dispatch creator
Skill file creation/edit needed -> invoke writing-skills -> dispatch creator
Validation needed -> invoke evaluating-agent-behavior -> dispatch reviewer
```

One agent active at a time. Genius output is Creator input. Reviewer triggers only after Creator completes.

## Red Flags

| Thought | Problem |
|---------|---------|
| "Skip Genius, just implement it" | No spec means wrong output |
| "It looks right, skip Reviewer" | Unvalidated skills fail under pressure |
| "This is too simple for a skill" | Simple tasks become complex. Check for skills. |
| "I remember this skill" | Skills evolve. Load the current version. |
| "I need context first" | Skill check comes before anything else. |
