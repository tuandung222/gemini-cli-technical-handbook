# 10. Confirmation Protocol and Human Gating

## Confirmation as protocol, not callback noise

`resolveConfirmation` is a looped protocol engine:
- requests confirmation details from invocation,
- publishes awaiting-approval state with correlation id,
- races UI/IDE confirmation channels,
- supports param modification and replay.

Evidence:
- loop core: `packages/core/src/scheduler/confirmation.ts:108`.
- state update with correlation: `packages/core/src/scheduler/confirmation.ts:152`.

## Correlation discipline

Each confirmation round generates UUID correlation id and waits for matching response.

Evidence:
- `packages/core/src/scheduler/confirmation.ts:145`.
- response matcher: `packages/core/src/scheduler/confirmation.ts:62`.

## Modify-with-editor semantics

The protocol supports:
- external editor modifications (`modify_with_editor`),
- inline payload modifications from IDE.

Evidence:
- external path: `packages/core/src/scheduler/confirmation.ts:221`.
- inline path: `packages/core/src/scheduler/confirmation.ts:260`.

## Waiting-time budget control

For subagent execution, waiting for confirmation pauses deadline timer:
- pause/resume on waiting callbacks in `LocalAgentExecutor`.

Evidence:
- `packages/core/src/agents/local-executor.ts:434`.

This avoids unfair timeout consumption during human decision latency.

## Policy update coupling

After confirmation outcome, scheduler invokes policy update logic:
- standard tools: dynamic allow additions (optional persistence),
- MCP tools: tool/server wildcard variants,
- edit-tool always-allow can switch mode to AUTO_EDIT.

Evidence:
- update call: `packages/core/src/scheduler/scheduler.ts:473`.
- update logic: `packages/core/src/scheduler/policy.ts:87`.

## Human-gating significance

Plan mode relies on this protocol for strategic checkpoints:
- entering constrained planning phase,
- exiting with approved plan artifact,
- optionally consulting user on approach selection (`ask_user`).

Without this protocol, plan mode would degrade to static rules without iterative human steering.
