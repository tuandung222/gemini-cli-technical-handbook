# 00. Reading Guide

## Why this dossier exists

`gemini-cli` planning is not just a UI mode toggle. It is a multi-layer runtime contract:
- prompt-level behavior constraints,
- tool-registry surface mutation,
- policy-engine priority filtering,
- scheduler-level confirmation protocol,
- and MAS subagent interactions under policy pressure.

This dossier maps these layers as one coherent orchestration system.

## Reader-specific paths

### Agent Architect

Read: `02`, `03`, `07`, `09`, `13`, `19`.

Focus:
- separation of concerns (Prompt vs Policy vs Scheduler),
- priority-lattice correctness,
- protocol contracts and failure boundaries.

### AI Agent Engineer

Read: `03`, `05`, `09`, `10`, `14`, `20`.

Focus:
- implementation sequence and invariants,
- safe mode transitions,
- completion protocol reliability.

### LLM Engineer

Read: `06`, `07`, `10`, `12`, `15`.

Focus:
- cognitive control via prompt sections,
- policy/rule interactions,
- mode-aware tooling behavior.

### LLM/Agent Data Scientist

Read: `16`, `18`, `19`, `21`.

Focus:
- telemetry semantics,
- test-backed invariant coverage,
- measurable future experiments.

## Primary mental model

Use this layered model while reading:
1. Intent Layer: model decides “plan vs execute” behavior under prompt constraints.
2. Capability Layer: tool registry and mode-specific tool availability.
3. Governance Layer: policy engine decides allow/deny/ask_user.
4. Protocol Layer: scheduler + confirmation + message bus.
5. Multi-agent Layer: subagents as tools, with anti-recursion and completion protocol.

## Evidence notation

Each chapter cites concrete code loci in the source repo with line anchors, e.g.:
- `packages/core/src/config/config.ts:1705`
- `packages/core/src/policy/policies/plan.toml:30`
- `packages/core/src/scheduler/scheduler.ts:424`

This is used as executable evidence, not anecdote.
