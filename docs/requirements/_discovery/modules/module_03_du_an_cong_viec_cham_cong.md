# Module 03 — Dự án · Công việc · Chấm công

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Dự án | `PRJ` | Web | Projects |
| Công việc | `TASK` | Web | Tasks |
| Chấm công | `TIME` | Web | Timesheets |

> Ba module **chung một file khám phá, KHÔNG chung prefix** — sau này vẫn sinh ra 3 thư mục và 3 tài liệu requirements riêng. Gộp file vì quan hệ chặt: công việc luôn thuộc một dự án, chấm công luôn ghi vào một công việc.
>
> User chốt 20-09-2026: **không** gộp `TIME` vào `PRJ` — `/admin/staff/timesheets` là màn hình riêng có bộ lọc riêng và có báo cáo riêng.

---

## `PRJ` — Dự án

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/projects` | Bảng + bộ lọc trạng thái |
| Thêm/Sửa | `/admin/projects/project[/<id>]` | Form |
| Chi tiết | `/admin/projects/view/<id>` | Trang 18 tab |
| Sơ đồ Gantt toàn cục | `/admin/projects/gantt` | Biểu đồ |

**Cột:** `#` · Project Name · Customer · Tags · Start Date · Deadline · Members · Status

**Thanh công cụ:** New Project · *(nút biểu tượng)* Gantt · Export

### Status flow — 5 trạng thái, kèm số bản ghi đo tại 20-09-2026

| Trạng thái | Số bản ghi |
|---|---|
| Not Started | 82 |
| In Progress | 70 |
| On Hold | 17 |
| Cancelled | 2 |
| Finished | 1 |

→ Tổng **172 dự án**. Bộ lọc trạng thái nằm ngay trên bảng, là thanh điều hướng chính của màn hình.

### Chi tiết dự án — 18 tab

Overview · Tasks · Timesheets · Milestones · Files · Discussions · Gantt · Tickets · Contracts · Sales · Proposals · Estimates · Invoices · Subscriptions · Expenses · Credit Notes · Notes · Activity

| Nhóm | Tab |
|---|---|
| **Thuộc `PRJ`** | Overview · Milestones · Files · Discussions · Gantt · Notes · Activity |
| **Thuộc module khác** | Tasks · Timesheets · Tickets · Contracts · Sales · Proposals · Estimates · Invoices · Subscriptions · Expenses · Credit Notes |

**Risk 🔴 Cao** — 172 bản ghi, 18 tab, 5 trạng thái, là trục nối sang gần như mọi module nghiệp vụ.

---

## `TASK` — Công việc

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/tasks` | Bảng |
| Bảng Kanban | `/admin/tasks/switch_kanban/` | Kanban |
| Tổng quan công việc | `/admin/tasks/detailed_overview` | Báo cáo |
| Chi tiết | `/admin/tasks/view/<id>` | Trang |
| Thêm mới | Hộp thoại — nút `New Task` có `href="#"` | Modal |

**Cột:** *(checkbox)* · `#` · Name · Status · Start Date · Due Date · Assigned to · Tags · Priority

**Thanh công cụ:** New Task · *(biểu tượng)* Kanban · Tasks Overview · Export · Bulk Actions

**Quy mô:** 259 công việc (đọc từ thẻ Dashboard `Tasks … 258 / 259`).

**Hai trục phân loại:** `Status` và `Priority` — giá trị đầy đủ ❔ chưa đọc, phải lấy ở tầng recon module.

**Risk 🔴 Cao** — hai trục trạng thái, có Kanban (kéo thả đổi trạng thái), có Bulk Actions, khối lượng lớn.

> ⚠️ Nút `New Task` mở **hộp thoại**, không điều hướng trang. Automation phải chờ modal render, không chờ điều hướng.

---

## `TIME` — Chấm công

| Màn hình | Route | Loại |
|---|---|---|
| Chấm công của tôi | `/admin/staff/timesheets` | Bảng + bộ lọc |
| Chấm công toàn bộ | `/admin/staff/timesheets?view=all` | Bảng (vào từ menu Reports) |

**Cột:** Task · Timesheet Tags · Start Time · End Time · Note · Related · Time (h) · Time (decimal)

**Bộ lọc:** View all timesheets · Today · Customer · Project · Apply · Export

**Ô tổng:** Total Logged Time · Last Month · This Month · Last Week · This Week — 5 ô, hiển thị dạng `HH:MM`.

> Bộ đếm giờ còn có ở thanh đầu trang (`a.top-timers`) — bấm là chạy/dừng đồng hồ. **Chưa bấm thử** vì sẽ ghi bản ghi chấm công thật vào môi trường dùng chung.

**Risk 🟡 Trung bình** — chỉ đọc + xuất tệp ở màn hình này, nhưng số liệu giờ công đi thẳng vào báo cáo và hoá đơn.

---

## Vùng chưa xác minh — cả 3 module

| Việc | Lý do |
|---|---|
| Danh sách trạng thái/độ ưu tiên đầy đủ của `TASK` | Chưa mở dropdown lọc |
| Nội dung 17/18 tab chi tiết dự án | Tầng khám phá chỉ đếm tab |
| Hộp thoại thêm công việc | Chưa mở — tránh tạo dữ liệu |
| Kéo thả Kanban | Thao tác kéo thả **ghi** dữ liệu ngay, không thử trên môi trường dùng chung |
| Bộ đếm giờ ở thanh đầu trang | Bấm là tạo bản ghi chấm công thật |
| `/admin/projects/gantt` · `/admin/tasks/detailed_overview` | Chưa mở |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`projects_list_viewport.png`](../evidence/projects_list_viewport.png) | Danh sách dự án | Mặc định — thấy đủ 5 ô lọc trạng thái kèm số đếm | Viewport |
| [`tasks_list_viewport.png`](../evidence/tasks_list_viewport.png) | Danh sách công việc | Mặc định — đủ thanh công cụ + hàng tiêu đề | Viewport |
| [`timesheets_overview_viewport.png`](../evidence/timesheets_overview_viewport.png) | Chấm công của tôi | Mặc định, bộ lọc `Today` | Viewport |

> Cả ba chụp viewport: ảnh full-page sẽ kéo theo tên dự án, tên khách hàng và nội dung công việc của dữ liệu nghiệp vụ — không liên quan tới việc chứng minh module tồn tại.
