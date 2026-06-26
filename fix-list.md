# agent-factory 待修复清单

**审查日期:** 2026-06-26  
**评估结论:** PASS WITH CAVEATS

---

## Critical（必须修复）

- [x] **SessionStart hook matcher 过于严格** — `hooks/hooks.json`  
  当前 `"matcher": "startup|clear|compact"` 导致大多数会话（以任务描述开头）无法触发 hook，`using-agent-factory` 技能无法注入。  
  **修复：** 删除 `matcher` 字段，使 hook 在每次会话启动时无条件触发。

---

## Important（影响质量，应在依赖插件前修复）

- [ ] **genius 和 reviewer 未显式列出允许的工具** — `agents/genius.md`、`agents/reviewer.md`  
  仅靠行为约束"MUST NOT 创建文件"不足以在压力下保持边界。`designing-agents` 技能要求显式列出工具白名单。  
  **修复：** 在两个 agent 定义中添加工具白名单。genius：`Read, Glob, Grep, WebSearch`；reviewer：同上加 `Agent`（用于分发子 agent）。

- [ ] **`designing-agents` 技能缺少 skill 设计部分** — `skills/designing-agents/SKILL.md`  
  genius 被要求对所有设计任务（包括设计 skill）使用此技能，但技能内容仅覆盖 agent 设计（Persona、Tool constraints 对 skill 不适用）。  
  **修复：** 添加"Designing Skills"章节，包含 skill 专属清单：触发条件、description 格式（以"Use when..."开头）、输出结构、成功标准。

- [ ] **verdict 格式冲突** — `agents/reviewer.md` vs `skills/evaluating-agent-behavior/SKILL.md`  
  `reviewer.md` 要求单行格式；`evaluating-agent-behavior` 要求多行格式（含 `Scenario:` 字段）。两者不一致导致 reviewer 产出格式不确定。  
  **修复：** 统一为 `evaluating-agent-behavior` 技能中的多行格式，并更新 `reviewer.md` 与之对齐。

- [ ] **`brainstorming/SKILL.md` description 不符合规范** — `skills/brainstorming/SKILL.md`  
  当前 description 以"You MUST use this..."开头，违反插件自身约束（所有 SKILL.md description 必须以"Use when..."开头）。该技能从 superpowers 移植时未做适配。  
  **修复：** 改写为：`"Use when beginning any creative or feature work — before writing code, building components, or modifying behavior."`

---

## Minor（质量/规范问题）

- [ ] **三个 agent 的 description 角色标签在前，触发条件在后** — `agents/genius.md`、`agents/creator.md`、`agents/reviewer.md`  
  违反 `designing-agents` 技能的 Common Mistakes 规则："Description must say WHEN to dispatch it, not what it does."  
  **修复：** 将 WHEN 子句移至最前，角色标签移至括号内。

- [ ] **`using-agent-factory` description 附加了流程摘要** — `skills/using-agent-factory/SKILL.md`  
  "establishes the three-agent dispatch model and skill invocation rules..." 描述了技能做什么，而非触发条件。  
  **修复：** 精简为 `"Use when starting any conversation in the agent-factory plugin."`

- [ ] **`verification-before-completion` description 附加了流程摘要** — `skills/verification-before-completion/SKILL.md`  
  "requires running verification commands..." 描述执行步骤，属于 SDO 违规。  
  **修复：** 精简为 `"Use when about to claim work is complete, fixed, or passing, before committing or creating PRs."`

- [ ] **技能数量说明有误** — `docs/superpowers/specs/2026-06-26-agent-factory-design.md`  
  设计文档写"10 ported + 2 new"，实际为"9 ported + 3 new"（`using-agent-factory` 是新写的，非移植）。  
  **修复：** 更新设计文档中的技能统计描述。

- [ ] **缺少协调者角色说明** — `skills/using-agent-factory/SKILL.md`  
  文档说明三 agent 模型，但未明确"当前会话（主上下文）是协调者，而非子 agent"。  
  **修复：** 添加一句：`"You — the session model — are the coordinator. You dispatch agents; you do not become one."`

- [ ] **`escape_for_json` 未转义所有控制字符** — `hooks/session-start`  
  仅转义了 `\`、`"`、`\n`、`\r`、`\t`，未覆盖其他 ASCII 控制字符（0x00–0x1F）。若 SKILL.md 含特殊字符会导致 JSON 无效。  
  **修复：** 添加注释说明 SKILL.md 内容须为纯文本，或改用 `jq` 构建 JSON。

---

## Gap（缺失功能）

- [ ] **缺少创作完成后的收尾工作流技能**  
  superpowers 包含 `finishing-a-development-branch`，agent-factory 未移植。对于需要提交和 PR 的 skill/agent 产物，收尾流程缺乏指导。  
  **建议：** 按需新增一个针对 agent/skill 产物发布场景的收尾技能。
