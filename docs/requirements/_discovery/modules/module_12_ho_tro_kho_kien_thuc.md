# Module 12 — Hỗ trợ · Kho kiến thức

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `TICKET` · `KBASE` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.
> Gộp vì cùng là nhóm chăm sóc khách hàng; bài Knowledge Base là nơi khách tự tra trước khi gửi ticket.

---

## `TICKET` — Support (Tickets)

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/tickets` · Tạo mới `/admin/tickets/add` |
| Loại màn hình | Danh sách + Form |
| Thanh công cụ | `New Ticket` · nút biểu đồ · bộ lọc · `Export` · `Bulk Actions` |
| Summary (status flow) | **Open · In Progress · Answered · On Hold · Closed** |
| Cột bảng | `#` · `Subject` · `Tags` · `Department` · `Service` · `Contact` · `Status` · `Priority` · `Last Reply` · `Created` |
| Form tạo mới | 13 field hiển thị · có lựa chọn **"Ticket without contact"** |
| Số bản ghi Staff thấy | 0 (`AMB-SYS-02`) |
| Ước REQ | ~35 |
| Risk | 🟡 — status flow 5 trạng thái · phân theo Department / Service / Priority · khách hàng mở ticket qua cổng |

**Ngoài quyền Staff:** Ticket Services · Ticket Priorities · Departments (Access denied). `/admin/utilities/ticket_pipe_log` → 404.

**Vùng chưa xác minh:** trả lời ticket · gán người xử lý · ticket qua email (không kiểm được tầng tích hợp) · phía khách hàng (`AMB-SYS-03`).

---

## `KBASE` — Knowledge Base

| Mục | Ghi nhận |
|---|---|
| Route | Quản trị `/admin/knowledge_base` · Tạo bài `/admin/knowledge_base/article` · Nhóm `/admin/knowledge_base/manage_groups` · Công khai `/knowledge-base` (cổng khách hàng, không cần đăng nhập) |
| Thanh công cụ | `New Article` · `Groups` · nút đổi chế độ xem · bộ lọc · `Export` |
| Cột bảng | `Article Name` · `Group` · `Date Published` |
| Form tạo bài | 4 field hiển thị · 2 dấu bắt buộc |
| Groups | Cột `Name · Active · Options` · `New Group` · nút `Articles` · có **2 nhóm** |
| Số bản ghi Staff thấy | 0 bài viết · trang công khai cũng chưa có bài nào |
| Báo cáo liên quan | Reports › KB Articles (`RPT`) |
| Ước REQ | ~15 |
| Risk | 🟢 — nội dung tĩnh; điểm cần chú ý là bài viết **công khai** ra ngoài cổng khách hàng |

**Vùng chưa xác minh:** bài nội bộ hay công khai · phản hồi "bài viết có hữu ích không" · thứ tự sắp xếp.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [ticket_overview_viewport.png](../evidence/ticket_overview_viewport.png) | Support — danh sách + Summary | Bảng rỗng | Viewport |
| [kbase_overview_viewport.png](../evidence/kbase_overview_viewport.png) | Knowledge Base — danh sách bài | Bảng rỗng | Viewport |
