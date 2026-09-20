# Module 11 — Tiện ích · Việc cần làm

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Tiện ích | `UTIL` | Web | Utilities · Media · Calendar |
| Việc cần làm | `TODO` | Web | To Do · Reminders |

> **Reminders gộp vào `TODO`** (user chốt 20-09-2026): cả hai là việc cá nhân của người đang đăng nhập, mỗi cái một màn hình nhỏ — tách thành 2 prefix là thừa.
>
> `UTIL` gom 4 màn hình công cụ rời rạc theo đúng quy tắc *"nhóm màn hình cấu hình rời rạc → gom 1 module"*, tránh đẻ 4 prefix cho 4 trang.

---

## `UTIL` — Tiện ích

| Màn hình | Route | Loại | Trạng thái |
|---|---|---|---|
| Quản lý tệp | `/admin/utilities/media` | Trình quản lý tệp (elFinder) | ✅ mở được |
| Xuất PDF hàng loạt | `/admin/utilities/bulk_pdf_exporter` | Form | ✅ mở được |
| Lịch | `/admin/utilities/calendar` | Lịch | ✅ mở được |
| Nhật ký hoạt động | `/admin/utilities/activity_log` | Bảng | 🚫 **`access_denied`** |

### Quản lý tệp

Trình quản lý elFinder nhúng trong trang, 2 thư mục gốc: `admin-example` · `public`. Nội dung: **1 tệp, 27 KB** (`image.jpg`). Thanh công cụ đầy đủ (tạo thư mục, tải lên, sao chép, xoá, đổi tên, xem trước…).

> ⚠️ Trang này sinh **lỗi ở bảng điều khiển trình duyệt** khi tải. Chưa truy nguyên — ghi nhận để tầng recon module soi.

### Xuất PDF hàng loạt

Form 4 trường: `export_type` (select) · `date-from` · `date-to` · `tag`

`export_type` có **6 lựa chọn** đọc nguyên văn: Invoices · Estimates · Payments · Credit Notes · Proposals · Expenses

### Lịch

Lịch sự kiện. Thêm sự kiện qua `/admin/utilities/calendar?new_event=true&date=<DD-MM-YYYY>` (liên kết `Event` ở menu tạo nhanh).

> Tham số `date` dùng định dạng **`DD-MM-YYYY`** — ghi nhận cho tầng automation.

**Risk 🟢 Thấp** — công cụ phụ trợ. Ngoại lệ: quản lý tệp cho **tải tệp lên và xoá tệp** → cần soi phần chặn loại tệp.

---

## `TODO` — Việc cần làm

| Màn hình | Route | Loại |
|---|---|---|
| Việc cần làm của tôi | `/admin/todo` | Danh sách + Kanban |
| Nhắc nhở | `/admin/misc/reminders` | Bảng |

**Chỉ báo:** biểu tượng ✓ ở thanh đầu trang mang huy hiệu **`2`** → 2 việc đang mở.

Dashboard có khối `My To Do Items` kèm hai liên kết `View All` · `New To Do`, và khối `Reminders` kèm `View All` trỏ `/admin/misc/reminders`.

**Risk 🟢 Thấp** — dữ liệu cá nhân, không liên quan nghiệp vụ tiền.

---

## Vùng chưa xác minh

| Việc | Lý do |
|---|---|
| **Nhật ký hoạt động** | 🚫 `access_denied` — đây cũng là câu trả lời cho dòng *"QA xem được nhật ký hoạt động không"* trong bảng Năng lực kiểm thử |
| Lỗi bảng điều khiển ở trang quản lý tệp | Chưa truy nguyên |
| Tải tệp lên · xoá tệp | Thao tác **ghi** trên môi trường dùng chung |
| Chạy thử xuất PDF hàng loạt | Sinh tệp; chưa thử |
| Thêm/sửa sự kiện lịch | Thao tác ghi |
| Cấu trúc màn hình nhắc nhở | Mới mở, chưa đọc chi tiết bảng |
| Kanban của việc cần làm | Chưa mở |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`utilities_media_viewport.png`](../evidence/utilities_media_viewport.png) | Quản lý tệp | Đã tải xong — thấy 2 thư mục gốc + 1 tệp + thanh công cụ | Viewport — **đã mở lại ảnh xác nhận** trình quản lý render xong, không bắt nhầm lúc đang tải |
| [`utilities_bulk_pdf_export_viewport.png`](../evidence/utilities_bulk_pdf_export_viewport.png) | Xuất PDF hàng loạt | Mặc định — dropdown đóng | Viewport |
| [`utilities_calendar_viewport.png`](../evidence/utilities_calendar_viewport.png) | Lịch | Mặc định | Viewport |
| [`todo_list_viewport.png`](../evidence/todo_list_viewport.png) | Việc cần làm | Mặc định | Viewport |
| [`access_denied_setup_area_viewport.png`](../evidence/access_denied_setup_area_viewport.png) | Trang từ chối truy cập | Sau khi mở `/admin/utilities/activity_log` | Viewport — dùng chung cho mọi route bị chặn |
