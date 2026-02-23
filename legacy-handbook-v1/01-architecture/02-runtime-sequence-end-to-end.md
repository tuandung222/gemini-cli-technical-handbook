# Runtime Sequence End To End

## 1) Luồng chính (interactive)

1. User nhập prompt trong CLI.
2. `packages/cli` gọi vào core client stream.
3. `GeminiClient.sendMessageStream()` tạo/duy trì vòng lặp turn.
4. Model trả về text chunk hoặc function call.
5. Tool calls được schedule qua `Scheduler`/`ToolExecutor`.
6. Tool responses quay lại model như function response parts.
7. Model tổng hợp final response -> CLI render cho user.

File tham chiếu:

- `packages/cli/src/gemini.tsx`
- `packages/core/src/core/client.ts`
- `packages/core/src/core/turn.ts`
- `packages/core/src/scheduler/scheduler.ts`

## 2) Các gate kiểm soát trong sequence

Trước model call:

- Hook `BeforeAgent`, `BeforeModel`, `BeforeToolSelection`
- Memory/context assembly
- Model routing decision

Trước tool execute:

- Policy decision (`ALLOW`, `DENY`, `ASK_USER`)
- Safety checker (optional)
- Confirmation bus

Sau tool execute:

- Hook `AfterTool`
- Tool output masking/truncation
- Telemetry events

## 3) Sequence khi fallback model xảy ra

Khi model lỗi (quota/capacity/retry semantics):

- classify failure kind
- đọc policy chain
- availability service đánh dấu trạng thái model
- chọn fallback candidate khả dụng
- quyết định silent retry / ask user theo policy action

Tham chiếu:

- `packages/core/src/fallback/handler.ts`
- `packages/core/src/availability/modelAvailabilityService.ts`
- `packages/core/src/routing/modelRouterService.ts`

## 4) Sequence khi compression xảy ra

Khi context vượt ngưỡng token:

- chạy pre-compress hook
- chọn split point theo tỉ lệ preserve
- nén/truncate function responses lớn
- thay thế history bằng summary + phần recent giữ lại

Tham chiếu:

- `packages/core/src/services/chatCompressionService.ts`

## 5) Điều cần theo dõi khi vận hành

- tỷ lệ tool call bị deny/ask_user
- số lần fallback theo model/failure kind
- tỷ lệ compression và ảnh hưởng chất lượng response
- latency phân rã theo model call vs tool execution

