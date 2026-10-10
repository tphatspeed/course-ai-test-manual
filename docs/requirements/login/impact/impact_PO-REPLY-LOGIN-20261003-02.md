# Impact Report — PO-REPLY-LOGIN-20261003-02 · 03-10-2026

> ← [Index module `REQUIREMENTS_LOGIN_SUMMARY.md`](../REQUIREMENTS_LOGIN_SUMMARY.md) · File nền tảng: [`web/requirements_login_web.md`](../web/requirements_login_web.md) · Đợt trước: [`impact_PO-REPLY-LOGIN-20261003.md`](impact_PO-REPLY-LOGIN-20261003.md)
> Input của `/update-testcases-from-impact`. Đọc **cùng** Impact Report đợt trước — đợt này sửa tiếp một số REQ đã đổi ở đợt đó.

| Mục | Giá trị |
|---|---|
| **Nguồn thay đổi** | PO/user trả lời AMB-LOGIN-26 → 29 trong chat ngày 03-10-2026 — không có ticket Jira; mã `PO-REPLY-LOGIN-20261003-02` do workflow đặt |
| **Nguyên văn câu trả lời** | AMB-LOGIN-26: *"yêu cầu user vui lòng nhập địa chỉ email hoặc mật khẩu chính xác"* · 27: *"không áp dụng cho cổng khách hàng"* · 28: *"trang 404"* · 29: *"giải thích thêm cái AMB này, tôi chưa hiểu để có thể trả lời"* |
| **Làm rõ qua câu hỏi bổ sung** | AMB-26: **một câu chung** cho mọi email (không chỉ cho email sai) · câu đưa ra là **nguyên văn tiếng Việt** · ca bỏ trống Email → **báo bắt buộc nhập**. AMB-29 (sau khi giải thích "tiêu đề tab"): **giống cổng — "Forgot Password?"** |
| **Module · Nền tảng** | `LOGIN` · Web |
| **Mốc git trước khi sửa** | `ae09f57` (tài liệu `docs/` chưa được commit) |
| **Bộ test case** | ⚠️ **Chưa có** — `docs/testcases/` không tồn tại. Không có TC nào bị stale |

---

## Tóm tắt

| Nhóm | Số lượng | REQ |
|---|---|---|
| 🟢 Thêm mới | 0 | — |
| 🟡 Sửa | 6 | REQ-LOGIN-34, 36, 53, 55, 64, 69 |
| 🔴 Bỏ | 0 | — |
| ⏸️ Trùng, không tác động | 1 | REQ-LOGIN-70 — chỉ biên tập (AMB-LOGIN-27 trùng giả định) |

**Sau cập nhật:** 72 REQ = 🟢 58 · 🟡 8 · 🔴 0 · ⚪ 6 · AMB 31 (❓ 2 · ✅ 29) · RISK 7.

### Thay đổi so với đợt trước

| REQ | Đợt 2 (`PO-REPLY-LOGIN-20261003`) | Đợt 3 (đợt này) |
|---|---|---|
| REQ-LOGIN-36 · 55 | Không hiện "Email not found" — nguyên văn đúng chưa chốt | Email tồn tại hay không đều hiện **cùng** câu "Vui lòng nhập địa chỉ email hoặc mật khẩu chính xác" (giao diện tiếng Việt) |
| REQ-LOGIN-34 · 53 | Bỏ trống → "Email not found" (PO chấp nhận) | Bỏ trống → "The Email Address field is required." — **hiện vi phạm** |
| REQ-LOGIN-64 | Không trả `5xx` | Trả trang **404**; thêm ca tham số sai |
| REQ-LOGIN-69 | `document.title` không chứa "Login" | `document.title` = "Forgot Password?" |

---

## Test case cần xử lý

Chưa có TC nào → cột TC ID để `—`. Khi sinh TC lần đầu, áp đúng chỉ dẫn dưới đây (**thay** chỉ dẫn của đợt trước cho các REQ này).

| TC ID | REQ liên quan | Hành động | Lý do |
|---|---|---|---|
| — | REQ-LOGIN-34 · 53 | ➕ Viết mới — gắn nhãn lỗi đã biết | Assert "The Email Address field is required." — **cấm** assert "Email not found". Sẽ FAIL tới khi sửa |
| — | REQ-LOGIN-36 | ➕ Viết mới — ca (a) giới hạn | Khu quản trị chưa đổi được sang tiếng Việt → ở giao diện tiếng Anh chỉ assert **không** hiện "Email not found" (`AMB-LOGIN-30`). Ca (b) email tồn tại: **không** chạy (gửi email tới tài khoản dùng chung — `RISK-LOGIN-03`) |
| — | REQ-LOGIN-55 | ➕ Viết mới | Chọn Language = Vietnamese ở cổng → assert thông báo **chứa** "Vui lòng nhập địa chỉ email hoặc mật khẩu chính xác". Ca (b): skip, chờ tài khoản contact (`AMB-SYS-03`) |
| — | REQ-LOGIN-64 | ➕ Viết mới — gắn nhãn lỗi đã biết | Ca (a): assert mã `404`, không assert chữ trên trang 404. Ca (b): skip — chưa biết định dạng liên kết thật, **không** đoán |
| — | REQ-LOGIN-69 | ➕ Viết mới — gắn nhãn lỗi đã biết | Assert `document.title` = "Forgot Password?" |
| — | REQ-LOGIN-70 | — | Không đổi so với đợt trước; không viết TC autocomplete cho form cổng |

---

## Ambiguity

| Mã | Chuyển trạng thái | Ghi chú |
|---|---|---|
| AMB-LOGIN-26 | ❓ → ✅ | ⚠️ **Khác giả định** — một câu chung tiếng Việt cho mọi email; bỏ trống báo bắt buộc nhập → REQ-LOGIN-34, 36, 53, 55 🟡 |
| AMB-LOGIN-08 | ✅ (sửa kết luận) | Kết luận đợt 2 "chấp nhận Email not found khi bỏ trống" bị thay bởi AMB-LOGIN-26 |
| AMB-LOGIN-27 | ❓ → ✅ | Trùng giả định — không áp cho cổng → REQ-LOGIN-70 biên tập |
| AMB-LOGIN-28 | ❓ → ✅ | Trang 404 → REQ-LOGIN-64 🟡 |
| AMB-LOGIN-29 | ❓ → ✅ | "Forgot Password?" → REQ-LOGIN-69 🟡 |
| AMB-LOGIN-30 | (mới) ❓ 🟡 | Nguyên văn tiếng Anh của câu chung + cách kiểm bản tiếng Việt ở khu quản trị khi chưa đăng nhập |
| AMB-LOGIN-31 | (mới) ❓ 🟢 | Câu chung nhắc "mật khẩu" dù trang chỉ có ô Email, và hiện cả cho người nhập đúng email — có muốn chỉnh câu chữ? |

---

## Cảnh báo

- ⚠️ **8 REQ hiện vi phạm** (34, 36, 53, 55, 63, 64, 69, 70) — TC sẽ FAIL có chủ đích (`RISK-LOGIN-07`). Đợt này thêm REQ-LOGIN-34 và 53.
- ⚠️ **REQ-LOGIN-36 chưa kiểm được bằng nguyên văn** — chờ `AMB-LOGIN-30`. Trong lúc chờ, TC chỉ chứng minh được lỗi cũ đã hết, chưa chứng minh câu mới đúng.
- ⚠️ Vế *"hai ca giống hệt nhau"* của REQ-LOGIN-36/55 — cốt lõi của việc vá lỗi lộ email — **chưa có ca nào chạy được** trong môi trường hiện tại (ca email tồn tại bị chặn ở cả admin lẫn cổng). Cần hộp thư test / tài khoản contact, hoặc đội Dev xác minh.
