# Module 02 — Khách hàng

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Khách hàng | `CUST` | Web | Customers · Clients · Contacts |

> Trạng thái recon tra ở [`docs/requirements/README.md`](../../README.md).

## Phạm vi & quyết định gộp

**Contacts gộp vào `CUST`** (user chốt 20-09-2026). Căn cứ đo được: `/admin/clients/all_contacts` chỉ có **một nút Export**, không có nút tạo mới ở cấp toàn cục — contact chỉ sinh ra bên trong một khách hàng (nút `Save and create contact` ở form thêm khách hàng). Không có vòng đời độc lập.

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách khách hàng | `/admin/clients` | Bảng dữ liệu |
| Thêm/Sửa khách hàng | `/admin/clients/client[/<id>]` | Form 2 tab |
| Chi tiết khách hàng | `/admin/clients/client/<id>` | Trang 22 tab |
| Danh sách liên hệ toàn cục | `/admin/clients/all_contacts` | Bảng chỉ đọc |
| Nhập khách hàng từ tệp | `/admin/clients/import` | Wizard |
| Nhóm khách hàng | `/admin/clients/groups` | 🚫 `access_denied` → thuộc `SETTING` |

## Danh sách — đọc từ DOM

**Cột:** *(checkbox)* · `#` · Company · Primary Contact · Primary Email · Phone · Active · Groups · Date Created

**Thanh công cụ:** New Customer · Import Customers · Contacts · Export · Bulk Actions

**Quy mô:** `dt-page-jump-clients` có **82 option** → ~2.050 bản ghi ở cỡ trang 25. Cỡ trang: **5 lựa chọn** (`clients_length`).

## Form thêm khách hàng

**2 tab:** Customer Details · Billing & Shipping — **30 trường** (13 hiện ở tab mặc định).

Tên trường thật: `company` · `vat` · `phonenumber` · `website` · `groups_in[]` · `default_currency` · `default_language` · `address` · `city` · `state` · `zip` · `country` · `billing_street` · `billing_city` · `billing_state` · `billing_zip` · `billing_country` · `shipping_street` · `shipping_city` · `shipping_state` · `shipping_zip` · `shipping_country` · `name` · `show_primary_contact`

Nút: **Save and create contact** · **Save** → hai nhánh kết thúc khác nhau, phải kiểm cả hai.

## Chi tiết khách hàng — 22 tab

Profile · Contacts · Notes · Statement · Invoices · Payments · Proposals · Credit Notes · Estimates · Subscriptions · Expenses · Contracts · Projects · Tasks · Tickets · Files · Vault · Reminders · Map · Customer Details · Billing & Shipping · Customer Admins

| Nhóm | Tab | Ghi chú |
|---|---|---|
| **Thuộc về `CUST`** | Profile · Contacts · Notes · Statement · Files · Vault · Reminders · Map · Customer Details · Billing & Shipping · Customer Admins | 11 tab — phạm vi recon của module này |
| **Tham chiếu module khác** | Invoices · Payments · Proposals · Credit Notes · Estimates · Subscriptions · Expenses · Contracts · Projects · Tasks · Tickets | 11 tab — chỉ kiểm **liên kết và bộ lọc theo khách hàng**, không recon lại nghiệp vụ ở đây |

## Danh sách liên hệ toàn cục

**Cột:** First Name · Last Name · Email · Company · Phone · Position · **Last Login** · Active

> Cột `Last Login` + `Active` chứng minh **liên hệ có tài khoản đăng nhập cổng khách hàng**. Cổng đó dùng URL khác, **ngoài phạm vi đợt này** (user chốt).

## CRUD & trạng thái

| | |
|---|---|
| CRUD | Đầy đủ + Import + Export + Bulk Actions |
| Status flow | Active / Inactive (cột `Active`) |
| Số tab | 2 (form) · 22 (chi tiết) |

## Risk: 🔴 Cao

- **Entity gốc** — 11 module khác tham chiếu tới. Sai ở đây lan ra toàn hệ thống
- Chứa dữ liệu cá nhân: email, điện thoại, địa chỉ, mã số thuế
- Module **lớn nhất hệ thống**: 30 trường × 22 tab
- `Vault` (kho bí mật của khách hàng) — cần soi kỹ quyền truy cập

## Vùng chưa xác minh

| Việc | Lý do |
|---|---|
| Nội dung 21/22 tab chi tiết | Tầng khám phá chỉ đếm tab, không mở từng tab |
| Validation từng trường · thông báo lỗi nguyên văn | Không bấm Save (kỷ luật chỉ-đọc, môi trường dùng chung) |
| Wizard `/admin/clients/import` | Chưa mở — tránh tạo dữ liệu |
| Số option thật của `groups_in[]` · `default_currency` · `country` | Cần đọc DOM ở tầng recon module |
| Nhóm khách hàng (`/admin/clients/groups`) | 🚫 `access_denied` |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`customers_list_viewport.png`](../evidence/customers_list_viewport.png) | Danh sách khách hàng | Mặc định, đủ thanh công cụ + hàng tiêu đề + cỡ trang | Viewport — chụp full-page sẽ kéo theo 25 hàng dữ liệu khách hàng không liên quan tới việc chứng minh module tồn tại |
