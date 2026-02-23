# CLI Interactive And Noninteractive

## 1) Entrypoint

CLI chạy từ `packages/cli/src/gemini.tsx`.

Nhiệm vụ chính:

- parse arguments
- load settings/trusted folders
- startup init/auth/theme
- route vào interactive Ink UI hoặc non-interactive mode

## 2) Interactive mode

Đặc trưng:

- render bằng Ink + React contexts
- streaming state cho output theo thời gian thực
- session lifecycle, keyboard/mouse/alternate buffer handling

File tiêu biểu:

- `packages/cli/src/ui/App.tsx`
- `packages/cli/src/ui/AppContainer.tsx`
- `packages/cli/src/core/initializer.ts`

## 3) Non-interactive mode

Đặc trưng:

- hỗ trợ scripting/CI/headless
- validate auth riêng (`validateNonInterActiveAuth`)
- định dạng output JSON/stream phù hợp machine consumers

File tiêu biểu:

- `packages/cli/src/nonInteractiveCli.ts`
- `packages/cli/src/nonInteractiveCliCommands.ts`

## 4) UX kỹ thuật đáng chú ý

- xử lý relaunch với memory args khi cần
- cleanup hooks cho terminal state/session artifacts
- cơ chế update check/auto-update có kiểm soát

## 5) Khuyến nghị cho Agent Engineer

- test riêng cho headless reliability (stdin piping, exit codes).
- tách telemetry events theo mode interactive/non-interactive.
- harden startup warnings để giảm auth/config confusion.

