# Module 03 — Khách hàng

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `CUST` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `CUST` — Customers (gồm Contacts)

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/clients` · Tạo mới `/admin/clients/client` · Chi tiết `/admin/clients/client/{id}?group=<tab>` · Danh sách contact `/admin/clients/all_contacts` |
| Loại màn hình | Danh sách + Form + Chi tiết nhiều tab |
| Thanh công cụ | `New Customer` · `Import Customers` · `Contacts` · `Export` · `Bulk Actions` · bộ lọc (icon phễu) · page size 25 · ô Search · nút reload |
| Summary | Total Customers · Active Customers · Inactive Customers · Active Contacts · Inactive Contacts · Contacts Logged In Today |
| Cột bảng | `#` · `Company` · `Primary Contact` · `Primary Email` · `Phone` · `Active` (toggle) · `Groups` · `Date Created` |
| Số bản ghi Staff thấy | 2.129 (03-10-2026 — số động) |
| Form tạo mới | 2 tab: `Customer Details` · `Billing & Shipping` · 12 field hiển thị ở tab đầu · 2 dấu bắt buộc. Cấu hình hệ thống `company_is_required = 1` |
| Chi tiết — 19 tab | Profile · Contacts · Notes · Statement · Invoices · Payments · Proposals · Credit Notes · Estimates · Subscriptions · Expenses · Contracts · Projects · Tasks · Tickets · Files · Vault · Reminders · Map |
| Contacts | Cột: `First Name · Last Name · Email · Company · Phone · Position · Last Login · Active` |
| Status flow | Không có status flow — chỉ cờ `Active` (customer và contact) |
| Thông điệp trùng lặp (từ bộ ngôn ngữ của trang, chưa kích hoạt) | "Email already exists" · "Phone number already exists" · "Website already exists" · "Company already exists" |
| Ước REQ | ~60 |
| Risk | 🔴 — dữ liệu khách hàng (email, điện thoại, địa chỉ) · 9 module phụ thuộc · 19 tab · có Import và Bulk Actions (thao tác hàng loạt) · có Vault (nơi lưu thông tin nhạy cảm) |

**Ngoài quyền Staff:** Customer Groups (`/admin/clients/groups` → Access denied) — quản lý nhóm thuộc `SETTING`.

**Vùng chưa xác minh:** nội dung tab Billing & Shipping · rule kiểm trùng nào thực sự chạy (bộ ngôn ngữ chỉ cho biết thông điệp **tồn tại**) · định dạng file Import · quyền Delete · mẫu network `POST /admin/clients/table`.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [cust_overview_viewport.png](../evidence/cust_overview_viewport.png) | Customers — danh sách | Mặc định · thân bảng **đã làm mờ** | Viewport |
