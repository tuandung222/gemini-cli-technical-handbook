# Model Routing Fallback And Availability

## 1) Model routing

`ModelRouterService` (`packages/core/src/routing/modelRouterService.ts`) dùng composite strategy:

- FallbackStrategy
- OverrideStrategy
- ClassifierStrategy
- NumericalClassifierStrategy
- DefaultStrategy

Routing decision gồm model + metadata source/reasoning/latency.

## 2) Model availability state

`ModelAvailabilityService` (`packages/core/src/availability/modelAvailabilityService.ts`) theo dõi health state:

- `terminal` (quota/capacity)
- `sticky_retry` (retry_once_per_turn)

Nó quyết định model nào còn khả dụng trong chuỗi fallback.

## 3) Fallback handler

`handleFallback()` (`packages/core/src/fallback/handler.ts`) làm:

- classify failure kind
- resolve policy chain + candidates
- chọn fallback model theo availability
- xử lý intent (`retry_always`, `retry_once`, `stop`, `upgrade`)

Điểm tốt:

- fallback không chỉ là hardcoded switch model, mà policy-aware.

## 4) Model config resolution

`ModelConfigService` (`packages/core/src/services/modelConfigService.ts`) hỗ trợ:

- alias chain inheritance
- scoped overrides (`overrideScope`, `isRetry`, ...)
- precedence có thứ tự rõ

Điều này cho phép tuning theo ngữ cảnh (subagent/retry/chat).

## 5) Rủi ro hệ routing

- chiến lược nhiều tầng khó giải thích nếu thiếu logging nhất quán.
- alias/override phức tạp dễ gây config surprises.
- fallback aggressive có thể đổi chất lượng output ngoài kỳ vọng.

## 6) Khuyến nghị

- xuất “routing explanation object” chuẩn cho debug UI.
- thêm static analyzer cho alias chain/override conflicts.
- thiết lập SLO fallback rate theo model + auth mode.

