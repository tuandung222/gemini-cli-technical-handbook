# Scheduler And Execution Pipeline

## 1) Hai lớp scheduler đang hiện diện

Codebase có cả:

- `CoreToolScheduler` (`packages/core/src/core/coreToolScheduler.ts`)
- `Scheduler` mới (`packages/core/src/scheduler/scheduler.ts`)

Cả hai đều xử lý lifecycle tool calls theo state machine.

## 2) Pipeline chuẩn

1. nhận batch tool call requests
2. resolve tool từ registry
3. validate params, tạo invocation
4. policy check + confirmation resolution
5. execute qua `ToolExecutor`
6. chuyển kết quả thành function responses
7. hoàn tất batch và trả về cho agent loop

## 3) ToolExecutor chi tiết

`packages/core/src/scheduler/tool-executor.ts`:

- xử lý live output callback
- xử lý special case cho shell PID tracking
- truncate output dài (đặc biệt shell) và lưu file tạm
- map success/error/cancelled thành `CompletedToolCall`

## 4) Hook integration trong execution

`executeToolWithHooks()` (`packages/core/src/core/coreToolHookTriggers.ts`) chèn:

- BeforeTool (có thể block/modify input)
- AfterTool (có thể block/augment output)

Điều này biến execution pipeline thành policyable runtime.

## 5) Operational risks

- queue contention khi nhiều tool call nối tiếp.
- behavior khó debug nếu hook sửa input ngầm.
- tool output lớn gây áp lực token window.

## 6) Khuyến nghị kỹ thuật

- thống nhất một scheduler abstraction và deprecate lớp cũ.
- emit explicit event khi hook modify params.
- thêm per-tool timeout budget/parallelism policy.

