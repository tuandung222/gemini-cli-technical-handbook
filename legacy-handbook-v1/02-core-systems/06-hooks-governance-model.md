# Hooks Governance Model

## 1) Hook system kiến trúc

`HookSystem` (`packages/core/src/hooks/hookSystem.ts`) gồm:

- `HookRegistry`
- `HookPlanner`
- `HookRunner`
- `HookAggregator`
- `HookEventHandler`

Đây là khung plugin runtime để can thiệp hành vi agent tại nhiều điểm.

## 2) Các hook events quan trọng

Theo `packages/core/src/hooks/types.ts`:

- `BeforeAgent`, `AfterAgent`
- `BeforeModel`, `AfterModel`
- `BeforeToolSelection`
- `BeforeTool`, `AfterTool`
- `PreCompress`, `SessionStart`, `SessionEnd`, `Notification`

## 3) Loại can thiệp

Hooks có thể:

- chặn thực thi (`block`/`deny`)
- dừng agent run (`continue=false`)
- sửa request model/tool config
- sửa tool input
- bổ sung ngữ cảnh thêm vào output

## 4) Ý nghĩa kiến trúc

Ưu điểm:

- governance linh hoạt mà không fork runtime core.
- mở đường cho enterprise compliance/custom guardrails.

Rủi ro:

- behavior emergent phức tạp khi nhiều hook cùng match.
- debug khó nếu thiếu trace đầy đủ trước/sau hook.

## 5) Mô hình vận hành hooks đề xuất

- phân lớp hook: `safety`, `compliance`, `productivity`, `observability`.
- enforce timeout và failure policy rõ cho hook commands.
- version hóa hook contracts để tránh drift khi nâng cấp runtime.

