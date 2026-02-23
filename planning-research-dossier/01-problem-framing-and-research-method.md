# 01. Problem Framing and Research Method

## Research question

How does `gemini-cli` implement planning as a safe orchestration mechanism instead of a plain prompt convention?

## Scope

In-scope:
- Plan mode lifecycle (`enter_plan_mode`, `exit_plan_mode`).
- Policy-tier and priority transformation mechanics.
- Scheduler/message-bus confirmation protocol.
- MAS/subagent behavior under plan-mode constraints.

Out-of-scope:
- generic model quality,
- unrelated UI rendering concerns,
- non-planning extension workflows.

## Method

### 1. Static control-path inspection

Inspected core files:
- mode transitions: `packages/core/src/tools/enter-plan-mode.ts`, `packages/core/src/tools/exit-plan-mode.ts`, `packages/core/src/config/config.ts`.
- plan artifact checks: `packages/core/src/utils/planUtils.ts`.
- policy lattice: `packages/core/src/policy/config.ts`, `packages/core/src/policy/toml-loader.ts`, `packages/core/src/policy/policy-engine.ts`, `packages/core/src/policy/policies/plan.toml`.
- orchestration protocol: `packages/core/src/scheduler/*`, `packages/core/src/confirmation-bus/*`.
- MAS: `packages/core/src/agents/*`.
- CLI mode behavior: `packages/cli/src/*`.

### 2. Behavioral cross-check via tests

Validated key invariants using tests:
- plan deny overrides dynamic subagent allow:
  `packages/core/src/policy/policy-engine.test.ts:1485`.
- built-in plan policy precedence:
  `packages/core/src/policy/toml-loader.test.ts:546`.
- plan file path/content safety:
  `packages/core/src/utils/planUtils.test.ts:32`.
- plan-mode write restrictions by path:
  `packages/cli/src/config/policy-engine.integration.test.ts:326`.

### 3. Protocol synthesis

Combined code + tests into:
- transition diagrams,
- invariant catalog,
- failure-mode taxonomy,
- critique and forward design hypotheses.

## Validity constraints

- Claims are made only where code path and tests are visible.
- Where behavior is inferred (not explicitly asserted), the dossier marks it as inference.
- Priority arithmetic is shown using actual transformation formula from loader logic.

## Core thesis

`gemini-cli` planning is implemented as **contractual orchestration**:
- prompt establishes strategic behavior,
- policy enforces capabilities,
- scheduler enforces human confirmation and completion flow,
- tests lock critical precedence invariants.
