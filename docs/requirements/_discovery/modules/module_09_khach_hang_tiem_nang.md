# Module 09 — Khách hàng tiềm năng

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Khách hàng tiềm năng | `LEAD` | Web | Leads |

> Đứng riêng một file: đây là **phễu bán hàng độc lập** có bảng Kanban riêng, kết thúc bằng việc chuyển đổi thành khách hàng — một nghiệp vụ không thuộc module nào khác.

## Màn hình

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/leads` | Bảng |
| Bảng Kanban | `/admin/leads/switch_kanban/1` | Kanban |
| Biểu đồ | Thanh công cụ — nút biểu tượng | Biểu đồ |
| Thêm/Sửa | Hộp thoại — nút `New Lead` có `href="#"` | Modal |

**Cột:** *(checkbox)* · `#` · Name · Company · Email · Phone · **Value** · Tags · Assigned · **Status** · **Source** · Last Contact · Created

**Thanh công cụ:** New Lead · *(biểu tượng)* Biểu đồ · *(biểu tượng)* Kanban · Export · Bulk Actions

## ⚠️ Quy mô: 0 bản ghi

Bảng hiển thị nguyên văn **`No entries found`** (xác nhận lại bằng cách mở ảnh evidence, không chỉ đọc DOM).

Hệ quả nghiêm trọng cho tầng recon module:

| Không quan sát được | Vì sao quan trọng |
|---|---|
| Các giá trị của `Status` | Là trục chính của Kanban — không biết có bao nhiêu cột |
| Các giá trị của `Source` | Master data ở `SETTING` 🚫 |
| Luồng **chuyển đổi lead → khách hàng** | Nghiệp vụ đắt giá nhất của module, hoàn toàn không thấy |
| Kanban trông thế nào | Cột rỗng hết |

→ Recon module này **cần dữ liệu mẫu**, hoặc phải chấp nhận tạo lead thử. Trên **môi trường dùng chung** thì phải dọn sau khi tạo.

## CRUD & trạng thái

| | |
|---|---|
| CRUD | Đầy đủ + Export + Bulk Actions |
| Status flow | Có — `Status` (Kanban) + `Source` (phân loại) |
| Số tab | ❔ chưa mở được chi tiết (không có bản ghi nào) |

## Risk: 🔴 Cao

- Có cột `Value` → gắn với tiền dự kiến, đi vào báo cáo `/admin/reports/leads`
- Là **điểm vào của dữ liệu cá nhân người chưa phải khách hàng** (tên, email, điện thoại)
- Có luồng chuyển đổi sang `CUST` — bước sinh dữ liệu gốc cho toàn hệ thống
- **Mức phủ quan sát bằng 0** → risk cao nhất không phải vì màn hình phức tạp, mà vì *chưa ai nhìn thấy nó chạy*

## Vùng chưa xác minh

Toàn bộ. Xem bảng ở mục Quy mô.

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`leads_list_empty_viewport.png`](../evidence/leads_list_empty_viewport.png) | Danh sách lead | **Rỗng** — `No entries found`, thấy đủ nút `New Lead` · Biểu đồ · Kanban · Export · Bulk Actions | Viewport — đã mở lại ảnh xác nhận đúng trạng thái rỗng |
