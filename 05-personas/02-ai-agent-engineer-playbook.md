# AI Agent Engineer Playbook

## 1) Mục tiêu thực thi

- thêm/sửa tools mà không phá safety
- tối ưu loop latency và tool success rate
- giảm số vòng lặp vô ích giữa model và tools

## 2) Vùng code nên nắm chắc

- `packages/core/src/tools/*`
- `packages/core/src/scheduler/*`
- `packages/core/src/core/client.ts`
- `packages/core/src/policy/*`
- `packages/core/src/hooks/*`

## 3) Quy trình thêm tool an toàn

1. Thiết kế schema rõ và mô tả tool intent cụ thể.
2. Viết invocation với error mapping nhất quán.
3. Gắn policy rules tối thiểu cần thiết.
4. Thêm unit + integration + eval case cho behavior sử dụng tool.

## 4) Tối ưu runtime loop

- gom tool calls theo batch hợp lý.
- giảm output phình to (truncation strategy).
- instrument latency theo từng phase scheduler.
- theo dõi tỷ lệ `ASK_USER` bất thường (dấu hiệu policy/schema chưa chuẩn).

## 5) Checklist vận hành

- tool mới có respect trust boundaries chưa?
- cancelation signal được propagate đầy đủ chưa?
- telemetry events có đủ để debug post-mortem chưa?

