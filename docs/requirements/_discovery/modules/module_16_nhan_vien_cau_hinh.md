# Module 16 — Nhân viên · Cấu hình hệ thống

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `STAFF` · `SETTING` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

> ⛔ **BLOCKED cả hai module** — tài khoản Staff bị chặn (`AMB-SYS-01`). Mọi route dưới đây: **tồn tại** (khác 404) nhưng chuyển về `/admin/access_denied` · toast **"Access denied"** · thân trang **"Something went wrong. Try again"** (`AMB-SYS-04`).
> Nguồn: **User xác nhận danh sách module ở checkpoint · route kiểm bằng URL trực tiếp** — nội dung màn hình **chưa xác minh UI**.

---

## `STAFF` — Nhân viên & Phân quyền

| Route đã thử | Kết quả |
|---|---|
| `/admin/staff` | Access denied |
| `/admin/roles` | Access denied |
| `/admin/departments` | Access denied |

| Mục | Ghi nhận |
|---|---|
| Bằng chứng tồn tại | Route trả Access denied (không phải 404) · Tasks Overview liệt kê nhân viên: "Project Manager", "Admin Anh Tester", "Admin Example" → hệ thống có ≥ 3 staff |
| Ước REQ | ❔ |
| Risk | 🔴 — phân quyền quyết định cột Admin/Staff của ma trận ở **mọi** module |

---

## `SETTING` — Cấu hình hệ thống & danh mục master

| Nhóm | Route đã thử | Kết quả |
|---|---|---|
| Cấu hình chung | `/admin/settings` | Access denied |
| Trường tuỳ biến | `/admin/custom_fields` | Access denied |
| Mẫu email | `/admin/emails` | Access denied |
| GDPR | `/admin/gdpr` | Access denied |
| Nhật ký hoạt động | `/admin/utilities/activity_log` | Access denied |
| Mục tiêu | `/admin/goals` | Access denied |
| Khảo sát | `/admin/surveys` | Access denied |
| Danh mục master | `/admin/clients/groups` · `/admin/leads/sources` · `/admin/leads/statuses` · `/admin/tickets/services` · `/admin/tickets/priorities` · `/admin/contracts/types` · `/admin/expenses/categories` · `/admin/taxes` · `/admin/currencies` · `/admin/paymentmodes` | Access denied (cả 10) |
| Module cài thêm | `/admin/modules` | Chuyển về Dashboard |

| Mục | Ghi nhận |
|---|---|
| Giá trị cấu hình đọc gián tiếp | Từ `app.options` trên mọi trang — xem bảng "Định dạng hệ thống" ở [`../../README.md`](../../README.md) |
| Ước REQ | ❔ |
| Risk | 🟡 — đổi cấu hình trên môi trường dùng chung ảnh hưởng mọi người; khi có tài khoản Admin vẫn phải **chỉ đọc** |

**Quyết định tạm:** Goals · Surveys · Activity Log gom vào `SETTING`. Khi mở được, nếu là entity có vòng đời riêng thì chạy `/discover-system` mode ADD để tách prefix mới — prefix `SETTING` giữ nguyên.

**Cần để mở khoá:** tài khoản Admin thật → bổ sung `.env` → `/discover-system` mode ADD cho `STAFF` · `SETTING`.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [staff_overview_viewport.png](../evidence/staff_overview_viewport.png) | `/admin/staff` | Access denied — toast + "Something went wrong. Try again" | Viewport |
| [setting_overview_viewport.png](../evidence/setting_overview_viewport.png) | `/admin/settings` | Access denied — như trên | Viewport |
