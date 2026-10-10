# Requirements — Đăng nhập (`LOGIN`) · Nền tảng Web

> ← [Index module `REQUIREMENTS_LOGIN_SUMMARY.md`](../REQUIREMENTS_LOGIN_SUMMARY.md) · Danh mục hệ thống: [`../../README.md`](../../README.md)
> File nền tảng — chứa REQ **chỉ áp Web**, Field Spec, Validation Messages, User Flow, Danh mục Evidence. Ma trận phân quyền, AMB/RISK, Phân rã Story, Nhật ký nằm ở **index**.

| Mục | Giá trị |
|---|---|
| **Module · Prefix** | Đăng nhập · `LOGIN` |
| **Nền tảng** | Web — khu quản trị `/admin/authentication*` + cổng khách hàng `/authentication/*` |
| **REQ trong file này** | `REQ-LOGIN-01` → `REQ-LOGIN-72` (72 REQ — dải đầy đủ của module xem ở index) |
| **Ngày khảo sát** | 03-10-2026 |
| **Cập nhật** | 03-10-2026 (đợt 4) — AMB-LOGIN-30, 31 (`PO-REPLY-LOGIN-20261003-03`): đổi câu chữ + thêm bản tiếng Anh cho REQ-LOGIN-36 · 55. Impact: [`../impact/impact_PO-REPLY-LOGIN-20261003-03.md`](../impact/impact_PO-REPLY-LOGIN-20261003-03.md) · 03-10-2026 (đợt 3) — PO trả lời AMB-LOGIN-26 → 29 (`PO-REPLY-LOGIN-20261003-02`): sửa REQ-LOGIN-34 · 36 · 53 · 55 · 64 · 69. Impact: [`../impact/impact_PO-REPLY-LOGIN-20261003-02.md`](../impact/impact_PO-REPLY-LOGIN-20261003-02.md) · 03-10-2026 (đợt 2) — PO trả lời AMB-LOGIN-01 → 25 (`PO-REPLY-LOGIN-20261003`): thêm `REQ-LOGIN-59` → `72`, sửa REQ-LOGIN-10 · 23 · 36 · 55. Impact Report: [`../impact/impact_PO-REPLY-LOGIN-20261003.md`](../impact/impact_PO-REPLY-LOGIN-20261003.md) |
| **Trình duyệt khảo sát** | Google Chrome 154 (Playwright MCP, headed) trên macOS · viewport đo được `1600×750` (`innerWidth × innerHeight`) · `navigator.language = en-US`. Mọi AC dựa trên thông báo mặc định của trình duyệt **chỉ đúng với trình duyệt này** và **cấm dùng làm assertion** |
| **Tầng network** | Quan sát thụ động bằng `browser_network_requests` + đọc thuộc tính cookie (tên · cờ · hạn — **không** đọc/ghi giá trị) qua Playwright. Không gọi API trực tiếp |
| **Tài khoản dùng** | 1 tài khoản Staff (`EMAIL_ADMIN` trong `.env`). **Agent không nhập mật khẩu thật** (host public) — 3 lần đăng nhập thành công trong phiên do **người dùng tự nhập** trên cửa sổ Playwright. Mọi ca lỗi dùng email **không tồn tại** `auto_login_<timestamp>@auto.test` để không khoá tài khoản dùng chung |
| **Phiên bản hệ thống** | `app.version = 316` · tài nguyên tĩnh `?v=3.1.6` |

---

## 1. Bản đồ phủ tài liệu

| Vùng | Nguồn | Mức phủ | REQ liên quan |
|---|---|---|---|
| Toàn module — đợt 1 | Không có tài liệu — khảo sát UI thực tế + tầng network (user xác nhận 03-10-2026) | ⬜ Trắng | REQ-LOGIN-01 → 58 |
| Quyết định nghiệp vụ cho 25 AMB | PO/user trả lời trong chat 03-10-2026 (`PO-REPLY-LOGIN-20261003`) | 🟨 Một phần — chỉ chốt hành vi | REQ-LOGIN-59 → 72 · sửa REQ-LOGIN-10, 23, 36, 55 |
| Nguyên văn hành vi đúng của các lỗi (AMB-LOGIN-26 → 29) | PO/user trả lời trong chat 03-10-2026 (`PO-REPLY-LOGIN-20261003-02`) | 🟨 Một phần — chốt hành vi + nguyên văn | sửa REQ-LOGIN-34, 36, 53, 55, 64, 69 |
| Câu chữ thông báo chung Quên mật khẩu (AMB-LOGIN-30, 31) | PO giao agent soạn lại + dịch Anh/Việt trong chat 03-10-2026 (`PO-REPLY-LOGIN-20261003-03`) | 🟩 Đủ nguyên văn EN + VI | sửa REQ-LOGIN-36, 55 |

REQ có `Nguồn` = `Tài liệu · PO …` **chưa** được mở UI kiểm lại — lượt kiểm chứng đầu tiên nâng lên `Tài liệu + kiểm chứng thực tế` hoặc mở AMB nếu lệch.

---

## 2. Yêu cầu chức năng

> Quy ước cột `Nguồn`: `Kiểm chứng thực tế` = đã thao tác và xác nhận · `UI thực tế (DOM)` = đọc DOM/mã trang, chưa kích hoạt · `Network · <method> <path> → <status>` = lấy từ tầng network · `Tài liệu · PO trả lời AMB-LOGIN-XX` = quyết định của PO, **chưa** kiểm chứng thực tế. REQ ghi chú **"hiện vi phạm"** = PO đã xác nhận hành vi hiện tại là lỗi → TC giữ AC đúng yêu cầu, sẽ FAIL tới khi Dev sửa, **không** hạ AC (`RISK-LOGIN-07`). Mã HTTP chuyển hướng `307` được ghi để tham khảo — xem `AMB-LOGIN-25`, **AC chỉ assert điểm dừng (URL cuối)**, không assert mã.

### STORY-LOGIN-01 — Đăng nhập khu quản trị: form & validation

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-01 | Chưa đăng nhập thì mọi trang quản trị chuyển về trang Login | Là hệ thống, tôi chặn người chưa xác thực vào khu `/admin/*` | Xoá sạch cookie của `crm.anhtester.com` → mở `/admin/` → URL cuối là `/admin/authentication`. Lặp lại với deep link `/admin/clients` → URL cuối cũng là `/admin/authentication`, URL **không** mang tham số quay lại | 🟢 | — | Kiểm chứng thực tế · Network `GET /admin/clients → 307` |
| REQ-LOGIN-02 | Trang Login quản trị hiển thị đủ thành phần | Là nhân viên, tôi thấy form đăng nhập | Tại `/admin/authentication`: heading `H1` = "Login" · nhãn "Email Address" (`input#email`, `name=email`) · nhãn "Password" (`input#password`, `name=password`) · checkbox "Remember me" (`input#remember`, `name=remember`) · nút submit "Login" · link "Forgot Password?" · logo. Form `method=post`, `action=/admin/authentication`. `document.title` **chứa** "Login" | 🟢 | — | Kiểm chứng thực tế · ảnh `admin_login_default_fullpage.png` |
| REQ-LOGIN-03 | Ô Email được đặt con trỏ sẵn khi mở trang | Người dùng gõ ngay được email | `input#email` có thuộc tính `autofocus`; sau khi tải trang `document.activeElement.id === "email"` | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-LOGIN-04 | Ô Password che ký tự đã nhập | Bảo mật mật khẩu khi nhập | `input#password` có `type="password"`. Không có nút hiện/ẩn mật khẩu | 🟢 | — | Kiểm chứng thực tế (DOM) · ảnh `admin_login_default_fullpage.png` |
| REQ-LOGIN-05 | Bỏ trống Email thì báo bắt buộc | Server chặn khi thiếu email | Email rỗng, Password có giá trị → bấm Login → trang tải lại, hiện 1 khối `.alert.alert-danger` nội dung "The Email Address field is required." · không đăng nhập | 🟢 | — | Kiểm chứng thực tế · Network `POST /admin/authentication → 200` |
| REQ-LOGIN-06 | Bỏ trống Password thì báo bắt buộc | Server chặn khi thiếu mật khẩu | Email `auto_login_<timestamp>@auto.test`, Password rỗng → bấm Login → hiện 1 khối `.alert.alert-danger` "The Password field is required." · không đăng nhập | 🟢 | — | Kiểm chứng thực tế · Network `POST /admin/authentication → 200` |
| REQ-LOGIN-07 | Bỏ trống cả hai trường thì hiện đủ hai thông báo | Người dùng thấy mọi lỗi trong một lần | Cả hai rỗng → bấm Login → đúng **2** khối `.alert.alert-danger`: "The Password field is required." và "The Email Address field is required." (thứ tự quan sát: Password trước — xem `AMB-LOGIN-22`, TC không nên assert thứ tự) | 🟢 | — | Kiểm chứng thực tế · ảnh `admin_login_submit_empty_fullpage.png` |
| REQ-LOGIN-08 | Email thiếu "@" bị trình duyệt chặn, không gửi lên server | Ô Email kiểu `type="email"` | Nhập Email `auto_login_<timestamp>` (không có `@`) + Password bất kỳ → bấm Login → `email.checkValidity() === false`, `validity.typeMismatch === true`, **không** phát sinh request `POST`. Chuỗi trình duyệt hiển thị (tham khảo, **cấm assert**, Chrome 154 en-US): "Please include an '@' in the email address. '…' is missing an '@'." | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-LOGIN-09 | Email có "@" nhưng tên miền không hợp lệ bị server từ chối | Server kiểm định dạng email | Email `auto_login_<timestamp>@autotest` (không có dấu chấm tên miền — trình duyệt cho qua) → bấm Login → `.alert.alert-danger` "The Email Address field must contain a valid email address." | 🟢 | — | Kiểm chứng thực tế · ảnh `admin_login_invalid_email_server_fullpage.png` |
| REQ-LOGIN-10 | Thông tin đăng nhập sai thì báo lỗi chung | Không đăng nhập được với email/mật khẩu không khớp — thông báo không cho biết email có tồn tại hay không | **(a)** Email đúng định dạng nhưng **không tồn tại** `auto_login_<timestamp>@auto.test` + Password bất kỳ → bấm Login → ở lại `/admin/authentication`, hiện `.alert.alert-danger` "Invalid email or password" (không có dấu chấm cuối). **(b)** Email **tồn tại** (tài khoản `EMAIL_ADMIN`) + mật khẩu sai `auto_wrong_<timestamp>` → **cùng** nguyên văn "Invalid email or password", cùng khối `.alert.alert-danger`, cùng URL — không phân biệt được với ca (a). Ca (b) chỉ gửi **1 lần** trên tài khoản dùng chung (`RISK-LOGIN-01`) | 🟡 | 03-10-2026 · PO-REPLY-LOGIN-20261003 | Ca (a): Kiểm chứng thực tế · ảnh `admin_login_invalid_credentials_fullpage.png` · Ca (b): Tài liệu · PO trả lời AMB-LOGIN-07 — chưa kiểm chứng |
| REQ-LOGIN-11 | Thông báo sai thông tin chỉ hiện một lần | Thông báo là flash message theo mẫu Post/Redirect/Get | Sau ca REQ-LOGIN-10: request là `POST → 303 → GET /admin/authentication`. Tải lại trang (F5 / mở lại URL) → **không** còn khối `.alert` nào | 🟢 | — | Kiểm chứng thực tế · Network `POST /admin/authentication → 303` |
| REQ-LOGIN-12 | Máy chủ không trả lại giá trị Email sau khi đăng nhập lỗi | Sau mọi lỗi server-side, ô Email được dựng lại rỗng | Sau REQ-LOGIN-06 / 10: `input#email` **không** có thuộc tính `value` (`getAttribute('value') === null`). ⚠️ Chỉ assert tầng HTML — **cấm** assert ô rỗng trên màn hình vì trình duyệt có thể tự điền | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-LOGIN-13 | Máy chủ không trả lại giá trị Password sau khi đăng nhập lỗi | Mật khẩu không bao giờ được render lại vào HTML | Sau REQ-LOGIN-05: `input#password` không có thuộc tính `value` (`getAttribute('value') === null`) | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-LOGIN-14 | Form đăng nhập gửi kèm CSRF token | Chống giả mạo request | Form có `input[type=hidden][name=csrf_token_name]`, giá trị dài **32** ký tự. Payload `POST /admin/authentication` gồm các trường `csrf_token_name`, `email`, `password`; trường `remember` **chỉ có** khi tick Remember me | 🟢 | — | Network `POST /admin/authentication` (đọc tên trường, không đọc giá trị) |
| REQ-LOGIN-15 | Link "Forgot Password?" mở trang quên mật khẩu quản trị | | Bấm "Forgot Password?" → URL `/admin/authentication/forgot_password`, heading "Forgot Password" | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-16 | Logo trang Login dẫn về trang chủ cổng khách hàng | | Logo là `<a href="https://crm.anhtester.com/">` (ảnh `alt` **chứa** "Anh Tester Demo") | 🟢 | — | UI thực tế (DOM) |
| REQ-LOGIN-59 | Tài khoản contact không đăng nhập được khu quản trị | Contact chỉ dùng cổng khách hàng | Tại `/admin/authentication` nhập email + mật khẩu **đúng** của một contact → bấm Login → ở lại `/admin/authentication`, không vào Dashboard; sau đó mở `/admin/` → vẫn về `/admin/authentication`. Nguyên văn thông báo chưa chốt — kiểm chứng rồi ghi lại | ⚪ | — | Tài liệu · PO trả lời AMB-LOGIN-02 — chưa kiểm chứng, chờ tài khoản contact (`AMB-SYS-03`) |
| REQ-LOGIN-61 | Đăng nhập sai nhiều lần liên tiếp không làm hiện CAPTCHA, form vẫn xử lý bình thường | PO xác nhận không có CAPTCHA / giới hạn | (1) Xoá sạch cookie → (2) xác nhận không còn cookie nào → (3) gửi liên tiếp **N = 10** lần (tham số kiểm thử, PO xác nhận không có ngưỡng) email không tồn tại `auto_login_<timestamp>@auto.test` + mật khẩu bất kỳ → (4) ở lần thứ N + 1: trang vẫn hiện "Invalid email or password" như REQ-LOGIN-10, **không** có phần tử CAPTCHA (`iframe[src*=recaptcha]`, `.g-recaptcha`), **không** có thông báo yêu cầu chờ | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-04 — chưa kiểm chứng |
| REQ-LOGIN-62 | Tài khoản không bị khoá sau nhiều lần nhập sai mật khẩu | PO xác nhận không có khoá tài khoản | Email tài khoản `EMAIL_ADMIN` + mật khẩu sai `auto_wrong_<timestamp>` gửi **5** lần liên tiếp (tham số kiểm thử — ngưỡng khoá phổ biến) → ngay sau đó nhập mật khẩu đúng (người dùng tự nhập) → vào Dashboard. ⚠️ Chạy trên tài khoản dùng chung — theo điều kiện của `RISK-LOGIN-01` | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-04 — chưa kiểm chứng |
| REQ-LOGIN-70 | Ô Email và Password của form Login quản trị khai báo thuộc tính `autocomplete` | Giúp trình quản lý mật khẩu điền đúng ô | `input#password.getAttribute('autocomplete') === "current-password"` · `input#email` có thuộc tính `autocomplete` khác rỗng (PO không chỉ định giá trị — `AMB-LOGIN-27` ✅). Không áp cho form Login cổng khách hàng. **Hiện vi phạm:** cả hai ô đều không có `autocomplete` (đọc DOM 03-10-2026) — lỗi đã xác nhận | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-21 + UI thực tế (DOM): hiện thiếu |
| REQ-LOGIN-71 | Email đăng nhập quản trị không phân biệt chữ hoa chữ thường | | Nhập email của tài khoản `EMAIL_ADMIN` đổi **toàn bộ** chữ cái sang chữ HOA, không thêm khoảng trắng + mật khẩu đúng (người dùng tự nhập) → vào Dashboard như REQ-LOGIN-17 | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-24 — chưa kiểm chứng |
| REQ-LOGIN-72 | Khoảng trắng đầu/cuối của Email không cản trở đăng nhập quản trị | | Nhập `"  <email EMAIL_ADMIN giữ nguyên hoa thường>  "` (2 dấu cách mỗi đầu) + mật khẩu đúng (người dùng tự nhập) → vào Dashboard. Ghi chú: với `type=email`, trình duyệt cắt khoảng trắng theo chuẩn HTML trước khi gửi — payload `email` không chứa khoảng trắng. REQ khẳng định kết quả người dùng thấy, không khẳng định server tự cắt | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-24 — chưa kiểm chứng |

### STORY-LOGIN-02 — Phiên đăng nhập & chuyển hướng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-17 | Đăng nhập đúng thì vào Dashboard | Luồng chính | Nhập email + mật khẩu hợp lệ của tài khoản Staff → bấm Login → `POST /admin/authentication → 303`, `Location: /admin` → trang Dashboard (`document.title` **chứa** "Dashboard") | 🟢 | — | Kiểm chứng thực tế — **người dùng tự nhập mật khẩu**, 3 lần trong phiên · Network `POST /admin/authentication → 303` |
| REQ-LOGIN-18 | Sau đăng nhập luôn vào Dashboard, không quay về trang đã định mở | Đúng thiết kế — PO xác nhận (`AMB-LOGIN-11` ✅) | Xoá sạch cookie → mở `/admin/clients` → bị chuyển về Login (REQ-LOGIN-01) → đăng nhập đúng → URL cuối là `/admin/` (Dashboard), **không** phải `/admin/clients` | 🟢 | — | Kiểm chứng thực tế · Network `POST → 303 → GET /admin/` |
| REQ-LOGIN-19 | Đã đăng nhập mà mở trang Login thì chuyển về Dashboard | Không hiện lại form khi đã có phiên | Đang đăng nhập → mở `/admin/authentication` → URL cuối là `/admin/` | 🟢 | — | Kiểm chứng thực tế · Network `GET /admin/authentication → 307` |
| REQ-LOGIN-20 | Cookie phiên được bảo vệ khỏi JavaScript và chỉ gửi qua HTTPS | Cookie phiên `sp_session` | Sau khi đăng nhập: cookie `sp_session` có `HttpOnly = true`, `Secure = true`, `SameSite = Lax`, `Path = /`, giá trị dài **40** ký tự. `document.cookie` **không** chứa `sp_session`. (Hạn của cookie đặc tả riêng ở REQ-LOGIN-66, 67 — TC của REQ này không assert hạn) | 🟢 | — | Kiểm chứng thực tế (thuộc tính cookie qua Playwright) |
| REQ-LOGIN-65 | Đăng nhập cấp session id mới (chống session fixation) | Session id của khách chưa đăng nhập không được dùng tiếp sau khi đăng nhập | (1) Xoá sạch cookie → (2) mở `/admin/authentication`, xác nhận đã có cookie `sp_session` (phiên khách) và ghi nhận giá trị vào bộ nhớ công cụ → (3) đăng nhập đúng (người dùng tự nhập) → (4) giá trị `sp_session` hiện tại **khác** giá trị ở bước 2 (so sánh trong bộ nhớ, không chép giá trị) | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-15 — chưa kiểm chứng (đợt 1 chưa đo được) |
| REQ-LOGIN-66 | Hạn phiên đăng nhập được gia hạn sau mỗi lần hoạt động | Hạn trượt 8 giờ | Đăng nhập **không** tick Remember me → ghi nhận hạn cookie `sp_session` (T1) → thực hiện một request quản trị ở thời điểm muộn hơn → hạn mới T2 ≈ thời điểm phản hồi đó + **8 giờ**, và T2 muộn hơn T1 | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-16 — chưa kiểm chứng |
| REQ-LOGIN-67 | Phiên đăng nhập hết hạn sau 8 giờ không hoạt động | | Đăng nhập **không** tick Remember me → không thực hiện request nào trong **8 giờ** (cộng biên vài phút) → mở `/admin/` → URL cuối `/admin/authentication`. Ghi chú: TC dài, chạy tay hoặc chạy đêm; **không** giả lập bằng sửa hạn cookie phía trình duyệt — cách đó chỉ chứng minh trình duyệt xoá cookie, không chứng minh server hết phiên | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-16 — chưa kiểm chứng |

### STORY-LOGIN-03 — Ghi nhớ đăng nhập (Remember me)

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-21 | Checkbox Remember me mặc định không tick | | Khi tải trang Login: `input#remember.checked === false`, không `disabled` | 🟢 | — | Kiểm chứng thực tế (DOM) · ảnh `admin_login_default_fullpage.png` |
| REQ-LOGIN-22 | Không tick Remember me thì không tạo cookie ghi nhớ | Khẳng định phủ định — kiểm bằng trạng thái sạch | (1) Xoá toàn bộ cookie của `crm.anhtester.com` → (2) xác nhận danh sách cookie rỗng → (3) đăng nhập đúng, **không** tick Remember me → (4) danh sách cookie chỉ có `csrf_cookie_name`, `sp_session` — **không** có `autologin` | 🟢 | — | Kiểm chứng thực tế (4 bước, 03-10-2026) |
| REQ-LOGIN-23 | Tick Remember me thì tạo cookie ghi nhớ đúng hình thái | Tác tạo — vế *được tạo đúng* | Đăng xuất → đăng nhập đúng **có** tick Remember me → xuất hiện cookie `autologin`: giá trị (sau URL-decode) có hình thái `a:2:{s:7:"user_id";s:<n>:"<id>";s:3:"key";s:16:"<16 ký tự hex>";}` · `Secure = true` · `SameSite = Lax` · `Path = /` · hạn = thời điểm đăng nhập + **62 ngày**. Cờ `HttpOnly` **tách sang REQ-LOGIN-63** — TC của REQ này không assert `HttpOnly` | 🟡 | 03-10-2026 · PO-REPLY-LOGIN-20261003 | Kiểm chứng thực tế (thuộc tính cookie, giá trị chỉ đọc hình thái) |
| REQ-LOGIN-24 | Cookie ghi nhớ đăng nhập lại được khi phiên đã mất | Tác tạo — vế *dùng được* | Sau REQ-LOGIN-23: xoá mọi cookie **trừ** `autologin` → xác nhận chỉ còn `autologin` → mở `/admin/` → vào thẳng Dashboard (không qua trang Login), hệ thống cấp `sp_session` mới | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-25 | Dùng cookie ghi nhớ thì hạn cookie được gia hạn | | Ở REQ-LOGIN-24: sau khi vào Dashboard, cookie `autologin` giữ **nguyên giá trị** nhưng hạn được đặt lại = thời điểm request + 62 ngày (muộn hơn hạn trước đó) | 🟢 | — | Kiểm chứng thực tế (so hạn trước/sau, không đọc giá trị) |
| REQ-LOGIN-63 | Cookie ghi nhớ đăng nhập không đọc được bằng JavaScript | Tách từ REQ-LOGIN-23 — cùng chuẩn với cookie phiên (REQ-LOGIN-20) | Sau REQ-LOGIN-23: cookie `autologin` có `HttpOnly = true` · `document.cookie` **không** chứa `autologin`. **Hiện vi phạm:** quan sát được `HttpOnly = false` (03-10-2026) — lỗi bảo mật đã xác nhận | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-05 + Kiểm chứng thực tế: hiện `HttpOnly = false` |

### STORY-LOGIN-04 — Đăng xuất

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-26 | Menu người dùng ở header có mục Logout dùng được | Lối đăng xuất chính ở viewport desktop | Bấm avatar `li.header-user-profile > a` → menu `li.header-user-profile > ul.dropdown-menu` mở với 5 mục con trực tiếp theo thứ tự: My Profile · My Timesheets · Edit Profile · Language · **Logout** (mục cuối `> li:last-child` có class `header-logout`; menu chứa thêm 32 `<li>` lồng trong submenu Language). Tại viewport `1600×750`, link Logout có `offsetParent !== null`, kích thước `160×32`. Mục Logout trong menu mobile (`#mobile-collapse`) bị ẩn (`0×0`, tổ tiên `display:none`) ở viewport này — lối đăng xuất ở viewport mobile đặc tả ở REQ-LOGIN-68 | 🟢 | — | Kiểm chứng thực tế · ảnh `header_user_menu_open_element.png` |
| REQ-LOGIN-27 | Bấm Logout thì kết thúc phiên và về trang Login | Khi **không** có timer công việc đang chạy | Bấm Logout (gọi `logout()` → `/admin/authentication/logout`) → URL cuối `/admin/authentication`. Sau đó mở `/admin/` → vẫn bị chuyển về `/admin/authentication` | 🟢 | — | Kiểm chứng thực tế · Network `GET /admin/authentication/logout → 307` |
| REQ-LOGIN-28 | Đang có timer công việc chạy thì Logout hỏi xác nhận | Tránh quên dừng timer | Hàm `logout()`: nếu `.started-timers-top li.timer` tồn tại → mở popup nội dung "Started tasks timers found! Are you sure you want to logout without stopping the timers?" kèm nút "Logout" (trỏ `/admin/authentication/logout`), **không** đăng xuất ngay. Chưa kích hoạt thực tế — `AMB-LOGIN-23` | 🟢 | — | UI thực tế (DOM — mã hàm `logout()` + template `#timers-logout-template-warning`) |
| REQ-LOGIN-29 | Đăng xuất cấp session id mới | Phiên cũ không dùng lại được | Ghi nhận giá trị `sp_session` khi đang đăng nhập → Logout → giá trị `sp_session` mới **khác** giá trị trước đó (so sánh trong bộ nhớ công cụ, không chép giá trị) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-30 | Bấm Back sau khi đăng xuất không hiển thị lại trang quản trị | Trang quản trị không bị lấy từ cache | Đăng xuất → `history.back()` → trình duyệt tải lại `/admin/` và URL cuối là `/admin/authentication` (form Login hiển thị), `performance.getEntriesByType('navigation')[0].type = "back_forward"` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-31 | Đăng xuất vô hiệu hoá cookie ghi nhớ ở phía máy chủ | Cookie `autologin` cũ không đăng nhập lại được | Đăng nhập có Remember me → lưu cookie `autologin` → Logout → xoá mọi cookie rồi chỉ đặt lại cookie `autologin` đã lưu → mở `/admin/` → URL cuối `/admin/authentication` (không vào được). Lưu ý: trình duyệt **vẫn giữ** cookie `autologin` sau Logout — `AMB-LOGIN-12` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-68 | Ở viewport mobile, menu mobile có mục Logout dùng được | Lối đăng xuất trên màn hình nhỏ hoạt động giống desktop | Đăng nhập → đặt viewport mobile (dưới breakpoint làm `#mobile-collapse` hiển thị — giá trị breakpoint đo khi kiểm chứng) → mở menu mobile → mục Logout có `offsetParent !== null`, kích thước khác `0×0` → bấm → URL cuối `/admin/authentication` như REQ-LOGIN-27. Ghi rõ viewport đã đo vào kết quả | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-17 — chưa kiểm chứng (đợt 1 chỉ đo ở `1600×750`) |

### STORY-LOGIN-05 — Quên mật khẩu (khu quản trị)

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-32 | Trang Quên mật khẩu quản trị hiển thị đủ thành phần | | Tại `/admin/authentication/forgot_password`: heading "Forgot Password" · nhãn "Email Address" (`input#email`, `type=email`, `name=email`) · nút submit "Confirm" · CSRF token ẩn · logo về `/`. Form `method=post`, `action=/admin/authentication/forgot_password`. **Không** có link quay lại Login (`AMB-LOGIN-14`) | 🟢 | — | Kiểm chứng thực tế · ảnh `admin_forgot_password_default_fullpage.png` |
| REQ-LOGIN-33 | Email thiếu "@" bị trình duyệt chặn | `type="email"` | Nhập `auto_login_<timestamp>` → bấm Confirm → `checkValidity() === false`, `typeMismatch === true`, **không** có request `POST`. Chuỗi trình duyệt chỉ tham khảo, cấm assert | 🟢 | — | Kiểm chứng thực tế (DOM + network) |
| REQ-LOGIN-34 | Bỏ trống Email ở Quên mật khẩu quản trị thì báo bắt buộc nhập | Bỏ trống không liên quan tới lộ email — báo đúng nghĩa như form Login (PO chốt ở AMB-LOGIN-26, thay kết luận AMB-LOGIN-08) | Giao diện tiếng Anh: Email rỗng → bấm Confirm → `.alert.alert-danger` "The Email Address field is required." (cùng nguyên văn REQ-LOGIN-05). **Hiện vi phạm:** hiện "Email not found" (03-10-2026) | 🟡 | 03-10-2026 · PO-REPLY-LOGIN-20261003-02 | Tài liệu · PO trả lời AMB-LOGIN-26 + Kiểm chứng thực tế: hiện vi phạm · ảnh `admin_forgot_password_submit_empty_fullpage.png` |
| REQ-LOGIN-35 | Email sai tên miền bị server từ chối | | Email `auto_login_<timestamp>@autotest` → bấm Confirm → `.alert.alert-danger` "The Email Address field must contain a valid email address." | 🟢 | — | Kiểm chứng thực tế · ảnh `admin_forgot_password_invalid_email_fullpage.png` |
| REQ-LOGIN-36 | Quên mật khẩu quản trị hiện cùng một thông báo dù email có tồn tại hay không | Chống dò danh sách email nhân viên | Giao diện **tiếng Anh** (mặc định): **(a)** email không tồn tại `auto_login_<timestamp>@auto.test` → bấm Confirm → thông báo **chứa** nguyên văn "If this email address exists in our system, we have sent password reset instructions." · **(b)** email tồn tại → **cùng** nội dung, cùng cách hiển thị (hệ thống vẫn gửi email đặt lại — REQ-LOGIN-38). Hai ca không phân biệt được. Bản tiếng Việt: "Nếu địa chỉ email này có trong hệ thống, chúng tôi đã gửi hướng dẫn đặt lại mật khẩu." — khu quản trị chưa đăng nhập không đổi được ngôn ngữ nên kiểm bản tiếng Việt ở cổng (REQ-LOGIN-55). Ca (b) **chưa chạy được** — gửi email đặt lại tới tài khoản dùng chung (`RISK-LOGIN-03`). **Hiện vi phạm:** ca (a) hiện "Email not found" (03-10-2026) | 🟡 | 03-10-2026 · PO-REPLY-LOGIN-20261003-03 | Tài liệu · PO trả lời AMB-LOGIN-06, 26, 30, 31 (câu chữ do agent soạn + dịch theo yêu cầu PO) + Kiểm chứng thực tế: hiện vi phạm · ảnh `admin_forgot_password_email_not_found_fullpage.png` |
| REQ-LOGIN-37 | Sau lỗi, ô Email giữ lại giá trị đã nhập | Khác form Login (REQ-LOGIN-12) | Sau REQ-LOGIN-35 / 36: `input#email.getAttribute('value')` = đúng chuỗi vừa nhập | 🟢 | — | Kiểm chứng thực tế (DOM) · ảnh `admin_forgot_password_email_not_found_fullpage.png` |
| REQ-LOGIN-38 | Email tồn tại thì hệ thống gửi email đặt lại mật khẩu | Tác tạo — vế *được tạo* | Chưa kiểm: gửi với email thật sẽ phát email tới tài khoản dùng chung, và QA không có quyền kiểm tầng tích hợp/hộp thư (`RISK-LOGIN-03`) | ⚪ | — | Chưa kiểm chứng — `AMB-LOGIN-06` |
| REQ-LOGIN-39 | Liên kết trong email đặt lại mật khẩu dùng được để đặt mật khẩu mới | Tác tạo — vế *dùng được* | Chưa kiểm (như REQ-LOGIN-38). Ca mở route đặt lại **không tham số** đặc tả ở REQ-LOGIN-64 | ⚪ | — | Chưa kiểm chứng |
| REQ-LOGIN-64 | Liên kết đặt lại mật khẩu quản trị không hợp lệ hiển thị trang 404 | Link đặt lại hỏng, bị cắt cụt hoặc sai mã | **(a)** Mở `GET /admin/authentication/reset_password` (không tham số — chỉ đọc) → mã phản hồi **404**, hiển thị trang 404 của hệ thống, không có trang lỗi PHP/máy chủ. **(b)** Liên kết đúng định dạng nhưng tham số sai → cũng 404 — **chưa chạy được**: định dạng liên kết thật chỉ thấy trong email đặt lại (`RISK-LOGIN-03`), **không** đoán định dạng. Nội dung chữ của trang 404 chưa quan sát → không assert chữ. **Hiện vi phạm:** ca (a) trả `HTTP 500` (03-10-2026) | 🟡 | 03-10-2026 · PO-REPLY-LOGIN-20261003-02 | Tài liệu · PO trả lời AMB-LOGIN-13, 28 + Network `GET /admin/authentication/reset_password → 500` (hiện vi phạm) |
| REQ-LOGIN-69 | Tiêu đề tab trang Quên mật khẩu quản trị là "Forgot Password?" | Thống nhất với trang Quên mật khẩu cổng (REQ-LOGIN-51) | Tại `/admin/authentication/forgot_password` (giao diện tiếng Anh, chưa đăng nhập): `document.title` = "Forgot Password?". **Hiện vi phạm:** `document.title` = "Perfex CRM \| Anh Tester Demo - Login" (03-10-2026) | 🟡 | 03-10-2026 · PO-REPLY-LOGIN-20261003-02 | Tài liệu · PO trả lời AMB-LOGIN-20, 29 + UI thực tế (DOM): hiện vi phạm |

### STORY-LOGIN-06 — Cổng khách hàng: Đăng nhập

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-40 | Trang Login cổng khách hàng hiển thị đủ thành phần | Là liên hệ (contact) của khách hàng, tôi thấy form đăng nhập | Tại `/authentication/login`: heading "Please login" · select "Language" (`select#language`, `name=language`, **26** option, chọn sẵn `english`) · "Email Address" (`input#email`, **`type=text`**, `autofocus`) · "Password" (`type=password`) · checkbox "Remember me" (mặc định không tick) · nút "Login" · link "Forgot Password?" (`/authentication/forgot_password`). Header: link "Knowledge Base" (`/knowledge-base`) + nút "Login". Footer **chứa** "Copyright Perfex CRM \| Anh Tester Demo" | 🟢 | — | Kiểm chứng thực tế · ảnh `portal_login_default_fullpage.png` |
| REQ-LOGIN-41 | Email sai định dạng bị server từ chối, hiển thị dưới ô Email | Ô `type=text` nên trình duyệt không chặn (`AMB-LOGIN-10`) | Email `auto_login_<timestamp>` (không `@`) + Password bất kỳ → bấm Login → request `POST /authentication/login` **được gửi** → dưới ô Email hiện `p.text-danger.alert-validation` "The Email Address field must contain a valid email address." | 🟢 | — | Kiểm chứng thực tế · ảnh `portal_login_invalid_email_fullpage.png` |
| REQ-LOGIN-42 | Bỏ trống Email thì báo bắt buộc ngay dưới ô | | Bấm Login khi Email rỗng → dưới ô Email (trong cùng `.form-group`) hiện `p.text-danger.alert-validation` "The Email Address field is required." (quan sát ở ca bỏ trống cả hai) | 🟢 | — | Kiểm chứng thực tế · ảnh `portal_login_submit_empty_fullpage.png` |
| REQ-LOGIN-43 | Bỏ trống Password thì báo bắt buộc ngay dưới ô | | Bấm Login khi Password rỗng → dưới ô Password hiện `p.text-danger.alert-validation` "The Password field is required." (quan sát ở ca bỏ trống cả hai) | 🟢 | — | Kiểm chứng thực tế · ảnh `portal_login_submit_empty_fullpage.png` |
| REQ-LOGIN-44 | Thông tin sai thì hiện toast lỗi tự ẩn | | Email không tồn tại `auto_login_<timestamp>@auto.test` + Password bất kỳ → `POST → 303 → GET /authentication/login` → toast `.float-alert.alert-danger` góc trên phải nội dung "Invalid username or password"; toast tự ẩn và bị xoá khỏi DOM sau **3.500 ms** (mặc định của `alert_float`). Khác chữ và cách hiển thị so với admin — `AMB-LOGIN-09` | 🟢 | — | Kiểm chứng thực tế · ảnh `portal_login_invalid_credentials_toast_viewport.png` · mã HTML trả về chứa `alert_float("danger","Invalid username or password")` |
| REQ-LOGIN-45 | Máy chủ không trả lại giá trị Email sau khi đăng nhập cổng lỗi | | Sau REQ-LOGIN-41 / 44: `input#email.getAttribute('value') === null`. Cấm assert ô rỗng trên màn hình | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-LOGIN-46 | Đổi Language thì trang Login hiển thị theo ngôn ngữ đã chọn | | Chọn `Vietnamese` → `GET /authentication/change_language/vietnamese → 307 → /authentication/login` → `html[lang="vi"]`, heading "Xin hãy đăng nhập", nhãn "Ngôn ngữ" · "Địa chỉ email" · "Mật khẩu" · "Giữ đăng nhập cho những lần sau", nút "Đăng nhập", link "Quên mật khẩu?" | 🟢 | — | Kiểm chứng thực tế · ảnh `portal_login_language_vietnamese_fullpage.png` |
| REQ-LOGIN-47 | Ngôn ngữ đã chọn được ghi nhớ khi tải lại | | Sau REQ-LOGIN-46 xuất hiện cookie `contact_language` (`HttpOnly = false`, `Secure = true`, hạn ≈ 3 tháng). Mở lại `/authentication/login` → trang vẫn tiếng Việt (`document.title` = "Xin hãy đăng nhập"). Chọn lại `English` → trang về tiếng Anh | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-48 | Ngôn ngữ cổng khách hàng không đổi ngôn ngữ trang Login quản trị | | Khi cổng đang ở `vietnamese` → mở `/admin/authentication` → trang vẫn tiếng Anh (`document.title` **chứa** "Login", heading "Login") | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-49 | Đăng ký tài khoản khách hàng đang tắt | | Mở `/authentication/register` → URL cuối `/authentication/login`. Trang Login cổng không có link Register | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-50 | Liên hệ đăng nhập cổng khách hàng thành công | Luồng chính của cổng | Chưa kiểm — chưa có tài khoản contact (`AMB-SYS-03`). PO xác nhận contact đăng nhập được cổng (`AMB-LOGIN-02` ✅) | ⚪ | — | Chưa kiểm chứng |
| REQ-LOGIN-60 | Tài khoản Staff không đăng nhập được cổng khách hàng | Tài khoản nhân viên và tài khoản contact là hai loại tách biệt | Tại `/authentication/login` nhập email + mật khẩu **đúng** của tài khoản Staff `EMAIL_ADMIN` (người dùng tự nhập) → hiện toast `.float-alert.alert-danger` "Invalid username or password" như REQ-LOGIN-44, URL cuối `/authentication/login`, không vào khu vực khách hàng | 🟢 | — | Tài liệu · PO trả lời AMB-LOGIN-03 — chưa kiểm chứng |

### STORY-LOGIN-07 — Cổng khách hàng: Quên mật khẩu

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-51 | Trang Quên mật khẩu cổng hiển thị đủ thành phần | | Tại `/authentication/forgot_password`: `document.title` = "Forgot Password?" · heading "Forgot Password" · "Email Address" (`input#email`, `type=email`) · nút submit "**Submit**" (admin là "Confirm") · header có "Knowledge Base" + nút "Login" (lối quay lại) | 🟢 | — | Kiểm chứng thực tế · ảnh `portal_forgot_password_default_fullpage.png` |
| REQ-LOGIN-52 | Email thiếu "@" bị trình duyệt chặn | `type="email"` | Nhập `auto_login_<timestamp>` → bấm Submit → `checkValidity() === false`, **không** có request `POST`. Chuỗi trình duyệt cấm assert | 🟢 | — | Kiểm chứng thực tế (DOM + network) |
| REQ-LOGIN-53 | Bỏ trống Email ở Quên mật khẩu cổng thì báo bắt buộc nhập | Giống admin (REQ-LOGIN-34) — PO chốt ở AMB-LOGIN-26 | Giao diện tiếng Anh: Email rỗng → bấm Submit → `.alert.alert-danger` "The Email Address field is required.". **Hiện vi phạm:** hiện "Email not found" (03-10-2026) | 🟡 | 03-10-2026 · PO-REPLY-LOGIN-20261003-02 | Tài liệu · PO trả lời AMB-LOGIN-26 + Kiểm chứng thực tế: hiện vi phạm · ảnh `portal_forgot_password_submit_empty_fullpage.png` |
| REQ-LOGIN-54 | Email sai tên miền bị server từ chối | | Email `auto_login_<timestamp>@autotest` → bấm Submit → `.alert.alert-danger` "The Email Address field must contain a valid email address." | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-LOGIN-55 | Quên mật khẩu cổng hiện cùng một thông báo dù email có tồn tại hay không | Chống dò danh sách email khách hàng | **(a)** Email không tồn tại `auto_login_<timestamp>@auto.test` → bấm Submit → giao diện tiếng Anh: thông báo **chứa** "If this email address exists in our system, we have sent password reset instructions." · đổi Language cổng sang `Vietnamese` (REQ-LOGIN-46) rồi lặp lại: thông báo **chứa** "Nếu địa chỉ email này có trong hệ thống, chúng tôi đã gửi hướng dẫn đặt lại mật khẩu." · **(b)** email contact tồn tại → **cùng** nội dung, cùng cách hiển thị. Ca (b) **chưa chạy được** — chưa có tài khoản contact (`AMB-SYS-03`). **Hiện vi phạm:** ca (a) hiện "Email not found" (03-10-2026) | 🟡 | 03-10-2026 · PO-REPLY-LOGIN-20261003-03 | Tài liệu · PO trả lời AMB-LOGIN-06, 26, 30, 31 (câu chữ do agent soạn + dịch theo yêu cầu PO) + Kiểm chứng thực tế: hiện vi phạm · ảnh `portal_forgot_password_email_not_found_fullpage.png` |
| REQ-LOGIN-56 | Sau lỗi, ô Email giữ lại giá trị đã nhập | | Sau REQ-LOGIN-54 / 55: `input#email.value` = đúng chuỗi vừa nhập | 🟢 | — | Kiểm chứng thực tế (DOM) · ảnh `portal_forgot_password_email_not_found_fullpage.png` |
| REQ-LOGIN-57 | Email contact tồn tại thì hệ thống gửi email đặt lại mật khẩu | Tác tạo — vế *được tạo* | Chưa kiểm — chưa có tài khoản contact, không kiểm được hộp thư | ⚪ | — | Chưa kiểm chứng — `AMB-SYS-03` · `RISK-LOGIN-03` |
| REQ-LOGIN-58 | Liên kết đặt lại mật khẩu cổng dùng được | Tác tạo — vế *dùng được* | Chưa kiểm (như REQ-LOGIN-57) | ⚪ | — | Chưa kiểm chứng — `AMB-SYS-03` · `RISK-LOGIN-03` |

---

## 3. Đặc tả trường dữ liệu (Field Specifications)

> Không ô nào có `required`, `maxlength`, `minlength`, `pattern` hay `autocomplete` (đọc DOM 03-10-2026) — mọi ràng buộc bắt buộc/định dạng nằm ở **server**. Thiếu `autocomplete` ở form Login quản trị là lỗi đã xác nhận (REQ-LOGIN-70); form Login cổng không áp dụng (`AMB-LOGIN-27` ✅).

### 3.1. Form Login quản trị — `POST /admin/authentication`

| Field (Label) | Loại UI | `name` gửi đi | Required | Ràng buộc | Mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|---|
| (ẩn) | `input[type=hidden]` | `csrf_token_name` | — | 32 ký tự | do server sinh | REQ-LOGIN-14 | Không đổi sau Logout — `AMB-LOGIN-19` |
| Email Address | `input#email[type=email]` | `email` | ✅ server | HTML5 `type=email` (cần `@`) · server kiểm định dạng email | rỗng · `autofocus` | 03 · 05 · 08 · 09 · 12 · 70 · 71 · 72 | Không render lại sau lỗi · không phân biệt hoa thường (REQ-LOGIN-71) |
| Password | `input#password[type=password]` | `password` | ✅ server | — | rỗng | 04 · 06 · 13 · 70 | Không có nút hiện mật khẩu |
| Remember me | `input#remember[type=checkbox]` | `remember` | — | Chỉ gửi khi tick | không tick | 21 · 22 · 23 | |
| Login | `button[type=submit]` | — | — | — | — | 17 | |

### 3.2. Form Quên mật khẩu quản trị — `POST /admin/authentication/forgot_password`

| Field (Label) | Loại UI | `name` gửi đi | Required | Ràng buộc | Mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|---|
| (ẩn) | `input[type=hidden]` | `csrf_token_name` | — | 32 ký tự | do server sinh | — | |
| Email Address | `input#email[type=email]` | `email` | ✅ server — mong đợi "The Email Address field is required." (hiện báo "Email not found") | HTML5 `type=email` · server kiểm định dạng · phải tồn tại | rỗng (không `autofocus`) | 33 → 37 | Giữ lại giá trị sau lỗi |
| Confirm | `button[type=submit]` | — | — | — | — | 38 | |

### 3.3. Form Login cổng khách hàng — `POST /authentication/login`

| Field (Label) | Loại UI | `name` gửi đi | Required | Ràng buộc | Mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|---|
| (ẩn) | `input[type=hidden]` | `csrf_token_name` | — | — | do server sinh | — | |
| Language | `select#language` (bootstrap-select, có ô tìm kiếm ẩn) | `language` | — | 26 option — `value`: `english, chinese, vietnamese, indonesia, bulgarian, portuguese_br, swedish, dutch, persian, turkish, catalan, spanish, portuguese, french, czech, japanese, italian, slovak, russian, polish, greek, finnish, norwegian, german, ukrainian, romanian` (không có option rỗng) | `english` | 46 · 47 · 48 | Đổi giá trị → điều hướng ngay `GET /authentication/change_language/<value>` |
| Email Address | `input#email[type=text]` | `email` | ✅ server | Server kiểm định dạng email | rỗng · `autofocus` | 41 · 42 · 45 | Khác admin (`type=email`) |
| Password | `input#password[type=password]` | `password` | ✅ server | — | rỗng | 43 | |
| Remember me | `input#remember[type=checkbox]` | `remember` | — | — | không tick | — | Tác dụng chưa kiểm (chưa có tài khoản contact) |
| Login | `button[type=submit]` | — | — | — | — | 50 | |

### 3.4. Form Quên mật khẩu cổng — `POST /authentication/forgot_password`

| Field (Label) | Loại UI | `name` gửi đi | Required | Ràng buộc | Mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|---|
| (ẩn) | `input[type=hidden]` | `csrf_token_name` | — | — | do server sinh | — | |
| Email Address | `input#email[type=email]` | `email` | ✅ server — mong đợi "The Email Address field is required." (hiện báo "Email not found") | HTML5 `type=email` · server kiểm định dạng · phải tồn tại | rỗng | 52 → 56 | Giữ lại giá trị sau lỗi |
| Submit | `button[type=submit]` | — | — | — | — | 57 | |

---

## 4. Business Rules & Validation Messages (nguyên văn)

| REQ ID | Màn hình | Rule / Trigger | Thông báo (nguyên văn) | Cách hiển thị |
|---|---|---|---|---|
| REQ-LOGIN-05 | Login quản trị | Email rỗng | `The Email Address field is required.` | Khối `.alert.alert-danger` trên form |
| REQ-LOGIN-06 | Login quản trị | Password rỗng | `The Password field is required.` | Khối `.alert.alert-danger` |
| REQ-LOGIN-08 | Login quản trị | Email thiếu `@` | *(trình duyệt)* `Please include an '@' in the email address. '<giá trị>' is missing an '@'.` — Chrome 154 en-US, **cấm assert** | Bong bóng HTML5 |
| REQ-LOGIN-09 | Login quản trị | Email sai tên miền | `The Email Address field must contain a valid email address.` | Khối `.alert.alert-danger` |
| REQ-LOGIN-10 | Login quản trị | Email/mật khẩu không khớp — email tồn tại hay không đều **cùng** thông báo | `Invalid email or password` | Khối `.alert.alert-danger` (flash, mất khi tải lại) |
| REQ-LOGIN-28 | Header quản trị | Logout khi có timer chạy | `Started tasks timers found!` `Are you sure you want to logout without stopping the timers?` | Popup hệ thống + nút `Logout` |
| REQ-LOGIN-34 · 53 | Quên mật khẩu (admin · cổng) | Email rỗng | **Mong đợi:** `The Email Address field is required.` **Hiện tại (lỗi đã xác nhận):** `Email not found` | Khối `.alert.alert-danger` |
| REQ-LOGIN-35 · 54 | Quên mật khẩu (admin · cổng) | Email sai tên miền | `The Email Address field must contain a valid email address.` | Khối `.alert.alert-danger` |
| REQ-LOGIN-36 · 55 | Quên mật khẩu (admin · cổng) | Email không tồn tại **và** email tồn tại — cùng một thông báo | **Mong đợi:** EN "If this email address exists in our system, we have sent password reset instructions." · VI "Nếu địa chỉ email này có trong hệ thống, chúng tôi đã gửi hướng dẫn đặt lại mật khẩu.". **Hiện tại (lỗi đã xác nhận):** `Email not found` cho email không tồn tại | Khối `.alert.alert-danger` (hiện tại) |
| REQ-LOGIN-64 | Đặt lại mật khẩu quản trị | Mở liên kết đặt lại thiếu hoặc sai tham số | **Mong đợi:** trang 404 (mã `404`). **Hiện tại (lỗi đã xác nhận):** `HTTP 500` khi thiếu tham số | Trang 404 |
| REQ-LOGIN-41 | Login cổng | Email sai định dạng | `The Email Address field must contain a valid email address.` | `p.text-danger.alert-validation` dưới ô Email |
| REQ-LOGIN-42 | Login cổng | Email rỗng | `The Email Address field is required.` | `p.text-danger.alert-validation` dưới ô Email |
| REQ-LOGIN-43 | Login cổng | Password rỗng | `The Password field is required.` | `p.text-danger.alert-validation` dưới ô Password |
| REQ-LOGIN-44 | Login cổng | Email/mật khẩu không khớp | `Invalid username or password` | Toast `.float-alert.alert-danger`, tự ẩn sau 3.500 ms |
| — | Mọi trang cổng (mã JS chung) | AJAX trả `419` | `Page expired, refresh the page make an action.` | Toast cảnh báo — ghi tham khảo, chưa kích hoạt |

---

## 5. Luồng xử lý (User Flows)

**Flow A — Đăng nhập khu quản trị**
1. Mở bất kỳ URL `/admin/*` khi chưa có phiên → chuyển về `/admin/authentication` (REQ-LOGIN-01)
2. Nhập Email + Password, tuỳ chọn tick Remember me → bấm Login
3. Lỗi bắt buộc / định dạng → trang dựng lại kèm khối thông báo, ô Email rỗng (REQ-LOGIN-05 → 09, 12)
4. Sai thông tin → `303` về lại trang Login kèm flash "Invalid email or password", email tồn tại hay không đều như nhau (REQ-LOGIN-10, 11); sai nhiều lần không khoá, không CAPTCHA (REQ-LOGIN-61, 62); tài khoản contact không vào được (REQ-LOGIN-59)
5. Đúng → `303` vào `/admin` Dashboard, không quay về URL định mở ban đầu (REQ-LOGIN-17, 18); cấp session id mới (REQ-LOGIN-65); phiên hạn trượt 8 giờ (REQ-LOGIN-66, 67); có tick Remember me thì sinh cookie `autologin` 62 ngày (REQ-LOGIN-23, 63)

**Flow B — Đăng xuất**
1. Avatar → Logout (REQ-LOGIN-26) · viewport mobile: menu `#mobile-collapse` → Logout (REQ-LOGIN-68)
2. Có timer đang chạy → popup xác nhận (REQ-LOGIN-28); không có → `/admin/authentication/logout` → về trang Login (REQ-LOGIN-27)
3. Session id đổi (REQ-LOGIN-29) · cookie `autologin` bị vô hiệu phía server nhưng còn trong trình duyệt (REQ-LOGIN-31, `AMB-LOGIN-12`)

**Flow C — Quên mật khẩu (quản trị / cổng)**
1. Từ trang Login bấm "Forgot Password?" (REQ-LOGIN-15)
2. Nhập Email → Confirm / Submit
3. Thiếu `@` → trình duyệt chặn · rỗng → báo bắt buộc nhập · sai tên miền → thông báo định dạng · email tồn tại hay không → **cùng** một thông báo "If this email address exists in our system, we have sent password reset instructions." (hiện tại rỗng và không tồn tại đều báo "Email not found" — lỗi đã xác nhận) (REQ-LOGIN-33 → 36, 52 → 55)
4. Mở liên kết đặt lại không hợp lệ → trang 404 (REQ-LOGIN-64)
5. Email tồn tại → gửi email đặt lại → bấm liên kết → đặt mật khẩu mới — **chưa kiểm** (⚪ REQ-LOGIN-38, 39, 57, 58)

**Flow D — Đăng nhập cổng khách hàng**
1. Mở `/authentication/login` (đăng ký tắt — REQ-LOGIN-49); tuỳ chọn đổi Language (REQ-LOGIN-46, 47)
2. Nhập Email + Password → Login
3. Lỗi bắt buộc / định dạng → thông báo dưới từng ô (REQ-LOGIN-41 → 43) · sai thông tin → toast 3,5 giây (REQ-LOGIN-44)
4. Đúng → **chưa kiểm** (⚪ REQ-LOGIN-50) · tài khoản Staff bị từ chối như sai thông tin (REQ-LOGIN-60)

---

## 6. Danh mục Evidence

> Mọi ảnh đã được mở lại và xác nhận đúng trạng thái khai (03-10-2026). Trang Login/Quên mật khẩu ngắn, không chứa dữ liệu nghiệp vụ → chụp full-page. Email trong ảnh là email giả không tồn tại `auto_login_1791012178774@…`.

| Ảnh | Màn hình | Trạng thái | Phạm vi | REQ làm bằng chứng |
|---|---|---|---|---|
| [admin_login_default_fullpage.png](evidence/admin_login_default_fullpage.png) | Login quản trị | Mặc định, ô Email đang focus | Full-page | 02 · 03 · 04 · 21 |
| [admin_login_submit_empty_fullpage.png](evidence/admin_login_submit_empty_fullpage.png) | Login quản trị | Submit khi bỏ trống cả hai | Full-page | 07 |
| [admin_login_invalid_email_server_fullpage.png](evidence/admin_login_invalid_email_server_fullpage.png) | Login quản trị | Email sai tên miền — lỗi server | Full-page | 09 |
| [admin_login_invalid_credentials_fullpage.png](evidence/admin_login_invalid_credentials_fullpage.png) | Login quản trị | Email không tồn tại — "Invalid email or password" | Full-page | 10 · 12 |
| [header_user_menu_open_element.png](evidence/header_user_menu_open_element.png) | Menu người dùng (header) | Menu đang mở, thấy mục Logout | **Element** — chỉ dropdown, tránh dữ liệu Dashboard phía sau | 26 |
| [admin_forgot_password_default_fullpage.png](evidence/admin_forgot_password_default_fullpage.png) | Quên mật khẩu quản trị | Mặc định | Full-page | 32 |
| [admin_forgot_password_submit_empty_fullpage.png](evidence/admin_forgot_password_submit_empty_fullpage.png) | Quên mật khẩu quản trị | Bỏ trống — "Email not found" | Full-page | 34 |
| [admin_forgot_password_invalid_email_fullpage.png](evidence/admin_forgot_password_invalid_email_fullpage.png) | Quên mật khẩu quản trị | Email sai tên miền, ô giữ giá trị | Full-page | 35 · 37 |
| [admin_forgot_password_email_not_found_fullpage.png](evidence/admin_forgot_password_email_not_found_fullpage.png) | Quên mật khẩu quản trị | Email không tồn tại, ô giữ giá trị | Full-page | 36 · 37 |
| [portal_login_default_fullpage.png](evidence/portal_login_default_fullpage.png) | Login cổng khách hàng | Mặc định (English) | Full-page | 40 |
| [portal_login_submit_empty_fullpage.png](evidence/portal_login_submit_empty_fullpage.png) | Login cổng khách hàng | Bỏ trống cả hai — lỗi dưới từng ô | Full-page | 42 · 43 |
| [portal_login_invalid_email_fullpage.png](evidence/portal_login_invalid_email_fullpage.png) | Login cổng khách hàng | Email thiếu `@` — lỗi server dưới ô | Full-page | 41 · 45 |
| [portal_login_invalid_credentials_toast_viewport.png](evidence/portal_login_invalid_credentials_toast_viewport.png) | Login cổng khách hàng | Toast "Invalid username or password" đang trượt vào | Viewport — toast nằm ở header, tự ẩn sau 3,5 s | 44 |
| [portal_login_language_vietnamese_fullpage.png](evidence/portal_login_language_vietnamese_fullpage.png) | Login cổng khách hàng | Đã đổi Language = Vietnamese | Full-page | 46 |
| [portal_forgot_password_default_fullpage.png](evidence/portal_forgot_password_default_fullpage.png) | Quên mật khẩu cổng | Mặc định | Full-page | 51 |
| [portal_forgot_password_submit_empty_fullpage.png](evidence/portal_forgot_password_submit_empty_fullpage.png) | Quên mật khẩu cổng | Bỏ trống — "Email not found" | Full-page | 53 |
| [portal_forgot_password_email_not_found_fullpage.png](evidence/portal_forgot_password_email_not_found_fullpage.png) | Quên mật khẩu cổng | Email không tồn tại, ô giữ giá trị | Full-page | 55 · 56 |

**REQ không có ảnh — chống lưng bằng số liệu DOM/network ghi trong AC:** 01 · 05 · 06 · 08 · 11 · 13 · 14 · 15 · 16 · 17 · 18 · 19 · 20 · 22 · 23 · 24 · 25 · 27 · 28 · 29 · 30 · 31 · 33 · 47 · 48 · 49 · 52 · 54. Không chụp ảnh cho 17/18/24 (Dashboard chứa dữ liệu nghiệp vụ của môi trường dùng chung) và cho cookie (giá trị là bí mật).

**REQ ⚪ chưa kiểm chứng (không có bằng chứng):** 38 · 39 · 50 · 57 · 58 · 59.

**REQ nguồn `Tài liệu · PO` (03-10-2026) — chưa có ảnh, chưa kiểm chứng:** 60 · 61 · 62 · 65 · 66 · 67 · 68 · 71 · 72 · ca (b) của 10. Các REQ **hiện vi phạm** (34 · 36 · 53 · 55 · 63 · 64 · 69 · 70) có bằng chứng về **hành vi hiện tại** (ảnh hoặc số liệu DOM/network ghi trong AC), **không** phải bằng chứng hành vi đúng. Ảnh `admin_forgot_password_email_not_found_fullpage.png` và `portal_forgot_password_email_not_found_fullpage.png` từ nay là bằng chứng của **lỗi** (AMB-LOGIN-06), không còn là bằng chứng REQ đạt. Tương tự từ 03-10-2026 (đợt 2): `admin_forgot_password_submit_empty_fullpage.png` và `portal_forgot_password_submit_empty_fullpage.png` là bằng chứng lỗi của REQ-LOGIN-34 · 53 (AMB-LOGIN-26).
