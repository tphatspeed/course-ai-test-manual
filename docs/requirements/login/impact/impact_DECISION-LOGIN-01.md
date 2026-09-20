# Impact Report — `DECISION-LOGIN-01` · 20-09-2026

← Tài liệu module: [`../requirements_login.md`](../requirements_login.md)

| Mục | Giá trị |
|---|---|
| **Nguồn** | Quyết định của người dùng dự án *(không phải ticket Jira)* — nội dung nguyên văn: *"AMB-LOGIN-01: chỉ có role admin, chưa áp dụng cho role khác. Các AMB còn lại hãy áp dụng assumption tạm"* |
| **Module** | `LOGIN` · nền tảng Web |
| **Loại thay đổi** | Đóng toàn bộ ambiguity bằng **giả định tạm** + thu hẹp phạm vi vai trò |
| **REQ trước → sau** | 58 → **62** |
| **Mã đã cấp thêm** | `REQ-LOGIN-59` → `REQ-LOGIN-62` |
| **Mã kế tiếp** | `REQ-LOGIN-63` · `AMB-LOGIN-18` · `RISK-LOGIN-06` |

---

## 1. Tóm tắt

| Nhóm | Số lượng | REQ |
|---|---|---|
| 🟢 **Thêm mới** | 4 | `REQ-LOGIN-59` · `60` · `61` · `62` |
| 🟡 **Sửa** | 9 | `REQ-LOGIN-13` · `14` · `17` · `18` · `26` · `27` · `28` · `42` · `54` |
| 🔴 **Bỏ** | 0 | — |
| ⏸️ **Không tác động** | 49 | Các REQ còn lại giữ nguyên |

**Ambiguity:** 17/17 chuyển `❓ Chờ trả lời` → `⏭️ Bỏ qua`.
**Risk:** không đổi — `RISK-LOGIN-01` → `05` vẫn nguyên hiệu lực.

---

## 2. Test case cần xử lý

> ⚠️ **Chưa có test case nào cho module `LOGIN`** — thư mục `docs/testcases/login/` chưa tồn tại tại thời điểm lập báo cáo này. Toàn bộ tác động dưới đây là **hướng dẫn cho lượt sinh TC đầu tiên**, không phải danh sách TC phải sửa.

| TC ID | REQ liên quan | Hành động | Lý do |
|---|---|---|---|
| — | `REQ-LOGIN-59` · `60` · `61` · `62` | ➕ **Viết mới, gắn nhãn `assumption-based`** | 4 REQ mới, cả 4 đều ⚪ dựa trên giả định chưa kiểm chứng |
| — | `REQ-LOGIN-13` · `28` · `54` | ➕ **Viết mới, gắn nhãn `assumption-based`** | Trước chỉ ghi "chưa kiểm chứng", nay đã có kỳ vọng cụ thể nên viết được TC |
| — | `REQ-LOGIN-14` · `17` · `18` · `26` · `27` · `42` | ➕ Viết mới, **kỳ vọng = khẳng định hiện trạng** | 6 hành vi được chốt là cố ý / khiếm khuyết chấp nhận được → TC khẳng định hệ thống làm đúng như vậy, **không** viết TC kỳ vọng hệ thống làm khác |
| — | 49 REQ còn lại | ➕ Viết mới bình thường | Không bị đợt cập nhật này tác động |

**Khi nào bảng này thành "sửa TC":** sau khi chạy `/generate-testcases-manual-rbt`, nếu PO trả lời một AMB khác với giả định đang áp → chạy lại `/update-requirements-from-ticket` rồi `/update-testcases-from-impact`; khi đó cột `TC ID` sẽ có dữ liệu thật.

---

## 3. Ambiguity — chuyển trạng thái

| Mã | Mức | Chuyển trạng thái | Kết quả |
|---|---|---|---|
| `AMB-LOGIN-01` | 🔴 | ❓ → ⏭️ | **Thu hẹp phạm vi** — đợt này chỉ vai trò quản trị. Ma trận phân quyền **giữ nguyên 22 ô `❔`** |
| `AMB-LOGIN-02` | 🔴 | ❓ → ⏭️ | Sinh `REQ-LOGIN-59` ⚪ *(không khoá tài khoản)* |
| `AMB-LOGIN-03` | 🔴 | ❓ → ⏭️ | Cập nhật `REQ-LOGIN-13` |
| `AMB-LOGIN-04` | 🟡 | ❓ → ⏭️ | Không sinh REQ. **Vẫn cấm assert mã `307`** |
| `AMB-LOGIN-05` | 🔴 | ❓ → ⏭️ | Sinh `REQ-LOGIN-60` ⚪ *(đổi mật khẩu khi bỏ trống ô xác nhận)* |
| `AMB-LOGIN-06` | 🟡 | ❓ → ⏭️ | `REQ-LOGIN-17` → 🟡 |
| `AMB-LOGIN-07` | 🟡 | ❓ → ⏭️ | `REQ-LOGIN-18` → 🟡 |
| `AMB-LOGIN-08` | 🟡 | ❓ → ⏭️ | `REQ-LOGIN-26` → 🟡 |
| `AMB-LOGIN-09` | 🟡 | ❓ → ⏭️ | `REQ-LOGIN-27` → 🟡 |
| `AMB-LOGIN-10` | 🔴 | ❓ → ⏭️ | Sinh `REQ-LOGIN-61` ⚪ — ⛔ **ngưỡng vẫn chưa có** |
| `AMB-LOGIN-11` | 🟡 | ❓ → ⏭️ | `REQ-LOGIN-14` → 🟡 |
| `AMB-LOGIN-12` | 🟡 | ❓ → ⏭️ | Không sinh REQ. Chấp nhận không biết cờ `Secure`/`SameSite` |
| `AMB-LOGIN-13` | 🔴 | ❓ → ⏭️ | Không sinh REQ — hành vi không quan sát được từ giao diện |
| `AMB-LOGIN-14` | 🟡 | ❓ → ⏭️ | Cập nhật `REQ-LOGIN-28` |
| `AMB-LOGIN-15` | 🟡 | ❓ → ⏭️ | Cập nhật `REQ-LOGIN-54`. Luồng Google Authenticator vẫn trắng |
| `AMB-LOGIN-16` | 🟡 | ❓ → ⏭️ | `REQ-LOGIN-42` → 🟡 |
| `AMB-LOGIN-17` | 🟡 | ❓ → ⏭️ | Sinh `REQ-LOGIN-62` ⚪ *(máy chủ kiểm lại trường bắt buộc)* |

**Ambiguity mới phát sinh:** không có. Dải `AMB-LOGIN` vẫn dừng ở `17`.

---

## 4. Cảnh báo

### 4.1. `⏭️ Bỏ qua` không phải `✅ Đã trả lời`

Toàn bộ 17 AMB đóng ở trạng thái **⏭️ Bỏ qua** — theo skill mục 6.7 nghĩa là *"test theo Assumption tạm và **chấp nhận rủi ro**"*. Không câu hỏi nào được giải đáp. Bảng AMB **không được xoá** — nó là danh sách phải mở lại khi có PO.

### 4.2. Bốn giả định thuộc về bảo mật

| AMB | Giả định | Hậu quả nếu sai |
|---|---|---|
| `AMB-LOGIN-02` | Không có cơ chế khoá tài khoản | Bỏ sót nhánh khoá tài khoản, **hoặc** hệ thống thật sự không chống được dò mật khẩu |
| `AMB-LOGIN-05` | Đổi mật khẩu được dù bỏ trống ô xác nhận | Người dùng gõ nhầm tự khoá mình, không test nào bắt được |
| `AMB-LOGIN-10` | Máy chủ ràng buộc độ dài tối thiểu | Đặt được mật khẩu 1 ký tự mà bộ kiểm thử vẫn xanh |
| `AMB-LOGIN-13` | Có giới hạn tần suất ở tầng hạ tầng | Cổng đăng nhập bị dò không giới hạn |

`REQ-LOGIN-59` và `REQ-LOGIN-60` **mô tả hành vi giả định, không xác nhận hành vi đó đúng đắn**. Nếu kiểm chứng sau này cho thấy giả định đúng → đó là **khiếm khuyết cần báo cáo**, không phải yêu cầu đã đạt.

### 4.3. Hai vùng vẫn không viết được test case đầy đủ

| Vùng | Vì sao | Cần gì để gỡ |
|---|---|---|
| Giá trị biên của độ dài mật khẩu *(`REQ-LOGIN-61`)* | Giả định nói *có* ràng buộc, không nói *bao nhiêu ký tự* | PO chốt con số |
| Luồng `Google Authenticator` *(`REQ-LOGIN-54`)* | Giả định chỉ phủ phương thức `email` | PO mô tả luồng, hoặc tài khoản riêng để thử |

### 4.4. Ma trận phân quyền vẫn chỉ tin được 9/33 ô

Quyết định *"chưa áp dụng cho vai trò khác"* thu hẹp **khối lượng công việc**, không biến `❔` thành dữ kiện. 22 ô vẫn là "chưa biết". Khi có tài khoản vai trò khác: điền cột đó, **không** phải sửa REQ nào.

### 4.5. Rủi ro thao tác không được gỡ bởi quyết định này

`RISK-LOGIN-02` *(không thử sai được)* · `RISK-LOGIN-03` *(đổi mật khẩu / 2FA một chiều)* · `RISK-LOGIN-04` *(dữ liệu hồ sơ dùng chung)* vẫn nguyên hiệu lực. **Đóng ambiguity không làm môi trường an toàn hơn** — vẫn cần tài khoản dùng riêng để kiểm chứng 7 REQ ⚪ nhóm giả định.

---

## 5. Bước tiếp theo

| Thứ tự | Việc | Command |
|---|---|---|
| 1 | Sinh test case lần đầu cho module *(chưa có TC nào)* | `/generate-testcases-manual-rbt` |
| 2 | Dựng ma trận truy vết sau khi có TC | `/generate-traceability-matrix` |
| — | *(Sau này)* khi PO trả lời thật một AMB → cập nhật requirements rồi đồng bộ TC | `/update-requirements-from-ticket` → `/update-testcases-from-impact` |

> `/update-testcases-from-impact` **chưa chạy được ở thời điểm này** vì chưa có bộ test case nào để cập nhật. Báo cáo này phục vụ lượt sinh TC đầu tiên.
