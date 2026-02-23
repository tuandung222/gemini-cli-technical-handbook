# Tool Registry And Declarative Tools

## 1) Abstraction của tool

Trong `packages/core/src/tools/tools.ts`:

- `BaseDeclarativeTool`: mô tả schema, metadata, build invocation
- `BaseToolInvocation`: instance đã validate params và sẵn sàng execute
- `ToolResult`: chuẩn kết quả trả cho model/UI

Thiết kế này tách:

- **declaration time** (tool schema)
- **invocation time** (tool params cụ thể)

## 2) ToolRegistry chức năng chính

`packages/core/src/tools/tool-registry.ts`:

- register/unregister tools
- truy xuất declarations cho model
- sort tools theo nhóm (built-in, discovered, MCP)
- discover tools từ command bên ngoài

Điểm kỹ thuật đáng chú ý:

- hỗ trợ alias và prefix cho discovered/MCP tools
- ghi đè tool name conflict có cảnh báo

## 3) Discovered tools

Discovered tool flow:

- chạy command discovery để lấy catalog
- khi invoke: spawn subprocess với params JSON
- stderr/non-zero/signal -> map thành tool execution error có cấu trúc

Lợi ích:

- mở rộng nhanh theo project-specific tooling.

Rủi ro:

- ranh giới bảo mật phụ thuộc policy + sandbox + command hygiene.

## 4) Tool contract và model behavior

Tool schema quality ảnh hưởng trực tiếp:

- tool selection rate
- argument correctness
- safety profile

Đây là vùng AI Agent Engineer cần tối ưu nhiều nhất.

## 5) Khuyến nghị

- thêm golden tests cho schema/tool description (steering stability).
- chuẩn hóa error taxonomy cho toàn bộ tools.
- thêm telemetry field cho tool argument validation failure.

