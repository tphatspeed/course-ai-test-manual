# Requirements — Đăng nhập & Tài khoản cá nhân (Web)

← Quay lại index: [`requirements_login.md`](../requirements_login.md)

> File này chứa **REQ chỉ áp nền tảng Web**, Đặc tả trường dữ liệu, Validation Messages, User Flow và Danh mục Evidence.
> Phần cắt ngang (ma trận phân quyền · trạng thái · AMB/RISK · Epic/Story · Nhật ký) nằm ở **index**.

---

## 0. Metadata khảo sát

| Mục | Giá trị |
|---|---|
| **Trình duyệt khảo sát** | Google Chrome `153.0.0.0` (Playwright MCP), viewport `1600×750` (đo `innerWidth×innerHeight`). Mọi AC dựa trên thông báo mặc định của trình duyệt **chỉ đúng với trình duyệt này** |
| **Tầng network** | `browser_network_requests` bật suốt phiên. Quan sát **thụ động** request do UI phát sinh — không gọi API trực tiếp |
| **Ngày khảo sát** | 20-09-2026 |
| **Môi trường** | Demo dùng chung (`RISK-LOGIN-01`). Ngôn ngữ giao diện: English |
| **Phạm vi thao tác đã được duyệt** | Thử sai **chỉ bằng email không tồn tại** · được đăng xuất/đăng nhập lại · submit form rỗng-sai để lấy validation · **không** Save dữ liệu thật · **không** gửi email đặt lại mật khẩu · **không** đổi ngôn ngữ |

### Bản đồ phủ tài liệu

Không có tài liệu — toàn bộ REQ sinh từ khảo sát UI thực tế.

---

## 1. Màn hình khảo sát

| Màn hình | Route | Trạng thái khảo sát |
|---|---|---|
| Đăng nhập | `/admin/authentication` | ✅ Đầy đủ (trừ phần khoá tài khoản) |
| Quên mật khẩu | `/admin/authentication/forgot_password` | 🟨 Chỉ giao diện — không submit (cam kết phạm vi) |
| Hồ sơ cá nhân | `/admin/profile` | ✅ Đầy đủ |
| Sửa hồ sơ (3 form) | `/admin/staff/edit_profile` | ✅ Validation đầy đủ · ❔ luồng lưu thành công |
| Đổi ngôn ngữ | `/admin/staff/change_language/<lang>` | 🟨 Chỉ đọc DOM — không kích hoạt |
| Đăng xuất | Menu tài khoản → `Logout` | ✅ Đầy đủ |

---

## 2. Yêu cầu Chức năng — Web

Quy ước cột `Nguồn`: `Kiểm chứng thực tế` = đã tương tác và xác nhận · `UI thực tế` = chỉ quan sát/đọc DOM · `API` = quan sát tầng network.

### 2.1. Đăng nhập

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-01 | Đăng nhập thành công bằng email và mật khẩu hợp lệ | Người dùng nhập đúng thông tin đăng nhập thì vào được khu quản trị | Nhập email + mật khẩu hợp lệ → bấm `Login` → URL **chứa** `/admin`, `document.title` = `Dashboard` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-02 | Bỏ trống Email bị máy chủ từ chối | Máy chủ chặn khi thiếu email, trả về cùng trang kèm thông báo | Để trống cả 2 ô → `Login` → trang hiển thị `.alert.alert-danger` nguyên văn **`The Email Address field is required.`** | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-03 | Bỏ trống Mật khẩu bị máy chủ từ chối | Rule độc lập với REQ-LOGIN-02 | Để trống cả 2 ô → `Login` → trang hiển thị `.alert.alert-danger` nguyên văn **`The Password field is required.`** | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-04 | Email sai định dạng bị chặn ngay tại trình duyệt | Ô Email là `type="email"` nên trình duyệt chặn trước khi gửi | Nhập `khong-phai-email` + mật khẩu bất kỳ → `Login` → **không phát sinh request POST nào** tới `/admin/authentication`; `form.checkValidity() === false`, `email.validity.typeMismatch === true`. ⚠️ **CẤM assert nội dung tooltip** — xem `AMB` ở index và mục 4 | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-05 | Sai thông tin đăng nhập hiển thị thông báo chung | Email không tồn tại hoặc mật khẩu sai đều cho cùng một thông báo | Nhập email không tồn tại + mật khẩu bất kỳ → `Login` → hiển thị `.alert.alert-danger` nguyên văn **`Invalid email or password`** | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-06 | Thông báo lỗi không tiết lộ email có tồn tại hay không | Chống dò tài khoản (user enumeration) | Thông báo ở REQ-LOGIN-05 **không** chứa chuỗi phân biệt như `email not found` / `user does not exist`; cùng một câu cho mọi trường hợp sai. ⚠️ Vế "mật khẩu sai trên email có thật cũng ra câu này" **chưa kiểm chứng** — xem `AMB-LOGIN-02` | 🟡 | — | Kiểm chứng thực tế (một phía) |
| REQ-LOGIN-07 | Máy chủ không trả lại giá trị Email sau khi đăng nhập lỗi | Trang trả về sau lỗi không mang theo giá trị đã nhập | Sau REQ-LOGIN-05: `outerHTML` của `#email` **không chứa** thuộc tính `value=`. ⚠️ **CẤM assert ô Email rỗng trên màn hình** — trình duyệt có thể tự điền lại | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-08 | Ô Mật khẩu rỗng sau khi submit lỗi | Mật khẩu không được giữ lại | Sau REQ-LOGIN-05: `document.querySelector('#password').value === ''` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-09 | Form đăng nhập có token CSRF | Form mang trường ẩn chống giả mạo yêu cầu | `form` chứa `input[type=hidden][name="csrf_token_name"]`; giá trị là chuỗi sinh theo phiên — **ghi hình thái, không chép giá trị** | 🟢 | — | UI thực tế |
| REQ-LOGIN-10 | Form đăng nhập không có CAPTCHA | Không có lớp chống máy nào ở giao diện | Không tồn tại `.g-recaptcha`, `iframe[src*="recaptcha"]`, phần tử có class chứa `captcha`. Xem `AMB-LOGIN-13` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-11 | Checkbox "Remember me" mặc định không được tích | Trạng thái ban đầu của form | Tải `/admin/authentication` lần đầu → `#remember.checked === false` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-12 | Checkbox "Remember me" mang giá trị gửi đi là `estimate` | Giá trị được gửi lên khi người dùng tích chọn | `#remember` có thuộc tính `value="estimate"`. ⚠️ Giá trị này **không liên quan** tới ngữ nghĩa "ghi nhớ đăng nhập" — xem `AMB-LOGIN-03`. Automation phải dùng đúng giá trị này khi dựng request | 🟢 | — | UI thực tế |
| REQ-LOGIN-13 | Tác dụng của "Remember me" đối với thời hạn phiên | Tích chọn thì phiên phải sống lâu hơn bình thường | 🅰️ **Theo giả định `AMB-LOGIN-03`** *(chốt 20-09-2026)*: máy chủ chỉ xét **sự có mặt** của tham số `remember`, không xét giá trị → chức năng hoạt động dù giá trị gửi lên là `estimate`. ❔ **Vẫn chưa kiểm chứng** — cần so sánh thời hạn cookie phiên giữa hai lần đăng nhập có/không tích; công cụ khảo sát không đọc được header `Set-Cookie` (`AMB-LOGIN-12`) | ⚪ | 20-09-2026 · DECISION-LOGIN-01 | Assumption tạm — `AMB-LOGIN-03` |
| REQ-LOGIN-14 | Hai loại lỗi đăng nhập đi theo hai đường phản hồi khác nhau | Lỗi thiếu trường và lỗi sai thông tin xử lý khác nhau ở tầng HTTP | Thiếu trường → `POST /admin/authentication` trả **`200`**, nội dung lỗi nằm ngay trong phản hồi (không điều hướng). Sai thông tin → `POST` trả **`303`** → `GET /admin/authentication` → `200`, lỗi hiện sau khi điều hướng. **Hệ quả cho automation:** kịch bản sai-thông-tin phải chờ điều hướng, kịch bản thiếu-trường thì không. 🅰️ **Chốt theo giả định `AMB-LOGIN-11`** *(20-09-2026)*: đây là **hành vi cố ý** của framework, không phải lỗi → không mở bug, kịch bản kiểm thử bám đúng hai đường này | 🟡 | 20-09-2026 · DECISION-LOGIN-01 | API · POST /admin/authentication · 200 và 303 |
| REQ-LOGIN-15 | Liên kết "Forgot Password?" dẫn tới màn hình Quên mật khẩu | Lối vào chức năng khôi phục mật khẩu | Trang đăng nhập có `a` nhãn `Forgot Password?` với `href` = `/admin/authentication/forgot_password` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-59 | Hệ thống không khoá tài khoản sau nhiều lần đăng nhập sai | Sai mật khẩu bao nhiêu lần cũng không bị chặn đăng nhập ở lần đúng tiếp theo | 🅰️ **Theo giả định `AMB-LOGIN-02`** *(chốt 20-09-2026)*: không có cơ chế khoá tài khoản; mọi lần sai đều trả cùng `Invalid email or password`, và ngay sau đó nhập đúng vẫn đăng nhập được bình thường. ❔ **Chưa kiểm chứng** — cần **tài khoản dùng riêng** mới thử được (`RISK-LOGIN-02`); tuyệt đối không thử trên tài khoản dùng chung. 🚨 Nếu giả định này **sai** thì kịch bản kiểm thử sẽ bỏ sót toàn bộ nhánh khoá tài khoản; nếu giả định **đúng** thì đây là **khiếm khuyết bảo mật** cần báo cáo, không phải hành vi mong muốn | ⚪ | 20-09-2026 · DECISION-LOGIN-01 | Assumption tạm — `AMB-LOGIN-02` |

### 2.2. Bảo vệ route và vòng đời phiên

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-16 | Route khu quản trị chặn người chưa đăng nhập | Truy cập thẳng URL khi chưa có phiên thì bị đưa về trang đăng nhập | Ở trạng thái chưa đăng nhập, mở `/admin/clients` → điểm dừng là `/admin/authentication`, `document.title` chứa `Login`. ⚠️ **CẤM assert mã trạng thái** của bước chuyển hướng — xem `AMB-LOGIN-04` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-17 | Hệ thống không nêu lý do khi chặn truy cập | Người dùng bị đưa về trang đăng nhập trắng, không có thông báo | Sau REQ-LOGIN-16: trang đăng nhập **không** hiển thị `.alert` nào. 🅰️ **Chốt theo giả định `AMB-LOGIN-06`** *(20-09-2026)*: đây là **hành vi cố ý**, không phải khiếm khuyết → kịch bản kiểm thử khẳng định "không có thông báo", **không** mở bug về việc thiếu thông báo | 🟡 | 20-09-2026 · DECISION-LOGIN-01 | Kiểm chứng thực tế |
| REQ-LOGIN-18 | Hệ thống không giữ lại trang đích sau khi buộc đăng nhập | Đăng nhập xong luôn về Dashboard, không quay lại nơi đang xem | Bị chặn ở `/admin/clients` (REQ-LOGIN-16) → đăng nhập hợp lệ → điểm dừng **chứa** `/admin`, **không** phải `/admin/clients`. URL trang đăng nhập cũng không mang tham số `redirect`/`return_url`. 🅰️ **Chốt theo giả định `AMB-LOGIN-07`** *(20-09-2026)*: **hành vi cố ý**, luôn về Dashboard → không mở bug | 🟡 | 20-09-2026 · DECISION-LOGIN-01 | Kiểm chứng thực tế |
| REQ-LOGIN-19 | Người đã đăng nhập không vào được trang đăng nhập | Vào lại trang đăng nhập khi đang có phiên thì bị đưa về Dashboard | Đang đăng nhập, mở `/admin/authentication` → điểm dừng **chứa** `/admin`, `document.title` = `Dashboard`. ⚠️ **CẤM assert mã trạng thái** — `AMB-LOGIN-04` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-20 | Cookie phiên không đọc được bằng JavaScript | Cookie phiên đặt cờ `HttpOnly` nên mã phía trang không lấy được | Đang ở trạng thái **đã đăng nhập** (phiên chắc chắn đang hoạt động), `document.cookie === ''` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-21 | Phiên đăng nhập cho phép truy cập khu quản trị | Vế "tác tạo dùng được" của phiên — phiên tạo ra phải thật sự mở được vùng cần quyền | Sau REQ-LOGIN-01, mở `/admin/clients` → vào được (điểm dừng vẫn là `/admin/clients`, không bị đưa về trang đăng nhập) | 🟢 | — | Kiểm chứng thực tế |

### 2.3. Đăng xuất

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-22 | Đăng xuất từ menu tài khoản đưa về trang đăng nhập | Mục `Logout` trong menu avatar kết thúc phiên | Mở menu avatar → bấm `Logout` → điểm dừng `/admin/authentication`, `document.title` = `Perfex CRM \| Anh Tester Demo - Login` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-23 | Đăng xuất huỷ phiên thật sự, không chỉ chuyển trang | Sau khi đăng xuất, phiên cũ không còn dùng được | Phép thử trạng thái sạch: (1) đăng xuất theo REQ-LOGIN-22 → (2) xác nhận đang ở `/admin/authentication` → (3) mở `/admin/` → (4) điểm dừng vẫn là `/admin/authentication` (không vào được) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-24 | Ở viewport desktop chỉ có một lối đăng xuất dùng được | Trang chứa 3 phần tử mang chữ "Logout" nhưng chỉ 1 phần tử người dùng thao tác được | Tại viewport `1600×750`, sau khi mở menu avatar: `li.icon.header-user-profile li.header-logout > a` có `offsetParent !== null` và kích thước `160×32`. Hai phần tử còn lại **không dùng được**: bản trong `div#mobile-collapse` bị tổ tiên `display:none` (rect `0×0`), bản trong `div#timers-logout-template-warning.hide` là template cảnh báo bấm giờ. ⚠️ Kết luận **chỉ đúng với viewport này** — viewport hẹp cần khảo sát riêng | 🟢 | — | Kiểm chứng thực tế |

### 2.4. Quên mật khẩu

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-25 | Màn hình Quên mật khẩu chỉ yêu cầu địa chỉ email | Form khôi phục gồm một ô nhập và một nút xác nhận | Mở `/admin/authentication/forgot_password` → tiêu đề khối là `Forgot Password`; form có đúng 1 ô nhập hiển thị (`input[type=email][name=email]`) + 1 nút `Confirm`; ngoài ra chỉ có `input[type=hidden][name=csrf_token_name]` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-26 | Màn hình Quên mật khẩu không có lối quay lại trang đăng nhập | Người dùng vào nhầm không có nút/liên kết nào để trở ra | Trên `/admin/authentication/forgot_password`, `document.querySelectorAll('a')` **không** có phần tử nào có nội dung chữ. 🅰️ **Chốt theo giả định `AMB-LOGIN-08`** *(20-09-2026)*: đây là **khiếm khuyết giao diện được chấp nhận** ở giai đoạn này → kịch bản kiểm thử ghi nhận hiện trạng, **không** mở bug chặn phát hành; người dùng dùng nút Back của trình duyệt | 🟡 | 20-09-2026 · DECISION-LOGIN-01 | Kiểm chứng thực tế |
| REQ-LOGIN-27 | Tiêu đề trang Quên mật khẩu không phản ánh màn hình đang xem | Tiêu đề vẫn là tiêu đề trang đăng nhập | Trên `/admin/authentication/forgot_password`, `document.title` = `Perfex CRM \| Anh Tester Demo - Login`. 🅰️ **Chốt theo giả định `AMB-LOGIN-09`** *(20-09-2026)*: **khiếm khuyết được chấp nhận**, không cố ý nhưng không sửa ở giai đoạn này → không mở bug | 🟡 | 20-09-2026 · DECISION-LOGIN-01 | Kiểm chứng thực tế |
| REQ-LOGIN-28 | Yêu cầu khôi phục mật khẩu trả thông báo chung cho mọi địa chỉ email | Không phân biệt email có tồn tại hay không — cùng nguyên tắc chống dò tài khoản như màn hình đăng nhập | 🅰️ **Theo giả định `AMB-LOGIN-14`** *(chốt 20-09-2026)*: gửi yêu cầu cho email **có thật** và email **không tồn tại** đều nhận **cùng một thông báo**, không tiết lộ địa chỉ nào đang tồn tại. ❔ **Chưa kiểm chứng** — phạm vi khảo sát loại trừ thao tác gửi email (hộp thư dùng chung). Nội dung nguyên văn của thông báo **chưa biết** → kịch bản kiểm thử chỉ so sánh **hai thông báo có giống nhau không**, không assert nội dung cụ thể | ⚪ | 20-09-2026 · DECISION-LOGIN-01 | Assumption tạm — `AMB-LOGIN-14` |

### 2.5. Trang Hồ sơ cá nhân

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-29 | Trang hồ sơ hiển thị 5 ô thống kê thời gian đã ghi nhận | Tổng quan giờ làm của chính người đang đăng nhập | Mở `/admin/profile` → hiển thị đúng 5 ô với nhãn: `Total Logged Time`, `Last Month Logged Time`, `This Month Logged Time`, `Last Week Logged Time`, `This Week Logged Time` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-30 | Thời gian ở 5 ô thống kê theo định dạng giờ:phút | Giá trị hiển thị có cấu trúc cố định | Mỗi ô ở REQ-LOGIN-29 khớp biểu thức `^\d+:\d{2}$`. ⚠️ **CẤM assert giá trị cụ thể** — số giờ thay đổi theo dữ liệu và theo thời điểm chạy | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-31 | Trang hồ sơ hiển thị thông tin liên hệ của người đang đăng nhập | Thẻ thông tin gồm tên, email, số điện thoại | Trên `/admin/profile`, khối thông tin hiển thị họ tên, địa chỉ email và số điện thoại của tài khoản đang đăng nhập | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-32 | Trang hồ sơ liệt kê các dự án của người đang đăng nhập | Bảng dữ liệu dự án gắn với tài khoản | Trên `/admin/profile` có bảng (DataTables) với đúng 4 cột: `Project Name`, `Start Date`, `Deadline`, `Status` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-33 | Trang hồ sơ có khối thông báo kèm thao tác đánh dấu đã đọc | Khu vực thông báo của tài khoản | Trên `/admin/profile` có khối tiêu đề chứa `Notifications` và thao tác `Mark all as read` | 🟢 | — | UI thực tế |
| REQ-LOGIN-34 | Tiêu đề trang hồ sơ mang tên người dùng | Tiêu đề trình duyệt phản ánh chủ hồ sơ | Trên `/admin/profile`, `document.title` **bắt đầu bằng** `Profile - ` và **chứa** họ tên người đang đăng nhập. ⚠️ Phần tên là động — không assert khớp tuyệt đối | 🟢 | — | Kiểm chứng thực tế |

### 2.6. Sửa hồ sơ — thông tin cá nhân

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-35 | Trang Sửa hồ sơ gồm ba biểu mẫu độc lập, mỗi biểu mẫu lưu riêng | Ba nhóm thiết lập tách biệt, không lưu chung một nút | `/admin/staff/edit_profile` có đúng 3 `form`: `#staff_profile_table` → `/admin/staff/edit_profile`, `#staff_password_change_form` → `/admin/staff/change_password_profile`, `#two_factor_auth_form` → `/admin/staff/update_two_factor`; mỗi form có đúng 1 nút `Save` kiểu `submit` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-36 | Trường First Name bắt buộc nhập | Không để trống được khi lưu hồ sơ | Xoá trắng `input[name=firstname]` → bấm `Save` của `#staff_profile_table` → hiển thị nguyên văn **`This field is required.`**, nhóm trường nhận class `has-error` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-37 | Trường Last Name bắt buộc nhập | Rule độc lập với REQ-LOGIN-36 | Nhãn `* Last Name` mang dấu bắt buộc như First Name. ⚠️ Chưa trigger riêng — suy từ cùng cơ chế nhãn `*`. Xem `AMB-LOGIN-17` | 🟡 | — | UI thực tế |
| REQ-LOGIN-38 | Trường Email trên form Sửa hồ sơ không cho sửa | Người dùng không tự đổi được email đăng nhập | `#email` trên `/admin/staff/edit_profile` có `disabled === true` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-39 | Giá trị Email không được gửi lên khi lưu hồ sơ | Email không nằm trong dữ liệu submit | `#email` **không có** thuộc tính `name` (và đang `disabled`) → không xuất hiện trong dữ liệu form. **Hệ quả:** không thể đổi email qua màn hình này kể cả khi gỡ `disabled` bằng công cụ phát triển | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-40 | Lỗi bắt buộc của form hồ sơ được chặn ngay tại trình duyệt | Dữ liệu không rời máy người dùng khi còn lỗi | Lặp lại REQ-LOGIN-36 → **không phát sinh request POST nào** tới `/admin/staff/edit_profile` | 🟢 | — | Kiểm chứng thực tế · API (không có POST) |
| REQ-LOGIN-41 | Danh sách Default Language có 27 lựa chọn | Người dùng chọn ngôn ngữ mặc định của tài khoản | `#default_language` có `options.length === 27`, trong đó lựa chọn đầu tiên có `value=""` nhãn `System Default`; 26 lựa chọn còn lại có `value` là tên ngôn ngữ viết thường (`english`, `chinese`, `vietnamese`, …) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-42 | Danh sách Direction có 3 lựa chọn, trong đó một lựa chọn trống | Chiều hiển thị giao diện | `#direction` có `options.length === 3`: lựa chọn đầu `value=""` **và nhãn cũng rỗng**, rồi `ltr` (`LTR`), `rtl` (`RTL`). ⚠️ Lựa chọn rỗng không nhìn thấy trên giao diện nhưng **vẫn được đếm** khi tự động hoá. 🅰️ **Chốt theo giả định `AMB-LOGIN-16`** *(20-09-2026)*: lựa chọn rỗng là **mục giữ chỗ vô hại**, máy chủ bỏ qua giá trị rỗng → không mở bug; kịch bản kiểm thử vẫn phải đếm đủ 3 và **không** chọn mục rỗng | 🟡 | 20-09-2026 · DECISION-LOGIN-01 | Kiểm chứng thực tế |
| REQ-LOGIN-43 | Các trường hồ sơ không có giới hạn độ dài ở tầng giao diện | Không có `maxlength`/`pattern` trên bất kỳ trường nào | Mọi `input`/`textarea` của `#staff_profile_table` đều không có thuộc tính `maxlength` và `pattern`. Ràng buộc (nếu có) nằm ở máy chủ — chưa kiểm chứng | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-44 | Lưu hồ sơ hợp lệ thành công | Dữ liệu hợp lệ được ghi nhận và hiển thị lại sau khi tải lại trang | ❔ **Chưa kiểm chứng** — phạm vi khảo sát không cho lưu dữ liệu thật trên môi trường dùng chung (`RISK-LOGIN-04`) | ⚪ | — | Chưa kiểm chứng |
| REQ-LOGIN-62 | Máy chủ kiểm tra lại trường bắt buộc của form hồ sơ | Bỏ qua lớp kiểm tra của trình duyệt thì máy chủ vẫn từ chối dữ liệu thiếu trường bắt buộc | 🅰️ **Theo giả định `AMB-LOGIN-17`** *(chốt 20-09-2026)*: gửi thẳng dữ liệu thiếu `firstname`/`lastname` (vô hiệu hoá JavaScript hoặc dựng request trực tiếp) → máy chủ **từ chối**, không ghi dữ liệu rỗng. Căn cứ của giả định: form đăng nhập đã chứng minh máy chủ biết kiểm tra trường bắt buộc (REQ-LOGIN-02 · 03). ❔ **Chưa kiểm chứng** — nằm ngoài phạm vi thao tác đã duyệt (không lưu dữ liệu thật). 🚨 Nếu giả định **sai** thì đây là lỗ hổng cho phép ghi dữ liệu rỗng vào hồ sơ | ⚪ | 20-09-2026 · DECISION-LOGIN-01 | Assumption tạm — `AMB-LOGIN-17` |

### 2.7. Đổi mật khẩu

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-45 | Trường Old password bắt buộc nhập | Phải xác thực mật khẩu hiện tại trước khi đổi | Để trống cả 3 ô → bấm `Save` của `#staff_password_change_form` → nhóm `oldpassword` hiển thị nguyên văn **`This field is required.`** | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-46 | Trường New password bắt buộc nhập | Rule độc lập với REQ-LOGIN-45 | Cùng thao tác REQ-LOGIN-45 → nhóm `newpassword` hiển thị nguyên văn **`This field is required.`** | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-47 | Trường Repeat new password **không** bắt buộc nhập | Bỏ trống ô nhập lại vẫn qua được lớp kiểm tra giao diện | Cùng thao tác REQ-LOGIN-45 → nhóm `newpasswordr` **không** nhận class `has-error` và **không** hiển thị thông báo bắt buộc. Nhãn cũng không có dấu `*`. Xem `AMB-LOGIN-05` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-48 | Repeat new password khi có nhập thì phải trùng New password | Kiểm tra khớp chỉ chạy khi ô được điền | Nhập `newpassword` và `newpasswordr` **khác nhau** → `Save` → nhóm `newpasswordr` hiển thị nguyên văn **`Please enter the same value again.`** | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-49 | Không có ràng buộc độ mạnh mật khẩu ở tầng giao diện | Giao diện không chặn mật khẩu yếu | 3 ô mật khẩu đều không có `minlength`, `maxlength`, `pattern`; không có chỉ báo độ mạnh. Ràng buộc phía máy chủ chưa kiểm chứng — xem `AMB-LOGIN-10` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-50 | Khối đổi mật khẩu hiển thị thời điểm đổi mật khẩu gần nhất | Người dùng biết lần đổi gần nhất cách đây bao lâu | Khối `Change your password` hiển thị dòng **bắt đầu bằng** `Password last changed:` kèm mô tả thời gian tương đối. ⚠️ Phần thời gian là động — **CẤM assert giá trị** | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-51 | Đổi mật khẩu thành công với thông tin hợp lệ | Mật khẩu được cập nhật và dùng được ở lần đăng nhập sau | ❔ **Chưa kiểm chứng** — đổi mật khẩu trên tài khoản dùng chung duy nhất sẽ chặn toàn bộ đợt kiểm thử (`RISK-LOGIN-03`) | ⚪ | — | Chưa kiểm chứng |
| REQ-LOGIN-60 | Đổi mật khẩu thành công khi bỏ trống ô nhập lại mật khẩu | Ô xác nhận để trống vẫn đổi được mật khẩu theo giá trị ở ô "New password" | 🅰️ **Theo giả định `AMB-LOGIN-05`** *(chốt 20-09-2026)*: nhập đúng `oldpassword` + `newpassword`, **để trống** `newpasswordr` → máy chủ chấp nhận và đổi mật khẩu theo `newpassword`. ❔ **Chưa kiểm chứng** (`RISK-LOGIN-03`). 🚨 Đây là **mô tả hành vi giả định, KHÔNG phải xác nhận rằng hành vi này đúng đắn** — nếu giả định đúng thì hệ thống thiếu cơ chế chống gõ nhầm và người dùng có thể tự khoá mình khỏi hệ thống; cần báo cáo như một khiếm khuyết | ⚪ | 20-09-2026 · DECISION-LOGIN-01 | Assumption tạm — `AMB-LOGIN-05` |
| REQ-LOGIN-61 | Máy chủ có ràng buộc độ dài tối thiểu cho mật khẩu mới | Mật khẩu quá ngắn bị máy chủ từ chối dù giao diện không chặn | 🅰️ **Theo giả định `AMB-LOGIN-10`** *(chốt 20-09-2026)*: máy chủ kiểm độ dài tối thiểu, giao diện không nêu ra. ⛔ **Ngưỡng cụ thể CHƯA XÁC ĐỊNH** — giả định chỉ khẳng định *có ràng buộc*, không cho biết *bao nhiêu ký tự*. **Hệ quả:** chưa viết được kịch bản kiểm thử giá trị biên; chỉ viết được kịch bản khẳng định "mật khẩu 1 ký tự bị từ chối". Cần PO chốt con số trước khi phủ biên | ⚪ | 20-09-2026 · DECISION-LOGIN-01 | Assumption tạm — `AMB-LOGIN-10` |

### 2.8. Xác thực hai lớp

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-52 | Xác thực hai lớp có ba phương thức loại trừ nhau | Người dùng chọn một trong ba | `#two_factor_auth_form` có 3 `input[type=radio][name=two_factor_auth]`: `value="off"` nhãn `Disabled` · `value="email"` nhãn `Enable Email Two Factor Authentication` · `value="google"` nhãn `Enable Google Authenticator`. Cả 3 đều `enabled` và nhìn thấy được | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-53 | Xác thực hai lớp mặc định ở trạng thái tắt | Trạng thái khởi điểm của tài khoản khảo sát | `#two_factor_auth_disabled.checked === true`, hai lựa chọn còn lại `checked === false`. ⚠️ Đây là trạng thái **của tài khoản đang khảo sát**, không phải mặc định của tài khoản mới tạo | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-54 | Bật xác thực hai lớp qua email thì đăng nhập có thêm bước nhập mã | Sau khi nhập đúng mật khẩu, hệ thống hỏi mã gửi qua email trước khi cho vào | 🅰️ **Theo giả định `AMB-LOGIN-15`** *(chốt 20-09-2026)*: chọn `email` → lần đăng nhập sau, sau bước nhập mật khẩu đúng, hiển thị **thêm một màn hình nhập mã** gửi tới hộp thư của tài khoản; nhập đúng mã mới vào Dashboard. ❔ **Chưa kiểm chứng** — bật 2FA trên tài khoản dùng chung có nguy cơ khoá quyền truy cập cả nhóm (`RISK-LOGIN-03`). Luồng của `google` (Google Authenticator) và cách khôi phục khi mất thiết bị **vẫn chưa có giả định nào** | ⚪ | 20-09-2026 · DECISION-LOGIN-01 | Assumption tạm — `AMB-LOGIN-15` |

### 2.9. Menu tài khoản & ngôn ngữ

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-55 | Menu tài khoản có 5 mục | Menu mở ra từ ảnh đại diện ở thanh đầu trang | Bấm `li.icon.header-user-profile > a` → `ul.dropdown-menu` có `display: block` và chứa đúng 5 mục con trực tiếp theo thứ tự: `My Profile`, `My Timesheets`, `Edit Profile`, `Language`, `Logout` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-56 | Menu Language liệt kê 27 lựa chọn ngôn ngữ | Đổi ngôn ngữ nhanh từ menu tài khoản | Trang chứa 27 liên kết `change_language` **khác nhau**: 1 mục `System Default` (`/admin/staff/change_language`) + 26 mục ngôn ngữ (`/admin/staff/change_language/<tên>`). Số lượng khớp với `#default_language` ở REQ-LOGIN-41 | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-57 | Đổi ngôn ngữ thực hiện bằng điều hướng tới đường dẫn riêng từng ngôn ngữ | Mỗi ngôn ngữ là một URL | Mỗi mục ngôn ngữ là thẻ `a` có `href` dạng `/admin/staff/change_language/<tên ngôn ngữ viết thường>` | 🟢 | — | UI thực tế |
| REQ-LOGIN-58 | Hiệu lực của việc đổi ngôn ngữ | Giao diện chuyển sang ngôn ngữ đã chọn và ghi nhớ cho lần sau | ❔ **Chưa kiểm chứng** — không kích hoạt: đổi ngôn ngữ ghi vào hồ sơ của tài khoản dùng chung và làm đổi toàn bộ chuỗi giao diện (`RISK-LOGIN-05`) | ⚪ | — | Chưa kiểm chứng |

---

## 3. Đặc tả Trường Dữ liệu

### 3.1. Form Đăng nhập — `/admin/authentication` (POST cùng URL)

| Field (Label) | Tên field | Loại UI | Required | Ràng buộc (đọc từ DOM) | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|
| Email Address | `email` | `input[type=email]` | ❌ **không** có `required` | Không `maxlength`, không `pattern`, không `placeholder`. Có `autofocus` | REQ-LOGIN-02 · 04 · 07 | Thiếu `required` nên trình duyệt không chặn rỗng — máy chủ chặn (REQ-LOGIN-02) |
| Password | `password` | `input[type=password]` | ❌ **không** có `required` | Không `maxlength`, không `minlength` | REQ-LOGIN-03 · 08 | Như trên |
| Remember me | `remember` | `input[type=checkbox]` | ❌ | `value="estimate"` · mặc định không tích | REQ-LOGIN-11 · 12 · 13 | Giá trị `estimate` bất thường — `AMB-LOGIN-03` |
| *(ẩn)* | `csrf_token_name` | `input[type=hidden]` | — | Giá trị sinh theo phiên — **không chép vào tài liệu** | REQ-LOGIN-09 | |
| Login | — | `button[type=submit]` | — | Không `name`, không `id` | REQ-LOGIN-01 | Locator phải dựa vào `type=submit` hoặc nhãn |

### 3.2. Form Quên mật khẩu — `/admin/authentication/forgot_password`

| Field (Label) | Tên field | Loại UI | Required | Ràng buộc | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|
| Email Address | `email` | `input[type=email]` | ❌ không có `required` | Không `maxlength`/`pattern` | REQ-LOGIN-25 | |
| *(ẩn)* | `csrf_token_name` | `input[type=hidden]` | — | Giá trị sinh theo phiên | REQ-LOGIN-25 | |
| Confirm | — | `button[type=submit]` | — | — | REQ-LOGIN-28 | Chưa kích hoạt |

### 3.3. Form Thông tin cá nhân — `#staff_profile_table` → `/admin/staff/edit_profile`

| Field (Label) | Tên field | Loại UI | Required | Ràng buộc (đọc từ DOM) | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|
| `* First Name` | `firstname` | `input[type=text]` | ✅ (kiểm tra ở trình duyệt, **không** bằng `required`) | Không `maxlength`/`pattern` | REQ-LOGIN-36 · 43 | Nhãn có `*`, DOM không có `required` — `AMB-LOGIN-17` |
| `* Last Name` | `lastname` | `input[type=text]` | ✅ (suy từ nhãn) | Không `maxlength`/`pattern` | REQ-LOGIN-37 | Chưa trigger riêng |
| `* Email` | *(không có `name`)* | `input[type=email]` **disabled** | — | `disabled="true"`, không có thuộc tính `name` | REQ-LOGIN-38 · 39 | Không sửa được, không gửi đi |
| Phone | `phonenumber` | `input[type=text]` | ❌ | Không ràng buộc định dạng số | REQ-LOGIN-43 | Kiểu `text`, không phải `tel` |
| Default Language | `default_language` | `select` | ❌ | **27 options**, option đầu `value=""` nhãn `System Default` | REQ-LOGIN-41 | Hiển thị qua thư viện dropdown (có ô tìm kiếm ẩn) |
| Direction | `direction` | `select` | ❌ | **3 options**: `""` (nhãn rỗng), `ltr` (`LTR`), `rtl` (`RTL`) | REQ-LOGIN-42 | Option đầu rỗng cả value lẫn nhãn |
| Facebook | `facebook` | `input[type=text]` | ❌ | Không ràng buộc URL | REQ-LOGIN-43 | |
| LinkedIn | `linkedin` | `input[type=text]` | ❌ | Không ràng buộc URL | REQ-LOGIN-43 | |
| Skype | `skype` | `input[type=text]` | ❌ | Không ràng buộc | REQ-LOGIN-43 | |
| Email Signature | `email_signature` | `textarea` | ❌ | Không `maxlength` | REQ-LOGIN-43 | |
| *(ẩn)* | `csrf_token_name` | `input[type=hidden]` | — | Giá trị sinh theo phiên | — | |
| Save | — | `button[type=submit]` | — | — | REQ-LOGIN-35 · 44 | Trang có 3 nút cùng nhãn `Save` → locator **phải** giới hạn trong form |

### 3.4. Form Đổi mật khẩu — `#staff_password_change_form` → `/admin/staff/change_password_profile`

| Field (Label) | Tên field | Loại UI | Required | Ràng buộc | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|
| `* Old password` | `oldpassword` | `input[type=password]` | ✅ | Không `minlength`/`pattern` | REQ-LOGIN-45 · 49 | |
| `* New password` | `newpassword` | `input[type=password]` | ✅ | Không `minlength`/`pattern` | REQ-LOGIN-46 · 49 | Không có chỉ báo độ mạnh |
| `Repeat new password` | `newpasswordr` | `input[type=password]` | ❌ **không bắt buộc** | Khi có nhập phải khớp `newpassword` | REQ-LOGIN-47 · 48 | Nhãn không có `*` — `AMB-LOGIN-05` |
| *(ẩn)* | `csrf_token_name` | `input[type=hidden]` | — | Giá trị sinh theo phiên | — | |
| Save | — | `button[type=submit]` | — | — | REQ-LOGIN-51 | |

### 3.5. Form Xác thực hai lớp — `#two_factor_auth_form` → `/admin/staff/update_two_factor`

| Field (Label) | Tên field | Loại UI | Required | Giá trị | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|
| `Disabled` | `two_factor_auth` | `input[type=radio]` id `two_factor_auth_disabled` | — | `off` | REQ-LOGIN-52 · 53 | Đang được chọn |
| `Enable Email Two Factor Authentication` | `two_factor_auth` | `input[type=radio]` id `two_factor_auth_enabled` | — | `email` | REQ-LOGIN-52 · 54 | Có biểu tượng trợ giúp `?` cạnh nhãn |
| `Enable Google Authenticator` | `two_factor_auth` | `input[type=radio]` id `google_two_factor_auth_enabled` | — | `google` | REQ-LOGIN-52 · 54 | |
| Save | — | `button[type=submit]` | — | — | REQ-LOGIN-54 | |

---

## 4. Business Rules & Validation Messages

Tất cả thông báo dưới đây ghi **nguyên văn** từ giao diện thực tế.

### 4.1. Thông báo do ứng dụng sinh — **được phép dùng làm assertion**

| REQ ID | Rule / Trigger | Tầng chặn | Thông báo nguyên văn |
|---|---|---|---|
| REQ-LOGIN-02 | Submit đăng nhập khi Email rỗng | Máy chủ (`200`) | `The Email Address field is required.` |
| REQ-LOGIN-03 | Submit đăng nhập khi Password rỗng | Máy chủ (`200`) | `The Password field is required.` |
| REQ-LOGIN-05 | Submit đăng nhập với email không tồn tại | Máy chủ (`303` → `200`) | `Invalid email or password` |
| REQ-LOGIN-36 | Lưu hồ sơ khi First Name rỗng | Trình duyệt (không gửi request) | `This field is required.` |
| REQ-LOGIN-45 | Lưu đổi mật khẩu khi Old password rỗng | Trình duyệt | `This field is required.` |
| REQ-LOGIN-46 | Lưu đổi mật khẩu khi New password rỗng | Trình duyệt | `This field is required.` |
| REQ-LOGIN-48 | Repeat new password khác New password | Trình duyệt | `Please enter the same value again.` |

> 📌 Thứ tự hiển thị ở màn hình đăng nhập khi bỏ trống cả hai ô: **`The Password field is required.` đứng trước `The Email Address field is required.`** — ngược với thứ tự các ô trên form. Kịch bản kiểm thử đối chiếu theo thứ tự phải bám con số này.

### 4.2. Thông báo do trình duyệt sinh — 🚫 **CẤM dùng làm assertion**

| REQ ID | Trigger | Chuỗi quan sát được | Điều kiện quan sát |
|---|---|---|---|
| REQ-LOGIN-04 | Email không chứa `@` | `Please include an '@' in the email address. '<giá trị đã nhập>' is missing an '@'.` | Google Chrome `153.0.0.0`, ngôn ngữ hệ điều hành tiếng Anh |

Chuỗi này **đổi theo trình duyệt, phiên bản và ngôn ngữ hệ điều hành**. Kịch bản kiểm thử chỉ được assert: `form.checkValidity() === false`, `email.validity.typeMismatch === true`, và **không có request POST nào được gửi**.

---

## 5. Luồng xử lý (User Flows)

### 5.1. Đăng nhập thành công

```
1. Mở /admin/authentication  (chưa có phiên)
2. Nhập Email Address hợp lệ
3. Nhập Password hợp lệ
4. (tuỳ chọn) Tích "Remember me"
5. Bấm Login
   → POST /admin/authentication  →  303
   → GET /admin                  →  200
6. Điểm dừng: URL chứa /admin · document.title = "Dashboard"
```

### 5.2. Đăng nhập sai thông tin

```
1–3. Như trên nhưng email hoặc mật khẩu sai
4. Bấm Login
   → POST /admin/authentication  →  303
   → GET /admin/authentication   →  200
5. Trang đăng nhập hiển thị: "Invalid email or password"
6. Ô Email KHÔNG được máy chủ điền lại · ô Password rỗng
```

### 5.3. Bị chặn khi chưa đăng nhập

```
1. Chưa có phiên, mở thẳng /admin/clients
2. Hệ thống chuyển hướng → /admin/authentication
3. KHÔNG có thông báo lý do
4. Đăng nhập hợp lệ
5. Điểm dừng: /admin  —  KHÔNG quay lại /admin/clients
```

### 5.4. Đăng xuất

```
1. Đang đăng nhập, bấm ảnh đại diện ở thanh đầu trang
2. Menu mở ra 5 mục
3. Bấm "Logout"
   → GET /admin/authentication/logout  →  chuyển hướng
4. Điểm dừng: /admin/authentication
5. Kiểm chứng phiên đã huỷ: mở /admin/ → vẫn bị đưa về /admin/authentication
```

### 5.5. Đổi mật khẩu *(chỉ tới bước kiểm tra giao diện — chưa chạy hết)*

```
1. Mở /admin/staff/edit_profile
2. Khối "Change your password": nhập Old password, New password, Repeat new password
3. Bấm Save của khối này
   → Trình duyệt kiểm tra: Old + New bắt buộc; Repeat nếu có nhập thì phải khớp New
   → Qua được thì POST /admin/staff/change_password_profile
4. ❔ Kết quả sau khi lưu chưa kiểm chứng — xem REQ-LOGIN-51
```

---

## 6. Yêu cầu Phi chức năng quan sát được — Web

| Mã | Hạng mục | Ghi nhận | REQ liên quan |
|---|---|---|---|
| NFR-W-01 | Bảo mật — cookie phiên | Không đọc được bằng JavaScript (`document.cookie` rỗng khi đang đăng nhập) → có cờ `HttpOnly` | REQ-LOGIN-20 |
| NFR-W-02 | Bảo mật — chống dò tài khoản | Thông báo lỗi đăng nhập không phân biệt "email không tồn tại" với "mật khẩu sai" | REQ-LOGIN-06 |
| NFR-W-03 | Bảo mật — chống giả mạo yêu cầu | Cả 4 form khảo sát đều mang trường ẩn `csrf_token_name` | REQ-LOGIN-09 |
| NFR-W-04 | Bảo mật — chống thử tự động | **Không** có CAPTCHA, **không** quan sát được cơ chế giới hạn số lần thử | REQ-LOGIN-10 · `AMB-LOGIN-13` |
| NFR-W-05 | Bộ nhớ đệm | Phản hồi trang đăng nhập có `cache-control: no-store, no-cache, must-revalidate` và `pragma: no-cache` | — |
| NFR-W-06 | Máy chủ | Header `server: LiteSpeed` | — |
| NFR-W-07 | Giao diện đa ngôn ngữ | 26 ngôn ngữ + tuỳ chọn theo mặc định hệ thống; có hỗ trợ chiều RTL | REQ-LOGIN-41 · 42 · 56 |
| NFR-W-08 | Khả năng đáp ứng (responsive) | Tồn tại menu riêng cho màn hình hẹp (`div#mobile-collapse`), bị ẩn ở viewport `1600×750` | REQ-LOGIN-24 |

---

## 7. Danh mục Evidence

Tất cả ảnh đã được **mở lại kiểm tra** sau khi chụp, xác nhận đúng trạng thái đang khai.

| Tệp | Màn hình | Trạng thái | Phạm vi chụp | REQ làm bằng chứng |
|---|---|---|---|---|
| [`login_form_default_fullpage.png`](evidence/login_form_default_fullpage.png) | Đăng nhập | Mặc định, 2 ô rỗng, checkbox chưa tích | Full-page — trang không chứa dữ liệu nghiệp vụ | REQ-LOGIN-11 · 15 |
| [`login_submit_empty_errors_fullpage.png`](evidence/login_submit_empty_errors_fullpage.png) | Đăng nhập | Sau khi submit form rỗng — 2 thông báo lỗi | Full-page | REQ-LOGIN-02 · 03 |
| [`login_invalid_credentials_fullpage.png`](evidence/login_invalid_credentials_fullpage.png) | Đăng nhập | Sau khi submit email không tồn tại — 1 thông báo | Full-page | REQ-LOGIN-05 · 06 · 07 · 08 |
| [`forgot_password_form_default_fullpage.png`](evidence/forgot_password_form_default_fullpage.png) | Quên mật khẩu | Mặc định | Full-page | REQ-LOGIN-25 · 26 |
| [`dashboard_user_menu_open_element.png`](evidence/dashboard_user_menu_open_element.png) | Bảng điều khiển | Menu tài khoản **đang mở**, 5 mục | Element — tránh kéo theo dữ liệu nghiệp vụ của Dashboard | REQ-LOGIN-55 |
| [`edit_profile_default_fullpage.png`](evidence/edit_profile_default_fullpage.png) | Sửa hồ sơ | Mặc định, 3 khối, 3 ô mật khẩu rỗng | Full-page — cần thấy trọn 3 form | REQ-LOGIN-35 · 38 · 50 |
| [`edit_profile_required_error_element.png`](evidence/edit_profile_required_error_element.png) | Sửa hồ sơ | First Name rỗng — viền đỏ + thông báo bắt buộc | Element (form hồ sơ) | REQ-LOGIN-36 · 40 |
| [`change_password_validation_errors_element.png`](evidence/change_password_validation_errors_element.png) | Sửa hồ sơ | Old password rỗng + 2 mật khẩu mới không khớp | Element (khối đổi mật khẩu) — mật khẩu hiển thị dạng `●` | REQ-LOGIN-45 · 47 · 48 · 50 |
| [`two_factor_auth_options_element.png`](evidence/two_factor_auth_options_element.png) | Sửa hồ sơ | 3 lựa chọn 2FA, `Disabled` đang chọn | Element | REQ-LOGIN-52 · 53 |
| [`profile_logged_time_stats_element.png`](evidence/profile_logged_time_stats_element.png) | Hồ sơ cá nhân | 5 ô thống kê giờ | Element — **cố ý không chụp full-page** để không kéo theo bảng dự án chứa dữ liệu nghiệp vụ | REQ-LOGIN-29 · 30 |

### REQ được chống lưng bằng đọc DOM (không cần ảnh)

Các REQ sau có số liệu DOM ghi thẳng trong Acceptance Criteria: REQ-LOGIN-01 · 04 · 09 · 10 · 12 · 14 · 16 · 17 · 18 · 19 · 20 · 21 · 22 · 23 · 24 · 27 · 31 · 32 · 33 · 34 · 39 · 41 · 42 · 43 · 49 · 56 · 57.

REQ trạng thái ⚪ (13 · 28 · 44 · 51 · 54 · 58) **không có** bằng chứng và được khai đúng như vậy.
