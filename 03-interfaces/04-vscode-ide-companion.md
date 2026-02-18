# VSCode IDE Companion

## 1) Vai trò

`packages/vscode-ide-companion` là extension bridge giữa IDE và CLI runtime.

File chính:

- `packages/vscode-ide-companion/src/extension.ts`

## 2) Chức năng nổi bật

- chạy `IDEServer` để trao đổi context với CLI
- quản lý diff accept/cancel qua custom URI scheme
- đồng bộ env vars theo workspace trust/folder changes
- command chạy nhanh `gemini` trong terminal IDE

## 3) Cơ chế diff workflow

- dùng `DiffContentProvider` và `DiffManager`
- mở tài liệu dạng `gemini-diff://...`
- user có command accept/cancel thay đổi

Điều này giảm friction khi model đề xuất chỉnh sửa file.

## 4) Update và managed surfaces

Extension có cơ chế check update marketplace.
Một số surface (ví dụ Firebase Studio, Cloud Shell) được coi là managed.

## 5) Ý nghĩa kỹ thuật

- IDE companion biến CLI từ text-only tool thành workflow coding full-cycle.
- tạo đường đi cho human-in-the-loop review trước khi apply edits.

## 6) Khuyến nghị

- thêm telemetry cho diff accept rate (signal chất lượng edits).
- chuẩn hóa protocol versioning giữa CLI và extension.

