# Module 12 — Nhân sự · Phân quyền · Cấu hình hệ thống

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Nhân sự | `STAFF` | Web | Staff · Employees |
| Vai trò & Phân quyền | `ROLE` | Web | Roles |
| Cấu hình hệ thống | `SETTING` | Web | Settings · Setup |

## 🚫 Cả ba module đều bị chặn quyền

Account khảo sát **không có quyền vào khu Setup**. Bằng chứng: thanh đầu trang **không có biểu tượng Setup**, và `#setup-menu-wrapper` tồn tại trong DOM nhưng danh sách bên trong **rỗng** (chỉ có tiêu đề "Setup" + nút đóng).

### Bảng đo route — 20-09-2026

Ba kiểu phản hồi phân biệt được rõ ràng, nên `access_denied` là **bằng chứng route tồn tại**, không phải phỏng đoán:

| Route | Phản hồi | Module |
|---|---|---|
| `/admin/staff` | → `/admin/access_denied` | `STAFF` |
| `/admin/roles` | → `/admin/access_denied` | `ROLE` |
| `/admin/settings` | → `/admin/access_denied` | `SETTING` |
| `/admin/departments` | → `/admin/access_denied` | `SETTING` |
| `/admin/taxes` | → `/admin/access_denied` | `SETTING` |
| `/admin/custom_fields` | → `/admin/access_denied` | `SETTING` |
| `/admin/emails` | → `/admin/access_denied` | `SETTING` |
| `/admin/clients/groups` | → `/admin/access_denied` | `SETTING` |
| `/admin/newsfeed` | **404** | — không tồn tại tên đó |
| `/admin/activity_log` | **404** | — tên đúng là `/admin/utilities/activity_log` |
| `/admin/contracts/contract_types` | **404** | ❔ master data loại hợp đồng ở đường dẫn khác, chưa tìm ra |

> ✅ Phép thử chỉ dùng **URL chỉ đọc**. Không thử bất kỳ URL sửa/xoá nào.

---

## `STAFF` — Nhân sự

Quản lý tài khoản nhân viên. Bằng chứng gián tiếp thấy được từ ngoài:

- `/admin/profile/2` — liên kết tới hồ sơ nhân viên `Admin Example`, id `2`
- `body.className` chứa `user-id-2` → người dùng hiện tại là nhân viên id 2
- Cột `Assigned to` (Tasks) · `Assigned` (Leads, Estimate Request) · `Members` (Projects) đều trỏ tới nhân viên

**Risk 🔴 Cao** — tạo/khoá tài khoản, gán vai trò. Là cửa sau nguy hiểm nhất nếu phân quyền sai.

## `ROLE` — Vai trò & Phân quyền

Định nghĩa vai trò và ma trận quyền. **Đây là module chặn tiến độ**: không đọc được nó thì ma trận phân quyền của **mọi** module khác chỉ ghi được ở mức `❔`.

**Risk 🔴 Cao** — quyết định ai thấy gì trong toàn hệ thống.

## `SETTING` — Cấu hình hệ thống

Gom mọi màn hình danh mục / cấu hình thành **một module thay vì 8+ prefix lẻ**. Các vùng đã chứng minh tồn tại qua `access_denied`:

| Vùng | Route đã đo | Ảnh hưởng tới module nào |
|---|---|---|
| Cấu hình chung | `/admin/settings` | Toàn hệ thống |
| Phòng ban | `/admin/departments` | `TIC` (cột Department) |
| Thuế suất | `/admin/taxes` | `ITEM` (Tax 1 · Tax 2) · `INV` · `EST` · `PROP` |
| Trường tuỳ chỉnh | `/admin/custom_fields` | Mọi module có form |
| Mẫu thư | `/admin/emails` | `INV` · `EST` · `PROP` · `TIC` · `SUB` |
| Nhóm khách hàng | `/admin/clients/groups` | `CUST` (cột Groups) |

Các vùng **nghi có nhưng chưa đo**, không đưa vào danh mục: tiền tệ · phương thức thanh toán · danh mục chi phí · trạng thái & độ ưu tiên phiếu hỗ trợ · dịch vụ hỗ trợ · trả lời soạn sẵn · trạng thái & nguồn lead · loại hợp đồng.

**Risk 🔴 Cao** — là master data cấp nguồn cho gần như mọi module nghiệp vụ.

---

## ⚠️ Phát hiện chất lượng — trang từ chối truy cập

Trang `/admin/access_denied` hiển thị nguyên văn:

> **"Something went wrong. Try again"**

Đây **không** phải thông báo thiếu quyền. Người dùng bị từ chối sẽ hiểu nhầm là hệ thống lỗi và thử lại mãi. Ứng viên `AMB-SYS` 🔴 — xem [system_map.md](../system_map.md) mục 7.

## Vùng chưa xác minh

**Toàn bộ ba module.** Cần account có quyền Setup. Trước khi có, mọi ô ma trận phân quyền ở các module khác ghi `❔`, **không** được làm tròn thành `❌`.

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`access_denied_setup_area_viewport.png`](../evidence/access_denied_setup_area_viewport.png) | Trang từ chối truy cập | Sau khi mở một route khu Setup — thấy nguyên văn *"Something went wrong. Try again"* và thanh bên **không có mục Setup** | Viewport — **đã mở lại ảnh xác nhận** đúng nội dung thông báo |
