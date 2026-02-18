# Testing Strategy

## 1) Test layers hiện có

### Unit tests

Nằm dày đặc trong `packages/*/src/*.test.ts(x)`.

Mục tiêu:

- xác nhận logic module-level
- kiểm tra edge-cases cho policy, scheduler, tools, hooks

### Integration tests

Nằm trong `integration-tests/`.

Mục tiêu:

- chạy binary thực tế
- verify end-to-end hành vi CLI với filesystem/sandbox matrix

### Behavioral evals

Nằm trong `evals/`.

Mục tiêu:

- đánh giá model steering và decision behavior
- phân loại `ALWAYS_PASSES` vs `USUALLY_PASSES`

## 2) Quy mô hiện trạng

- test files trong `packages/*/src`: ~650
- integration-tests files: 69
- eval files: 20

Đây là độ bao phủ phù hợp cho platform agent phức tạp.

## 3) Cách đọc failure hiệu quả

- Unit fail: thường là regression logic deterministic.
- Integration fail: thường là contract/runtime break (CLI-sandbox-tooling).
- Eval fail: thường là steering drift, prompt/tool description side effects.

## 4) Gợi ý nâng cấp test strategy

- contract tests giữa `core` và `sdk`.
- snapshot tests cho tool schemas/model declarations.
- chaos tests cho fallback/availability/scheduler cancellation.

## 5) Signal cần gắn dashboard

- flaky rate theo test suite
- avg time to detect regression
- pass-rate trend của evals nightly

