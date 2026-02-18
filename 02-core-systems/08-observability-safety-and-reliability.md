# Observability Safety And Reliability

## 1) Telemetry nền tảng

`packages/core/src/telemetry/index.ts` export đầy đủ cho:

- traces
- metrics
- logs
- performance monitors
- activity/memory monitors

Target hỗ trợ:

- local
- GCP exporters

Điều này phù hợp cho cả local debug và production telemetry pipeline.

## 2) Reliability mechanisms

Các cơ chế chính:

- retry with backoff ở nhiều lớp
- invalid stream detection/retry
- loop detection
- chat compression để giữ context trong token budget
- tool output truncation + persisted artifacts

Tham chiếu:

- `packages/core/src/core/geminiChat.ts`
- `packages/core/src/services/chatCompressionService.ts`
- `packages/core/src/scheduler/tool-executor.ts`

## 3) Safety checker runner

`CheckerRunner` (`packages/core/src/safety/checker-runner.ts`) hỗ trợ:

- in-process checker
- external checker process (spawn + timeout + schema validation)

Fail-safe mặc định:

- checker lỗi/time out -> `DENY`

Đây là lựa chọn an toàn cho enterprise governance.

## 4) Compression logic và token economy

`ChatCompressionService`:

- trigger theo ngưỡng token
- giữ phần recent history (preserve threshold)
- truncate function responses lớn về placeholder có file ref

Trade-off:

- giảm token pressure vs nguy cơ mất chi tiết lịch sử.

## 5) Đề xuất quan sát hệ thống

Nhóm metric tối thiểu nên có dashboard:

- model latency, error rate, fallback rate
- tool call success/error/cancel ratio
- ask_user/deny ratio theo tool
- compression frequency + pre/post token counts
- hook intervention rate

## 6) Maturity checklist

- Có correlation id xuyên model/tool events.
- Có incident playbook khi fallback spike.
- Có regression detection từ eval + telemetry song song.

