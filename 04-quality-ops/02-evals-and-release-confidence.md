# Evals And Release Confidence

## 1) Behavioral eval framework

Theo `evals/README.md`:

- `ALWAYS_PASSES`: dùng cho hành vi bắt buộc ổn định
- `USUALLY_PASSES`: dùng cho hành vi có tính non-deterministic cao hơn

Điểm mạnh:

- phân biệt expectation deterministic vs probabilistic.
- phù hợp bản chất LLM system testing.

## 2) Release confidence checklist

Theo `docs/release-confidence.md`, release gate gồm 3 tầng:

1. automated gates (CI/E2E/smoke)
2. manual verification + dogfooding
3. telemetry/dashboard + model eval review

Đây là mô hình tốt cho sản phẩm AI có rủi ro regression hành vi.

## 3) Workflow maturity

Repo có 37 workflows, bao phủ:

- CI, E2E, evals nightly
- release nightly/preview/stable
- patch/rollback/promote
- issue/PR automation

Tín hiệu: hệ vận hành ở mức production-grade.

## 4) Khuyến nghị cho tổ chức AI platform

- giữ `preview soak period` đủ dài trước stable.
- dùng scorecard hợp nhất giữa eval pass-rate và production telemetry.
- bổ sung “behavior regression RCA template” cho mỗi incident.

