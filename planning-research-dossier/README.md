# Gemini CLI Planning Research Dossier

This dossier is a research-grade technical analysis of planning/orchestration inside `gemini-cli`, focused on:
- planning mode as a constrained execution protocol,
- policy priority algebra and governance,
- scheduler/message-bus confirmation loops,
- MAS/subagent implications for orchestration safety.

The writing style is intentionally deep and formal, targeted at:
- Agent Architect
- AI Agent Engineer
- LLM Engineer
- LLM/Agent Data Scientist

## How to read

1. Start with `00-reading-guide.md` and `01-problem-framing-and-research-method.md`.
2. Read control-surface and lifecycle (`02` to `06`).
3. Read policy and protocol core (`07` to `12`).
4. Read MAS/subagent sections (`13`, `14`).
5. End with operations, security, invariants, and critique (`15` to `21`).

## Chapters

- `00-reading-guide.md`
- `01-problem-framing-and-research-method.md`
- `02-runtime-boundary-and-control-surface.md`
- `03-plan-mode-transition-model.md`
- `04-plan-artifact-lifecycle-and-storage.md`
- `05-tool-surface-restriction-and-capability-theory.md`
- `06-prompt-contract-and-cognitive-governance.md`
- `07-policy-priority-lattice-and-tiering.md`
- `08-plan-toml-semantics-and-override-math.md`
- `09-scheduler-state-machine-and-call-pipeline.md`
- `10-confirmation-protocol-and-human-gating.md`
- `11-message-bus-correlation-contract.md`
- `12-mcp-readonly-semantics-and-mixed-priority-risk.md`
- `13-mas-subagent-policy-and-recursion-control.md`
- `14-complete-task-protocol-and-recovery-logic.md`
- `15-cli-plan-ux-and-noninteractive-semantics.md`
- `16-telemetry-and-evaluation-signals.md`
- `17-security-threat-model-and-failure-analysis.md`
- `18-test-backed-invariants-catalog.md`
- `19-research-critiques-and-forward-design.md`
- `20-architecture-patterns-and-implementation-recipes.md`
- `21-glossary-formalism.md`

## Source evidence basis

All claims are grounded in inspected source from:
- `packages/core/src/tools/*plan*`
- `packages/core/src/prompts/*`
- `packages/core/src/policy/*`
- `packages/core/src/scheduler/*`
- `packages/core/src/confirmation-bus/*`
- `packages/core/src/agents/*`
- `packages/cli/src/*` (Plan UX and non-interactive behavior)
- corresponding tests in `*.test.ts`
