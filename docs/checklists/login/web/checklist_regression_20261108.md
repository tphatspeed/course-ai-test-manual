## Đăng nhập (`LOGIN`) · Web — Checklist Regression (40 mục · ~45 phút)

**Mã checklist:** `CL-regression_20261108` — mã mục khi chạy = `CL-regression_20261108-<số mục 2 chữ số>`
**Loại:** Regression (module). Yêu cầu ghi `testcase chính`, không thuộc 4 loại chuẩn → chọn Regression vì phủ cả luồng đúng lẫn luồng sai chính của module; chỉ muốn rà nhanh luồng sống còn thì chạy các mục `P1`.
**Nguồn:** TC-based — rút gọn từ [`TEST_CASES_LOGIN_SUMMARY.md`](../../../testcases/login/TEST_CASES_LOGIN_SUMMARY.md) (88 TC · [part 01](../../../testcases/login/web/parts/part_01_web_smoke.md) · [part 02](../../../testcases/login/web/parts/part_02_web_quan_tri.md) · [part 03](../../../testcases/login/web/parts/part_03_web_quen_mat_khau_cong.md) · [part 04](../../../testcases/login/web/parts/part_04_web_ky_thuat_phi_chuc_nang.md)) · Requirements: [`REQUIREMENTS_LOGIN_SUMMARY.md`](../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md)
**Scope:** 4 màn hình xác thực (khu quản trị + cổng khách hàng), phiên đăng nhập, đăng xuất, ranh giới Staff / contact, 7 bug đang mở — lấy từ V1 + V2 + V3 `Permission` + rà lại bug cũ. Vai trò: Staff (`EMAIL_ADMIN`) · contact (`EMAIL_CUSTOMER`).
**Ngày lập:** 08-11-2026

### Điều kiện chung

| Mục | Quy ước |
|---|---|
| Địa chỉ | Gốc `https://crm.anhtester.com` (`BASE_URL`) — mọi đường dẫn trong bảng nối sau địa chỉ này |
| 4 màn hình | Login quản trị `/admin/authentication` · Quên mật khẩu quản trị `/admin/authentication/forgot_password` · Login cổng `/authentication/login` · Quên mật khẩu cổng `/authentication/forgot_password` |
| Trạng thái đầu mục | Cửa sổ ẩn danh mới, chưa đăng nhập, giao diện tiếng Anh — trừ nhóm có ghi điều kiện riêng |
| Tài khoản | `EMAIL_ADMIN` / `PASSWORD_ADMIN` (Staff) · `EMAIL_CUSTOMER` / `PASSWORD_CUSTOMER` (contact) trong `.env` — **người chạy tự nhập**, không chép giá trị vào báo cáo |
| Email mẫu | `auto_login_<số>…` là email **không tồn tại** — mỗi lần chạy đổi phần số thành timestamp hiện tại |
| Ký hiệu | 🔧 cần DevTools · `P1` luồng sống còn (đăng nhập, phiên, đăng xuất, phân quyền) · `P2` màn hình và validation · `P3` lỗi nhỏ |
| ⚠️ Môi trường dùng chung | Mục 12 chỉ gửi sai **đúng 1 lần** trên tài khoản dùng chung · **Không** gửi Quên mật khẩu bằng `EMAIL_ADMIN` / `EMAIL_CUSTOMER` (sẽ phát email thật) |

### Nhóm: Giao diện & lối vào

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 1 | ☐ | Mở Login quản trị, gõ `abc123` vào ô Password | Đủ logo, tiêu đề `Login`, ô `Email Address` có con trỏ sẵn, ô `Password` chỉ hiện dấu chấm, `Remember me` chưa tích, nút `Login`, liên kết `Forgot Password?` | P2 | REQ-LOGIN-02, REQ-LOGIN-03, REQ-LOGIN-04, REQ-LOGIN-21 | CRM_LOGIN_TC_001 |
| 2 | ☐ | Bấm `Forgot Password?` ở Login quản trị | Sang `/admin/authentication/forgot_password`: tiêu đề `Forgot Password`, ô `Email Address` trống, nút `Confirm`, không có liên kết về Login | P2 | REQ-LOGIN-15, REQ-LOGIN-32 | CRM_LOGIN_TC_002, CRM_LOGIN_TC_006 |
| 3 | ☐ | Mở Login cổng | Tiêu đề `Please login`, `Language` hiện `English`, ô `Email Address`, `Password`, `Remember me` chưa tích, nút `Login`, liên kết `Forgot Password?`; không có `Register` | P2 | REQ-LOGIN-40, REQ-LOGIN-49 | CRM_LOGIN_TC_003 |
| 4 | ☐ | Bấm `Forgot Password?` ở Login cổng, rồi bấm `Login` đầu trang | Trang `Forgot Password` có ô `Email Address`, nút `Submit`, tab ghi `Forgot Password?`; bấm `Login` về `Please login` | P2 | REQ-LOGIN-40, REQ-LOGIN-51 | CRM_LOGIN_TC_004, CRM_LOGIN_TC_007 |
| 5 | ☐ | Mở `/admin/clients` khi chưa đăng nhập, rồi đăng nhập đúng | Bị đưa về `/admin/authentication`, không thấy nội dung quản trị; đăng nhập xong vào Dashboard, không phải trang Customers | P1 | REQ-LOGIN-01, REQ-LOGIN-18 | CRM_LOGIN_TC_005, CRM_LOGIN_TC_024 |

### Nhóm: Đăng nhập khu quản trị

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 6 | ☐ | Đăng nhập khu quản trị bằng `EMAIL_ADMIN` / `PASSWORD_ADMIN`, không tích `Remember me` | Vào Dashboard: tab có chữ `Dashboard`, địa chỉ `/admin/`, không còn form đăng nhập | P1 | REQ-LOGIN-17 | CRM_LOGIN_TC_008 |
| 7 | ☐ | Đăng nhập khu quản trị bằng `EMAIL_ADMIN` viết HOA, thêm 2 dấu cách hai đầu | Vào Dashboard, không báo lỗi định dạng hay sai thông tin | P2 | REQ-LOGIN-71, REQ-LOGIN-72 | CRM_LOGIN_TC_017, CRM_LOGIN_TC_018 |
| 8 | ☐ | Bỏ trống Email và Password ở Login quản trị, bấm `Login` | Đúng 2 khung đỏ `The Password field is required.` và `The Email Address field is required.` (không chấm thứ tự); không vào Dashboard | P2 | REQ-LOGIN-05, REQ-LOGIN-06, REQ-LOGIN-07 | CRM_LOGIN_TC_012, CRM_LOGIN_TC_013, CRM_LOGIN_TC_014 |
| 9 | ☐ | Nhập Email `auto_login_1791100002`, Password `Auto@12345` ở Login quản trị, bấm `Login` | Trình duyệt hiện bong bóng cảnh báo dưới ô Email; trang không nạp lại, không có khung đỏ, chữ ở hai ô còn nguyên | P2 | REQ-LOGIN-08 | CRM_LOGIN_TC_015 |
| 10 | ☐ | Nhập Email `auto_login_1791100003@autotest`, Password `Auto@12345` ở Login quản trị, bấm `Login` | 1 khung đỏ `The Email Address field must contain a valid email address.`; ô Email không hiện lại chuỗi đã nhập | P2 | REQ-LOGIN-09, REQ-LOGIN-12 | CRM_LOGIN_TC_016 |
| 11 | ☐ | Nhập Email `auto_login_1791100004@auto.test`, Password `Auto@12345` ở Login quản trị, bấm `Login`, nhấn F5 | Khung đỏ `Invalid email or password`, hai ô trống; sau F5 khung đỏ biến mất, không hỏi gửi lại biểu mẫu | P1 | REQ-LOGIN-10, REQ-LOGIN-11, REQ-LOGIN-12, REQ-LOGIN-13 | CRM_LOGIN_TC_019, CRM_LOGIN_TC_021 |
| 12 | ☐ | Nhập `EMAIL_ADMIN`, mật khẩu sai `auto_wrong_1791100005` ở Login quản trị, bấm `Login` đúng 1 lần | Cùng khung đỏ `Invalid email or password` như mục 11, cùng địa chỉ — không phân biệt được email có tồn tại | P1 | REQ-LOGIN-10 | CRM_LOGIN_TC_020 |
| 13 | ☐ | Bấm `Login` hai lần thật nhanh với tài khoản `EMAIL_ADMIN` đúng | Vào Dashboard; không hiện trang lỗi, không bị đòi đăng nhập lại | P2 | REQ-LOGIN-17 | CRM_LOGIN_TC_035 |

### Nhóm: Phiên đăng nhập & Ghi nhớ

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 14 | ☐ | Bấm Back sau khi đăng nhập, nhấn F5, rồi mở Login quản trị | Không lần nào hiện form đăng nhập, luôn ở Dashboard `/admin/` | P1 | REQ-LOGIN-17, REQ-LOGIN-19 | CRM_LOGIN_TC_009, CRM_LOGIN_TC_036 |
| 15 | ☐ | Đăng nhập không tích `Remember me`, xoá cookie `sp_session`, mở `/admin/` 🔧 | Bị đưa về trang Login quản trị, không tự đăng nhập lại | P1 | REQ-LOGIN-22 | CRM_LOGIN_TC_025 |
| 16 | ☐ | Đăng nhập có tích `Remember me`, xoá mọi cookie trừ `autologin`, mở `/admin/` 🔧 | Vào thẳng Dashboard, không đi qua trang Login | P1 | REQ-LOGIN-23, REQ-LOGIN-24 | CRM_LOGIN_TC_026, CRM_LOGIN_TC_027 |

### Nhóm: Đăng xuất

> **Điều kiện nhóm:** đã đăng nhập khu quản trị bằng `EMAIL_ADMIN`, thanh đầu trang không có bộ đếm giờ đang chạy.
> **Riêng mục 21:** trước khi chạy, tạo task `auto_login_timer_1791100008` ở menu `Tasks`, bấm `Start Timer`. Sau khi chạy: đăng nhập lại, dừng bộ đếm, xoá đúng task đó.

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 17 | ☐ | Bấm ảnh đại diện, bấm `Logout`, rồi mở `/admin/` | Menu đúng 5 mục `My Profile` · `My Timesheets` · `Edit Profile` · `Language` · `Logout`; về `/admin/authentication`; mở lại vẫn về Login | P1 | REQ-LOGIN-26, REQ-LOGIN-27 | CRM_LOGIN_TC_010 |
| 18 | ☐ | Bấm nút Back của trình duyệt ngay sau khi `Logout` | Không thấy lại Dashboard hay dữ liệu quản trị; hiện trang Login, địa chỉ `/admin/authentication` | P1 | REQ-LOGIN-27, REQ-LOGIN-30 | CRM_LOGIN_TC_033 |
| 19 | ☐ | Mở tab hai trang `Customers`, `Logout` ở tab một, bấm `Dashboard` ở tab hai | Tab hai bị đưa về trang Login quản trị, không hiện Dashboard | P2 | REQ-LOGIN-01, REQ-LOGIN-27 | CRM_LOGIN_TC_037 |
| 20 | ☐ | Sao chép `sp_session`, bấm `Logout`, gán lại giá trị cũ, mở `/admin/` 🔧 | Vẫn ở trang Login quản trị — phiên cũ không dùng lại được | P1 | REQ-LOGIN-27, REQ-LOGIN-29 | CRM_LOGIN_TC_032 |
| 21 | ☐ | Bấm `Logout` khi bộ đếm giờ đang chạy, rồi chọn `Logout` trong popup | Chưa đăng xuất, hiện popup `Started tasks timers found!` `Are you sure you want to logout without stopping the timers?`; chọn `Logout` thì về trang Login | P2 | REQ-LOGIN-28 | CRM_LOGIN_TC_031 |

### Nhóm: Quên mật khẩu

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 22 | ☐ | Nhập `auto_login_1791100010@autotest` ở Quên mật khẩu quản trị, bấm `Confirm` | Khung đỏ `The Email Address field must contain a valid email address.`; ô Email giữ nguyên chuỗi đã nhập | P2 | REQ-LOGIN-35, REQ-LOGIN-37 | CRM_LOGIN_TC_041 |
| 23 | ☐ | Nhập `auto_login_1791100017@autotest` ở Quên mật khẩu cổng, bấm `Submit` | Khung đỏ `The Email Address field must contain a valid email address.`; ô Email giữ nguyên chuỗi đã nhập | P2 | REQ-LOGIN-54, REQ-LOGIN-56 | CRM_LOGIN_TC_057 |

### Nhóm: Đăng nhập cổng khách hàng

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 24 | ☐ | Đăng nhập Login cổng bằng `EMAIL_CUSTOMER` / `PASSWORD_CUSTOMER` | Rời trang Login, về trang chủ cổng có menu `Projects` · `Invoices`; không còn form `Please login`, không có thông báo lỗi | P1 | REQ-LOGIN-50 | CRM_LOGIN_TC_011 |
| 25 | ☐ | Bỏ trống Email và Password ở Login cổng, bấm `Login` | Chữ đỏ ngay dưới từng ô: `The Email Address field is required.` · `The Password field is required.` | P2 | REQ-LOGIN-42, REQ-LOGIN-43 | CRM_LOGIN_TC_048, CRM_LOGIN_TC_049 |
| 26 | ☐ | Nhập Email `auto_login_1791100013`, Password `Auto@12345` ở Login cổng, bấm `Login` | Trình duyệt không chặn, trang nạp lại; dưới ô Email hiện chữ đỏ `The Email Address field must contain a valid email address.`, ô Email trống | P2 | REQ-LOGIN-41, REQ-LOGIN-45 | CRM_LOGIN_TC_047 |
| 27 | ☐ | Nhập Email `auto_login_1791100015@auto.test`, Password `Auto@12345` ở Login cổng, bấm `Login` | Góc trên phải hiện thông báo nổi `Invalid username or password`, tự ẩn sau khoảng 3,5 giây; ô Email trống, vẫn ở `Please login` | P1 | REQ-LOGIN-44, REQ-LOGIN-45 | CRM_LOGIN_TC_050 |
| 28 | ☐ | Chọn `Language` = `Vietnamese` ở Login cổng, mở lại trang, rồi mở Login quản trị | Cổng hiện `Xin hãy đăng nhập` · `Địa chỉ email` · `Mật khẩu`, mở lại vẫn tiếng Việt; Login quản trị vẫn tiếng Anh | P2 | REQ-LOGIN-46, REQ-LOGIN-47, REQ-LOGIN-48 | CRM_LOGIN_TC_051, CRM_LOGIN_TC_052, CRM_LOGIN_TC_053 |
| 29 | ☐ | Mở `/authentication/register` khi chưa đăng nhập | Không hiện form đăng ký; tự về `Please login`, địa chỉ chứa `/authentication/login` | P2 | REQ-LOGIN-49 | CRM_LOGIN_TC_054 |

### Nhóm: Phân quyền hai khu xác thực

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 30 | ☐ | Đăng nhập Login cổng bằng `EMAIL_ADMIN` / `PASSWORD_ADMIN` (Staff) | Thông báo nổi `Invalid username or password`; vẫn ở `Please login`, không vào khu khách hàng | P1 | REQ-LOGIN-60 | CRM_LOGIN_TC_061 |
| 31 | ☐ | Đăng nhập Login quản trị bằng `EMAIL_CUSTOMER` / `PASSWORD_CUSTOMER` (contact), rồi mở `/admin/` | Không vào Dashboard, ở lại trang Login kèm khung đỏ báo lỗi; mở `/admin/` vẫn về Login | P1 | REQ-LOGIN-59 | CRM_LOGIN_TC_062 |

### Nhóm: Rà lại bug đang mở

> Cả 9 mục FAIL ở lần chạy 10-10-2026 và ứng với 7 bug còn mở ([danh mục bug](../../../bugs/README.md)). Vẫn FAIL → ghi **FAIL — lỗi đã biết**, không mở bug mới, **không** hạ kỳ vọng (`RISK-LOGIN-07`). PASS → chạy `/retest-fixed-bugs` để đóng bug.

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 32 | ☐ | So giá trị `sp_session` trước và sau khi đăng nhập quản trị 🔧 | Hai giá trị khác nhau — đăng nhập cấp mã phiên mới (so trong công cụ, không chép ra báo cáo) | P1 | REQ-LOGIN-65 | CRM_LOGIN_TC_070 |
| 33 | ☐ | Nhập `auto_login_1791100011@auto.test` ở Quên mật khẩu quản trị, bấm `Confirm` | Thông báo chứa `If this email address exists in our system, we have sent password reset instructions.`; không có `Email not found` | P2 | REQ-LOGIN-36 | CRM_LOGIN_TC_042 |
| 34 | ☐ | Nhập `auto_login_1791100018@auto.test` ở Quên mật khẩu cổng, bấm `Submit` | Thông báo chứa `If this email address exists in our system, we have sent password reset instructions.`; không có `Email not found` | P2 | REQ-LOGIN-55 | CRM_LOGIN_TC_058 |
| 35 | ☐ | Đăng nhập có tích `Remember me`, xem cờ `HttpOnly` của cookie `autologin` 🔧 | Cookie `autologin` có `HttpOnly` bật — script trên trang không đọc được | P2 | REQ-LOGIN-63 | CRM_LOGIN_TC_071 |
| 36 | ☐ | Bỏ trống Email ở Quên mật khẩu quản trị, bấm `Confirm` | Khung đỏ `The Email Address field is required.`; không có `Email not found` | P2 | REQ-LOGIN-34 | CRM_LOGIN_TC_040 |
| 37 | ☐ | Bỏ trống Email ở Quên mật khẩu cổng, bấm `Submit` | Khung đỏ `The Email Address field is required.`; không có `Email not found` | P2 | REQ-LOGIN-53 | CRM_LOGIN_TC_056 |
| 38 | ☐ | Mở `/admin/authentication/reset_password` không kèm tham số | Hiện trang báo không tìm thấy (Not Found), không phải trang lỗi máy chủ | P3 | REQ-LOGIN-64 | CRM_LOGIN_TC_045 |
| 39 | ☐ | Đọc tiêu đề tab trang Quên mật khẩu quản trị | Đúng nguyên văn `Forgot Password?`, không chứa chữ `Login` | P3 | REQ-LOGIN-69 | CRM_LOGIN_TC_046 |
| 40 | ☐ | Mở Login quản trị, xem tab Console của DevTools 🔧 | Không có cảnh báo của Chrome về ô nhập thiếu thuộc tính tự điền (`autocomplete`) | P3 | REQ-LOGIN-70 | CRM_LOGIN_TC_076 |

### Không áp dụng cho module này

- **Data Table / List** — module không có bảng dữ liệu.
- **CRUD Lifecycle** — module không tạo / sửa / xoá bản ghi nghiệp vụ (task ở mục 21 chỉ là dữ liệu dựng để hiện popup).
- **Status Flow** — không có entity mang trạng thái; chuyển trạng thái phiên đã rà ở nhóm Phiên đăng nhập và Đăng xuất.
- **Field-Level ngoài 4 loại Email · Password · Checkbox · Dropdown** — module không có trường ngày, số, tiền, file, rich text.

### Phần đã cắt khỏi checklist

> Module có 88 TC; ngưỡng Regression là 40 mục. Checklist phủ 52 TC. Phần dưới vẫn chạy được từ bộ TC chi tiết.

| Phần cắt | TC | Lý do |
|---|---|---|
| V3 `Security` (ngoài các bug đang mở) | `_063`–`_069` · `_072` · `_073` | Regression chỉ lấy nhánh V3 `Permission` |
| V4 `Accessibility` · `Responsive` · `Compatibility` | `_074` · `_075` · `_077`–`_079` · `_081`–`_083` · `_086`–`_088` | Regression bỏ V4, trừ phần rà lại bug cũ |
| Chờ hộp thư test | `_043` · `_044` · `_059` · `_060` | BLOCKED — chưa đọc được email đặt lại mật khẩu (`RISK-LOGIN-03`) |
| Không kiểm được trong 2 phút | `_028` · `_029` (chờ ≥ 2 phút, cần DevTools) · `_030` (chờ 8 giờ 5 phút) | Vượt giới hạn mục checklist. `_029` FAIL ở lần chạy 10-10-2026, chưa có bug report |
| Cần hơn 3 thao tác | `_034` | Sao chép, đăng xuất, xoá cookie, gán lại cookie — giữ ở mức test case |
| Giữ trong ngưỡng 40 mục | `_022` · `_023` · `_038` · `_039` · `_055` · `_080` · `_084` · `_085` | Rủi ro thấp, hoặc cùng cơ chế với mục đã có (`_039` · `_055` như mục 9) |
| Biến thể của TC đã gom | `_012-b` · `_013-b` · `_015-b→e` · `_016-b` · `_019-b` · `_047-b→e` · `_050-b` · `_058-b` | Checklist rà biến thể đại diện, mục 34 chạy tiếng Anh |

**REQ chưa có mục (56 / 72 REQ có mục):** REQ-LOGIN-14 · 16 · 20 · 25 · 31 · 33 · 38 · 39 · 52 · 57 · 58 · 61 · 62 · 66 · 67 · 68 — đều có TC ở phần đã cắt, cần rà thì chạy bộ TC.

### Ký nhận

| Môi trường | Build / Version | Người thực hiện | Ngày | Kết quả (Pass/Fail/Blocked) |
|---|---|---|---|---|
|  |  |  |  |  |
