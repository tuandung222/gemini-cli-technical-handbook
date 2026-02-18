# SDK Agent Abstractions

## 1) Mục tiêu SDK

`packages/sdk` cho phép tạo agent bằng code nhưng reuse runtime lõi của Gemini CLI.

Class chính:

- `GeminiCliAgent` (`packages/sdk/src/agent.ts`)
- `SdkTool` / helper `tool()` (`packages/sdk/src/tool.ts`)

## 2) Agent loop qua SDK

`GeminiCliAgent.sendStream()`:

1. lazy init auth + config + core runtime
2. đăng ký SDK tools vào core registry
3. gửi prompt vào `GeminiClient.sendMessageStream`
4. thu `ToolCallRequest` events
5. schedule tool calls bằng `scheduleAgentTools`
6. gửi function responses trở lại model

## 3) Dynamic instructions

`instructions` có thể là function nhận `SessionContext`:

- đọc transcript/cwd/time
- gọi fs/shell wrappers
- xây system instructions theo ngữ cảnh runtime

Đây là pattern mạnh cho agent specialization.

## 4) Tool definition model

SDK dùng Zod schema -> JSON schema cho model.

Ưu điểm:

- type-safe cho lập trình viên
- model-visible contract rõ ràng

Cơ chế lỗi:

- `ModelVisibleError` để chọn lỗi nào trả về model.

## 5) Gợi ý ứng dụng thực tế

- dùng SDK cho embedded agent trong sản phẩm riêng.
- chuẩn hóa catalog tools theo domain business.
- giữ governance ở core, tránh reimplement policy/scheduler.

