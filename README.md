# Gemini CLI Technical Handbook

Bộ tài liệu này phân tích sâu mã nguồn `google-gemini/gemini-cli` cho bốn nhóm độc giả:

- Agent Architect
- AI Agent Engineer
- LLM Engineer
- LLM/Agent Data Scientist

## Mục tiêu

- Bóc tách kiến trúc runtime và boundary giữa các package.
- Làm rõ cơ chế agent loop, tool scheduling, policy, hooks, routing, fallback.
- Tạo playbook hành động theo từng persona kỹ thuật.
- Đề xuất hướng refactor/scale dựa trên hiện trạng code.

## Phiên bản phân tích

- Source repo: `https://github.com/google-gemini/gemini-cli`
- Snapshot đã phân tích: nhánh `main`, commit `f1aa38b25`
- Thời điểm phân tích: 2026-02-18

## Cấu trúc tài liệu

### 00-overview
- `00-overview/01-executive-overview.md`
- `00-overview/02-codebase-at-a-glance.md`
- `00-overview/03-reading-roadmap-by-persona.md`

### 01-architecture
- `01-architecture/01-system-context-and-boundaries.md`
- `01-architecture/02-runtime-sequence-end-to-end.md`
- `01-architecture/03-agent-loop-state-machine.md`
- `01-architecture/04-package-dependency-map.md`

### 02-core-systems
- `02-core-systems/01-configuration-bootstrap.md`
- `02-core-systems/02-prompt-memory-context-assembly.md`
- `02-core-systems/03-tool-registry-and-declarative-tools.md`
- `02-core-systems/04-scheduler-and-execution-pipeline.md`
- `02-core-systems/05-policy-engine-and-approvals.md`
- `02-core-systems/06-hooks-governance-model.md`
- `02-core-systems/07-model-routing-fallback-and-availability.md`
- `02-core-systems/08-observability-safety-and-reliability.md`

### 03-interfaces
- `03-interfaces/01-cli-interactive-and-noninteractive.md`
- `03-interfaces/02-sdk-agent-abstractions.md`
- `03-interfaces/03-a2a-server-design.md`
- `03-interfaces/04-vscode-ide-companion.md`

### 04-quality-ops
- `04-quality-ops/01-testing-strategy.md`
- `04-quality-ops/02-evals-and-release-confidence.md`

### 05-personas
- `05-personas/01-agent-architect-playbook.md`
- `05-personas/02-ai-agent-engineer-playbook.md`
- `05-personas/03-llm-engineer-playbook.md`
- `05-personas/04-llm-agent-data-scientist-playbook.md`

### 06-roadmap
- `06-roadmap/01-modernization-and-scaling-roadmap.md`

### appendix
- `appendix/01-source-index.md`

## Lộ trình đọc nhanh

- Agent Architect: bắt đầu từ `01-architecture` -> `02-core-systems` -> `05-personas/01-*`
- AI Agent Engineer: bắt đầu từ `02-core-systems/03,04,05,06` -> `03-interfaces/01,02` -> `05-personas/02-*`
- LLM Engineer: bắt đầu từ `02-core-systems/02,07,08` -> `04-quality-ops/02-*` -> `05-personas/03-*`
- Data Scientist: bắt đầu từ `02-core-systems/08` -> `04-quality-ops` -> `05-personas/04-*`

## Kế hoạch thực thi tài liệu (đã hoàn tất)

1. Clone source và đóng băng snapshot commit để phân tích ổn định.
2. Lập bản đồ package, module lõi, và luồng runtime.
3. Viết tài liệu theo 3 lớp: architecture, systems, operational playbook.
4. Thêm index nguồn tham chiếu để truy vết trực tiếp vào code.

