# Impact Report — PO-REPLY-LOGIN-20261003-03 · 03-10-2026

> ← [Index module `REQUIREMENTS_LOGIN_SUMMARY.md`](../REQUIREMENTS_LOGIN_SUMMARY.md) · File nền tảng: [`web/requirements_login_web.md`](../web/requirements_login_web.md) · Đợt trước: [`-02`](impact_PO-REPLY-LOGIN-20261003-02.md) · [`gốc`](impact_PO-REPLY-LOGIN-20261003.md)
> Input của `/update-testcases-from-impact`. Với REQ-LOGIN-36 và 55, chỉ dẫn ở file này **thay** chỉ dẫn của đợt `-02`.

| Mục | Giá trị |
|---|---|
| **Nguồn thay đổi** | PO/user trả lời AMB-LOGIN-30, 31 trong chat ngày 03-10-2026 — mã `PO-REPLY-LOGIN-20261003-03` do workflow đặt |
| **Nguyên văn câu trả lời** | AMB-LOGIN-30: *"dịch cho tôi từ tiếng việt sang tiếng anh và cập nhật vào"* · AMB-LOGIN-31: *"có hãy chỉnh lại"* |
| **Câu chữ do agent soạn** | PO giao agent chỉnh câu (AMB-31) rồi dịch (AMB-30). Đã chỉnh trước, dịch sau để hai bản khớp nghĩa:<br>• VI: "Nếu địa chỉ email này có trong hệ thống, chúng tôi đã gửi hướng dẫn đặt lại mật khẩu."<br>• EN: "If this email address exists in our system, we have sent password reset instructions." |
| **Bộ test case** | ⚠️ **Chưa có** — không có TC nào bị stale |

---

## Tóm tắt

| Nhóm | Số lượng | REQ |
|---|---|---|
| 🟢 Thêm mới | 0 | — |
| 🟡 Sửa | 2 | REQ-LOGIN-36, REQ-LOGIN-55 (vẫn 🟡 — cập nhật nội dung + ngày) |
| 🔴 Bỏ | 0 | — |
| ⏸️ Trùng, không tác động | 0 | — |

**Sau cập nhật:** 72 REQ = 🟢 58 · 🟡 8 · 🔴 0 · ⚪ 6 (không đổi) · AMB 31 — **treo 0** · RISK 7.

| REQ | Đợt 3 (`-02`) | Đợt 4 (đợt này) |
|---|---|---|
| REQ-LOGIN-36 | Câu "Vui lòng nhập địa chỉ email hoặc mật khẩu chính xác" — chỉ có tiếng Việt, khu quản trị không kiểm được nguyên văn | Kiểm bằng **tiếng Anh**: "If this email address exists in our system, we have sent password reset instructions." |
| REQ-LOGIN-55 | Câu cũ, chỉ kiểm tiếng Việt | Kiểm **cả hai** ngôn ngữ — câu mới EN + VI |

---

## Test case cần xử lý

| TC ID | REQ liên quan | Hành động | Lý do |
|---|---|---|---|
| — | REQ-LOGIN-36 | ➕ Viết mới — gắn nhãn lỗi đã biết | Ca (a): giao diện tiếng Anh, assert thông báo **chứa** câu EN mới. Ca (b): vẫn không chạy (`RISK-LOGIN-03`) |
| — | REQ-LOGIN-55 | ➕ Viết mới — gắn nhãn lỗi đã biết | Ca (a): assert câu EN, đổi sang Vietnamese rồi assert câu VI. Ca (b): skip, chờ `AMB-SYS-03` |
| — | Mọi REQ khác | — | Không đổi so với các đợt trước |

⚠️ **Cấm** dùng câu cũ "Vui lòng nhập địa chỉ email hoặc mật khẩu chính xác" trong TC — câu này đã bị thay.

---

## Ambiguity

| Mã | Chuyển trạng thái | Ghi chú |
|---|---|---|
| AMB-LOGIN-30 | ❓ → ✅ | Có bản tiếng Anh → REQ-LOGIN-36 kiểm bằng tiếng Anh; bản tiếng Việt kiểm ở cổng |
| AMB-LOGIN-31 | ❓ → ✅ | ⚠️ Khác giả định (giả định: giữ nguyên văn) — câu chữ đã chỉnh |

---

## Cảnh báo

- ⚠️ Câu chữ EN/VI do **agent soạn** theo yêu cầu PO — nên được PO/Dev duyệt trước khi Dev đưa vào file ngôn ngữ của hệ thống. Nếu Dev dùng câu khác, chạy lại workflow này để cập nhật nguyên văn.
- ⚠️ Vế *"hai ca giống hệt nhau"* của REQ-LOGIN-36/55 vẫn **chưa có ca nào chạy được** (ca email tồn tại bị chặn) — không đổi so với đợt trước.
- ✅ Module `LOGIN` **không còn AMB nào treo**.
