# 02. Runtime Boundary and Control Surface

## System boundary

Planning touches five runtime domains:
1. Config domain (`Config`) controls active approval mode and tool surface.
2. Prompt domain (`PromptProvider` + snippets) controls model behavior instructions.
3. Policy domain (`PolicyEngine`) controls runtime allow/deny/ask_user.
4. Scheduler domain controls execution state, confirmations, and updates.
5. Agent domain maps subagents into callable tools with dedicated protocol rules.

## Control-surface map

### Config as mode authority

- `setApprovalMode` is authoritative mode mutation point:
  `packages/core/src/config/config.ts:1705`.
- It logs mode switch/duration telemetry and performs plan-transition side effects:
  `packages/core/src/config/config.ts:1714`, `packages/core/src/config/config.ts:1728`.

### Tool surface mutability

- `syncPlanModeTools` enforces enter/exit tool swap:
  - in PLAN: remove `enter_plan_mode`, register `exit_plan_mode`;
  - outside PLAN: inverse behavior, gated by experimental plan flag.
  `packages/core/src/config/config.ts:1736`.

### Prompt as cognitive control layer

- PromptProvider injects `planningWorkflow` only when mode is PLAN:
  `packages/core/src/prompts/promptProvider.ts:171`.
- It constructs plan-tool list from actually enabled tools and read-only MCP tools:
  `packages/core/src/prompts/promptProvider.ts:67`, `packages/core/src/prompts/promptProvider.ts:74`.

### Policy as hard gate

- Plan catch-all deny is policy-level, not only prompt-level:
  `packages/core/src/policy/policies/plan.toml:30`.
- Explicit plan allows/asks override deny by higher priority (within tier):
  `packages/core/src/policy/policies/plan.toml:38`, `packages/core/src/policy/policies/plan.toml:45`.

### Scheduler as protocol executor

- Tool call flow: validate -> policy check -> confirmation loop -> policy update -> execute:
  `packages/core/src/scheduler/scheduler.ts:424`.

## Architectural implication

The planning mechanism is intentionally multi-layered. A prompt jailbreak alone is insufficient if policy denies execution. A policy misconfiguration can still weaken planning guarantees even with correct prompt text. Safety comes from composition, not any single layer.
