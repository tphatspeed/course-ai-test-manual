# Module 06 — Hàng hoá · Đề xuất · Báo giá

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `ITEM` · `PROP` · `EST` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.
> Gộp vì cả 3 cùng là nhóm chứng từ bán hàng trước hoá đơn, cùng dùng dòng hàng Item.

---

## `ITEM` — Items

| Mục | Ghi nhận |
|---|---|
| Route | `/admin/invoice_items` (menu Sales › Items) |
| Loại màn hình | Danh sách + Modal (tạo/sửa item, quản lý nhóm) |
| Thanh công cụ | `New Item` · `Import Items` · `Groups` · `Export` · `Bulk Actions` · page size · Search · reload |
| Cột bảng | `Description` · `Long Description` · `Rate` · `Tax 1` · `Tax 2` · `Unit` · `Group Name` |
| Số bản ghi Staff thấy | 86 |
| Status flow | Không |
| Ước REQ | ~20 |
| Risk | 🟡 — là dòng hàng cho Proposal / Estimate / Invoice / Credit Note; sai Rate / Tax lan sang mọi chứng từ |

**Vùng chưa xác minh:** modal New Item (field, rule thuế) · định dạng Import · danh mục thuế (`/admin/taxes` → Access denied).

---

## `PROP` — Proposals

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/proposals` · chi tiết dạng chia đôi màn hình `/admin/proposals/list_proposals/{id}` · Tạo mới `/admin/proposals/proposal` |
| Thanh công cụ | `New Proposal` · nút Kanban · nút thu/mở bảng (`«`) · bộ lọc · `Export` |
| Status flow | **Draft · Sent · Open · Revised · Declined · Accepted** (mã: 6 · 4 · 1 · 5 · 2 · 3 — đọc từ link Dashboard) |
| Cột bảng | `Proposal #` · `Subject` · `To` · `Total` · `Date` · `Open Till` · `Project` · `Tags` · `Date Created` · `Status` |
| Form tạo mới | 29 field hiển thị · 7 dấu bắt buộc · có bảng dòng hàng |
| Chi tiết — 6 tab | Proposal · Comments · Reminders · Tasks · Notes · Templates |
| Số bản ghi Staff thấy | 5 (`AMB-SYS-02`) |
| Ước REQ | ~35 |
| Risk | 🟡 — status flow 6 trạng thái · khách hàng chấp nhận / từ chối qua cổng (`AMB-SYS-03`) |

---

## `EST` — Estimates

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/estimates` · chi tiết `/admin/estimates/list_estimates/{id}` · Tạo mới `/admin/estimates/estimate` |
| Thanh công cụ | `Create New Estimate` · nút Kanban · nút thu/mở bảng · nút biểu đồ · bộ lọc · `Export` |
| Status flow | **Draft · Sent · Expired · Declined · Accepted** (mã: 1 · 2 · 5 · 3 · 4) + bộ lọc `Not Sent` |
| Cột bảng | `Estimate #` · `Amount` · `Total Tax` · `Customer` · `Project` · `Tags` · `Date` · `Expiry Date` · `Reference #` · `Status` |
| Form tạo mới | 25 field hiển thị · 4 dấu bắt buộc · có bảng dòng hàng |
| Chi tiết — 5 tab | Estimate · Tasks · Activity Log · Reminders · Notes |
| Thông điệp (từ bộ ngôn ngữ) | "This estimate number exists for the ongoing year." → số báo giá duy nhất **trong năm** |
| Số bản ghi Staff thấy | 3 — cả 3 đều **Expired** |
| Ước REQ | ~40 |
| Risk | 🟡 — status flow · hết hạn tự động · chuyển thành Invoice |

**Vùng chưa xác minh (PROP + EST):** nút chuyển đổi Proposal → Estimate / Invoice và Estimate → Invoice · gửi email · xuất PDF · rule đánh số.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [item_overview_viewport.png](../evidence/item_overview_viewport.png) | Items — danh sách | Mặc định · thân bảng **đã làm mờ** | Viewport |
| [prop_overview_viewport.png](../evidence/prop_overview_viewport.png) | Proposals — danh sách | Mặc định · menu Sales mở · thân bảng **đã làm mờ** | Viewport |
| [est_overview_viewport.png](../evidence/est_overview_viewport.png) | Estimates — danh sách | Mặc định · thân bảng **đã làm mờ** | Viewport |
