# 14. complete_task Protocol and Recovery Logic

## Protocol requirement

Local subagents must terminate by calling `complete_task`.

Evidence:
- protocol statement: `packages/core/src/agents/local-executor.ts:90`.
- tool name constant: `packages/core/src/agents/local-executor.ts:67`.

If model emits no function calls, executor treats it as protocol violation.

Evidence:
- `packages/core/src/agents/local-executor.ts:257`.
- test: `packages/core/src/agents/local-executor.test.ts:812`.

## Completion schema

`prepareToolsList` always injects `complete_task` declaration and required output arg:
- custom output schema when `outputConfig` exists,
- default required `result` otherwise.

Evidence:
- injection: `packages/core/src/agents/local-executor.ts:1093`.

## Turn processing behavior

`processFunctionCalls` handles `complete_task` synchronously:
- validates output schema/required arg,
- prevents duplicate completion in same turn,
- revokes completion if validation fails.

Evidence:
- branch start: `packages/core/src/agents/local-executor.ts:837`.
- missing result error: `packages/core/src/agents/local-executor.ts:947`.

## Graceful recovery protocol

On recoverable termination (`TIMEOUT`, `MAX_TURNS`, protocol violation), executor runs one grace turn with warning demanding immediate `complete_task`.

Evidence:
- final warning semantics: `packages/core/src/agents/local-executor.ts:296`.
- recovery turn: `packages/core/src/agents/local-executor.ts:325`.

Tests verify success/failure paths:
- `packages/core/src/agents/local-executor.test.ts:1663`.
- `packages/core/src/agents/local-executor.test.ts:1711`.

## Planning relevance

In planning/orchestration research terms, `complete_task` is an explicit terminal action that converts open-ended chain-of-thought/tool loops into a verifiable protocol with liveness and recoverability guarantees.
