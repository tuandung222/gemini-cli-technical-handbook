# 11. Message Bus Correlation Contract

## Message contract surface

Core bus message types include:
- tool confirmation request/response,
- policy update,
- tool calls update,
- ask-user request/response.

Evidence:
- `packages/core/src/confirmation-bus/types.ts:14`.

## Embedded policy check on confirmation request

`MessageBus.publish` handles `TOOL_CONFIRMATION_REQUEST` by immediately consulting policy engine and emitting:
- direct confirmation response if ALLOW,
- rejection + response if DENY,
- pass-through request to UI if ASK_USER.

Evidence:
- `packages/core/src/confirmation-bus/message-bus.ts:54` to `packages/core/src/confirmation-bus/message-bus.ts:84`.

## Correlation and request-response helper

Bus exposes `request(...)` helper:
- auto-generates correlation id,
- subscribes for matching response,
- timeout-protected cleanup.

Evidence:
- `packages/core/src/confirmation-bus/message-bus.ts:116`.

## Scheduler state broadcast

`SchedulerStateManager` publishes full tool call snapshots as `TOOL_CALLS_UPDATE` with `schedulerId`.

Evidence:
- `packages/core/src/scheduler/state-manager.ts:220`.

This supports multiplexed orchestration visibility across root and subagent schedulers.

## Ask-user protocol coupling

Serialized confirmation details include `ask_user` and `exit_plan_mode` variants:
- `type: 'ask_user'`
- `type: 'exit_plan_mode'`

Evidence:
- `packages/core/src/confirmation-bus/types.ts:100` and `packages/core/src/confirmation-bus/types.ts:105`.

## Protocol reliability notes

Strengths:
- explicit correlation IDs,
- typed message variants,
- centralized policy-aware branch.

Residual risks:
- EventEmitter transport has no durable queueing or replay.
- Cross-process reliability relies on host runtime behavior.

For local CLI this is usually acceptable; for distributed MAS runtimes, a durable transport would be needed.
