# 21. Glossary and Formalism

## Glossary

- Approval Mode: Runtime governance state (`default|autoEdit|yolo|plan`).
- Plan Mode: Constrained mode for analysis + planning artifact creation before implementation.
- Policy Rule: Priority-ordered decision mapping over tool name, args pattern, and mode.
- Tiered Priority: Priority transformed as `tier + p/1000` for TOML-loaded policies.
- Dynamic Rule: Runtime-inserted rule outside TOML loading pipeline.
- Confirmation Correlation ID: Identifier binding request/response in confirmation protocol.
- Subagent Tool: Agent exposed as a callable tool in orchestrator runtime.
- `complete_task`: Mandatory terminal tool in local subagent protocol.

## Formal predicates

Let:
- `M` be current approval mode,
- `R` be ordered policy rules by descending priority,
- `C` be a tool call.

Define `match(r, C, M)` with mode, tool, args filters.

Policy decision function:
`D(C, M) = decision(r*)` where `r* = first r in R such that match(r, C, M)`;
if none, default decision.

Non-interactive coercion:
`ASK_USER -> DENY`.

## Planning safety invariant (informal)

For any tool call `C` in Plan Mode,
if `C` is not in explicit plan allow-set and not path-constrained plan artifact write/edit,
then `D(C, PLAN) = DENY` or `ASK_USER` (which may coerce to DENY non-interactively).

## Liveness invariant for local subagent

A local subagent run is successful iff there exists a turn `t` where model calls `complete_task` with schema-valid payload before terminal failure without successful recovery.

## Research note

These predicates can be encoded in model checking or property-based test generators to validate future refactors without relying only on manually curated regression tests.
