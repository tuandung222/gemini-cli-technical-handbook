# Agent Architect Playbook

## 1) Quyết định kiến trúc cần đóng đinh

- Boundary runtime: UI vs orchestration vs governance vs observability.
- Extension model: tool/plugin/hook contracts có versioning.
- Safety model: policy-first hay checker-first, và precedence cụ thể.

## 2) Điểm học trực tiếp từ Gemini CLI

- dùng core runtime trung tâm để phục vụ nhiều interface (`cli`, `sdk`, `a2a`, `vscode`).
- đặt governance ở core để behavior nhất quán xuyên channel.
- xây release-confidence pipeline có cả deterministic tests lẫn behavior evals.

## 3) Architectural anti-pattern cần tránh

- runtime context object quá lớn (God object).
- plugin side-effects không quan sát được.
- thiếu contract tests giữa interface và runtime.

## 4) Blueprint áp dụng cho tổ chức

1. Tạo `RuntimeKernel` độc lập UI.
2. Định nghĩa `ToolContract`, `PolicyContract`, `HookContract` chuẩn.
3. Xây telemetry schema chuẩn từ đầu.
4. Thiết kế release lanes: nightly -> preview -> stable.

## 5) KPI kiến trúc

- mean time to isolate regression cause
- percentage behavior regressions caught pre-release
- tool safety incident rate

