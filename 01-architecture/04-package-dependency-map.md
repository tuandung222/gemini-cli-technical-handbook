# Package Dependency Map

## 1) Bản đồ phụ thuộc cấp package

- `cli` -> phụ thuộc mạnh vào `core`
- `sdk` -> phụ thuộc `core` APIs để dựng programmable agent
- `a2a-server` -> phụ thuộc `core` cho config/runtime/tooling
- `vscode-ide-companion` -> dùng `core` ide definitions/protocol constants

## 2) Bản đồ phụ thuộc nội bộ core (logic domains)

- `core/*` gọi:
  - `config/*`
  - `tools/*`
  - `scheduler/*`
  - `policy/*`
  - `hooks/*`
  - `services/*`
  - `telemetry/*`

`Config` là điểm hội tụ dependency.

## 3) Tác động kỹ thuật

Ưu điểm:

- trung tâm hóa wiring và lifecycle.

Nhược điểm:

- nguy cơ phình interface của `Config`.
- tăng coupling test nếu không phân tách contract interface.

## 4) Mẫu tổ chức có thể tham khảo

- Tách `RuntimeKernel` (model loop + scheduler) khỏi `PlatformContext` (storage/policy/hook/telemetry).
- Dùng interfaces nhỏ cho từng concern thay vì pass full `Config` vào mọi nơi.
- Giảm implicit dependency để tăng khả năng benchmark từng module.

## 5) Checklist kiểm soát phụ thuộc

- Module mới có phụ thuộc ngược lên UI không?
- Có thể khởi tạo module bằng interface tối thiểu thay vì full config không?
- Test module này có cần boot toàn hệ thống không?

