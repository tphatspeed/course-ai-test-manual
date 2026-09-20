# Module 07 — Chi phí · Danh mục hàng hoá

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Chi phí | `EXP` | Web | Expenses |
| Danh mục hàng hoá | `ITEM` | Web | Items · Invoice Items |

> Chung file vì cả hai **cấp dòng hàng và số tiền cho chứng từ bán hàng**: hàng hoá cấp dòng cho hoá đơn/dự toán, chi phí có thể tính lại vào hoá đơn cho khách.

---

## `EXP` — Chi phí

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/expenses` | Bảng |
| Ghi nhận chi phí | `/admin/expenses/expense[/<id>]` | Form |
| Nhập từ tệp | `/admin/expenses/import` | Wizard |

**Cột:** *(checkbox)* · Category · Amount · Name · **Receipt** · Date · Project · Customer · **Invoice** · Reference # · Payment Mode

**Thanh công cụ:** Record Expense · Import Expenses · *(2 nút biểu tượng)* · Export · Bulk Actions

**Quy mô:** 1 bản ghi.

> Cột `Invoice` → **chi phí tính lại được cho khách hàng**, sinh ra hoá đơn. Cột `Receipt` → có đính kèm tệp chứng từ. Hai điểm này là phần nghiệp vụ nặng nhất của module.

**Risk 🟡 Trung bình** — tiền chiều ra, có đính kèm tệp, có nhập hàng loạt; nhưng ít trạng thái và khối lượng nhỏ.

---

## `ITEM` — Danh mục hàng hoá

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/invoice_items` | Bảng |
| Thêm/Sửa | Hộp thoại — nút `New Item` có `href="#"` | Modal |
| Nhóm hàng hoá | Hộp thoại — nút `Groups` có `href="#"` | Modal |
| Nhập từ tệp | `/admin/invoice_items/import` | Wizard |

**Cột:** *(checkbox)* · Description · Long Description · **Rate** · **Tax 1** · **Tax 2** · Unit · Group Name

**Thanh công cụ:** New Item · Import Items · Groups · Export · Bulk Actions

**Quy mô:** 29 bản ghi trên trang đầu.

> **Hai cột thuế riêng biệt** (`Tax 1`, `Tax 2`) → một mặt hàng chịu được hai sắc thuế chồng nhau. Đây là nguồn sai số tính tiền kinh điển, phải kiểm kỹ ở tầng recon module. Giá trị thuế là master data của `SETTING` — 🚫 `access_denied`.

**Risk 🟡 Trung bình** — là master data, sai một dòng thì mọi chứng từ dùng dòng đó đều sai; nhưng bản thân màn hình đơn giản.

---

## Vùng chưa xác minh — cả 2 module

| Việc | Lý do |
|---|---|
| Hộp thoại thêm hàng hoá · quản lý nhóm | Chưa mở — tránh tạo dữ liệu |
| Danh sách thuế khả dụng cho `Tax 1` / `Tax 2` | Master data ở `SETTING` — 🚫 `access_denied` |
| Danh mục chi phí (`Category`) có giá trị nào | Cùng lý do |
| Phương thức thanh toán của chi phí | Cùng lý do |
| Luồng tính lại chi phí vào hoá đơn | Là thao tác ghi |
| Đính kèm tệp chứng từ | Là thao tác ghi, và tải tệp lên môi trường dùng chung |
| Hai wizard nhập từ tệp | Chưa mở |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`expenses_list_viewport.png`](../evidence/expenses_list_viewport.png) | Danh sách chi phí | Mặc định — thấy cột `Receipt` và `Invoice` | Viewport |
| [`items_list_viewport.png`](../evidence/items_list_viewport.png) | Danh mục hàng hoá | Mặc định — thấy hai cột `Tax 1` · `Tax 2` | Viewport |
