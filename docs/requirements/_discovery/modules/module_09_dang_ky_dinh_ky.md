# Module 09 — Đăng ký định kỳ

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `SUB` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · tài khoản Staff · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `SUB` — Subscriptions

| Mục | Ghi nhận |
|---|---|
| Route | Danh sách `/admin/subscriptions` · Tạo mới `/admin/subscriptions/create` |
| Loại màn hình | Danh sách + Form |
| Thanh công cụ | `New Subscription` · bộ lọc · `Export` |
| Tích hợp | Logo **Stripe** cạnh tiêu đề "Subscriptions Summary" → thu tiền định kỳ qua Stripe |
| Summary (status flow) | **Not Subscribed · Active · Future · Past Due · Unpaid · Incomplete · Canceled · Incomplete Expired** — 8 trạng thái (theo mô hình Stripe) |
| Cột bảng | `#` · `Subscription Name` · `Customer` · `Project` · `Status` · `Next Billing Cycle` · `Date Subscribed` · `Last Sent` |
| Form tạo mới | 11 field hiển thị · 5 dấu bắt buộc |
| Số bản ghi Staff thấy | 0 |
| Ước REQ | ~25 |
| Risk | 🔴 — tiền định kỳ · phụ thuộc Stripe (QA **không** kiểm được tầng tích hợp — xem Năng lực kiểm thử ở README) · 8 trạng thái do bên ngoài điều khiển |

**Vùng chưa xác minh:** Stripe có được cấu hình trên môi trường này không · luồng khách hàng đăng ký qua cổng · sinh Invoice theo kỳ.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [sub_overview_viewport.png](../evidence/sub_overview_viewport.png) | Subscriptions — danh sách + Summary | Bảng rỗng | Viewport |
