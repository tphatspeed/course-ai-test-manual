# Software Requirements Specification (SRS)
## Module: Login (Đăng nhập)

**Hệ thống:** Perfex CRM – Anh Tester Demo
**URL kiểm thử:** https://crm.anhtester.com/admin/authentication
**Ngày khảo sát:** 26/08/2026
**Người thực hiện:** Claude (Cowork) – khảo sát trực tiếp trên hệ thống thật

---

## 1. Giới thiệu

### 1.1 Mục đích
Tài liệu này mô tả các yêu cầu chức năng và phi chức năng của module **Login** (Đăng nhập) trong hệ thống Perfex CRM, phục vụ cho việc thiết kế test case và kiểm thử chức năng đăng nhập vào khu vực quản trị (admin panel).

### 1.2 Phạm vi
Module Login cho phép người dùng (Admin/Staff) xác thực bằng **Email** và **Mật khẩu** để truy cập vào hệ thống quản trị Perfex CRM tại đường dẫn `/admin/authentication`. Tài liệu bao gồm:
- Màn hình đăng nhập (Login)
- Chức năng "Ghi nhớ đăng nhập" (Remember me)
- Chức năng "Quên mật khẩu" (Forgot Password)
- Chức năng Đăng xuất (Logout)

Lưu ý: Hệ thống Perfex CRM có 2 cổng đăng nhập riêng biệt:
- **Admin/Staff login**: `https://crm.anhtester.com/admin/authentication`
- **Customer/Client portal login**: `https://crm.anhtester.com/authentication/login`

Tài liệu này tập trung vào **Admin/Staff login**.

### 1.3 Đối tượng sử dụng
- Nhân viên QA thiết kế test case, thực hiện kiểm thử
- Business Analyst / Developer tham chiếu hành vi hệ thống

---

## 2. Mô tả tổng quan

### 2.1 Giao diện màn hình Login
Màn hình đăng nhập gồm các thành phần:

| STT | Thành phần | Loại | Bắt buộc | Ghi chú |
|---|---|---|---|---|
| 1 | Logo hệ thống | Hình ảnh | - | Hiển thị logo "AnhTester Automation Testing" |
| 2 | Tiêu đề "Login" | Text | - | |
| 3 | Email Address | Input (type=email) | Có | Có validate định dạng email (HTML5) |
| 4 | Password | Input (type=password) | Có | Ký tự nhập vào được ẩn |
| 5 | Remember me | Checkbox | Không | Ghi nhớ phiên đăng nhập |
| 6 | Nút Login | Button | - | Submit form đăng nhập |
| 7 | Link "Forgot Password?" | Link | - | Điều hướng tới trang khôi phục mật khẩu |

### 2.2 Luồng nghiệp vụ chính (Main flow)
1. Người dùng truy cập `https://crm.anhtester.com/admin/authentication`
2. Người dùng nhập Email và Password
3. (Tuỳ chọn) Tick chọn "Remember me"
4. Người dùng bấm nút **Login**
5. Hệ thống kiểm tra thông tin đăng nhập:
   - Nếu hợp lệ → chuyển hướng vào trang **Dashboard** (`/admin/` – "Bảng tin")
   - Nếu không hợp lệ → hiển thị thông báo lỗi tương ứng, ở lại trang Login

### 2.3 Luồng "Quên mật khẩu"
1. Người dùng bấm link **Forgot Password?**
2. Hệ thống điều hướng tới `https://crm.anhtester.com/admin/authentication/forgot_password`
3. Người dùng nhập Email đã đăng ký, bấm **Confirm**
4. Hệ thống gửi email đặt lại mật khẩu (trên môi trường demo hiện ghi nhận thông báo lỗi "Error setting new password" do server mail chưa được cấu hình)

### 2.4 Luồng Đăng xuất
1. Sau khi đăng nhập thành công, người dùng bấm vào avatar ở góc trên bên phải
2. Chọn **Đăng xuất** (Logout) trong menu sổ xuống
3. Hệ thống huỷ phiên đăng nhập và điều hướng về lại trang Login

---

## 3. Yêu cầu chức năng chi tiết

### 3.1 UC-01: Đăng nhập thành công
**Điều kiện tiên quyết:** Người dùng có tài khoản hợp lệ trong hệ thống, chưa đăng nhập.

**Input:** Email hợp lệ đã tồn tại + Password đúng

**Kết quả mong đợi:**
- Hệ thống xác thực thành công
- Điều hướng người dùng đến trang Dashboard (`/admin/`)
- Phiên làm việc (session) được khởi tạo
- Hiển thị đầy đủ menu điều hướng (Khách hàng, Các dự án, Công việc, Hợp đồng, Doanh số, Thuê bao, Chi phí, Hỗ trợ, Khách tiềm năng, Yêu cầu báo giá, Kiến thức, Tiện ích, Báo cáo...)

### 3.2 UC-02: Đăng nhập thất bại – sai Email hoặc Password
**Input:** Email hoặc Password không đúng với dữ liệu hệ thống

**Kết quả mong đợi:**
- Hệ thống **không** cho đăng nhập, ở lại trang Login
- Hiển thị thông báo lỗi: **"Invalid email or password"**
- Không tiết lộ cụ thể là sai email hay sai mật khẩu (bảo mật)

### 3.3 UC-03: Bỏ trống trường bắt buộc
**Input:** Bỏ trống cả Email và Password, bấm Login

**Kết quả mong đợi:**
- Hệ thống hiển thị 2 thông báo lỗi riêng biệt:
  - **"The Email Address field is required."**
  - **"The Password field is required."**
- Form không được submit, ở lại trang Login

### 3.4 UC-04: Nhập sai định dạng Email
**Input:** Nhập chuỗi không đúng định dạng email (ví dụ: "notanemail") vào trường Email

**Kết quả mong đợi:**
- Trình duyệt chặn submit và hiển thị cảnh báo validate HTML5 ngay tại trường nhập:
  - **"Please include an '@' in the email address. '...' is missing an '@'."**
- Form chưa được gửi lên server

### 3.5 UC-05: Chức năng "Remember me"
**Input:** Tick chọn checkbox "Remember me" trước khi đăng nhập thành công

**Kết quả mong đợi:**
- Hệ thống lưu lại phiên đăng nhập lâu hơn (cookie ghi nhớ), người dùng không phải đăng nhập lại trong khoảng thời gian quy định dù đóng trình duyệt

### 3.6 UC-06: Quên mật khẩu (Forgot Password)
**Input:** Truy cập link "Forgot Password?" → nhập Email đã đăng ký → bấm Confirm

**Kết quả mong đợi:**
- Điều hướng sang trang `/admin/authentication/forgot_password`
- Sau khi Confirm, hệ thống gửi email chứa link đặt lại mật khẩu đến hộp thư người dùng
- Hiển thị thông báo xác nhận đã gửi (hoặc thông báo lỗi nếu email không tồn tại/hệ thống mail gặp sự cố)

> Ghi chú kiểm thử thực tế: trên môi trường demo, chức năng này trả về thông báo **"Error setting new password"**, nghi ngờ do server gửi mail chưa được cấu hình trên môi trường demo – cần xác nhận lại với môi trường thật.

### 3.7 UC-07: Quên mật khẩu với Email không tồn tại
**Input:** Nhập email không tồn tại trong hệ thống vào form Forgot Password

**Kết quả mong đợi:** Hệ thống nên hiển thị thông báo phù hợp (không tiết lộ email có tồn tại hay không, theo nguyên tắc bảo mật), hoặc thông báo lỗi rõ ràng.

### 3.8 UC-08: Đăng xuất
**Điều kiện tiên quyết:** Đã đăng nhập thành công

**Input:** Bấm avatar góc phải → chọn "Đăng xuất"

**Kết quả mong đợi:**
- Phiên làm việc bị huỷ
- Điều hướng về trang Login
- Người dùng không thể truy cập lại các trang trong `/admin/` bằng cách back trình duyệt (session đã invalid)

### 3.9 UC-09: Truy cập URL admin khi chưa đăng nhập
**Input:** Truy cập trực tiếp `https://crm.anhtester.com/admin/` (hoặc bất kỳ URL admin nào khác) khi chưa đăng nhập

**Kết quả mong đợi:** Hệ thống tự động chuyển hướng về trang Login (`/admin/authentication`)

---

## 4. Yêu cầu phi chức năng

| Hạng mục | Yêu cầu |
|---|---|
| Bảo mật | Mật khẩu phải được ẩn (masked) khi nhập; không hiển thị chi tiết lỗi (sai email/sai password) riêng biệt để tránh dò tài khoản |
| Hiệu năng | Thời gian phản hồi sau khi bấm Login không quá 3 giây trong điều kiện mạng bình thường |
| Khả năng sử dụng | Thông báo lỗi rõ ràng, hiển thị ngay tại vị trí liên quan; hỗ trợ điều hướng bằng phím Tab/Enter |
| Đa ngôn ngữ | Hệ thống hỗ trợ đổi ngôn ngữ hiển thị (quan sát được ở cổng Customer login có dropdown Language; cần kiểm tra thêm ở cổng Admin) |
| Khả năng tương thích | Form Email sử dụng input type="email" nên tận dụng validate mặc định của trình duyệt |
| Giới hạn đăng nhập sai | Cần xác minh thêm: hệ thống có khoá tài khoản/áp dụng CAPTCHA sau nhiều lần đăng nhập sai liên tiếp hay không (chưa quan sát được trong lần khảo sát này) |

---

## 5. Ma trận trường dữ liệu (Field Specification)

| Trường | Kiểu | Bắt buộc | Validate | Thông báo lỗi quan sát được |
|---|---|---|---|---|
| Email Address | email | Có | Định dạng email (HTML5), không rỗng | "The Email Address field is required." / "Please include an '@' in the email address..." |
| Password | password | Có | Không rỗng | "The Password field is required." |
| Remember me | checkbox | Không | - | - |

---

## 6. Các trường hợp cần kiểm thử bổ sung (Out of scope / cần khảo sát thêm)
- Giới hạn số lần đăng nhập sai (rate limiting / khoá tài khoản)
- Độ dài tối đa/tối thiểu của Email và Password
- Kiểm tra XSS/SQL Injection tại các trường input
- Kiểm tra tính năng "Remember me" thực tế qua nhiều phiên trình duyệt
- Kiểm tra gửi email thực tế khi Forgot Password (đã gặp lỗi "Error setting new password" trên môi trường demo)
- Responsive trên thiết bị di động / tablet
- Kiểm thử với các trình duyệt khác nhau (Chrome, Firefox, Safari, Edge)

---

## 7. Phụ lục – Ảnh chụp màn hình tham khảo
- Màn hình Login mặc định
- Màn hình báo lỗi "Invalid email or password"
- Màn hình báo lỗi trường bắt buộc
- Màn hình cảnh báo định dạng Email không hợp lệ (HTML5 validation)
- Màn hình Forgot Password
- Màn hình Dashboard sau khi đăng nhập thành công (Bảng tin)
- Menu Đăng xuất
