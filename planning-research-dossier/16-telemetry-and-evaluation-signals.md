# 16. Telemetry and Evaluation Signals

## Plan-related telemetry events

Three core event families are exposed:
- approval mode switch,
- approval mode duration,
- plan execution.

Evidence:
- `packages/core/src/telemetry/types.ts:1967`.
- `packages/core/src/telemetry/types.ts:1997`.
- `packages/core/src/telemetry/types.ts:2027`.

## Emission points

- mode switch and duration emitted from `Config.setApprovalMode` path:
  `packages/core/src/config/config.ts:1714`.
- plan execution emitted on approved `exit_plan_mode` execution:
  `packages/core/src/tools/exit-plan-mode.ts:230`.

Logging helpers:
- `packages/core/src/telemetry/loggers.ts:731`.
- `packages/core/src/telemetry/loggers.ts:757`.

## Observable planning KPIs (derived)

From existing telemetry, one can compute:
- Plan Entry Rate: transitions to PLAN / session.
- Plan Dwell Time: duration in PLAN mode.
- Plan Approval Conversion: approved exits / exit attempts.
- Post-Plan Execution Mode Split: DEFAULT vs AUTO_EDIT approvals.

These metrics can characterize user trust and planning utility.

## Evaluation extensions (recommended)

Current telemetry lacks first-class fields for:
- rejection reason taxonomy,
- number of plan revision cycles,
- plan size/complexity proxies,
- implementation adherence to approved plan.

Additive proposal:
1. `plan_rejected` event with categorical reason tags.
2. `plan_revision_count` at approval time.
3. `approved_plan_path_hash` for privacy-safe longitudinal analysis.

## Data-science note

Because planning is a protocol layer, success metrics should include both outcome quality and safety-preserving behavior (e.g., zero policy bypass incidents while maintaining throughput).
