# Module 01 — Đăng nhập & Tài khoản cá nhân

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng |
|---|---|---|
| Đăng nhập & Tài khoản cá nhân | `LOGIN` | Web |

> Trạng thái recon tra ở [`docs/requirements/README.md`](../../README.md) — file này **không** nhân bản trạng thái.

## Phạm vi

Cổng vào hệ thống và mọi thứ thuộc về tài khoản của chính người đang đăng nhập.

| Màn hình | Route | Loại | Ghi nhận |
|---|---|---|---|
| Đăng nhập | `/admin/authentication` | Form | 2 ô nhập (Email Address · Password) · checkbox **Remember me** · nút Login · liên kết Forgot Password |
| Quên mật khẩu | `/admin/authentication/forgot_password` | Form | ✅ *(đã khảo sát ở recon module)* 1 ô Email + nút `Confirm`, không có lối quay lại trang đăng nhập |
| Hồ sơ cá nhân | `/admin/profile` | Trang xem | Thẻ thông tin (tên · email · số điện thoại) · 5 ô tổng giờ đã log · bảng Projects của người dùng |
| Sửa hồ sơ | `/admin/staff/edit_profile` | Form | ⚠️ *(sửa 20-09-2026)* **3 form độc lập**, không phải một: `#staff_profile_table` · `#staff_password_change_form` · `#two_factor_auth_form` — 3 action riêng, 3 nút `Save` riêng |
| Đổi ngôn ngữ | `/admin/staff/change_language/<lang>` | Hành động | ⚠️ *(sửa 20-09-2026)* **26 ngôn ngữ** + System Default = **27 lựa chọn** *(bản ghi cũ: 25)* |
| Đăng xuất | `/admin/authentication/logout` | Hành động | Nằm ở menu avatar |

## Đặc tả sơ bộ — Sửa hồ sơ

> 📌 **Đặc tả đầy đủ đã có ở tầng module:** [`../../login/web/requirements_login_web.md`](../../login/web/requirements_login_web.md) mục 3. Phần dưới chỉ giữ mức khám phá, đã sửa các điểm sai phát hiện lúc recon.

Trường đọc từ DOM (`input`/`select`/`textarea`, đã loại `hidden`):

`firstname` · `lastname` · ~~`email` (type=email)~~ → ⚠️ **`email` `disabled` và KHÔNG có thuộc tính `name`** — không sửa được, không gửi lên khi lưu *(sửa 20-09-2026)* · `phonenumber` · `default_language` *(27 lựa chọn)* · `direction` *(3 lựa chọn, lựa chọn đầu rỗng cả giá trị lẫn nhãn)* · `facebook` · `linkedin` · `skype` · `email_signature` (textarea)

Đổi mật khẩu: `oldpassword` · `newpassword` · `newpasswordr` — **3 ô `type=password`**, xác nhận bằng đếm DOM. ⚠️ `newpasswordr` **không bắt buộc**.

**Xác thực 2 lớp:** `two_factor_auth` — **3 radio**, nhãn đã đọc *(sửa 20-09-2026)*: `off` → `Disabled` *(đang chọn)* · `email` → `Enable Email Two Factor Authentication` · `google` → `Enable Google Authenticator`.

**Đăng xuất:** DOM chứa **3** phần tử mang chữ `Logout`; ở viewport `1600×750` chỉ **1** phần tử người dùng thao tác được (menu avatar). Hai phần tử còn lại nằm trong `div#mobile-collapse` (bị ẩn ở viewport desktop) và `div#timers-logout-template-warning.hide` (template cảnh báo bấm giờ).

## CRUD & trạng thái

| | |
|---|---|
| CRUD | Không có (không tạo/xoá tài khoản ở đây — việc đó thuộc `STAFF`, hiện bị chặn quyền) |
| Status flow | Không |
| Số tab | 0 |

## Risk: 🔴 Cao

- Là cổng vào duy nhất — hỏng là chặn toàn bộ đợt kiểm thử
- Chạm trực tiếp vào bí mật: mật khẩu, xác thực 2 lớp, phiên đăng nhập
- **Không kiểm chứng được phần âm tính** trong phiên khảo sát này: khoá tài khoản sau N lần sai, thông báo lỗi sai thông tin, hành vi Remember me. Thử sai mật khẩu trên **môi trường dùng chung** có nguy cơ khoá chính tài khoản đang dùng

## Vùng chưa xác minh

> ✅ **Cập nhật 20-09-2026 sau recon module.** Bốn vùng đã được gỡ; hai vùng còn treo và đã thành ambiguity có mã ở tầng module.

| Việc | Tình trạng |
|---|---|
| Màn hình Quên mật khẩu | ✅ **Đã gỡ** — user cho phép đăng xuất, đã khảo sát giao diện (`REQ-LOGIN-25` → `27`). Luồng *gửi email* vẫn chưa chạy → `AMB-LOGIN-14` |
| Thông báo lỗi khi đăng nhập sai | ✅ **Đã gỡ** — thử bằng email không tồn tại nên không đụng bộ đếm của tài khoản admin. Nguyên văn: `Invalid email or password` (`REQ-LOGIN-05`) |
| 3 lựa chọn của xác thực 2 lớp | ✅ **Đã gỡ** — đọc đủ nhãn và giá trị (`REQ-LOGIN-52`) |
| Luồng đổi mật khẩu | ✅ **Đã gỡ phần kiểm tra giao diện** — 3 thông báo validation nguyên văn (`REQ-LOGIN-45` → `48`). Luồng lưu thành công vẫn không chạy → `REQ-LOGIN-51` ⚪ |
| Quy tắc khoá tài khoản | ⛔ **Còn treo** → `AMB-LOGIN-02` 🔴. Chỉ kiểm chứng được nếu có tài khoản dùng riêng |
| Thời hạn phiên · cờ của cookie phiên | ⛔ **Còn treo** → `AMB-LOGIN-12`. Công cụ khảo sát không trả về header `Set-Cookie`; cần công cụ phát triển của trình duyệt |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`login_overview_fullpage.png`](../evidence/login_overview_fullpage.png) | Đăng nhập | Mặc định, chưa nhập | Full-page — trang không chứa dữ liệu nghiệp vụ nên chụp trọn được |
| [`account_edit_profile_viewport.png`](../evidence/account_edit_profile_viewport.png) | Sửa hồ sơ | Mặc định | Viewport — 3 ô mật khẩu đang rỗng, ảnh không chứa bí mật |
