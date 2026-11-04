# Tách YangLab thành repository độc lập

Ngày chuẩn bị: 04/11/2026. Đây là chuyển vị trí mã và cập nhật tài liệu/CI;
không viết lại ứng dụng hoặc tạo lịch sử phát triển mới.

## Nguồn chính xác và lịch sử

- Repository nguồn: [nguyenhonggiang2407/DTDD](https://github.com/nguyenhonggiang2407/DTDD).
- Nhánh nguồn: [yanglab](https://github.com/nguyenhonggiang2407/DTDD/tree/yanglab).
- Commit nguồn: [`4ff287fbfdf97ff6c8015888e4fcc83056558f74`](https://github.com/nguyenhonggiang2407/DTDD/commit/4ff287fbfdf97ff6c8015888e4fcc83056558f74).
- Repository đích: [nguyenhonggiang2407/YangLab](https://github.com/nguyenhonggiang2407/YangLab), nhánh `main`.

Clone nhánh nguồn bằng `--single-branch` **không dùng `--depth`**, giữ đủ năm
commit reachable. Nhánh `main` bắt đầu từ commit nguồn và nối thêm commit chuyển
repository. Không filter, squash, rebase hoặc thay author/date của commit.
Nhánh `yanglab` trong DTDD không bị sửa hoặc xóa.

Remote nguồn trong bản chuẩn bị được đổi thành `upstream` để tránh push nhầm
vào DTDD. Một clone mới từ YangLab chỉ có remote đích; thông tin nguồn còn được
giữ trong tài liệu này và lịch sử.

## Vì sao giữ cả monorepo

YangMinutes, YangLaunch và YangSpec dùng chung `server.mjs`, `lib/store.mjs`,
validation, SQLite, phiên workspace, ownership, expected version, revision và CI.
Giữ nguyên cả `apps/`, `lib/`, `public/`, test và runtime chung trong YangLab.
Không cắt ba app thành repository riêng làm thiếu các thành phần phụ thuộc.

README con và screenshot tiếp tục dùng đường dẫn tương đối trong cùng cấu trúc.
Nghiệp vụ, SQLite schema, package lock, Dockerfile và Compose giữ nguyên. Không
di chuyển database, cookie hoặc dữ liệu của ứng dụng đang chạy.

Các thay đổi giới hạn ở clone URL, badge/giới thiệu repository, branch filter
`main` của workflow và tài liệu lần chuyển. Các nhận định về nguồn MIT được giữ
đúng phạm vi; không thay toàn bộ chữ DTDD trong lịch sử hoặc ghi nguồn.

## Giấy phép và nguồn tham chiếu

Giữ nguyên byte `LICENSE`, `NOTICE.md` và toàn bộ `upstream/`. Git object tại
commit nguồn:

| Nội dung         | Git object SHA                             |
| ---------------- | ------------------------------------------ |
| `LICENSE`        | `86f0abeafd90fb3c5f3a29e16d3269515a757ba7` |
| `NOTICE.md`      | `3489c072f8dc21399b02262403faa2ed509fe026` |
| Tree `upstream/` | `d7327ff24eb732f50e30a8a9b2ae1beede898303` |

[NOTICE.md](../NOTICE.md) giữ tác giả patchy631, snapshot từ fork Trinhvhao,
bản tham chiếu MIT và phân định phần port/mô hình tham khảo với phần triển khai
mới của YangLab. Tách repository không thay đổi quyền tác giả của các phần này.

## Kiểm chứng cục bộ

Windows, Node.js 24.19.0; dev dependency được cài từ lockfile bằng `npm ci`.

| Kiểm tra   | Kết quả                                                                                       |
| ---------- | --------------------------------------------------------------------------------------------- |
| `npm test` | 24/24 qua: API/quyền workspace, ngày, checklist, workflow, CSV/ICS, OpenAPI, version và quota |
| Browser    | `PLAYWRIGHT_CHANNEL=chrome npm run test:ui`, dùng tùy chọn channel có sẵn trong test          |
| Docker     | Máy audit không có Docker CLI; không tuyên bố đã build/run container                          |

Browser có **7/7 test qua**. Lệnh browser PowerShell tương ứng:

```powershell
$env:PLAYWRIGHT_CHANNEL = 'chrome'
npm run test:ui
```

Test browser dùng SQLite in-memory, app thật trên loopback và Google Chrome
cài sẵn. Để dùng Chromium do Playwright quản lý, cài bằng
`npx playwright install chromium` rồi chạy `npm run test:ui` không đặt channel.

Máy audit có `core.autocrlf=true`, nên `npm run format:check` mặc định báo khác
line ending ở các file nguồn được checkout thành CRLF. Kiểm tra
`prettier --check . --end-of-line auto` qua; không format lại mã nghiệp vụ hoặc
NOTICE chỉ để đổi line ending của máy audit. CI Linux tiếp tục kiểm tra định dạng
chuẩn của Git source.

Chưa có demo internet hoặc CI repository đích được xác minh trong lần chuẩn bị.
Workspace cookie bảy ngày không phải tài khoản có đăng nhập. YangMinutes không
nhận dạng âm thanh, YangLaunch không tự đăng mạng xã hội và YangSpec không gọi
API thật; các giới hạn này tiếp tục được ghi trong README.
