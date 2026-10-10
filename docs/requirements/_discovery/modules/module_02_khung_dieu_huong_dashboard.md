# Module 02 — Khung điều hướng · Dashboard

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `NAV` · `DASH` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `NAV` — Khung điều hướng chung

| Mục | Ghi nhận |
|---|---|
| Phạm vi | Thành phần có trên **mọi** trang quản trị: sidebar · header · tìm kiếm toàn cục · Quick Create · menu người dùng · timer · chuông thông báo |
| Sidebar | 14 mục cấp 1 · 3 nhóm có menu con: Sales (6) · Utilities (3) · Reports (6). Mục đang mở được tô sáng. Cây đầy đủ: [system_map mục 2.1](../system_map.md#21-khu-vực-quản-trị--sidebar-tài-khoản-staff) |
| Menu Setup | `#setup-menu-wrapper` có trong DOM nhưng **rỗng** với Staff (chỉ có tiêu đề "Setup", không hiển thị) |
| Header | Nút thu/mở sidebar · ô `Search...` (tìm kiếm toàn cục; trang có sẵn danh sách "tìm gần đây" của người dùng) · nút `+` Quick Create · `Share documents, ideas..` (Newsfeed → `NEWS`) · To-do kèm badge số · avatar ▸ menu người dùng · timer · chuông thông báo |
| Quick Create | 13 mục: Invoice · Estimate · Proposal · Credit Note · Customer · Subscription · Project · **Task** (`href="#"` → mở modal) · Expense · Contract · Article · Ticket · Event (`/admin/utilities/calendar?new_event=true&date=<ngày hôm nay d-m-Y>`) |
| Ước REQ | ~20 |
| Risk | 🟡 — dùng ở mọi trang; Quick Create là lối tắt vào 13 form của 11 module khác |

**Vùng chưa xác minh:** kết quả tìm kiếm toàn cục (tìm những entity nào) · hành vi sidebar thu gọn · timer.

---

## `DASH` — Dashboard

| Mục | Ghi nhận |
|---|---|
| Route | `/admin/` |
| Loại màn hình | Dashboard — widget kéo thả (có tay nắm ⠿) · nút `Dashboard Options` |
| Widget quan sát được | Invoice overview · Estimate overview · Proposal overview (đếm theo trạng thái, mỗi dòng là link lọc sang module) · chọn năm + `Outstanding Invoices` / `Past Due Invoices` / `Paid Invoices` · 4 thanh tiến độ: `Invoices Awaiting Payment` · `Converted Leads` · `Projects In Progress` · `Tasks Not Finished` · tab `My Tasks` / `My Projects` / `My Reminders` / `Tickets` / `Announcements` · Calendar · `Payment Records` (Full Report, Weekly) · `Contracts Expiring Soon` · `My To Do Items` · `Leads Overview` · `Statistics by Project Status` · `Latest Project Activity` |
| Link lọc trạng thái (lộ mã trạng thái) | Invoice: `status=6` Draft · `filter=not_sent` · `1` Unpaid · `3` Partially Paid · `4` Overdue · `2` Paid · Estimate: `1` Draft · `not_sent=1` · `2` Sent · `5` Expired · `3` Declined · `4` Accepted · Proposal: `6` Draft · `4` Sent · `1` Open · `5` Revised · `2` Declined · `3` Accepted |
| Hành động đổi trạng thái | `/admin/staff/reset_dashboard` (Dashboard Options) — **không mở khi khảo sát** |
| Ước REQ | ~25 |
| Risk | 🟡 — tổng hợp số liệu từ 8 module; số đếm thay đổi liên tục trên môi trường dùng chung (`RISK-SYS-01`) |

**Network:** `GET /admin/utilities/get_calendar_data?csrf_token_name=<32 ký tự hex>&start=…&end=…` → 200 · `POST /admin/tasks/table` → 200 (bảng My Tasks).

**Ghi nhận rủi ro:** bảng My Tasks hiển thị link `Delete` dạng **GET** `/admin/tasks/delete_task/{id}` (`RISK-SYS-03`).

**Vùng chưa xác minh:** nội dung Dashboard Options (bật/tắt widget) · widget có phụ thuộc quyền không (cần so với Admin).

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [dash_overview_viewport.png](../evidence/dash_overview_viewport.png) | Dashboard | Mặc định | Viewport — chỉ phần đầu (overview + KPI); widget bên dưới không cần để chứng minh module tồn tại |
| [nav_quick_create_open_viewport.png](../evidence/nav_quick_create_open_viewport.png) | Header + sidebar | **Menu Quick Create đang mở** — đủ 13 mục | Viewport |
