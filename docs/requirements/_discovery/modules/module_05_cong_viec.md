# Module 05 — Công việc

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `TASK` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `TASK` — Tasks (gồm Timesheets)

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/tasks` (cũng `/admin/tasks/list_tasks`) · Chi tiết `/admin/tasks/view/{id}` · Tasks Overview `/admin/tasks/detailed_overview` · Timesheets của tôi `/admin/staff/timesheets` · Timesheets toàn bộ `/admin/staff/timesheets?view=all` (menu Reports) |
| Loại màn hình | Danh sách + Kanban (nút icon ⠿) + Modal tạo/sửa (Quick Create Task `href="#"`) |
| Thanh công cụ | `New Task` · nút Kanban · `Tasks Overview` · bộ lọc · `Export` · `Bulk Actions` · page size 25 · Search · reload |
| Summary (status flow) | **Not Started · In Progress · Testing · Awaiting Feedback · Complete** — 5 trạng thái, mỗi ô có dòng "Tasks assigned to me: N" |
| Cột bảng | `#` · `Name` · `Status` · `Start Date` · `Due Date` · `Assigned to` · `Tags` · `Priority` |
| Hành động trên dòng | `Start Timer` · `Edit` · `Delete` — Delete là link **GET** `/admin/tasks/delete_task/{id}` (`RISK-SYS-03`) |
| Đặc điểm | Có **task định kỳ** (nhãn "Recurring Task") · task gắn vào Project / Customer / Contract / Invoice / Estimate / Proposal · `has_permission_create_task = 1` · `has_permission_tasks_checklist_items_delete = 1` (đọc từ `app.options`) |
| Số bản ghi Staff thấy | 272 (03-10-2026) — Summary "assigned to me" khớp tổng → nghi Staff chỉ thấy task được giao cho mình |
| Tasks Overview | Lọc theo nhân viên (Project Manager · Admin Anh Tester · Admin Example · All Staff Members) và theo tháng |
| Timesheets | Cột: (`Staff Member` ở chế độ all) · `Task` · `Timesheet Tags` · `Start Time` · `End Time` · `Note` · `Related` · `Time (h)` · `Time (decimal)` · lọc `Today` / Customer / Project · `Apply` · `Export` |
| Ước REQ | ~55 |
| Risk | 🔴 — status flow 5 trạng thái · timer / timesheet tính giờ · task định kỳ · gắn vào 6 loại entity · link xoá dạng GET |

**Network:** `POST /admin/tasks/table` → 200 (DataTables).

**Vùng chưa xác minh:** nội dung modal tạo task (chưa mở để tránh tạo nhầm) · checklist · bình luận · file đính kèm · Kanban kéo thả đổi trạng thái.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [task_overview_viewport.png](../evidence/task_overview_viewport.png) | Tasks — danh sách + Summary | Mặc định · thân bảng **đã làm mờ** | Viewport |
