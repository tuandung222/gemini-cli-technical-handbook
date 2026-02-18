# Policy Engine And Approvals

## 1) Vai trò

`PolicyEngine` (`packages/core/src/policy/policy-engine.ts`) quyết định runtime action cho tool call:

- `ALLOW`
- `DENY`
- `ASK_USER`

Nó là lớp kiểm soát trung tâm trước khi tool chạy.

## 2) Inputs quyết định

- tool name (hỗ trợ wildcard `server__*`)
- args pattern
- approval mode hiện tại (`DEFAULT`, `AUTO_EDIT`, `YOLO`, ...)
- server context (đặc biệt MCP)
- shell parsing semantics (subcommands, redirection)

## 3) Shell-specific policy logic

Policy engine parse shell command và đánh giá từng phần:

- command chain được split
- nếu có redirection không cho phép -> hạ cấp sang `ASK_USER`
- nếu một phần bị `DENY` -> toàn câu lệnh `DENY`

Điểm mạnh: giảm false-safe với command phức hợp.

## 4) Approval workflow

- Tool invocation gửi decision request qua `MessageBus`
- Scheduler/legacy listeners phản hồi `requiresUserConfirmation`
- UI xử lý confirm và có thể update policy rule về sau

Các cấu phần chính:

- `packages/core/src/tools/tools.ts`
- `packages/core/src/confirmation-bus/*`
- `packages/core/src/scheduler/confirmation.ts`

## 5) Rủi ro và anti-pattern

- policy rule quá rộng dễ mở cửa ngoài ý muốn.
- approval mode thay đổi động cần audit telemetry.
- cần tránh hành vi “silent allow” cho unknown tool classes.

## 6) Khuyến nghị

- thêm policy linting (phát hiện overlap/contradiction rules).
- log `matched rule id` cho mỗi decision để audit truy vết.
- tách policy cho environment local vs CI/headless.

