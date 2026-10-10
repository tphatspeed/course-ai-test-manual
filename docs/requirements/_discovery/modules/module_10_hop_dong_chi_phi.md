# Module 10 — Hợp đồng · Chi phí

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `CTR` · `EXP` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.
> Gộp vì cả hai đều 🟡, cùng gắn Customer + Project, quy mô vừa.

---

## `CTR` — Contracts

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/contracts` · Tạo mới / Chi tiết `/admin/contracts/contract` · `/admin/contracts/contract/{id}` |
| Loại màn hình | Danh sách + biểu đồ + Form + Chi tiết nhiều tab |
| Thanh công cụ | `New Contract` · bộ lọc · `Export` |
| Summary | **Active · Expired · About to Expire · Recently Added · Trash** |
| Biểu đồ | "Contracts by Type" · "Contracts Value by Type (USD)" |
| Cột bảng | `#` · `Subject` · `Customer` · `Contract Type` · `Contract Value` · `Start Date` · `End Date` · `Project` · `Signature` |
| Form tạo mới | 9 field hiển thị · 2 dấu bắt buộc |
| Chi tiết — 7 tab | Contract · Attachments · Comments · Renewal History · Tasks · Notes · Templates |
| Số bản ghi Staff thấy | 135 |
| Tương tác khách hàng | Thông báo "New comment from customer on contract …" → khách hàng bình luận (và có thể ký) qua cổng |
| Ước REQ | ~35 |
| Risk | 🟡 — ký điện tử · gia hạn · hết hạn · Trash (xoá mềm) |

**Ngoài quyền Staff:** Contract Types (`/admin/contracts/types` → Access denied).

**Vùng chưa xác minh:** luồng ký (cột Signature) · gia hạn · khôi phục từ Trash · phía khách hàng (`AMB-SYS-03`).

---

## `EXP` — Expenses

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/expenses` · Tạo mới `/admin/expenses/expense` |
| Thanh công cụ | `Record Expense` · `Import Expenses` · nút thu/mở bảng · nút biểu đồ · bộ lọc · `Export` · `Bulk Actions` |
| Cột bảng | `Category` · `Amount` · `Name` · `Receipt` · `Date` · `Project` · `Customer` · `Invoice` · `Reference #` · `Payment Mode` |
| Form tạo mới | 12 field hiển thị · 4 dấu bắt buộc · có upload chứng từ (Receipt) |
| Số bản ghi Staff thấy | 0 (`AMB-SYS-02`) |
| Ước REQ | ~25 |
| Risk | 🟡 — có thể tính lại vào Invoice (cột `Invoice`) · upload file · đi vào Reports Expenses vs Income |

**Ngoài quyền Staff:** Expense Categories (`/admin/expenses/categories` → Access denied).

**Vùng chưa xác minh:** chi phí định kỳ · chuyển chi phí thành hoá đơn · giới hạn file upload.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [ctr_overview_viewport.png](../evidence/ctr_overview_viewport.png) | Contracts — Summary + biểu đồ | Mặc định (bảng nằm dưới, ngoài viewport) | Viewport |
| [exp_overview_viewport.png](../evidence/exp_overview_viewport.png) | Expenses — danh sách | Bảng rỗng | Viewport |
