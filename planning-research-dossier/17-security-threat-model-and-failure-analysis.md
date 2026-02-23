# 17. Security Threat Model and Failure Analysis

## Threat model overview

Planning flow defends against:
1. unintended code mutation during planning,
2. path traversal/symlink abuse for plan artifacts,
3. subagent-based bypass of plan constraints,
4. shell/script execution during planning.

## Defense mapping

### 1) Mutation containment

- Plan policy catch-all deny in PLAN mode.
- Explicit narrow allow-list only.

Evidence:
- `packages/core/src/policy/policies/plan.toml:30`.

### 2) Plan-path confinement

- synchronous confinement check in exit tool params,
- async realpath + existence validation,
- content non-empty validation.

Evidence:
- `packages/core/src/tools/exit-plan-mode.ts:79`.
- `packages/core/src/utils/planUtils.ts:32`.

### 3) Subagent bypass control

- dynamic subagent allow at 1.05 does not beat plan deny 1.06.

Evidence:
- `packages/core/src/policy/policy-engine.test.ts:1485`.

### 4) Script execution block

- deny message explicitly communicates scripts blocked in Plan Mode.

Evidence:
- `packages/core/src/policy/policies/plan.toml:34`.

## Failure modes

### Mixed priority scales

Raw dynamic priorities (e.g., MCP read-only = 50) can dominate tiered ranges unexpectedly.

Evidence:
- `packages/core/src/tools/mcp-client.ts:1040`.
- tiered formula reference: `packages/core/src/policy/toml-loader.ts:200`.

### Prompt-policy divergence

If prompt advertises behavior not represented in policy rules, user/model expectations drift from actual behavior.

### Non-interactive semantic collapse

`ask_user` conversion to deny can cause planning dead-ends for tasks requiring human selection.

Evidence:
- `packages/core/src/policy/policy-engine.ts:614`.

## Hardening recommendations

1. Priority normalization guard in `PolicyEngine.addRule`.
2. Automated prompt-policy consistency tests.
3. Explicit non-interactive plan mode capability matrix in docs/UI.
4. Structured rejection reasons for operational tuning.
