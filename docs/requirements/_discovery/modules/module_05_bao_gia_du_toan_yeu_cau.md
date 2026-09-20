# Module 05 — Đề xuất · Dự toán · Yêu cầu báo giá

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Dự toán | `EST` | Web | Estimates |
| Đề xuất | `PROP` | Web | Proposals |
| Yêu cầu báo giá | `ESTREQ` | Web | Estimate Request |

> Ba module chung file vì chúng là **một chuỗi tiền bán hàng**: khách gửi yêu cầu → ra dự toán / đề xuất → chấp nhận → thành hoá đơn. Prefix vẫn tách riêng.

---

## `EST` — Dự toán

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/estimates` | Bảng |
| Lọc theo trạng thái | `/admin/estimates/list_estimates?status=<n>` | Bảng |
| Sơ đồ đường ống | `/admin/estimates/pipeline/1` | Kanban |
| Thêm/Sửa | `/admin/estimates/estimate[/<id>]` | Form dòng hàng |

**Cột:** Estimate # · Amount · Total Tax · Customer · Project · Tags · Date · Expiry Date · Reference # · Status

### Status flow — 6 trạng thái, số đo tại 20-09-2026

| Trạng thái | Tham số lọc | Số bản ghi |
|---|---|---|
| Draft | `status=1` | 0 |
| Not Sent | `not_sent=1` | 3 |
| Sent | `status=2` | 0 |
| Expired | `status=5` | 3 |
| Declined | `status=3` | 0 |
| Accepted | `status=4` | 0 |

> Cùng kiểu bất thường như hoá đơn: `Not Sent` dùng tham số riêng (`not_sent=1`), không phải `status=`. Ứng viên `AMB` — nhất quán với phát hiện ở [module 04](module_04_hoa_don_giay_bao_co.md).
>
> Có `Expiry Date` + trạng thái `Expired` → **tồn tại tác vụ tự hết hạn theo thời gian**. Kiểm được điều này cần sửa ngày hệ thống hoặc chờ — ghi rõ là hạn chế.

**Risk 🔴 Cao** — tiền, 6 trạng thái, có chuyển đổi sang hoá đơn, có hết hạn tự động.

---

## `PROP` — Đề xuất

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/proposals` | Bảng |
| Lọc theo trạng thái | `/admin/proposals/list_proposals?status=<n>` | Bảng |
| Sơ đồ đường ống | `/admin/proposals/pipeline/1` | Kanban |
| Thêm/Sửa | `/admin/proposals/proposal[/<id>]` | Form dòng hàng |

**Cột:** Proposal # · Subject · To · Total · Date · Open Till · Project · Tags · Date Created · Status

### Status flow — 6 trạng thái, số đo tại 20-09-2026

| Trạng thái | Tham số lọc | Số bản ghi |
|---|---|---|
| Draft | `status=6` | 0 |
| Sent | `status=4` | 1 |
| Open | `status=1` | 4 |
| Revised | `status=5` | 0 |
| Declined | `status=2` | 0 |
| Accepted | `status=3` | 0 |

> Cả 6 trạng thái đều dùng `status=` — **khác** hoá đơn và dự toán. Củng cố nghi vấn `Not Sent` ở hai module kia là bộ lọc chứ không phải trạng thái.
>
> Cột `To` (không phải `Customer`) → đề xuất gửi được cho **cả lead lẫn khách hàng**. Phải xác minh ở tầng recon module.

**Risk 🔴 Cao** — tiền, 6 trạng thái, khách hàng xem và phản hồi được từ cổng ngoài (Dashboard có thông báo *"New comment from customer on contract…"* → cơ chế bình luận từ phía khách có thật).

---

## `ESTREQ` — Yêu cầu báo giá

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/estimate_request` | Bảng |
| Trình tạo biểu mẫu | `/admin/estimate_request/form` | Form builder |

**Cột:** `#` · Email · Tags · Assigned · Status · Created

**Thanh công cụ:** New Form · Export

**Quy mô:** **`No entries found`** — 0 bản ghi.

> Đây là **biểu mẫu công khai cho người ngoài điền**, kết quả rơi vào danh sách này. `New Form` là trình tạo biểu mẫu, không phải tạo một yêu cầu.

**Risk 🟡 Trung bình** — điểm tiếp nhận dữ liệu từ người dùng ẩn danh bên ngoài (cần soi kỹ phần chống lạm dụng), nhưng khối lượng nhỏ và chưa có dữ liệu.

---

## Vùng chưa xác minh — cả 3 module

| Việc | Lý do |
|---|---|
| Hành vi hết hạn tự động của dự toán | Cần can thiệp thời gian hệ thống |
| Đề xuất gửi cho lead hay cho khách hàng | Cột `To` chưa truy được nguồn |
| Toàn bộ nghiệp vụ `ESTREQ` | **0 bản ghi** — không quan sát được luồng trạng thái nào |
| Trình tạo biểu mẫu `/admin/estimate_request/form` | Chưa mở |
| Sơ đồ đường ống của `EST` · `PROP` | Chưa mở — kéo thả ở đó **ghi** dữ liệu |
| Form thêm dự toán / đề xuất | Chưa mở — là chứng từ tiền |
| Luồng chuyển dự toán/đề xuất → hoá đơn | Là thao tác ghi |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`estimates_list_viewport.png`](../evidence/estimates_list_viewport.png) | Danh sách dự toán | Mặc định — nhãn `Expired` hiện trong bảng | Viewport |
| [`proposals_list_viewport.png`](../evidence/proposals_list_viewport.png) | Danh sách đề xuất | Mặc định | Viewport |
| [`estimate_request_list_empty_viewport.png`](../evidence/estimate_request_list_empty_viewport.png) | Yêu cầu báo giá | **Rỗng** — `No entries found` | Viewport — chính trạng thái rỗng là thứ cần chứng minh |
