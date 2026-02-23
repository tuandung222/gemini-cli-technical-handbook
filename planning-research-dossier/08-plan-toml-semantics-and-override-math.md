# 08. Plan TOML Semantics and Override Math

## Core plan policy

`plan.toml` encodes a two-phase rule strategy:
1. catch-all deny in Plan Mode,
2. explicit allow/ask overrides for authorized operations.

Evidence:
- catch-all deny: `packages/core/src/policy/policies/plan.toml:30`.
- explicit allow/ask: `packages/core/src/policy/policies/plan.toml:38`.

## Path-restricted write/edit allowance

`write_file` and `replace` are only allowed when `file_path` matches plans-dir markdown regex.

Evidence:
- `packages/core/src/policy/policies/plan.toml:50`.

Integration tests verify both sides:
- allow inside plans dir: `packages/cli/src/config/policy-engine.integration.test.ts:329`.
- deny outside plans dir: `packages/cli/src/config/policy-engine.integration.test.ts:356`.

## Deny message semantics

Catch-all deny includes explicit user/model message about script execution block:
- `packages/core/src/policy/policies/plan.toml:34`.

Test confirms deny message propagation:
- `packages/core/src/policy/policy-engine.test.ts:2337`.

## Mathematical override pattern

Using transformed priorities (default tier):
- catch-all deny: `60 -> 1.060`
- explicit allow: `70 -> 1.070`

Therefore explicit allow set dominates deny for named tools and scoped write/edit paths.

## Non-interactive compatibility

`ask_user` decisions become `deny` in non-interactive mode by policy engine semantics:
- `packages/core/src/policy/policy-engine.ts:614`.

CLI additionally excludes prompt-requiring tools in non-interactive contexts:
- `packages/cli/src/config/config.ts:633`.

## Practical read

The plan policy is a closed-world contract:
- default denied,
- only narrowly carved exceptions,
- exceptions themselves can require human confirmation (`ask_user`).
