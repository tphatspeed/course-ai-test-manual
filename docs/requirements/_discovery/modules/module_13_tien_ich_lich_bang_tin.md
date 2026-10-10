# Module 13 — Tiện ích · Lịch · Bảng tin

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `UTIL` · `CAL` · `NEWS` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.
> Gộp vì cả 3 đều 🟢, quy mô nhỏ, là công cụ hỗ trợ dùng chung.

---

## `UTIL` — Utilities (Media · Bulk PDF Export)

| Mục | Ghi nhận |
|---|---|
| Route | Media `/admin/utilities/media` · Bulk PDF Export `/admin/utilities/bulk_pdf_exporter` |
| Media | Trình quản lý file (elFinder): thanh công cụ upload, tạo thư mục, tải xuống, xem, sao chép, dán, xoá, đổi tên, nén/giải nén, tìm kiếm · 2 vùng: thư mục **riêng của người dùng** + thư mục `public` |
| Bulk PDF Export | Chọn loại chứng từ (select "Nothing selected") · 4 field · nút `Export` |
| Ước REQ | ~15 |
| Risk | 🟢 — nhưng **Media có thao tác xoá file** và thư mục `public` dùng chung → khảo sát/TC không được xoá file không do mình tạo |

**Vùng chưa xác minh:** quyền ghi vào `public` · giới hạn dung lượng (cấu hình PHP `max_php_ini_upload_size_bytes = 134217728` = 128 MB) · định dạng cho phép · Bulk PDF Export có những loại chứng từ nào.

---

## `CAL` — Calendar

| Mục | Ghi nhận |
|---|---|
| Route | `/admin/utilities/calendar` · tạo sự kiện `?new_event=true&date=<d-m-Y>` (từ Quick Create) |
| Loại màn hình | Lịch: `<` `>` · `Today` · `Month` / `Week` / `Day` · `Filter By` |
| Nội dung | Sự kiện riêng + hạn chót của Task / Project (thấy trên lịch ở Dashboard) · `calendar_events_limit = 4` sự kiện/ô rồi hiện "+N more" · tuần bắt đầu Chủ nhật (`calendar_first_day = 0`) · mặc định chế độ tháng |
| Network | `GET /admin/utilities/get_calendar_data?…` |
| Ước REQ | ~15 |
| Risk | 🟢 |

**Vùng chưa xác minh:** form sự kiện · sự kiện công khai/riêng · nhắc nhở.

---

## `NEWS` — Bảng tin & Thông báo (Newsfeed + Announcements)

| Mục | Ghi nhận |
|---|---|
| Route | Announcements `/admin/announcements` — **không có trên menu** của Staff, vào được bằng URL (`AMB-SYS-05`) · Newsfeed mở bằng nút `Share documents, ideas..` ở header (modal) · tab `Announcements` trên Dashboard |
| Announcements | Danh sách cột `Name · Date` · `Export` · **không có nút tạo** với Staff · bảng rỗng |
| Newsfeed | Đăng bài, chia sẻ tài liệu — `newsfeed_maximum_files_upload = 10` |
| Ước REQ | ~12 |
| Risk | 🟢 |

**Vùng chưa xác minh:** nội dung modal Newsfeed (chưa mở để tránh đăng nhầm) · ai được tạo Announcement (nghi chỉ Admin).

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [util_overview_viewport.png](../evidence/util_overview_viewport.png) | Media (elFinder) | Mặc định · menu Utilities mở · vùng file **đã làm mờ** | Viewport |
| [cal_overview_viewport.png](../evidence/cal_overview_viewport.png) | Calendar | Chế độ tháng · sự kiện **đã làm mờ** | Viewport |
| [news_overview_viewport.png](../evidence/news_overview_viewport.png) | Announcements | Bảng rỗng · sidebar **không** có mục Announcements | Viewport |
