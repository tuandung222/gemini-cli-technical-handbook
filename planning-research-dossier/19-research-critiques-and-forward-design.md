# 19. Research Critiques and Forward Design

## Critique C1: Mixed priority scales (tiered vs raw)

Observation:
- TOML rules use transformed tiered scale (1.x/2.x/3.x).
- Some dynamic runtime insertions use raw values (e.g., 50).

Risk:
- unintuitive dominance and hard-to-reason precedence.

Proposal:
- mandatory normalized insertion API:
  - `addTieredRule(tier, localPriority)` and
  - `addAbsoluteRule(priorityClass, value)`.

## Critique C2: Prompt-policy coupling lacks automated drift tests

Observation:
- prompt text encodes normative workflow,
- policy encodes actual authority,
- drift can occur silently.

Proposal:
- generate machine-readable capability matrix from policy,
- assert prompt-declared capabilities are subset of matrix.

## Critique C3: Plan artifact semantics not fully typed

Observation:
- approved plan path is stored as string and consumed via prompt rules.

Proposal:
- add plan metadata object with:
  - hash,
  - revision number,
  - approval timestamp,
  - selected approach id.

This enables tighter governance and analytics.

## Critique C4: Non-interactive plan semantics are under-specified

Observation:
- non-interactive mode collapses `ask_user` to deny.

Proposal:
- explicit non-interactive planning strategy:
  - deterministic fallback for approach selection,
  - auto-approval policy profile with stricter read-only boundaries,
  - fail-fast reason taxonomy.

## Critique C5: Confirmation protocol is local-event based

Observation:
- current MessageBus uses EventEmitter.

Proposal for distributed MAS future:
- pluggable transport (in-memory vs durable queue),
- correlation-id tracing across process boundaries,
- idempotent confirmation handlers.

## Forward roadmap (research)

1. Formal specification
- Write TLA+/statechart model for mode transition + policy decisions.

2. Property-based testing
- fuzz rule sets for precedence anomalies and redirection edge cases.

3. Counterfactual evaluation
- compare planning outcomes with/without consult step and with varying policy strictness.

4. Human-in-the-loop efficiency
- optimize approval prompts using rejection reason telemetry.
