# 03. Plan Mode Transition Model

## Entry transition

### Tool semantics

`EnterPlanModeTool`:
- asks for confirmation (unless policy ALLOW),
- on execution sets `ApprovalMode.PLAN`.

Evidence:
- confirmation flow: `packages/core/src/tools/enter-plan-mode.ts:84`.
- mode switch: `packages/core/src/tools/enter-plan-mode.ts:121`.

### Transition side effects

At config level:
- logs duration of previous mode,
- logs switch event,
- syncs plan tools,
- updates system instruction when crossing Plan boundary.

Evidence:
- `packages/core/src/config/config.ts:1714` to `packages/core/src/config/config.ts:1730`.

## Exit transition

`ExitPlanModeTool` has stricter semantics than simple mode toggle.

### Precondition checks

1. Synchronous path-confinement check during param validation.
2. Async path validation (`validatePlanPath`): subpath + file exists.
3. Async content validation (`validatePlanContent`): non-empty/readable.

Evidence:
- sync check: `packages/core/src/tools/exit-plan-mode.ts:72`.
- async checks: `packages/core/src/tools/exit-plan-mode.ts:142`, `packages/core/src/tools/exit-plan-mode.ts:152`.

### Approval payload semantics

If approved:
- switches mode to `default` or `autoEdit` (not `plan`/`yolo`),
- stores approved plan path,
- logs plan execution telemetry.

Evidence:
- mode + path set: `packages/core/src/tools/exit-plan-mode.ts:227` to `packages/core/src/tools/exit-plan-mode.ts:230`.

If rejected/cancelled:
- remains in Plan Mode,
- returns feedback-guided revision response.

Evidence:
- cancel branch: `packages/core/src/tools/exit-plan-mode.ts:216`.
- rejection branches: `packages/core/src/tools/exit-plan-mode.ts:242`.

## Transition-state sketch

States: `DEFAULT`, `AUTO_EDIT`, `YOLO`, `PLAN`.

Allowed planning transitions:
- `DEFAULT|AUTO_EDIT|YOLO -> PLAN` via `enter_plan_mode` (trusted-folder constraints still apply globally).
- `PLAN -> DEFAULT|AUTO_EDIT` via approved `exit_plan_mode` payload.
- `PLAN -> PLAN` on reject/cancel/invalid plan.

Guardrails:
- invalid exit payload to `YOLO` or `PLAN` is treated as unexpected at description function level:
  `packages/core/src/tools/exit-plan-mode.ts:39` to `packages/core/src/tools/exit-plan-mode.ts:43`.

## Design read

Entry is intentionally light; exit is intentionally heavy. This asymmetry reflects safety posture: easy to enter constrained state, hard to leave without a validated artifact and explicit human approval.
