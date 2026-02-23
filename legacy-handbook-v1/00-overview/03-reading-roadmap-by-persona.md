# Reading Roadmap By Persona

## Agent Architect

Mục tiêu: hiểu kiến trúc platform và quyết định boundary.

Thứ tự khuyến nghị:

1. `01-architecture/01-system-context-and-boundaries.md`
2. `01-architecture/02-runtime-sequence-end-to-end.md`
3. `02-core-systems/05-policy-engine-and-approvals.md`
4. `02-core-systems/06-hooks-governance-model.md`
5. `06-roadmap/01-modernization-and-scaling-roadmap.md`

Câu hỏi cần trả lời:

- Boundary nào bắt buộc tách module?
- Governance nằm ở layer nào để không khóa đổi mới?
- Telemetry nào là SLO cho agent runtime?

## AI Agent Engineer

Mục tiêu: xây/chỉnh behavior loop và tool execution ổn định.

Thứ tự khuyến nghị:

1. `02-core-systems/03-tool-registry-and-declarative-tools.md`
2. `02-core-systems/04-scheduler-and-execution-pipeline.md`
3. `02-core-systems/05-policy-engine-and-approvals.md`
4. `03-interfaces/01-cli-interactive-and-noninteractive.md`
5. `03-interfaces/02-sdk-agent-abstractions.md`

Câu hỏi cần trả lời:

- Tool call đi qua những gate nào trước khi execute?
- Làm sao thêm tool mới mà không phá approval UX?
- Cách test regression cho tool behavior?

## LLM Engineer

Mục tiêu: tối ưu model steering, routing, token efficiency.

Thứ tự khuyến nghị:

1. `02-core-systems/02-prompt-memory-context-assembly.md`
2. `02-core-systems/07-model-routing-fallback-and-availability.md`
3. `02-core-systems/08-observability-safety-and-reliability.md`
4. `04-quality-ops/02-evals-and-release-confidence.md`

Câu hỏi cần trả lời:

- Prompt context được assemble/refresh khi nào?
- Fallback logic thay đổi model ra sao theo failure kind?
- Compression có làm méo reasoning không?

## LLM/Agent Data Scientist

Mục tiêu: thiết kế metric, eval protocol, phân tích drift hành vi.

Thứ tự khuyến nghị:

1. `02-core-systems/08-observability-safety-and-reliability.md`
2. `04-quality-ops/01-testing-strategy.md`
3. `04-quality-ops/02-evals-and-release-confidence.md`
4. `05-personas/04-llm-agent-data-scientist-playbook.md`

Câu hỏi cần trả lời:

- Metric nào tách được bug hệ thống vs model variance?
- Khi nào nên chuyển behavior test sang eval probabilistic?
- Cách đọc pipeline release để truy hồi regression?

