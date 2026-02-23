# 04. Plan Artifact Lifecycle and Storage

## Plan artifact lifecycle

A plan artifact passes these stages:
1. Directory provisioning.
2. Draft write/update.
3. Path/content validation.
4. Approval binding to session context.
5. Read-back as execution source of truth.

## Storage topology

Plan directory path is under project temp:
- with session id: `<projectTemp>/<sessionId>/plans`.
- without session id: `<projectTemp>/plans`.

Evidence:
- `packages/core/src/config/storage.ts:246` to `packages/core/src/config/storage.ts:250`.

## Initialization behavior

When experimental plan is enabled:
- plans dir is created during config initialization,
- plans dir is added to workspace context.

Evidence:
- `packages/core/src/config/config.ts:942` to `packages/core/src/config/config.ts:946`.
- tests: `packages/core/src/config/config.test.ts:2496`.

## Validation gates

### Path safety

`validatePlanPath`:
- resolves real paths,
- enforces plan file inside plans dir,
- enforces existence.

Evidence:
- `packages/core/src/utils/planUtils.ts:32` to `packages/core/src/utils/planUtils.ts:49`.

### Content safety

`validatePlanContent`:
- rejects empty or unreadable plan files.

Evidence:
- `packages/core/src/utils/planUtils.ts:57`.

### Symlink traversal defense

Tests validate symlink escape denial:
- `packages/core/src/utils/planUtils.test.ts:54`.
- `packages/core/src/tools/exit-plan-mode.test.ts:430`.

## Artifact binding to execution

On approval, `Config` stores the approved path:
- getter/setter: `packages/core/src/config/config.ts:1986`, `packages/core/src/config/config.ts:1990`.
- prompt workflow subsequently instructs model to read this plan first:
  `packages/core/src/prompts/snippets.ts:493`.

## Architectural note

The approved plan path works as a session-level execution anchor. It is not a mere chat summary; it is a persistent control artifact feeding next-turn behavior via prompt regeneration.
