# LLM Agent Data Scientist Playbook

## 1) Trọng tâm phân tích

- đo chất lượng hành vi agent theo thời gian
- tách biến động do model khỏi biến động do code/tool/policy
- thiết kế thí nghiệm đánh giá thay đổi prompt/tool contracts

## 2) Nguồn dữ liệu chính

- telemetry events (`packages/core/src/telemetry/*`)
- eval results (`evals/*` và nightly workflows)
- integration test artifacts
- tool truncation/cancellation/fallback signals

## 3) Bộ metric đề xuất

- Tool Selection Accuracy (theo eval tasks)
- Tool Execution Success Rate
- Fallback Invocation Rate
- AskUser Rate by Tool Category
- Compression Rate + Token Delta
- Mean Turn Completion Time

## 4) Thiết kế thí nghiệm

- A/B prompt/tool description bằng fixed scenario bank.
- bootstrap confidence intervals cho usually-pass evals.
- drift detection theo rolling windows (nightly).

## 5) Tương tác với engineering team

- chuẩn hóa taxonomy lỗi cho gán nhãn tự động.
- mỗi regression cần RCA gắn: root cause domain (prompt/tool/policy/model).
- đưa insight thành action item cụ thể trong release checklist.

