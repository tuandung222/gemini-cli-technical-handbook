# A2A Server Design

## 1) Vai trò

`packages/a2a-server` cung cấp bề mặt server để điều khiển agent qua HTTP/A2A thay vì terminal tương tác trực tiếp.

Điểm vào:

- `packages/a2a-server/src/http/app.ts`

## 2) Cấu trúc thực thi

`createApp()` làm:

- load config/settings/extensions
- optional git checkpoint service
- setup task store (in-memory hoặc GCS)
- tạo `CoderAgentExecutor`
- dựng endpoints A2A + custom endpoints (`/tasks`, `/executeCommand`, `/listCommands`)

## 3) Điểm mạnh

- chuyển runtime CLI thành service interface tương đối nhanh.
- task abstraction giúp hỗ trợ async/streaming workflows.
- có persistence option (GCS) cho môi trường phân tán.

## 4) Điểm cần lưu ý

- cần harden authn/authz vì endpoint có khả năng execute command.
- command registry cần validation input và quota/ratelimit.
- cần request correlation + audit logs đầy đủ.

## 5) Khuyến nghị production

- đặt API gateway + mTLS/OAuth phía trước.
- cách ly executor bằng sandbox container profile rõ.
- thêm idempotency key cho task creation.

