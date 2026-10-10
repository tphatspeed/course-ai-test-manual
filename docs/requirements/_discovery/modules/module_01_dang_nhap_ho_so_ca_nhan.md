# Module 01 — Đăng nhập · Hồ sơ cá nhân

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `LOGIN` · `PROFILE` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `LOGIN` — Đăng nhập

| Mục | Ghi nhận |
|---|---|
| Route | Admin: `/admin/authentication` (vào `/admin/` khi chưa đăng nhập cũng bị chuyển về đây) · Quên mật khẩu admin: `/admin/authentication/forgot_password` · Cổng khách hàng: `/authentication/login` · Quên mật khẩu khách hàng: `/authentication/forgot_password` · Đăng xuất: `/admin/authentication/logout` |
| Loại màn hình | Form |
| Thành phần — admin | Heading "Login" · `Email Address` · `Password` · checkbox `Remember me` · nút `Login` · link `Forgot Password?` · logo trỏ về trang chủ |
| Thành phần — cổng khách hàng | Heading "Please login" · select `Language` (mặc định English) · `Email Address` · `Password` · `Remember me` · `Login` · `Forgot Password?` · header có link `Knowledge Base` + nút `Login` |
| Đăng ký khách hàng | `/authentication/register` → chuyển về `/authentication/login` → **đang tắt**. Trang login không có link Register |
| Status flow | Không |
| Ước REQ | ~20 (2 form đăng nhập · 2 form quên mật khẩu · Remember me · đăng xuất · khoá/giới hạn sai mật khẩu nếu có) |
| Risk | 🔴 — cửa vào của mọi module; một tài khoản dùng chung cho nhiều người nên khoá do sai mật khẩu sẽ chặn cả lớp (`RISK-SYS-02`) |

**Ghi nhận phụ:** console trình duyệt báo `Input elements should have autocomplete attributes (suggested: "current-password")` ở trang login admin → ô mật khẩu thiếu `autocomplete`.

**Vùng chưa xác minh:** validation message khi bỏ trống / sai định dạng / sai mật khẩu · khoá tài khoản sau N lần sai · CAPTCHA (chưa thấy) · luồng email đặt lại mật khẩu (không kiểm được tầng tích hợp) · đăng nhập cổng khách hàng (chưa có tài khoản contact — `AMB-SYS-03`).

---

## `PROFILE` — Hồ sơ cá nhân

| Mục | Ghi nhận |
|---|---|
| Route | `/admin/profile` (hồ sơ: bảng Projects của tôi + Notifications) · `/admin/staff/edit_profile` · `/admin/todo` · `/admin/misc/reminders` · `/admin/profile?notifications=true` · `/admin/staff/change_language/<ngôn ngữ>` |
| Loại màn hình | Form + Danh sách |
| Edit Profile | Ảnh đại diện (có nút xoá ×) · `First Name`* · `Last Name`* · `Email`* · `Phone` · `Default Language` · `Direction` (LTR) · … — 16 field hiển thị · nút `Save` |
| Đổi mật khẩu | `Old password`* · `New password`* · `Repeat new password` · dòng "Password last changed: …" · `Save` |
| Xác thực 2 lớp | Radio `Disabled` (đang chọn) · `Enable Email Two Factor Authentication` · `Enable Google Authenticator` · `Save` |
| To-do | `/admin/todo`: "Unfinished to do's" · "Latest finished to do's" · `New To Do` · `Load More` · checkbox đánh dấu xong |
| Reminders | `/admin/misc/reminders` — "Showing your reminders and reminders created by you." · cột `Related to · Description · Date · Remind · Is notified?` · `Export` |
| Notifications | Chuông header · `Mark all as read` · `View all notifications` · `Load More` |
| Ngôn ngữ | System Default + 26 ngôn ngữ (English, Vietnamese, Chinese…) — bấm là **đổi ngay** cài đặt tài khoản |
| Ước REQ | ~30 |
| Risk | 🟡 — đổi mật khẩu / 2FA / ngôn ngữ trên **tài khoản dùng chung** ảnh hưởng mọi người (`RISK-SYS-02`) → TC phải chạy trên tài khoản riêng |

**Vùng chưa xác minh:** quy tắc độ mạnh mật khẩu · Email có cho sửa không (ảnh cho thấy ô Email nền xám, **chưa đọc DOM** để biết `disabled` hay `readonly`) · luồng 2FA (cần tài khoản riêng) · To-do có sửa/xoá được không.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [login_overview_fullpage.png](../evidence/login_overview_fullpage.png) | Login admin | Mặc định, chưa nhập | Full-page — trang ngắn, không có dữ liệu nghiệp vụ |
| [portal_login_viewport.png](../evidence/portal_login_viewport.png) | Login cổng khách hàng | Mặc định | Viewport |
| [profile_overview_viewport.png](../evidence/profile_overview_viewport.png) | Edit Profile | Mặc định — giá trị First/Last Name, Email, Phone **đã làm mờ** | Viewport |
