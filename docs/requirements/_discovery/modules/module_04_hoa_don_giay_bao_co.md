# Module 04 — Hoá đơn & Thanh toán · Giấy báo có

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Hoá đơn & Thanh toán | `INV` | Web | Invoices · Payments · Recurring Invoices |
| Giấy báo có | `CN` | Web | Credit Notes |

> **Payments gộp vào `INV`** (user chốt 20-09-2026). Căn cứ đo được: `/admin/payments` chỉ có **một nút Export**, không có nút tạo/sửa — thanh toán được ghi nhận từ bên trong hoá đơn. Gộp giữ các trường hợp kiểm *"thanh toán một phần → hoá đơn đổi trạng thái"* nằm cùng một chỗ.

---

## `INV` — Hoá đơn & Thanh toán

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách hoá đơn | `/admin/invoices` | Bảng |
| Lọc theo trạng thái | `/admin/invoices/list_invoices?status=<n>` | Bảng |
| Thêm/Sửa hoá đơn | `/admin/invoices/invoice[/<id>]` | Form dòng hàng |
| Hoá đơn định kỳ | `/admin/invoices/recurring` | Bảng |
| Thanh toán hàng loạt | Thanh công cụ — `Batch Payments` | Modal/Trang |
| Sổ thanh toán | `/admin/payments` | Bảng chỉ đọc |

**Cột hoá đơn:** Invoice # · Amount · Total Tax · Date · Customer · Project · Tags · Due Date · Status

**Cột thanh toán:** Payment # · Invoice # · Payment Mode · Transaction ID · Customer · Amount · Date

**Thanh công cụ hoá đơn:** Create New Invoice · Batch Payments · Recurring Invoices · *(2 nút biểu tượng)* · Filter by status · Export

### Status flow — 6 trạng thái, số đo tại 20-09-2026

| Trạng thái | Tham số lọc | Số bản ghi |
|---|---|---|
| Draft | `status=6` | 1 |
| Not Sent | `filter=not_sent` | 4 |
| Unpaid | `status=1` | 3 |
| Partially Paid | `status=3` | 0 |
| Overdue | `status=4` | 0 |
| Paid | `status=2` | 3 |

> ⚠️ `Not Sent` dùng `filter=`, năm trạng thái còn lại dùng `status=` → **Not Sent không phải một trạng thái ngang hàng**, mà là điều kiện lọc chồng lên trạng thái. Điểm này phải làm rõ ở tầng recon module (ứng viên `AMB`).
>
> Báo cáo bán hàng ghi nguyên văn: *"Cancelled invoices are excluded from the report"* → **tồn tại trạng thái `Cancelled`** không xuất hiện trong 6 ô lọc trên Dashboard. Phải truy ở tầng recon module.

**Số tiền đo trên Dashboard (năm 2026):** Outstanding Invoices `$2.00` · Past Due `$0.00` · Paid `$14.00`

**Risk 🔴 Cao** — tiền, thuế, nhiều trạng thái, có hoá đơn định kỳ (tác vụ chạy nền theo lịch), có thanh toán hàng loạt.

---

## `CN` — Giấy báo có

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/credit_notes` | Bảng |
| Thêm/Sửa | `/admin/credit_notes/credit_note[/<id>]` | Form dòng hàng |

**Cột:** Credit Note # · Credit Note Date · Customer · **Status** · Project · Reference # · Amount · **Remaining Amount**

**Thanh công cụ:** New Credit Note · *(nút biểu tượng)* · Export

**Quy mô:** 1 bản ghi tại thời điểm khảo sát.

> Cột `Remaining Amount` cho thấy giấy báo có **được dùng dần** để bù trừ vào hoá đơn → tồn tại nghiệp vụ *áp dụng giấy báo có vào hoá đơn*, và số dư còn lại phải khớp. Đây là vùng dễ sai số nhất của module.

**Risk 🔴 Cao** — bù trừ tiền, số dư luỹ kế, liên kết ngược sang `INV`.

---

## Vùng chưa xác minh — cả 2 module

| Việc | Lý do |
|---|---|
| Trạng thái `Cancelled` của hoá đơn | Chỉ thấy gián tiếp qua câu chú thích ở báo cáo, chưa thấy bản ghi nào |
| Ranh giới `Not Sent` (bộ lọc) ↔ trạng thái thật | Hai kiểu tham số khác nhau, chưa xác minh |
| Danh sách trạng thái đầy đủ của giấy báo có | Chỉ có 1 bản ghi, không đủ để quan sát |
| Form hoá đơn / giấy báo có | Chưa mở — form có dòng hàng, thuế, giảm giá; mở ra rồi bấm Save là tạo chứng từ tiền thật |
| `/admin/invoices/recurring` · `Batch Payments` | Chưa mở |
| Cách ghi nhận thanh toán từ trong hoá đơn | Là thao tác **ghi**, không thử trên môi trường dùng chung |
| Phương thức thanh toán (`Payment Mode`) có những giá trị nào | Master data nằm ở `SETTING` — 🚫 `access_denied` |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`invoices_list_viewport.png`](../evidence/invoices_list_viewport.png) | Danh sách hoá đơn | Mặc định — nhãn trạng thái `Paid` và `Unpaid` hiện trong bảng | Viewport |
| [`payments_list_viewport.png`](../evidence/payments_list_viewport.png) | Sổ thanh toán | Mặc định — chỉ có nút Export, chứng minh màn hình là chỉ đọc | Viewport |
| [`credit_notes_list_viewport.png`](../evidence/credit_notes_list_viewport.png) | Danh sách giấy báo có | Mặc định — thấy cột `Remaining Amount` | Viewport |

> Chụp viewport: ảnh full-page kéo theo số tiền và tên khách hàng của chứng từ thật.
