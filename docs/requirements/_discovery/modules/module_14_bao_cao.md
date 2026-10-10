# Module 14 — Báo cáo

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `RPT` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `RPT` — Reports

| Báo cáo | Route | Ghi nhận |
|---|---|---|
| Sales | `/admin/reports/sales` | **Sales Report:** Invoices · Items · Payments Received · Credit Notes · Proposals · Estimates · Customers. **Charts Based Report:** Total Income · Payment Modes (Transactions) · Total Value By Customer Groups. Ghi chú đỏ: "Cancelled invoices are excluded from the report" |
| Expenses | `/admin/reports/expenses` | Bảng theo danh mục × 12 tháng + cột `Year (2026)` · nút `Detailed Report` |
| Expenses vs Income | `/admin/reports/expenses_vs_income` | Biểu đồ |
| Leads | `/admin/reports/leads` | "This Week Leads Conversions" · "Sources Conversion" · "Monthly" (chọn tháng) · nút `Switch to staff report`. `document.title` chung chung (`AMB-SYS-07`) |
| Timesheets overview | `/admin/staff/timesheets?view=all` | Xem ở `TASK` (module 05) — báo cáo này dùng chung màn hình Timesheets |
| KB Articles | `/admin/reports/knowledge_base_articles` | Chọn nhóm (`Choose Group`) |

| Mục | Ghi nhận |
|---|---|
| Loại màn hình | Báo cáo chỉ đọc — bảng mở rộng/thu gọn + biểu đồ |
| Ước REQ | ~25 |
| Risk | 🟡 — số liệu tiền tổng hợp từ `INV` · `PAY` · `CRN` · `EST` · `PROP` · `EXP` · `LEAD`; Staff có phạm vi dữ liệu hẹp (`AMB-SYS-02`) nên báo cáo có thể không phản ánh toàn hệ thống |

**Vùng chưa xác minh:** bộ lọc kỳ báo cáo / tiền tệ · xuất file · báo cáo có lọc theo quyền xem của Staff không · nội dung khi mở rộng từng báo cáo (chưa mở để giảm tải).

**Thứ tự:** khảo sát **sau cùng** — cần hiểu rule của các module nguồn trước.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [rpt_overview_viewport.png](../evidence/rpt_overview_viewport.png) | Reports › Sales | Mặc định — mọi báo cáo đang thu gọn · menu Reports mở | Viewport |
