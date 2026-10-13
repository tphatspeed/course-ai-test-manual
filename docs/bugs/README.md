# Danh mục Bug Report — Perfex CRM (Anh Tester Demo)

| Mục | Giá trị |
|---|---|
| Hệ thống | Perfex CRM — bản demo của Anh Tester · `https://crm.anhtester.com` |
| Quy ước mã bug | `BUG_<module>_<timestamp>_<TC_ID>` |
| Đường dẫn file | `docs/bugs/<module>/<nền-tảng>/BUG_<module>_<timestamp>_<TC_ID>.md` |
| Ngày cập nhật | 13-10-2026 |

## 1. Danh mục bug

| Mã bug | Module | Nền tảng | Tiêu đề ngắn | Severity | Priority | Trạng thái | TC liên quan | Ngày phát hiện |
|---|---|---|---|---|---|---|---|---|
| [BUG_login_1791897544_TC070](login/web/BUG_login_1791897544_TC070.md) | `LOGIN` | `web` | Không cấp mã phiên mới sau khi đăng nhập (session fixation) | 🔴 Critical | P1 | 🔴 **Đang mở** | `CRM_LOGIN_TC_070` | 10-10-2026 |
| [BUG_login_1791897545_TC042](login/web/BUG_login_1791897545_TC042.md) | `LOGIN` | `web` | Quên mật khẩu báo "Email not found" — lộ email có tài khoản (quản trị + cổng) | 🟠 Major | P2 | 🔴 **Đang mở** | `CRM_LOGIN_TC_042` · `CRM_LOGIN_TC_058-a` · `CRM_LOGIN_TC_058-b` | 10-10-2026 |
| [BUG_login_1791897547_TC071](login/web/BUG_login_1791897547_TC071.md) | `LOGIN` | `web` | Cookie `autologin` thiếu HttpOnly, script đọc được | 🟠 Major | P2 | 🔴 **Đang mở** | `CRM_LOGIN_TC_071` | 10-10-2026 |
| [BUG_login_1791897546_TC040](login/web/BUG_login_1791897546_TC040.md) | `LOGIN` | `web` | Quên mật khẩu bỏ trống Email báo "Email not found" thay vì bắt buộc nhập (quản trị + cổng) | 🟡 Minor | P2 | 🔴 **Đang mở** | `CRM_LOGIN_TC_040` · `CRM_LOGIN_TC_056` | 10-10-2026 |
| [BUG_login_1791897548_TC045](login/web/BUG_login_1791897548_TC045.md) | `LOGIN` | `web` | Liên kết đặt lại mật khẩu thiếu tham số trả HTTP 500 thay vì 404 | 🟡 Minor | P3 | 🔴 **Đang mở** | `CRM_LOGIN_TC_045` | 10-10-2026 |
| [BUG_login_1791897549_TC046](login/web/BUG_login_1791897549_TC046.md) | `LOGIN` | `web` | Tiêu đề tab Quên mật khẩu quản trị là "… - Login" | 🟢 Trivial | P3 | 🔴 **Đang mở** | `CRM_LOGIN_TC_046` | 10-10-2026 |
| [BUG_login_1791897550_TC076](login/web/BUG_login_1791897550_TC076.md) | `LOGIN` | `web` | Ô Email / Password Login quản trị thiếu `autocomplete` | 🟢 Trivial | P3 | 🔴 **Đang mở** | `CRM_LOGIN_TC_076` | 10-10-2026 |
