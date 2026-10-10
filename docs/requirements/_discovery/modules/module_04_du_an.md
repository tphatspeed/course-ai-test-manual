# Module 04 — Dự án

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `PRJ` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `PRJ` — Projects

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/projects` · Tạo mới `/admin/projects/project` · Chi tiết `/admin/projects/view/{id}` |
| Loại màn hình | Danh sách + Form + Chi tiết nhiều tab |
| Thanh công cụ | `New Project` · nút icon (chế độ xem, không nhãn) · `Export` · bộ lọc · page size 25 · Search · reload |
| Summary (status flow) | **Not Started · In Progress · On Hold · Cancelled · Finished** — 5 trạng thái |
| Cột bảng | `#` · `Project Name` · `Customer` · `Tags` · `Start Date` · `Deadline` · `Members` · `Status` |
| Số bản ghi Staff thấy | Bảng: "Showing 1 to 25 of **184** entries" · Summary cộng lại: 94 + 70 + 24 + 2 + 1 = **191** (03-10-2026) |
| Form tạo mới | 2 tab: `Project` · `Project Settings` · 12 field hiển thị · 4 dấu bắt buộc |
| Chi tiết — 12 tab | Overview · Tasks · Timesheets · Milestones · Files · Discussions · Gantt · Tickets · Contracts · Sales · Notes · Activity |
| Ước REQ | ~70 — vượt ngưỡng, khi recon sẽ tách Story theo nhóm tab |
| Risk | 🔴 — 12 tab · status flow 5 trạng thái · gắn với Tasks, Timesheets, Tickets, Contracts, chứng từ bán hàng · Dashboard và Reports đọc số liệu từ đây |

**Vùng chưa xác minh:**
- **Lệch 191 (Summary) vs 184 (bảng)** — có thể bảng áp bộ lọc mặc định loại một số trạng thái. Kiểm khi recon, nếu không giải thích được thì mở AMB ở cấp module
- Nội dung tab Project Settings (quyền khách hàng xem gì trên cổng)
- Tab Gantt / Discussions — tương tác phức tạp
- Quyền Delete và đổi trạng thái

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [prj_overview_viewport.png](../evidence/prj_overview_viewport.png) | Projects — danh sách + Summary | Mặc định · thân bảng **đã làm mờ** | Viewport |
