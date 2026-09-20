# Module 08 — Hỗ trợ · Cơ sở tri thức

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Hỗ trợ | `TIC` | Web | Support · Tickets |
| Cơ sở tri thức | `KB` | Web | Knowledge Base · Articles |

> Chung file vì cùng phục vụ **kênh chăm sóc khách hàng**: bài viết tri thức là thứ khách tự đọc trước khi mở phiếu hỗ trợ.

---

## `TIC` — Hỗ trợ

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách phiếu | `/admin/tickets` | Bảng |
| Thêm phiếu | `/admin/tickets/add` | Form |
| Chi tiết phiếu | `/admin/tickets/ticket/<id>` | Trang hội thoại |

**Cột:** *(checkbox)* · `#` · Subject · Tags · **Department** · **Service** · **Contact** · **Status** · **Priority** · Last Reply · Created

**Thanh công cụ:** New Ticket · *(nút biểu tượng)* · Export · Bulk Actions

**Quy mô:** 1 bản ghi.

> Ba cột `Department` · `Service` · `Contact` đều trỏ tới master data **nằm ngoài tầm với**: phòng ban (`/admin/departments` 🚫), dịch vụ hỗ trợ (thuộc `SETTING` 🚫), liên hệ (thuộc `CUST`). Trạng thái và độ ưu tiên cũng là master data cấu hình được.
>
> → Recon module này **sẽ bị chặn một phần** cho tới khi có account thấy được khu Setup.

**Risk 🟡 Trung bình** — hai trục trạng thái và luồng hội thoại hai chiều với khách, nhưng không chạm tiền.

---

## `KB` — Cơ sở tri thức

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách bài viết | `/admin/knowledge_base` | Bảng |
| Thêm/Sửa bài viết | `/admin/knowledge_base/article[/<id>]` | Trình soạn thảo |
| Quản lý nhóm | `/admin/knowledge_base/manage_groups` | ✅ **mở được** — route ẩn ngoài menu |

**Cột:** Article Name · Group · Date Published

**Thanh công cụ:** New Article · Groups · *(nút biểu tượng)* · Export

**Quy mô:** **`No entries found`** — 0 bản ghi.

> `/admin/knowledge_base/manage_groups` **không nằm trong menu điều hướng** nhưng truy cập được (đo 20-09-2026). Vào từ nút `Groups` trên thanh công cụ.

**Risk 🟢 Thấp** — nội dung tĩnh, không trạng thái, không tiền. Nhưng bài viết **hiển thị ra ngoài cho khách hàng** → cần kiểm phần phân quyền hiển thị.

---

## Vùng chưa xác minh — cả 2 module

| Việc | Lý do |
|---|---|
| Danh sách trạng thái / độ ưu tiên phiếu hỗ trợ | Chỉ 1 bản ghi, và master data ở `SETTING` 🚫 |
| Danh sách phòng ban · dịch vụ hỗ trợ | 🚫 `access_denied` |
| Toàn bộ nghiệp vụ `KB` | **0 bản ghi** |
| Trình soạn thảo bài viết | Chưa mở |
| Luồng trả lời phiếu · trả lời soạn sẵn | Là thao tác ghi; trả lời soạn sẵn nằm ở `SETTING` 🚫 |
| Hiển thị bài viết ở cổng khách hàng | URL khác, ngoài phạm vi đợt này |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`support_tickets_list_viewport.png`](../evidence/support_tickets_list_viewport.png) | Danh sách phiếu hỗ trợ | Mặc định | Viewport |
| [`knowledge_base_list_empty_viewport.png`](../evidence/knowledge_base_list_empty_viewport.png) | Cơ sở tri thức | **Rỗng** — `No entries found` | Viewport |
