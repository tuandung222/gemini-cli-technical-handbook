# System Context And Boundaries

## 1) Boundary chính

### `packages/cli` (presentation + interaction)

Vai trò:

- lifecycle app terminal, render Ink/React
- phân luồng interactive vs non-interactive
- quản lý UX state, startup, session utilities

File tiêu biểu:

- `packages/cli/src/gemini.tsx`
- `packages/cli/src/nonInteractiveCli.ts`
- `packages/cli/src/ui/App.tsx`
- `packages/cli/src/core/initializer.ts`

### `packages/core` (orchestration + policy + tools)

Vai trò:

- build/refresh `Config`
- tạo `GeminiClient` và điều phối vòng lặp model/tool
- quản lý tool registry/scheduler/policy/hook/telemetry

File tiêu biểu:

- `packages/core/src/config/config.ts`
- `packages/core/src/core/client.ts`
- `packages/core/src/core/turn.ts`
- `packages/core/src/scheduler/scheduler.ts`

### Extension surfaces

- `packages/sdk`: API để lập trình agent custom dùng core runtime.
- `packages/a2a-server`: HTTP/A2A server để chạy agent ngoài terminal.
- `packages/vscode-ide-companion`: bridge IDE context/diff vào CLI flow.

## 2) Boundary logic quan trọng

- UI không trực tiếp execute tool; execution semantics nằm trong core.
- Core không phụ thuộc UI rendering framework, giúp tái dùng runtime ở SDK/server.
- Governance (policy/hooks/safety) cũng nằm trong core -> nhất quán giữa interface.

## 3) Ưu điểm thiết kế

- Tái sử dụng orchestration logic giữa CLI, SDK, A2A.
- Tách test concern: UI test độc lập với core behavior test.
- Cho phép thêm interface mới mà ít sửa logic business.

## 4) Rủi ro boundary

- `Config` trở thành God Object: nhiều concern cùng trú ở một nơi.
- Cross-cutting concern (telemetry, hooks, policy) dễ tạo coupling ngầm.
- Cần kiểm soát contract giữa package để tránh vòng phụ thuộc theo thời gian.

## 5) Kiến nghị cho Architect

- Duy trì contract rõ cho 4 lớp: `interaction`, `runtime`, `governance`, `observability`.
- Đặt API compatibility tests giữa `core` và `sdk`.
- Chuẩn hóa plugin contract để mở rộng tools/hook/checkers mà không chạm lõi.

