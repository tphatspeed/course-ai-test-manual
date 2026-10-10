# Impact Report — PO-REPLY-LOGIN-20261003 · 03-10-2026

> ← [Index module `REQUIREMENTS_LOGIN_SUMMARY.md`](../REQUIREMENTS_LOGIN_SUMMARY.md) · File nền tảng: [`web/requirements_login_web.md`](../web/requirements_login_web.md)
> Input của `/update-testcases-from-impact`.

| Mục | Giá trị |
|---|---|
| **Nguồn thay đổi** | PO/user trả lời 25 AMB của module `LOGIN` trong chat ngày 03-10-2026 — không có ticket Jira; mã `PO-REPLY-LOGIN-20261003` do workflow đặt để truy vết |
| **Nguyên văn câu trả lời** | AMB-LOGIN-01: *"chỉ test tài khoản admin này"* · 02: *"như assumption"* · 03: *"như assumption"* · 04: *"không"* · 05: *"như giả định"* · 06: *"lỗi"* · *"các AMB còn lại như giả định"* (07 → 25) |
| **Module · Nền tảng** | `LOGIN` · Web |
| **Mốc git trước khi sửa** | `ae09f57` (tài liệu `docs/` lúc đó **chưa** được commit — so sánh bằng nội dung tài liệu, không bằng diff git) |
| **Bộ test case** | ⚠️ **Chưa có** — `docs/testcases/login/` không tồn tại. Không có TC nào bị stale; mọi hành động bên dưới là chỉ dẫn cho lần **sinh TC đầu tiên** |

---

## Tóm tắt

| Nhóm | Số lượng | REQ |
|---|---|---|
| 🟢 Thêm mới | 14 | REQ-LOGIN-59 → 72 (13 🟢 · 1 ⚪ — REQ-LOGIN-59) |
| 🟡 Sửa | 4 | REQ-LOGIN-10, REQ-LOGIN-23, REQ-LOGIN-36, REQ-LOGIN-55 |
| 🔴 Bỏ | 0 | — |
| ⚪ Chưa build | 0 | — *(REQ-LOGIN-59 là ⚪ "chưa kiểm chứng" theo quy ước đợt 1 — chờ tài khoản contact, không phải chưa build)* |
| ⏸️ Trùng, không tác động | 11 AMB | AMB-LOGIN-08, 09, 10, 12, 14, 18, 19, 22, 23, 25 chốt đúng hành vi đang ghi → REQ giữ nguyên · AMB-LOGIN-01 loại cột Admin khỏi phạm vi |
| ✏️ Biên tập (giữ 🟢) | 9 | REQ-LOGIN-18, 20, 26, 34, 39, 50, 53, 57, 58 — chỉ đổi tham chiếu AMB/REQ, hành vi không đổi |

**Sau cập nhật:** 72 REQ = 🟢 62 · 🟡 4 · 🔴 0 · ⚪ 6 · AMB 29 (❓ 4 · ✅ 25) · RISK 7.

### ⚠️ Lỗi đã xác nhận — TC sẽ FAIL có chủ đích

PO xác nhận 5 hành vi hiện tại là **lỗi** → 6 REQ ghi hành vi **đúng**, hệ thống hiện **vi phạm**. TC viết theo AC, **không** viết theo hành vi hiện tại (`RISK-LOGIN-07`):

| REQ | Lỗi | Hiện tại | Mong đợi | AMB gốc |
|---|---|---|---|---|
| REQ-LOGIN-36 · 55 | Quên mật khẩu lộ email tồn tại | "Email not found" | Không lộ — nguyên văn chờ `AMB-LOGIN-26` | AMB-LOGIN-06 |
| REQ-LOGIN-63 | Cookie `autologin` thiếu HttpOnly | `HttpOnly = false` | `HttpOnly = true` | AMB-LOGIN-05 |
| REQ-LOGIN-64 | Route đặt lại mật khẩu thiếu tham số | `HTTP 500` | Không `5xx` — chờ `AMB-LOGIN-28` | AMB-LOGIN-13 |
| REQ-LOGIN-69 | Tiêu đề tab trang Quên mật khẩu quản trị | chứa "Login" | không chứa "Login" — chờ `AMB-LOGIN-29` | AMB-LOGIN-20 |
| REQ-LOGIN-70 | Form Login thiếu `autocomplete` | không có thuộc tính | `current-password` cho Password · khác rỗng cho Email | AMB-LOGIN-21 |

→ Nên chạy `/create-bug-report` cho 5 lỗi này để TC có bug để link tới.

---

## Test case cần xử lý

Chưa có TC nào → cột TC ID để `—`. Khi sinh TC lần đầu, áp đúng hành động dưới đây.

| TC ID | REQ liên quan | Hành động | Lý do |
|---|---|---|---|
| — | REQ-LOGIN-10 | ➕ Viết mới — **2 ca** | Thêm ca (b): email tồn tại + sai mật khẩu → cùng "Invalid email or password". Ca (b) chỉ 1 lần sai trên tài khoản dùng chung |
| — | REQ-LOGIN-23 | ➕ Viết mới — **không** assert `HttpOnly` | Cờ HttpOnly tách sang REQ-LOGIN-63 |
| — | REQ-LOGIN-36 · 55 | ➕ Viết mới theo AC mới | ⚠️ **Cấm** assert "Email not found" — assert *không* hiện thông báo lộ tồn tại email. Sẽ FAIL tới khi sửa lỗi |
| — | REQ-LOGIN-59 | ➕ Viết mới, đánh dấu skip | ⚪ — chờ tài khoản contact (`AMB-SYS-03`) |
| — | REQ-LOGIN-60 | ➕ Viết mới | Chạy được ngay — người dùng tự nhập mật khẩu (`RISK-LOGIN-02`) |
| — | REQ-LOGIN-61 | ➕ Viết mới | Email không tồn tại, 10 lần — an toàn với tài khoản dùng chung |
| — | REQ-LOGIN-62 | ➕ Viết mới — có điều kiện | Tối đa 5 lần sai trên tài khoản dùng chung, đăng nhập đúng ngay sau đó, không chạy song song (`RISK-LOGIN-01`) |
| — | REQ-LOGIN-63 · 64 · 69 · 70 | ➕ Viết mới — gắn nhãn lỗi đã biết | Hiện vi phạm — FAIL có chủ đích (`RISK-LOGIN-07`) |
| — | REQ-LOGIN-65 · 66 | ➕ Viết mới | Đọc thuộc tính cookie, so trong bộ nhớ — không chép giá trị |
| — | REQ-LOGIN-67 | ➕ Viết mới — TC dài | Chờ 8 giờ không hoạt động; chạy tay/đêm, không giả lập bằng sửa hạn cookie |
| — | REQ-LOGIN-68 | ➕ Viết mới | Viewport mobile — ghi viewport đã đo vào kết quả |
| — | REQ-LOGIN-71 · 72 | ➕ Viết mới | Cần mật khẩu thật — người dùng tự nhập |
| — | REQ-LOGIN-18 · 20 · 26 · 34 · 39 · 50 · 53 · 57 · 58 | — | Chỉ biên tập tham chiếu, viết TC theo AC như cũ |

---

## Ambiguity

| Mã | Chuyển trạng thái | Ghi chú |
|---|---|---|
| AMB-LOGIN-01 | ❓ → ✅ | Không cấp tài khoản Admin — chỉ test tài khoản `EMAIL_ADMIN` (đo được là **Staff**). Cột Admin của ma trận → ngoài phạm vi |
| AMB-LOGIN-02 | ❓ → ✅ | Trùng giả định → REQ-LOGIN-59. Việc xin tài khoản contact chuyển về `AMB-SYS-03` |
| AMB-LOGIN-03 | ❓ → ✅ | Trùng giả định → REQ-LOGIN-60 |
| AMB-LOGIN-04 | ❓ → ✅ | Không có khoá/CAPTCHA/giới hạn → REQ-LOGIN-61, 62 |
| AMB-LOGIN-05 | ❓ → ✅ | Lỗi (trùng giả định) → REQ-LOGIN-63, REQ-LOGIN-23 🟡 |
| AMB-LOGIN-06 | ❓ → ✅ | ⚠️ **KHÁC giả định** — là lỗi → REQ-LOGIN-36, 55 🟡 |
| AMB-LOGIN-07 | ❓ → ✅ | Trùng giả định → REQ-LOGIN-10 🟡 |
| AMB-LOGIN-11 · 15 · 16 · 17 · 20 · 21 · 24 | ❓ → ✅ | Trùng giả định → sinh REQ-LOGIN-65 → 72 (xem bảng Tóm tắt) / biên tập REQ-18 |
| AMB-LOGIN-13 | ❓ → ✅ | Lỗi (trùng giả định) → REQ-LOGIN-64 |
| AMB-LOGIN-08 · 09 · 10 · 12 · 14 · 18 · 19 · 22 · 23 · 25 | ❓ → ✅ | Trùng giả định — không đổi REQ |
| AMB-LOGIN-26 | (mới) ❓ 🟡 | Nguyên văn thông báo đúng của Quên mật khẩu sau khi sửa AMB-LOGIN-06; có áp cho ca bỏ trống Email không |
| AMB-LOGIN-27 | (mới) ❓ 🟢 | `autocomplete` có áp cho form Login cổng không; giá trị cho ô Email |
| AMB-LOGIN-28 | (mới) ❓ 🟢 | Route đặt lại mật khẩu thiếu tham số: 404 hay chuyển về Login |
| AMB-LOGIN-29 | (mới) ❓ 🟢 | Nguyên văn tiêu đề tab đúng của trang Quên mật khẩu quản trị |

---

## Cảnh báo

- ⚠️ **AMB-LOGIN-06 khác giả định** — mọi TC/checklist/automation viết tay ngoài repo (nếu có) đang assert "Email not found" cho email không tồn tại đều **sai** từ nay.
- ⚠️ **Không có tài khoản riêng** (AMB-LOGIN-01) — REQ-LOGIN-62 (không khoá) buộc chạy trên tài khoản dùng chung. Dựa trên xác nhận *không có khoá* của PO; nếu thực tế có khoá, tài khoản của cả lớp bị chặn. Tuân thủ điều kiện ở `RISK-LOGIN-01`.
- ⚠️ **10 REQ nguồn `Tài liệu · PO` chưa kiểm chứng** (60, 61, 62, 65, 66, 67, 68, 71, 72 và ca (b) của REQ-10) — lượt chạy TC đầu tiên cũng là lượt kiểm chứng; lệch → mở AMB mới, không tự hạ AC.
- ⚠️ Câu trả lời cho AMB-LOGIN-01 có thể áp cả cho `AMB-SYS-01` (xin tài khoản Admin cấp hệ thống — chặn `STAFF`, `SETTING`). Workflow này **không** đóng `AMB-SYS-01` — cần user xác nhận riêng.
- ℹ️ File web có 72 REQ, cách ngưỡng tách file story (80) 8 REQ — đợt cập nhật sau cân nhắc tách `web/stories/`.
