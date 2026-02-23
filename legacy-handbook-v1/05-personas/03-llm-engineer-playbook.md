# LLM Engineer Playbook

## 1) Mục tiêu

- tăng chất lượng steering (đúng tool, đúng thời điểm, đúng output format)
- giảm token waste
- cải thiện stability khi model/network biến động

## 2) Vùng code ưu tiên

- `packages/core/src/core/prompts.ts`
- `packages/core/src/services/contextManager.ts`
- `packages/core/src/services/modelConfigService.ts`
- `packages/core/src/routing/modelRouterService.ts`
- `packages/core/src/fallback/handler.ts`
- `packages/core/src/services/chatCompressionService.ts`

## 3) Thực hành tuning

- tách alias model theo use-case: chat, compression, subagent.
- dùng scoped overrides (`overrideScope`, `isRetry`) thay vì hardcode model.
- đo hiệu quả routing bằng pass-rate + latency + fallback rate.

## 4) Rủi ro đặc trưng

- prompt bloat từ memory/context layers.
- fallback model thay đổi behavior mà test deterministic không bắt được.
- compression làm mất chi tiết dẫn đến reasoning drift.

## 5) Checklist trước release

- eval suites (always + usually) không regress.
- fallback spikes không bất thường trên telemetry.
- token efficiency không xấu đi trên workload chuẩn.

