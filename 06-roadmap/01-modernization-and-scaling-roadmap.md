# Modernization And Scaling Roadmap

## Giai đoạn 1: Làm gọn kiến trúc lõi

- Tách `Config` thành các context nhỏ: runtime, governance, observability.
- Hợp nhất scheduler abstraction và deprecate đường cũ.
- Chuẩn hóa event schema xuyên model/tool/hook/policy.

## Giai đoạn 2: Nâng cấp governance

- Policy rule analyzer (conflict/overlap/unreachable rules).
- Hook sandboxing + resource budgets + deterministic timeout policy.
- Bổ sung policy simulation mode cho CI.

## Giai đoạn 3: Reliability theo SLO

- định nghĩa SLO cho latency, fallback, tool error.
- tự động cảnh báo regression từ telemetry + eval trend.
- chaos drills cho MCP failure, checker timeout, fallback storms.

## Giai đoạn 4: Platform extension maturity

- versioned extension contracts
- compatibility matrix giữa core và companion/sdk
- migration guides cho plugin authors

## Giai đoạn 5: Data-driven continuous improvement

- thống nhất quality scorecard: tests + evals + prod telemetry
- tự động tạo release risk report trước promote stable
- đóng vòng feedback từ incidents vào prompt/policy/tool design

