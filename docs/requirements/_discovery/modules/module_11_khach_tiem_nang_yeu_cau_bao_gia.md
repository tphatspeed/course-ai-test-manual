# Module 11 — Khách tiềm năng · Yêu cầu báo giá

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `LEAD` · `ESTREQ` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.
> Gộp vì cả hai là đầu phễu bán hàng (thu nhận yêu cầu từ bên ngoài → giao người xử lý → theo trạng thái).

---

## `LEAD` — Leads

| Mục | Ghi nhận |
|---|---|
| Route | `/admin/leads` · chi tiết / tạo mới mở dạng modal |
| Loại màn hình | Danh sách + Kanban (nút icon) + Modal |
| Thanh công cụ | `New Lead` · nút biểu đồ · nút Kanban · bộ lọc · `Export` · `Bulk Actions` |
| Cột bảng | `#` · `Name` · `Company` · `Email` · `Phone` · `Value` · `Tags` · `Assigned` · `Status` · `Source` · `Last Contact` · `Created` |
| Số bản ghi Staff thấy | 0 (`AMB-SYS-02`) · Dashboard "Converted Leads 0 / 0" |
| Status flow | Có cột `Status` — danh sách trạng thái do admin cấu hình (`/admin/leads/statuses` → Access denied) |
| Ước REQ | ~40 |
| Risk | 🟡 — chuyển đổi Lead → Customer · Kanban · dữ liệu cá nhân (email, điện thoại) |

**Ngoài quyền Staff:** Lead Sources · Lead Statuses (Access denied).

**Vùng chưa xác minh:** form modal · chuyển đổi thành customer · import · form web-to-lead.

---

## `ESTREQ` — Estimate Request

| Mục | Ghi nhận |
|---|---|
| Route | `/admin/estimate_request` |
| Loại màn hình | Danh sách yêu cầu + Form builder (`New Form` — tạo form công khai để khách gửi yêu cầu báo giá) |
| Thanh công cụ | `New Form` · `Export` |
| Cột bảng | `#` · `Email` · `Tags` · `Assigned` · `Status` · `Created` |
| Số bản ghi Staff thấy | 0 |
| Ước REQ | ~15 |
| Risk | 🟢 — ít dữ liệu, chưa thấy liên kết bắt buộc với module khác |

**Vùng chưa xác minh:** form builder · URL form công khai · trạng thái yêu cầu · có tạo Lead / Estimate từ yêu cầu không.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [lead_overview_viewport.png](../evidence/lead_overview_viewport.png) | Leads — danh sách | Bảng rỗng | Viewport |
| [estreq_overview_viewport.png](../evidence/estreq_overview_viewport.png) | Estimate Request — danh sách | Bảng rỗng | Viewport |
