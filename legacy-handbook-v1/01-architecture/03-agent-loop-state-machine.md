# Agent Loop State Machine

## 1) Mô hình trạng thái logic

Có thể mô hình hóa vòng lặp như state machine:

- `Idle`
- `PreparingTurn`
- `CallingModel`
- `HandlingModelEvents`
- `SchedulingTools`
- `ExecutingTools`
- `SubmittingToolResponses`
- `Completed`
- `Errored`

Tương ứng code:

- `GeminiClient` orchestrates high-level loop: `packages/core/src/core/client.ts`
- `Turn` chuẩn hóa event stream: `packages/core/src/core/turn.ts`
- `Scheduler` quản lý lifecycle tool call: `packages/core/src/scheduler/scheduler.ts`

## 2) Event-driven contracts

`Turn` phát ra typed events:

- content/thought/citation
- tool_call_request / tool_call_response
- retry / invalid_stream / context_window_will_overflow
- agent_execution_stopped / blocked

Điểm mạnh:

- UI tách khỏi logic: UI chỉ consume event stream.
- Dễ test từng event path.

## 3) Retry semantics

Retry xuất hiện ở hai lớp:

- network/content validity retry trong `GeminiChat`
- model availability retry/fallback trong routing/fallback layer

Rủi ro:

- retry chồng nhiều tầng có thể tạo behavior khó dự đoán nếu telemetry không đủ chi tiết.

## 4) Cơ chế chống loop

`LoopDetectionService` theo dõi lặp logic hội thoại/tooling.
Mục tiêu là tránh vòng vô hạn của agent do planning/tool confusion.

## 5) Khuyến nghị cải tiến state model

- Chuẩn hóa state transition graph thành artifact riêng (JSON schema) để test invariant.
- Log transition idempotently để debug incident nhanh.
- Gắn correlation id xuyên suốt model-call và tool-call batch.

