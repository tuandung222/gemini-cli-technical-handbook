# Prompt Memory Context Assembly

## 1) Nguồn context

Context gửi model được lắp từ nhiều lớp:

- System prompt lõi
- User/project memory (GEMINI.md và biến thể)
- Extension memory
- JIT subdirectory memory
- IDE context
- Hội thoại lịch sử (chat history)

Module chính:

- `packages/core/src/services/contextManager.ts`
- `packages/core/src/utils/memoryDiscovery.ts`
- `packages/core/src/core/prompts.ts`
- `packages/core/src/core/client.ts`

## 2) Cơ chế memory discovery

`memoryDiscovery` tìm theo:

- global path (`~/.gemini/...`)
- project upward/downward scan
- trusted folder checks
- import processing (`@import` style logic)

Điểm kỹ thuật quan trọng:

- có concurrency limits để tránh EMFILE khi quét nhiều file.
- track loaded paths để JIT context không nạp trùng.

## 3) ContextManager semantics

`ContextManager.refresh()` phân loại memory thành:

- global
- extension
- project/environment

`discoverContext(accessedPath, trustedRoots)` cho JIT context theo đường dẫn vừa đụng tới.

## 4) Trade-off

Ưu điểm:

- context giàu tín hiệu dự án hơn prompt stateless.
- có trust boundary để hạn chế lộ dữ liệu ngoài workspace.

Hạn chế:

- nhiều nguồn context dễ tạo prompt bloat.
- nếu memory quality thấp thì model dễ bị nhiễu hướng dẫn.

## 5) Thực hành tốt

- giữ GEMINI.md ngắn, có thứ tự ưu tiên rõ.
- dùng memory import có kiểm soát, tránh include toàn bộ docs.
- đo token cost của context và theo dõi drift qua telemetry.

