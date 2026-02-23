# Codebase At A Glance

## 1) Monorepo topology

Cấu trúc chính:

- `packages/cli`: CLI app, UI, command handling, settings/hydration
- `packages/core`: runtime cốt lõi, model client, tools, scheduler, policy, hooks
- `packages/sdk`: lập trình agent bằng code (tool definitions, sendStream loop)
- `packages/a2a-server`: HTTP/A2A surface cho agent execution
- `packages/vscode-ide-companion`: VS Code extension để đồng bộ IDE context/diff

## 2) Điểm vào runtime

- CLI runtime entry: `packages/cli/src/gemini.tsx`
- Core configuration and bootstrap: `packages/core/src/config/config.ts`
- Agent loop orchestration: `packages/core/src/core/client.ts`
- Turn/event abstraction: `packages/core/src/core/turn.ts`

## 3) Hệ module nổi bật trong core

- Tooling: `packages/core/src/tools/*`
- Scheduling: `packages/core/src/scheduler/*`
- Policy: `packages/core/src/policy/*`
- Hooks: `packages/core/src/hooks/*`
- Routing/fallback: `packages/core/src/routing/*`, `packages/core/src/fallback/*`, `packages/core/src/availability/*`
- Memory/context: `packages/core/src/services/contextManager.ts`, `packages/core/src/utils/memoryDiscovery.ts`
- Telemetry/safety: `packages/core/src/telemetry/*`, `packages/core/src/safety/*`

## 4) Sơ đồ phụ thuộc khái niệm

- `cli` gọi `core` như backend runtime.
- `core` sở hữu registry và execution semantics của tools.
- `sdk` tận dụng trực tiếp `core` API (Config, GeminiEventType, scheduling).
- `a2a-server` và `vscode-companion` mở rộng bề mặt tương tác ngoài terminal.

## 5) Tín hiệu trưởng thành hệ thống

- Test lớn ở cả unit + integration + eval hành vi.
- Workflow release nhiều nhánh: nightly/preview/stable, smoke, rollback, patch.
- Có tài liệu chính thức khá đầy đủ trong `docs/` nhưng chưa đóng gói sâu theo persona kỹ thuật; đây là khoảng trống bộ handbook này lấp đầy.

