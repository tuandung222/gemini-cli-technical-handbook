# Configuration Bootstrap

## 1) Vai trò của `Config`

`Config` trong `packages/core/src/config/config.ts` là trung tâm lifecycle:

- chứa runtime settings
- khởi tạo service graph
- tạo tool registry
- tạo `GeminiClient`
- đồng bộ policy/hook/skills/mcp

## 2) Trình tự khởi tạo thực tế

Từ `initialize()` -> `_initialize()`:

1. init storage
2. thêm workspace/pending directories
3. init file discovery + optional git service
4. init prompt/resource registry
5. init agent registry
6. tạo tool registry + discover/sort tools
7. khởi động MCP clients + extensions
8. discover skills và re-register `ActivateSkillTool` nếu cần
9. init hook system (nếu bật)
10. init context manager (JIT context)
11. init `GeminiClient`
12. sync plan-mode tools

## 3) Auth và content generator

`refreshAuth()` dựng `ContentGenerator` theo auth mode:

- OAuth login
- Gemini API key
- Vertex AI
- Compute ADC

Tham chiếu:

- `packages/core/src/core/contentGenerator.ts`

Điểm quan trọng:

- reset model availability khi đổi auth
- strip thoughts nếu chuyển auth mode không tương thích history

## 4) Tool registration strategy

`createToolRegistry()` đăng ký core tools có điều kiện:

- file tools (`ls`, `read-file`, `glob`, `grep`/`ripgrep`)
- mutating tools (`edit`, `write-file`, `shell`, `memory`)
- planning tools, ask-user, write-todos
- subagent tools (from `AgentRegistry`)

Sau đó chạy `discoverAllTools()` và `sortTools()`.

## 5) Đánh giá kiến trúc bootstrap

Điểm mạnh:

- một đường khởi tạo rõ ràng, ít hidden magic.
- hỗ trợ mode-based initialization (interactive/non-interactive).

Rủi ro:

- `Config` quá nhiều trách nhiệm.
- thời gian startup tăng khi nhiều extension/server.

## 6) Khuyến nghị

- tách startup phases thành profile đo được (cold-start budget).
- chuẩn hóa dependency injection cho test nhỏ hơn.
- thêm health report cuối bootstrap để debug production incidents.

