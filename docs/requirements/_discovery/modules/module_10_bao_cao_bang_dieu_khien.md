# Module 10 — Báo cáo · Bảng điều khiển

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Báo cáo | `RPT` | Web | Reports |
| Bảng điều khiển | `DASH` | Web | Dashboard |

> Chung file vì cả hai **chỉ đọc và chỉ tổng hợp lại số liệu của module khác** — không sở hữu entity nào.

---

## `RPT` — Báo cáo

| Báo cáo | Route |
|---|---|
| Bán hàng | `/admin/reports/sales` |
| Chi phí | `/admin/reports/expenses` |
| Chi phí ↔ Thu nhập | `/admin/reports/expenses_vs_income` |
| Khách hàng tiềm năng | `/admin/reports/leads` |
| Tổng quan chấm công | `/admin/staff/timesheets?view=all` |
| Bài viết tri thức | `/admin/reports/knowledge_base_articles` |

### Báo cáo Bán hàng — 8 mục con (đọc nguyên văn từ trang)

Invoices Report · Items Report · Payments Received · Credit Notes Report · Proposals Report · Estimates Report · Customers Report · Charts Based Report

Kèm 3 biểu đồ: **Total Income** · **Payment Modes (Transactions)** · **Total Value By Customer Groups**

> Trang ghi nguyên văn: **"Cancelled invoices are excluded from the report"** — đây là **business rule đọc được trực tiếp từ UI**, và là manh mối duy nhất cho thấy hoá đơn có trạng thái `Cancelled` (xem [module 04](module_04_hoa_don_giay_bao_co.md)).

> ⚠️ `Tổng quan chấm công` nằm trong menu Reports nhưng trỏ sang route của `TIME` (`/admin/staff/timesheets?view=all`) — **không** phải màn hình riêng của `RPT`. Ghi nhận để không đếm trùng.

**Risk 🟡 Trung bình** — chỉ đọc nên không phá dữ liệu, nhưng **số sai ở báo cáo dẫn tới quyết định sai**, và báo cáo là nơi lộ ra lỗi tính toán của các module tiền.

---

## `DASH` — Bảng điều khiển

| Màn hình | Route |
|---|---|
| Bảng điều khiển | `/admin/` |
| Đặt lại bố cục | `/admin/staff/reset_dashboard` |

### Thành phần đo được tại 20-09-2026

| Khối | Nội dung |
|---|---|
| 3 thẻ tổng quan | Invoice overview · Estimate overview · Proposal overview — mỗi thẻ 6 dòng trạng thái kèm số đếm **và phần trăm** |
| Bộ chọn năm | Dropdown `2026` |
| 3 ô tiền | Outstanding Invoices `$2.00` · Past Due Invoices `$0.00` · Paid Invoices `$14.00` |
| Hàng chỉ số | Invoices Awaiting Payment `3 / 6` · Converted Leads `0 / 0` · Projects `70 / 172` · Tasks `258 / 259` |
| Khối danh sách | My To Do Items · Tasks (bảng) · Reminders · Projects gần đây |
| Tuỳ biến | Nút **Dashboard Options** (góc phải) · các khối **kéo thả được** (thấy tay cầm kéo `⠿` bên trái mỗi khối) |

> Các số phần trăm được tính tại chỗ (VD `4 Not Sent → 57.14%` trên tổng 7 hoá đơn). Đây là **nguồn sai số dễ bỏ sót**: mẫu số là gì khi có hoá đơn `Cancelled`?

**Risk 🟢 Thấp** — không sở hữu dữ liệu, không thao tác ghi ngoài việc sắp xếp bố cục. Nhưng là **màn hình đầu tiên mọi người nhìn thấy** → smoke test bắt buộc.

---

## Vùng chưa xác minh — cả 2 module

| Việc | Lý do |
|---|---|
| Nội dung 8 mục con của báo cáo bán hàng | Chưa mở từng mục |
| 5 báo cáo còn lại | Chưa mở |
| Cách tính phần trăm ở thẻ tổng quan | Cần đối chiếu với dữ liệu gốc |
| Hộp thoại `Dashboard Options` | Chưa mở |
| Kéo thả sắp xếp khối · `reset_dashboard` | Thao tác **ghi** vào hồ sơ người dùng — không thử trên môi trường dùng chung |
| Bộ chọn năm có những năm nào | Chưa mở dropdown |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`reports_sales_viewport.png`](../evidence/reports_sales_viewport.png) | Báo cáo bán hàng | Mặc định — thấy 8 mục con + câu chú thích về hoá đơn `Cancelled` | Viewport |
| [`dashboard_overview_viewport.png`](../evidence/dashboard_overview_viewport.png) | Bảng điều khiển | Mặc định sau đăng nhập — thấy 3 thẻ tổng quan, 3 ô tiền, nút Dashboard Options | Viewport |
