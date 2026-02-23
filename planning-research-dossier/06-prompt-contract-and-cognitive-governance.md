# 06. Prompt Contract and Cognitive Governance

## Why prompt still matters in a policy-enforced system

Policy can block dangerous actions, but prompt governs reasoning behavior:
- when to plan,
- how to decompose,
- when to consult user,
- what artifact structure is mandatory.

In `gemini-cli`, this is explicit and mode-aware.

## Dynamic prompt composition

`PromptProvider` computes:
- current approval mode,
- enabled tools,
- approved plan path,
- modern/legacy snippet family.

Evidence:
- mode detection: `packages/core/src/prompts/promptProvider.ts:53`.
- plan tools list build: `packages/core/src/prompts/promptProvider.ts:67`.
- approved plan injection: `packages/core/src/prompts/promptProvider.ts:165`.

## Planning workflow contract

`renderPlanningWorkflow` defines explicit normative behavior:
- read-only exploration,
- plan file in designated directory,
- inquiry vs directive branching,
- required plan structure,
- workflow phases with consult and approval request.

Evidence:
- section body: `packages/core/src/prompts/snippets.ts:444`.
- rules and structure: `packages/core/src/prompts/snippets.ts:461`.
- workflow steps: `packages/core/src/prompts/snippets.ts:482`.

## Approved-plan continuation contract

If approved plan exists, prompt instructs:
- read plan first,
- iterate existing plan by default,
- create new plan only if explicitly requested.

Evidence:
- `packages/core/src/prompts/snippets.ts:491`.

## Prompt as a “cognitive governor”

In this system, prompt performs soft-governance while policy performs hard-governance.

Soft-governance examples:
- classify request as inquiry vs directive.
- decide whether to consult user on multi-approach tasks.

Hard-governance examples:
- deny shell in plan mode regardless of model intention.
- deny write/edit outside plans path.

## Failure mode: prompt-policy drift

A future risk is drift where prompt advertises behavior not matched by policies, or policies allow actions that prompt assumes impossible. This dossier recommends routine drift audits (see `19-research-critiques-and-forward-design.md`).
