# 15. CLI Plan UX and Non-Interactive Semantics

## CLI `/plan` command

`/plan` does two things:
1. sets approval mode to PLAN,
2. displays currently approved plan file if available.

Evidence:
- `packages/cli/src/ui/commands/planCommand.ts:18`.

## Approval-mode cycling behavior

UI mode cycle includes PLAN only when `allowPlanMode` is enabled.

Evidence:
- `packages/cli/src/ui/hooks/useApprovalModeIndicator.ts:80`.

## Auto-approval behavior excludes PLAN

When switching modes, UI auto-approves pending tool calls only for:
- `YOLO`
- `AUTO_EDIT` (and only edit tools)

PLAN is intentionally excluded.

Evidence:
- `packages/cli/src/ui/hooks/useGeminiStream.ts:1427`.

## CLI config parsing constraints

`--approval-mode=plan` is accepted only if `experimental.plan` enabled.

Evidence:
- `packages/cli/src/config/config.ts:556`.

## Non-interactive implications

In non-interactive mode:
- `ask_user` is always excluded,
- additional tools are excluded by mode,
- for PLAN non-interactive, default approval-requiring tools are excluded.

Evidence:
- `packages/cli/src/config/config.ts:633` to `packages/cli/src/config/config.ts:658`.

At policy-engine layer, `ASK_USER -> DENY` under non-interactive setting.

Evidence:
- `packages/core/src/policy/policy-engine.ts:614`.

## Interpretation

Plan mode is fundamentally human-in-the-loop. Non-interactive contexts can emulate some planning constraints but cannot preserve the same collaborative approval semantics.
