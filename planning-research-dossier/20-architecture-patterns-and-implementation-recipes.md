# 20. Architecture Patterns and Implementation Recipes

## Pattern P1: Two-key execution gate

Design:
- Key A: policy decision must permit execution.
- Key B: confirmation protocol must approve when required.

Recipe:
1. Implement policy check before tool side effects.
2. Serialize confirmation details with correlation id.
3. Block execution until terminal confirmation outcome.

Reference implementation:
- `packages/core/src/scheduler/scheduler.ts:430`.
- `packages/core/src/scheduler/confirmation.ts:145`.

## Pattern P2: Constrained-plan artifact loop

Design:
- planning writes only in dedicated plan sandbox,
- plan approval transitions to implementation mode.

Recipe:
1. create isolated plan directory at initialization.
2. enforce path + content validation on exit request.
3. bind approved artifact path into execution prompt.

Reference:
- `packages/core/src/config/config.ts:942`.
- `packages/core/src/utils/planUtils.ts:32`.
- `packages/core/src/prompts/snippets.ts:493`.

## Pattern P3: Priority lattice for governance layering

Design:
- default, user, admin tiers with deterministic precedence.

Recipe:
1. transform local priorities by tier.
2. keep dynamic rules in explicit documented bands.
3. add regression tests for critical overrides.

Reference:
- `packages/core/src/policy/toml-loader.ts:200`.
- `packages/core/src/policy/config.ts:216`.

## Pattern P4: MAS-safe subagent embedding

Design:
- expose subagents as tools,
- block recursive self-invocation,
- require explicit terminal tool (`complete_task`).

Recipe:
1. isolate tool registry per agent.
2. remove subagent names from allowed tools.
3. enforce completion protocol and grace recovery.

Reference:
- `packages/core/src/agents/local-executor.ts:119`.
- `packages/core/src/agents/local-executor.ts:257`.

## Persona-oriented usage

### Agent Architect

Adopt P1 + P3 first to guarantee governance clarity.

### AI Agent Engineer

Adopt P2 + P4 for safe implementation loops and stable agent behavior.

### LLM Engineer

Use prompt contract rules in tandem with policy proofs; avoid relying on prompt-only constraints.

### LLM/Agent Data Scientist

Instrument P1-P4 with telemetry counters for approval rates, rejection causes, and completion reliability.
