# Module 15 — Cổng khách hàng

> ← [Bản đồ hệ thống](../system_map.md) · Prefix: `PORTAL` · Trạng thái recon: xem [`../../README.md`](../../README.md)
> Khảo sát 03-10-2026 · mode UI · **chưa đăng nhập** · chỉ đọc. Tầng khám phá — **không** chứa mã REQ.

---

## `PORTAL` — Cổng khách hàng (sau đăng nhập)

> ⛔ **BLOCKED** — chưa có tài khoản contact (`AMB-SYS-03`). Phần **đăng nhập** của cổng thuộc `LOGIN` (module 01).

| Mục | Ghi nhận |
|---|---|
| Route quan sát được | `/` → chuyển `/authentication/login` · `/knowledge-base` (công khai) · `/authentication/register` → chuyển về login (đăng ký **tắt**) |
| Header (chưa đăng nhập) | Logo · `Knowledge Base` · `Login` · chân trang "2026 Copyright Perfex CRM \| Anh Tester Demo" |
| Bằng chứng cổng đang được dùng | Thông báo phía admin "New comment from customer on contract …" (nhiều lần) → khách hàng có đăng nhập và bình luận hợp đồng |
| Nguồn | **UI thực tế** cho phần trước đăng nhập · phần sau đăng nhập: chưa có nguồn |
| Ước REQ | ❔ — chưa quan sát được |
| Risk | 🔴 — giao diện cho **người ngoài** · khách hàng xem/chấp nhận Proposal & Estimate, thanh toán Invoice, ký Contract, gửi Ticket · rủi ro lộ dữ liệu giữa các khách hàng |

**Cần để mở khoá:** 1 tài khoản contact (tốt nhất là 2 contact thuộc 2 customer khác nhau, để kiểm cách ly dữ liệu giữa khách hàng) → bổ sung `.env` → chạy `/discover-system` mode ADD cho `PORTAL`.

---

## Evidence

| Ảnh | Màn hình | Trạng thái | Phạm vi |
|---|---|---|---|
| [portal_login_viewport.png](../evidence/portal_login_viewport.png) | Cổng khách hàng — Login | Chưa đăng nhập (dùng chung với `LOGIN`) | Viewport |
