# 07. Policy Priority Lattice and Tiering

## Decision algebra

`PolicyDecision` domain:
- `allow`
- `deny`
- `ask_user`

Evidence:
- `packages/core/src/policy/types.ts:9`.

## Tier transformation formula

TOML rule priority is transformed by tier:

`effective_priority = tier + priority / 1000`

where:
- default tier = 1,
- user tier = 2,
- admin tier = 3.

Evidence:
- tier constants: `packages/core/src/policy/config.ts:39`.
- transform function: `packages/core/src/policy/toml-loader.ts:200`.

## Why this matters

This guarantees ordering invariant:
`admin > user > default`, independent of local file priority values.

Example:
- default priority 999 => 1.999
- user priority 1 => 2.001
Thus any user-tier rule still outranks any default-tier rule.

## Dynamic/settings band in user tier

Documented dynamic bands:
- 2.95: user confirmed always-allow
- 2.9: MCP excluded
- 2.4: excluded tools
- 2.3: allowed tools
- 2.2: trusted MCP servers
- 2.1: allowed MCP servers

Evidence:
- comments + insertion logic:
  `packages/core/src/policy/config.ts:216` to `packages/core/src/policy/config.ts:340`.

## Plan-mode interaction with subagent dynamic policy

Subagent dynamic policy constant:
- `PRIORITY_SUBAGENT_TOOL = 1.05`

Evidence:
- `packages/core/src/policy/types.ts:287`.

Plan catch-all deny in default tier becomes 1.060, so it outranks 1.05.

Regression tests assert this:
- `packages/core/src/policy/policy-engine.test.ts:1485`.
- `packages/core/src/policy/toml-loader.test.ts:546`.

## Safety implication

This numeric ordering ensures that enabling local subagent tools does not silently bypass Plan Mode. The guarantee is arithmetic, not heuristic.
