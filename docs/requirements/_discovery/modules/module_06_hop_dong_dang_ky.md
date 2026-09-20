# Module 06 — Hợp đồng · Đăng ký định kỳ

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Hợp đồng | `CTR` | Web | Contracts |
| Đăng ký định kỳ | `SUB` | Web | Subscriptions |

> Chung file vì cả hai là **cam kết dài hạn với khách hàng**, đều sinh doanh thu lặp lại và đều gắn vào một khách hàng + một dự án.

---

## `CTR` — Hợp đồng

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/contracts` | Bảng |
| Thêm/Sửa | `/admin/contracts/contract[/<id>]` | Form |
| Chi tiết | `/admin/contracts/contract/<id>` | Trang |

**Cột:** `#` · Subject · Customer · **Contract Type** · Contract Value · Start Date · End Date · Project · **Signature**

**Thanh công cụ:** New Contract · Export

**Quy mô:** ≥ 25 bản ghi (kín một trang đầu tiên).

### Hai điểm đáng chú ý

| Điểm | Ghi nhận |
|---|---|
| **Chữ ký điện tử** | Cột `Signature` → hợp đồng có luồng khách hàng ký từ cổng ngoài |
| **Bình luận từ khách** | Thanh thông báo chứa nguyên văn *"New comment from customer on contract TC022 Contract 1779369541"*, liên kết tới `/admin/contracts/contract/412` → khách hàng bình luận được vào hợp đồng từ cổng ngoài. **Cơ chế này đã chạy thật**, không phải suy diễn |
| **Loại hợp đồng** | `/admin/contracts/contract_types` trả **404** — master data loại hợp đồng nằm ở đường dẫn khác, ❔ chưa tìm ra. Thuộc `SETTING` |

**Risk 🟡 Trung bình** — có giá trị tiền và chữ ký, nhưng không có luồng trạng thái nhiều bước như hoá đơn. Nâng lên 🔴 nếu phần ký điện tử vào phạm vi kiểm thử.

---

## `SUB` — Đăng ký định kỳ

| Màn hình | Route | Loại |
|---|---|---|
| Danh sách | `/admin/subscriptions` | Bảng |
| Thêm mới | `/admin/subscriptions/create` | Form |

**Cột:** `#` · Subscription Name · Customer · Project · **Status** · **Next Billing Cycle** · Date Subscribed · **Last Sent**

**Thanh công cụ:** New Subscription · Export

**Quy mô:** 1 bản ghi.

> `Next Billing Cycle` + `Last Sent` → có **tác vụ chạy nền theo lịch** tự phát hành hoá đơn và gửi thư. Đây là vùng khó kiểm nhất: kết quả chỉ xuất hiện sau khi tác vụ nền chạy, không bấm nút nào ra được.

**Risk 🔴 Cao** — tiền định kỳ + tác vụ nền + gửi thư tự động. Sai ở đây thì khách bị tính tiền sai mà không ai bấm nút nào.

---

## Vùng chưa xác minh — cả 2 module

| Việc | Lý do |
|---|---|
| Danh sách trạng thái đầy đủ của đăng ký định kỳ | Chỉ có 1 bản ghi |
| Các giá trị `Contract Type` | Master data ở `SETTING` — route cũ trả 404, chưa tìm ra đường đi |
| Luồng ký điện tử | Diễn ra ở **cổng khách hàng** — URL khác, ngoài phạm vi đợt này |
| Luồng khách bình luận vào hợp đồng | Cùng lý do — chỉ thấy được phía nhận thông báo |
| Tác vụ nền phát hành hoá đơn định kỳ | Không kích hoạt được từ giao diện quản trị |
| Form thêm hợp đồng / đăng ký | Chưa mở — kỷ luật chỉ-đọc |

## Evidence

| Tệp | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [`contracts_list_viewport.png`](../evidence/contracts_list_viewport.png) | Danh sách hợp đồng | Mặc định — thấy cột `Contract Type` và `Signature` | Viewport |
| [`subscriptions_list_viewport.png`](../evidence/subscriptions_list_viewport.png) | Danh sách đăng ký | Mặc định — thấy cột `Next Billing Cycle` và `Last Sent` | Viewport |
