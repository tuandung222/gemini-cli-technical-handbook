# 18. Test-Backed Invariants Catalog

## Purpose

This chapter lists high-value invariants and their direct test evidence.

## Invariant I1: Plan mode blocks subagent execution by default

Statement:
- In PLAN mode, dynamic subagent allow rule (1.05) is overridden by plan deny (1.06).

Evidence:
- `packages/core/src/policy/policy-engine.test.ts:1485`.
- `packages/core/src/policy/toml-loader.test.ts:546`.

## Invariant I2: Only plan-markdown writes are allowed in Plan mode

Statement:
- `write_file`/`replace` allowed only for matching plans-dir markdown paths.

Evidence:
- `packages/core/src/policy/policies/plan.toml:50`.
- `packages/cli/src/config/policy-engine.integration.test.ts:329`.
- `packages/cli/src/config/policy-engine.integration.test.ts:356`.

## Invariant I3: Plan file path traversal and symlink escape are rejected

Evidence:
- `packages/core/src/utils/planUtils.test.ts:42`.
- `packages/core/src/utils/planUtils.test.ts:54`.
- `packages/core/src/tools/exit-plan-mode.test.ts:430`.

## Invariant I4: Exit plan mode requires non-empty plan

Evidence:
- `packages/core/src/utils/planUtils.test.ts:80`.
- `packages/core/src/tools/exit-plan-mode.test.ts:162`.

## Invariant I5: Plan-mode tool swap is deterministic

Statement:
- in PLAN: register `ExitPlanModeTool`, unregister `EnterPlanModeTool`.
- outside PLAN (with plan feature enabled): inverse swap.

Evidence:
- `packages/core/src/config/config.test.ts:2540`.
- `packages/core/src/config/config.test.ts:2567`.

## Invariant I6: System instruction refresh occurs only when crossing Plan boundary

Evidence:
- `packages/core/src/config/config.test.ts:1326`.
- `packages/core/src/config/config.test.ts:1341`.
- `packages/core/src/config/config.test.ts:1359`.

## Invariant I7: Local agent must terminate via `complete_task`

Evidence:
- protocol violation test:
  `packages/core/src/agents/local-executor.test.ts:812`.
- recovery behavior tests:
  `packages/core/src/agents/local-executor.test.ts:1663`.

## Invariant I8: Remote agents require ASK_USER dynamic policy by default

Evidence:
- `packages/core/src/agents/registry.test.ts:681`.

## Invariant I9: Plan denial message consistency across schedulers

Evidence:
- `packages/core/src/scheduler/policy.test.ts:473`.

## Confidence notes

Coverage is strong for precedence, path safety, and lifecycle transitions. Residual gaps are mostly in cross-process/runtime integration and priority-scale normalization for dynamically inserted rules.
