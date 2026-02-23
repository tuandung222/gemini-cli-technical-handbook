# 12. MCP ReadOnly Semantics and Mixed Priority Risk

## MCP read-only hint onboarding

During MCP tool discovery:
- if `annotations.readOnlyHint === true`, tool is tagged read-only,
- policy rule is dynamically added for Plan mode.

Evidence:
- extraction: `packages/core/src/tools/mcp-client.ts:1017`.
- dynamic rule insertion: `packages/core/src/tools/mcp-client.ts:1037`.

## Added rule characteristics

Inserted rule:
- `decision: ASK_USER`
- `priority: 50`
- `modes: [PLAN]`
- source annotation identifies server.

Evidence:
- `packages/core/src/tools/mcp-client.ts:1039`.
- tests: `packages/core/src/tools/mcp-client.test.ts:391`.

## Critical priority nuance

This dynamic priority (`50`) is not transformed by tier formula because it is inserted directly at runtime, not loaded from TOML.

Implication:
- It numerically dominates all tiered TOML priorities (1.x, 2.x, 3.x).

This can be desirable (strong explicit override) but creates a **mixed-priority scale** hazard:
- some rules are in transformed range (e.g., 1.070),
- some dynamic rules are raw integers (e.g., 50).

## Governance effect in Plan mode

Given plan catch-all deny is around 1.060, raw 50 ASK_USER dominates. Therefore read-only MCP tools can become user-confirmed callable in Plan mode even when catch-all deny would otherwise apply.

This appears intentional for read-only tools, but should be documented as explicit design choice.

## Recommendation

Normalize all runtime inserted priorities to same transformed scale or define explicit “priority classes” API to prevent accidental dominance from raw numbers.

Potential hardening options:
1. inject as `1.05`-style plan-band priority,
2. introduce priority namespaces (tiered + absolute),
3. enforce priority validator in `PolicyEngine.addRule`.
