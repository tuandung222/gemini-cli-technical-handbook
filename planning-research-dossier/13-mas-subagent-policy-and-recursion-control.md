# 13. MAS Subagent Policy and Recursion Control

## Subagents as tools

Subagents are exposed as declarative tools (`Kind.Think`) with schema validation.

Evidence:
- `packages/core/src/agents/subagent-tool.ts:22`.
- wrapper dispatch local vs remote: `packages/core/src/agents/subagent-tool-wrapper.ts:72`.

## Dynamic agent policy registration

`AgentRegistry` adds dynamic rule per registered agent:
- local agent -> `ALLOW` at `1.05`.
- remote agent -> `ASK_USER` at `1.05`.

Evidence:
- registration logic: `packages/core/src/agents/registry.ts:273`.
- tests: `packages/core/src/agents/registry.test.ts:681`.

It skips this insertion when user policy already exists (`ignoreDynamic=true`):
- `packages/core/src/agents/registry.ts:281`.

## Recursion defense

`LocalAgentExecutor.create` filters out subagent tools to prevent recursive agent calling:
- checks against all agent names,
- skips registration of matching names.

Evidence:
- `packages/core/src/agents/local-executor.ts:129`.
- test: `packages/core/src/agents/local-executor.test.ts:443`.

## Agent-specific scheduler isolation

Subagent tool execution uses dedicated scheduler with:
- agent-specific tool registry via config proxy,
- `schedulerId` and `parentCallId` propagation.

Evidence:
- `packages/core/src/agents/agent-scheduler.ts:56`.

This forms a nested orchestration topology while reusing common policy/scheduler semantics.

## Plan-mode interaction

Because plan deny catch-all (1.06) is greater than subagent dynamic allow (1.05), subagent tools remain blocked in Plan mode unless explicitly allowed by stronger rules.

Evidence:
- regression test: `packages/core/src/policy/policy-engine.test.ts:1485`.

## MAS interpretation

This is a constrained hierarchical MAS pattern:
- child agents are capability-limited,
- recursive explosion is structurally blocked,
- parent governance remains centralized in shared policy/scheduler layers.
