# Execution Report — Đăng nhập (`LOGIN`) · Part 01 Smoke (TC 001–011 · 080)

| Thông tin | Nội dung |
|---|---|
| Run ID | run_1791623905 |
| Nền tảng | `web` |
| Loại nguồn | `Bộ TC` |
| Nguồn TC | [docs/testcases/login/web/parts/part_01_web_smoke.md](../../../../testcases/login/web/parts/part_01_web_smoke.md) |
| Phạm vi | 12 TC — toàn bộ part 01: `CRM_LOGIN_TC_001` → `CRM_LOGIN_TC_010` (đợt 1), sau đó user yêu cầu chạy tiếp `CRM_LOGIN_TC_011` · `CRM_LOGIN_TC_080` (đợt 2), rồi chạy lại `CRM_LOGIN_TC_011` khi user đã bổ sung tài khoản customer vào `.env` (đợt 3) · loại 0 TC `@Deprecated` |
| Môi trường | `BASE_URL` trong `.env` (`https://crm.anhtester.com`) — môi trường demo dùng chung |
| Build / Version | `app.version = 316` (theo `docs/requirements/README.md`) |
| Tài khoản | `EMAIL_ADMIN` trong `.env` (Staff — TC_008→010) · `EMAIL_CUSTOMER` trong `.env` (contact cổng khách hàng — TC_011). Người chạy tự nhập Email/Password |
| Trình duyệt | Chromium qua Playwright MCP · headed · viewport `1600×750` |
| Người thực hiện | tphatspeed (agent hỗ trợ) |
| Bắt đầu → Kết thúc | 10-10-2026 16:18 → 16:37 (≈ 19 phút, gồm thời gian chờ người chạy nhập tài khoản và bổ sung `.env`) |
| Môi trường dùng chung? | Có — auto-skip TC phá huỷ đang BẬT (12 TC trong phạm vi không có thao tác phá huỷ) |

> "Cửa sổ ẩn danh mới" được dựng bằng cách xoá toàn bộ cookie của browser context trước TC.

## 1. Tổng kết

| Trạng thái | Số lượng | Tỷ lệ |
|---|---|---|
| ✅ PASS | 12 | 100% |
| ❌ FAIL | 0 | 0% |
| ⚠️ BLOCKED | 0 | 0% |
| ⏭️ SKIPPED | 0 | 0% |
| **Tổng** | **12** | 100% |

> **Pass rate (không tính SKIPPED):** 12/12 = 100% · Tổng TC trong phạm vi: 12 · TC thực chạy: 12

## 2. Kết quả từng TC

| TC ID | Test Scenario | Kết quả | Bước fail | Ghi chú |
|---|---|---|---|---|
| CRM_LOGIN_TC_001 | Trang Login quản trị hiển thị đủ thành phần, đúng nhãn và trạng thái mặc định | ✅ PASS | — | Bảng kiểm 1–4 đạt · bước 4: đích liên kết logo xác nhận qua thuộc tính `href` = `https://crm.anhtester.com/` (thanh trạng thái của trình duyệt không chụp được) · 🔧 `alt` logo = `Perfex CRM \| Anh Tester Demo` ✅ |
| CRM_LOGIN_TC_002 | Trang Quên mật khẩu quản trị hiển thị đủ thành phần và trạng thái mặc định | ✅ PASS | — | Bảng kiểm 1–3 đạt · trang chỉ có 1 liên kết (logo → `https://crm.anhtester.com/`), không có liên kết về Login · tiêu đề tab là `Perfex CRM \| Anh Tester Demo - Login` (TC không chấm) |
| CRM_LOGIN_TC_003 | Trang Login cổng khách hàng hiển thị đủ thành phần, đúng nhãn và trạng thái mặc định | ✅ PASS | — | Bảng kiểm 1–5 đạt · danh sách `Language` đúng 26 ngôn ngữ, 0 dòng trống, `English` đang chọn · `Esc` đóng danh sách, vẫn `English` |
| CRM_LOGIN_TC_004 | Trang Quên mật khẩu cổng khách hàng hiển thị đủ thành phần, đúng nhãn | ✅ PASS | — | Tiêu đề tab đúng `Forgot Password?` · nút xanh `Submit` · chân trang `2026 Copyright Perfex CRM \| Anh Tester Demo` |
| CRM_LOGIN_TC_005 | Chưa đăng nhập mà mở trang quản trị thì bị đưa về trang Login quản trị | ✅ PASS | — | Biến thể `a` (`/admin/`) và `b` (`/admin/clients`) đều về đúng `https://crm.anhtester.com/admin/authentication`, không kèm `?…`, không có nội dung quản trị |
| CRM_LOGIN_TC_006 | Từ Login quản trị mở trang Quên mật khẩu và quay lại bằng Back | ✅ PASS | — | Bước 2 → `/admin/authentication/forgot_password`, tiêu đề `Forgot Password` · bước 3 Back → `/admin/authentication` |
| CRM_LOGIN_TC_007 | Đi qua lại giữa Login cổng và Quên mật khẩu cổng | ✅ PASS | — | Bước 2 → `/authentication/forgot_password` · bước 3 nút `Login` đầu trang → `/authentication/login`, trang `Please login` |
| CRM_LOGIN_TC_008 | Đăng nhập khu quản trị bằng tài khoản Staff hợp lệ thì vào Dashboard | ✅ PASS | — | Người chạy tự nhập Email/Password · `Remember me` không tích · sau `Login`: tiêu đề tab `Dashboard`, địa chỉ `https://crm.anhtester.com/admin/`, không còn form đăng nhập |
| CRM_LOGIN_TC_009 | Đã đăng nhập thì phiên được giữ, mở lại trang Login tự về Dashboard | ✅ PASS | — | Dùng phiên của TC_008 (không ghi nhớ) · F5 vẫn ở Dashboard · mở `/admin/authentication` → tự chuyển về `https://crm.anhtester.com/admin/`, không hiện form |
| CRM_LOGIN_TC_010 | Đăng xuất qua menu ảnh đại diện thì kết thúc phiên và về trang Login | ✅ PASS | — | Pre-condition: 0 bộ đếm giờ đang chạy (huy hiệu đang ẩn) · menu đúng 5 mục theo thứ tự `My Profile` · `My Timesheets` · `Edit Profile` · `Language` ▸ · `Logout` (cuối) · Logout → `/admin/authentication` · mở `/admin/` vẫn bị đưa về Login |
| CRM_LOGIN_TC_011 | Contact đăng nhập cổng khách hàng thành công | ✅ PASS | — | Đợt 2 BLOCKED vì thiếu tài khoản contact → đợt 3 chạy lại bằng `EMAIL_CUSTOMER`. Sau `Login`: rời `/authentication/login`, sang **`https://crm.anhtester.com/`** (trang chủ cổng, lời chào `Good Afternoon Customer!`, menu cổng `Projects` · `Invoices` · `Contracts` · `Estimates` · `Proposals` · `Support`). Không còn form `Please login` và nút `Login` đầu trang, không có `Invalid username or password`, không có chữ `404` / `500` / `Error`. `@NeedsVerify` đã xác nhận trên UI: đề nghị gỡ tag, ghi trang đích vào Expected và đóng `ASM-10` |
| CRM_LOGIN_TC_080 | Bấm logo trên trang Login quản trị thì rời khu quản trị, sang cổng khách hàng | ✅ PASS | — | `@NeedsVerify` đã xác nhận trên UI: đích cuối là `https://crm.anhtester.com/authentication/login` (trang `Please login` của cổng), không chứa `/admin`, không có chữ `404` / `500` / `Error`. Đề nghị gỡ `@NeedsVerify` và đóng `ASM-17` |

## 3. Chi tiết TC FAIL

Không có TC FAIL.

## 4. TC BLOCKED

Không có TC BLOCKED. `CRM_LOGIN_TC_011` BLOCKED ở đợt 2 (chưa có tài khoản contact — `AMB-SYS-03`) và đã PASS khi chạy lại ở đợt 3.

## 5. Dữ liệu đã tạo & dọn dẹp

| Dữ liệu | ID | Nơi tạo | Đã xoá? |
|---|---|---|---|
| — | — | — | — |

> 12 TC trong phạm vi chỉ đọc giao diện và đăng nhập / đăng xuất, **không tạo bản ghi nào**. Phiên đăng nhập của TC_008 đã kết thúc bằng Logout ở TC_010. Phiên contact của TC_011 đã kết thúc bằng `/authentication/logout`; mở lại `/clients/projects` bị đưa về `Please login`. Môi trường giữ nguyên trạng.
>
> Evidence: không chụp ảnh vào `evidence/`. Không có TC FAIL, và ảnh Dashboard chứa dữ liệu nghiệp vụ nên không lưu. Ảnh tạm dùng để kiểm giao diện (logo, nút, menu người dùng) đã xoá sau khi chấm.

## 6. Đề xuất bước tiếp theo

- Không có FAIL → chưa cần `/create-bug-report`.
- TC_011 và TC_080 (`@NeedsVerify`) đều PASS → dùng `/review-testcases` mode FIX để gỡ `@NeedsVerify`, ghi trang đích vào Expected (TC_011: `https://crm.anhtester.com/` · TC_080: `https://crm.anhtester.com/authentication/login`), rồi đóng `ASM-10` · `ASM-17` và cập nhật `AMB-SYS-03` (đã có tài khoản contact). **Không** sửa TC trong lần chạy này.
- `.env` đã có thêm `EMAIL_PROJECT-MANAGER`. Part 01 không dùng tài khoản này; dùng cho các TC phân quyền ở part 02 / part 04.
- TC_001 bước 4 (thanh trạng thái hiện đích liên kết logo) được xác nhận gián tiếp qua `href`. Muốn chấm đúng nguyên văn thì tester rê chuột kiểm bằng mắt trên Chrome thật.
