# 05. Tool Surface Restriction and Capability Theory

## Capability baseline in Plan Mode

Canonical plan-mode tool list:
- `glob`
- `grep_search`
- `read_file`
- `list_directory`
- `google_web_search`
- `ask_user`
- `activate_skill`
- `exit_plan_mode`

Evidence:
- `packages/core/src/tools/tool-names.ts:120`.

Prompt additionally advertises write/edit only for plan files in plan directory (subject to policy):
- `packages/core/src/prompts/snippets.ts:457`.

## Dynamic registration behavior

`syncPlanModeTools` mutates registry by mode:
- PLAN mode removes `enter_plan_mode`, inserts `exit_plan_mode`.
- Non-PLAN does reverse, gated by feature flag.

Evidence:
- `packages/core/src/config/config.ts:1740` to `packages/core/src/config/config.ts:1758`.
- tests: `packages/core/src/config/config.test.ts:2531`.

## Important separation

Plan-mode tool visibility in prompt is not equivalent to runtime ability.

Runtime ability = prompt suggestion intersected with policy decision.

Example:
- Prompt may list plan write/edit semantics.
- Policy still denies writes not matching plans path regex.

Evidence:
- `packages/core/src/policy/policies/plan.toml:50`.
- integration tests deny outside plans dir:
  `packages/cli/src/config/policy-engine.integration.test.ts:356`.

## Ask-user asymmetry

In Plan Mode, `ask_user` and `exit_plan_mode` remain `ask_user` decisions by policy (not direct allow):
- `packages/core/src/policy/policies/plan.toml:45`.

This preserves explicit human control at two strategic checkpoints:
1. ambiguity/approach selection,
2. plan approval and mode exit.

## UX masking nuance

`write_file` and `replace` calls are hidden from standard history UI in Plan Mode:
- `packages/core/src/utils/tool-utils.ts:40` to `packages/core/src/utils/tool-utils.ts:62`.

This is a presentation choice; execution still occurs and is policy-enforced.

## Capability-theory interpretation

Plan Mode implements a constrained capability environment where:
- exploration capabilities are broad but non-destructive,
- artifact authoring is narrowly scoped by path/extension regex,
- authority to execute implementation is separated by explicit approval protocol.
