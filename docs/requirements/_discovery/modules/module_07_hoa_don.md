# Module 07 — Hoá đơn

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `INV` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `INV` — Invoices (gồm Recurring Invoices, Batch Payments)

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/invoices` · chi tiết `/admin/invoices/list_invoices/{id}` · lọc `/admin/invoices/list_invoices?status=<mã>` · Tạo mới `/admin/invoices/invoice` · Recurring `/admin/invoices/recurring` |
| Loại màn hình | Danh sách + Form chứng từ (có bảng dòng hàng) + chi tiết chia đôi màn hình |
| Thanh công cụ | `Create New Invoice` · `Batch Payments` (modal) · `Recurring Invoices` · nút thu/mở bảng · nút biểu đồ · `Filter by status` · `Export` |
| Status flow | **Draft (6) · Unpaid (1) · Paid (2) · Partially Paid (3) · Overdue (4)** + bộ lọc `Not Sent`. Trạng thái **Cancelled** có tồn tại (Reports › Sales ghi "Cancelled invoices are excluded from the report") — chưa thấy trên danh sách |
| Cột bảng | `Invoice #` · `Amount` · `Total Tax` · `Date` · `Customer` · `Project` · `Tags` · `Due Date` · `Status` |
| Recurring Invoices | Cột: `Invoice # · Amount · Customer · Frequency · Cycles Remaining · Last Child Invoice Date · Next Invoice` · `Create New Invoice` · `Go Back` |
| Form tạo mới | 27 field hiển thị · 4 dấu bắt buộc |
| Chi tiết — 6 tab | Invoice · Payments · Tasks · Activity Log · Reminders · Notes |
| Thông điệp (từ bộ ngôn ngữ) | "This invoice number exists for the ongoing year." · "Enter at least one item." · "Have you forgotten to add this item?" · "Started timers found" (lập hoá đơn từ task có timer) |
| Số bản ghi Staff thấy | 6 (`AMB-SYS-02`) |
| Ước REQ | ~50 |
| Risk | 🔴 — tiền · status flow 6 trạng thái · hoá đơn định kỳ sinh tự động · liên kết Payment / Credit Note / Subscription / Expense · nguồn số liệu chính của Reports và Dashboard |

**Vùng chưa xác minh:** tính thuế / chiết khấu / làm tròn (2 chữ số thập phân) · chuyển Overdue tự động · Batch Payments · lập hoá đơn từ task/timesheet · gửi email · cổng thanh toán (không kiểm được tầng tích hợp) · số hoá đơn duy nhất trong năm.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [inv_overview_viewport.png](../evidence/inv_overview_viewport.png) | Invoices — danh sách | Mặc định · menu Sales mở · thân bảng **đã làm mờ** | Viewport |
