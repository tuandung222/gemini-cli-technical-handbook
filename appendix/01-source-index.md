# Source Index

Danh sách file nguồn trọng tâm đã dùng để phân tích:

## Core runtime

- `packages/core/src/config/config.ts`
- `packages/core/src/core/client.ts`
- `packages/core/src/core/turn.ts`
- `packages/core/src/core/geminiChat.ts`
- `packages/core/src/core/contentGenerator.ts`

## Tooling và execution

- `packages/core/src/tools/tools.ts`
- `packages/core/src/tools/tool-registry.ts`
- `packages/core/src/scheduler/scheduler.ts`
- `packages/core/src/scheduler/tool-executor.ts`
- `packages/core/src/core/coreToolHookTriggers.ts`

## Governance và safety

- `packages/core/src/policy/policy-engine.ts`
- `packages/core/src/hooks/hookSystem.ts`
- `packages/core/src/hooks/types.ts`
- `packages/core/src/safety/checker-runner.ts`

## Routing, fallback, context

- `packages/core/src/routing/modelRouterService.ts`
- `packages/core/src/availability/modelAvailabilityService.ts`
- `packages/core/src/fallback/handler.ts`
- `packages/core/src/services/modelConfigService.ts`
- `packages/core/src/services/contextManager.ts`
- `packages/core/src/utils/memoryDiscovery.ts`
- `packages/core/src/services/chatCompressionService.ts`

## Interface layers

- `packages/cli/src/gemini.tsx`
- `packages/cli/src/core/initializer.ts`
- `packages/cli/src/ui/App.tsx`
- `packages/sdk/src/agent.ts`
- `packages/sdk/src/tool.ts`
- `packages/a2a-server/src/http/app.ts`
- `packages/vscode-ide-companion/src/extension.ts`

## Ops docs/workflows

- `docs/architecture.md`
- `docs/integration-tests.md`
- `docs/release-confidence.md`
- `evals/README.md`
- `.github/workflows/*`

