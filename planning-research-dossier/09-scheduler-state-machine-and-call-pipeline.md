# 09. Scheduler State Machine and Call Pipeline

## Execution pipeline

Scheduler call path for each validating tool call:
1. `checkPolicy`
2. if `ASK_USER`: confirmation loop
3. `updatePolicy`
4. execute tool

Evidence:
- main orchestration: `packages/core/src/scheduler/scheduler.ts:424`.

## State machine

Core statuses:
- `validating`
- `scheduled`
- `awaiting_approval`
- `executing`
- terminal: `success|error|cancelled`

Evidence:
- status enum: `packages/core/src/scheduler/types.ts:25`.
- typed transitions: `packages/core/src/scheduler/state-manager.ts:240`.

## Queueing and batch behavior

Scheduler supports:
- active batch with internal queue,
- external request queue when already busy,
- cancel propagation to active + queued calls.

Evidence:
- enqueue/dequeue path: `packages/core/src/scheduler/scheduler.ts:165`.
- cancelAll behavior: `packages/core/src/scheduler/scheduler.ts:202`.

## Tool-call context propagation

Scheduler enriches requests with:
- `schedulerId`
- `parentCallId`

Evidence:
- request enrichment: `packages/core/src/scheduler/scheduler.ts:252`.

This enables nested MAS observability and parent-child call lineage.

## Policy denial behavior

On deny:
- constructs policy violation error,
- finalizes as error call without execution.

Evidence:
- `packages/core/src/scheduler/scheduler.ts:433`.
- denial formatting helper: `packages/core/src/scheduler/policy.ts:32`.

## Design interpretation

Scheduler is a deterministic protocol machine. It does not “trust” tool self-confirmation logic; it wraps it with centralized policy checks and structured state transitions.
