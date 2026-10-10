# Module 08 — Thanh toán · Phiếu ghi có

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `PAY` · `CRN` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.
> Gộp vì cả hai đều là bút toán **áp vào Invoice** (một bên trả tiền, một bên trừ công nợ).

---

## `PAY` — Payments

| Mục | Ghi nhận |
|---|---|
| Route | `/admin/payments` (menu Sales › Payments) |
| Loại màn hình | Danh sách — **không có nút tạo mới** → payment được ghi từ Invoice (tab Payments / Batch Payments) |
| Thanh công cụ | `Export` · page size · Search · reload |
| Cột bảng | `Payment #` · `Invoice #` · `Payment Mode` · `Transaction ID` · `Customer` · `Amount` · `Date` |
| Số bản ghi Staff thấy | 1 |
| Ước REQ | ~15 |
| Risk | 🔴 — tiền; ghi thanh toán đổi trạng thái Invoice (Unpaid → Partially Paid → Paid) |

**Ngoài quyền Staff:** Payment Modes (`/admin/paymentmodes` → Access denied).

**Vùng chưa xác minh:** form ghi thanh toán (nằm ở Invoice) · biên lai PDF · xoá payment có hoàn lại trạng thái Invoice không.

---

## `CRN` — Credit Notes

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/credit_notes` · Tạo mới `/admin/credit_notes/credit_note` |
| Thanh công cụ | `New Credit Note` · nút thu/mở bảng · bộ lọc · `Export` |
| Cột bảng | `Credit Note #` · `Credit Note Date` · `Customer` · `Status` · `Project` · `Reference #` · `Amount` · `Remaining Amount` |
| Form tạo mới | 21 field hiển thị · 4 dấu bắt buộc · có bảng dòng hàng |
| Thông điệp (từ bộ ngôn ngữ) | "Credit note number already exists" · "Total credits amount is bigger then invoice balance due" · "Total credits amount is bigger then remaining credits" |
| Status flow | Có cột `Status` — danh sách trạng thái ❔ (chưa có bản ghi nào) |
| Số bản ghi Staff thấy | 0 |
| Ước REQ | ~30 |
| Risk | 🟡 — trừ công nợ Invoice; rule không vượt số dư |

**Vùng chưa xác minh:** danh sách trạng thái · luồng áp credit vào invoice · hoàn tiền.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [pay_overview_viewport.png](../evidence/pay_overview_viewport.png) | Payments — danh sách | Mặc định · thân bảng **đã làm mờ** | Viewport |
| [crn_overview_viewport.png](../evidence/crn_overview_viewport.png) | Credit Notes — danh sách | Bảng rỗng | Viewport |
