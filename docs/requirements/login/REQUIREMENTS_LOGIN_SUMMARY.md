# Requirements — Đăng nhập (`LOGIN`)

> **INDEX module — tên file bất biến.** Điểm vào cho mọi workflow phía sau. Đọc file này trước, rồi theo [Bản đồ tài liệu](#bản-đồ-tài-liệu) sang file nền tảng.
> ← Danh mục hệ thống: [`../README.md`](../README.md) · Tầng khám phá: [`../_discovery/modules/module_01_dang_nhap_ho_so_ca_nhan.md`](../_discovery/modules/module_01_dang_nhap_ho_so_ca_nhan.md)

| Mục | Giá trị |
|---|---|
| **Hệ thống** | Perfex CRM — bản demo Anh Tester (`app.version = 316`) |
| **Module · Prefix** | Đăng nhập · `LOGIN` |
| **Nền tảng** | Web ✅ (khảo sát 03-10-2026) · Android/iOS/API: không có |
| **Dải mã đã dùng** | `REQ-LOGIN-01` → `REQ-LOGIN-72` · `AMB-LOGIN-01` → `AMB-LOGIN-31` · `RISK-LOGIN-01` → `RISK-LOGIN-07` · `STORY-LOGIN-01` → `STORY-LOGIN-07` (đợt 1: UI recon 03-10-2026 → REQ 01→58 · AMB 01→25 · RISK 01→06 · đợt 2: PO trả lời AMB `PO-REPLY-LOGIN-20261003` → REQ 59→72 · AMB 26→29 · RISK 07 · đợt 3: `PO-REPLY-LOGIN-20261003-02` → AMB 30→31, không cấp REQ mới · đợt 4: `PO-REPLY-LOGIN-20261003-03` — không cấp mã mới) |
| **Mã kế tiếp** | Đợt phân tích sau bắt đầu từ `REQ-LOGIN-73` · `AMB-LOGIN-32` · `RISK-LOGIN-08` · `STORY-LOGIN-08` — **KHÔNG đánh lại từ 01** |
| **Nguồn** | UI thực tế + tầng network (Playwright MCP) · PO/user trả lời AMB trong chat 03-10-2026 (`PO-REPLY-LOGIN-20261003` — AMB 01→25 · `PO-REPLY-LOGIN-20261003-02` — AMB 26→29 · `PO-REPLY-LOGIN-20261003-03` — AMB 30→31) |
| **Tài khoản đã dùng** | 1 tài khoản `EMAIL_ADMIN` (`.env`) — đo được là **Staff**, không phải Admin (`AMB-SYS-01`). PO chốt **chỉ test bằng tài khoản này** → cột Admin ngoài phạm vi (`AMB-LOGIN-01` ✅). Contact: chưa có (`AMB-SYS-03`) |
| **Trạng thái REQ** | 🟢 58 · 🟡 8 · 🔴 0 · ⚪ 6 — tổng 72 |

---

## 1. Tổng quan

Module **Đăng nhập** là cửa vào của toàn bộ hệ thống, gồm hai khu xác thực tách biệt:

- **Khu quản trị** (`/admin/authentication`) — nhân viên (Staff/Admin) đăng nhập, ghi nhớ đăng nhập, quên mật khẩu, đăng xuất.
- **Cổng khách hàng** (`/authentication/login`) — liên hệ (contact) của khách hàng đăng nhập, chọn ngôn ngữ, quên mật khẩu. Đăng ký tự do đang tắt.

**Trong phạm vi:** form Login + Quên mật khẩu của cả hai khu · validation phía server và trình duyệt · flash/toast thông báo · phiên đăng nhập, chuyển hướng · Remember me (tạo cookie, dùng được, gia hạn, bị vô hiệu khi logout) · đăng xuất (menu, popup timer, đổi session id, nút Back) · đổi ngôn ngữ trang Login cổng · trạng thái tắt của trang Register.

**Ngoài phạm vi:** đổi mật khẩu khi đã đăng nhập và xác thực 2 lớp (thuộc `PROFILE`) · cổng khách hàng sau khi đăng nhập (thuộc `PORTAL`) · cấu hình bảo mật đăng nhập trong Setup (thuộc `SETTING`, đang Access denied — `AMB-SYS-01`).

**Bản đồ phủ tài liệu:** REQ-LOGIN-01 → 58 sinh từ khảo sát UI thực tế (không có tài liệu). REQ-LOGIN-59 → 72 và phần sửa của REQ-LOGIN-10, 23, 36, 55 lấy từ câu trả lời của PO cho AMB — **chưa** kiểm chứng thực tế (chi tiết ở file nền tảng mục 1).

---

## Bản đồ tài liệu

| Nền tảng | File | Story | REQ bao phủ |
|---|---|---|---|
| Chung ≥ 2 nền tảng | chính file này | — | *(không có — module mới có một nền tảng)* |
| Web | [web/requirements_login_web.md](web/requirements_login_web.md) — mục 2 | STORY-LOGIN-01 → 07 | REQ-LOGIN-01 → 72 |
| Web — evidence | [web/evidence/](web/evidence/) | — | 17 ảnh — danh mục ở file web mục 6 |
| Impact Report | [impact/impact_PO-REPLY-LOGIN-20261003.md](impact/impact_PO-REPLY-LOGIN-20261003.md) · [impact/impact_PO-REPLY-LOGIN-20261003-02.md](impact/impact_PO-REPLY-LOGIN-20261003-02.md) · [impact/impact_PO-REPLY-LOGIN-20261003-03.md](impact/impact_PO-REPLY-LOGIN-20261003-03.md) | — | Đợt 2 · 3 · 4 — input của `/update-testcases-from-impact` |

---

## 2. Yêu cầu dùng chung nhiều nền tảng

Không có — module chỉ có nền tảng Web. Toàn bộ REQ nằm ở [file web](web/requirements_login_web.md).

---

## 3. Ma trận Phân quyền

> Ranh giới trong module này là **loại tài khoản × khu đăng nhập**. Phân quyền chức năng sau khi đăng nhập thuộc từng module nghiệp vụ.

| Hành động | Staff | Admin | Contact (khách hàng) | Không có tài khoản |
|---|---|---|---|---|
| Đăng nhập khu quản trị `/admin/authentication` | ✅ (REQ-LOGIN-17) | — ngoài phạm vi | ⚠️❌ (REQ-LOGIN-59) | ❌ "Invalid email or password" (REQ-LOGIN-10) |
| Đăng nhập cổng khách hàng `/authentication/login` | ⚠️❌ (REQ-LOGIN-60) | — ngoài phạm vi | ⚠️✅ (REQ-LOGIN-50) | ❌ "Invalid username or password" (REQ-LOGIN-44) |
| Đăng xuất khu quản trị | ✅ (REQ-LOGIN-27) | — ngoài phạm vi | — | — |

> Ký hiệu `⚠️` ở ma trận này nghĩa là **PO đã xác nhận qua câu trả lời AMB (03-10-2026), chưa đăng nhập thử** — cùng mức tin cậy với "suy từ cấu hình" (mục 3.1.2 của skill).

```
Tổng 12 ô = Đã kiểm chứng 4 · Suy diễn (PO xác nhận) 3 · Chưa rõ 0 · Không áp dụng 2 · Ngoài phạm vi 3
Ô "không áp dụng": [Đăng xuất × Không có tài khoản] — không có phiên để đăng xuất · [Đăng xuất × Contact] — contact không có phiên quản trị (REQ-LOGIN-59).
Ô "ngoài phạm vi": cột Admin — PO chốt chỉ test bằng tài khoản EMAIL_ADMIN (đo được là Staff) (AMB-LOGIN-01 ✅).
3 ô ⚠️ chờ kiểm chứng: Staff ở cổng (REQ-LOGIN-60 — chạy được ngay, người dùng tự nhập mật khẩu) · 2 ô Contact (chờ tài khoản — AMB-SYS-03).
```

---

## 4. Ma trận Trạng thái

Không áp dụng — module không có entity mang status flow. Trạng thái phiên (chưa đăng nhập → đã đăng nhập → đã đăng xuất) được mô tả bằng REQ ở STORY-LOGIN-02 và STORY-LOGIN-04.

---

## 5. Yêu cầu phi chức năng (quan sát được)

| Hạng mục | Ghi nhận | REQ / AMB |
|---|---|---|
| Bảo mật cookie | `sp_session` HttpOnly + Secure + SameSite=Lax · `autologin` Secure + SameSite=Lax nhưng **không** HttpOnly — PO xác nhận là lỗi | REQ-LOGIN-20 · REQ-LOGIN-23 · REQ-LOGIN-63 (hiện vi phạm) |
| Chống CSRF | Mọi form xác thực có `csrf_token_name` (32 ký tự) — token không đổi khi đăng xuất | REQ-LOGIN-14 · `AMB-LOGIN-19` |
| Bộ nhớ đệm | Trang xác thực trả `Cache-Control: no-store, no-cache, must-revalidate` · Back sau logout không lộ trang quản trị | REQ-LOGIN-30 |
| Trợ năng / tự điền | Ô Email/Password không có `autocomplete`; Chrome cảnh báo trong console `suggested: "current-password"` — PO xác nhận là lỗi | REQ-LOGIN-70 (hiện vi phạm) · `AMB-LOGIN-27` |
| Đa ngôn ngữ | Cổng khách hàng 26 ngôn ngữ, chọn ngay trên trang Login; khu quản trị không có lựa chọn trên trang Login | REQ-LOGIN-46 → 48 |
| Responsive | Chỉ khảo sát viewport `1600×750`; menu mobile chưa khảo sát — PO xác nhận hoạt động giống desktop | REQ-LOGIN-68 |
| Phiên đăng nhập | Cấp session id mới khi đăng nhập · hạn trượt 8 giờ không hoạt động | REQ-LOGIN-65 · 66 · 67 |
| Chống dò mật khẩu / tài khoản | Không khoá, không CAPTCHA (PO xác nhận) · thông báo sai thông tin không lộ email tồn tại · Quên mật khẩu phải hiện cùng một thông báo cho mọi email — hiện đang lộ email tồn tại (lỗi) | REQ-LOGIN-10 · 61 · 62 · 36 · 55 |

---

## 6. Điểm Mơ Hồ & Rủi Ro

### 6.1. Ambiguities

> Đợt 2 (03-10-2026): PO/user trả lời **cả 25 AMB** (`PO-REPLY-LOGIN-20261003`) — AMB-LOGIN-01 → 06 trả lời riêng, AMB-LOGIN-07 → 25 trả lời chung *"như giả định"*. Phát sinh 4 AMB mới (26 → 29) về nguyên văn hành vi đúng của các lỗi đã xác nhận.

| Mã | Câu hỏi | Nguy cơ | Mức độ | Assumption tạm | Trạng thái | Kết luận |
|---|---|---|---|---|---|---|
| AMB-LOGIN-01 | Xin tài khoản **Admin** thật để kiểm chứng 3 ô cột Admin của ma trận phân quyền (đăng nhập quản trị · cổng · đăng xuất) | Không biết Admin có luồng khác (VD 2FA bắt buộc, chuyển hướng khác) | 🔴 | Admin đăng nhập giống Staff | ✅ Đã trả lời 03-10-2026 | **Không cấp.** PO: *"chỉ test tài khoản admin này"* — tức tài khoản `EMAIL_ADMIN` (đo được là Staff). Cột Admin của ma trận → ngoài phạm vi. Hệ quả: không có tài khoản riêng cho ca rủi ro (`RISK-LOGIN-01`) |
| AMB-LOGIN-02 | Xin tài khoản **contact** để kiểm chứng 3 ô cột Contact và 5 REQ ⚪ (REQ-LOGIN-50, 57, 58 + Remember me cổng) | Cả luồng đăng nhập cổng khách hàng không có TC chạy được | 🔴 | Contact chỉ đăng nhập được cổng, không vào được khu quản trị | ✅ Đã trả lời 03-10-2026 | Trùng giả định → sinh **REQ-LOGIN-59** (contact không vào được khu quản trị). Việc xin tài khoản contact chuyển về `AMB-SYS-03` — REQ-LOGIN-50, 57, 58, 59 vẫn ⚪ |
| AMB-LOGIN-03 | Tài khoản **Staff** có đăng nhập được cổng khách hàng không? (bảng staff và bảng contact khác nhau) | Thiếu ca kiểm ranh giới hai khu xác thực | 🔴 | Không — trả "Invalid username or password" | ✅ Đã trả lời 03-10-2026 | Trùng giả định → sinh **REQ-LOGIN-60** |
| AMB-LOGIN-04 | Có **khoá tài khoản / CAPTCHA / giới hạn** sau N lần đăng nhập sai không? Ngưỡng và thời gian khoá? | Không có thì dễ bị dò mật khẩu; có thì TC sai mật khẩu trên tài khoản dùng chung sẽ khoá cả lớp | 🔴 | Không có cơ chế khoá (không thấy CAPTCHA, không thấy cấu hình) — **không được** thử trên tài khoản dùng chung | ✅ Đã trả lời 03-10-2026 | **Không có** khoá / CAPTCHA / giới hạn — trùng giả định → sinh **REQ-LOGIN-61** (không CAPTCHA) + **REQ-LOGIN-62** (không khoá). Ca chạy trên tài khoản dùng chung theo điều kiện `RISK-LOGIN-01` |
| AMB-LOGIN-05 | Cookie `autologin` **không có HttpOnly**: JavaScript đọc được `user_id` + `key` ghi nhớ. Cố ý hay lỗi? | Một lỗ XSS bất kỳ đủ đánh cắp phiên ghi nhớ 62 ngày | 🔴 | Nghi là lỗi bảo mật — chuyển đội Dev | ✅ Đã trả lời 03-10-2026 | **Lỗi** — trùng giả định. Tách cờ HttpOnly khỏi REQ-LOGIN-23 (🟡) thành **REQ-LOGIN-63** (`HttpOnly = true`, hiện vi phạm) |
| AMB-LOGIN-06 | Quên mật khẩu trả **"Email not found"** cho email không tồn tại (cả admin và cổng). Email tồn tại thì trả thông báo khác → lộ email nào có tài khoản. Cố ý? | Kẻ xấu dò được danh sách email nhân viên/khách hàng | 🔴 | Hành vi hiện tại là có chủ đích của sản phẩm; TC ghi nhận đúng như REQ-LOGIN-36/55 | ✅ Đã trả lời 03-10-2026 | ⚠️ **KHÁC giả định — là LỖI.** REQ-LOGIN-36, 55 → 🟡: thông báo không được lộ email tồn tại (hiện vi phạm). Nguyên văn thông báo đúng → `AMB-LOGIN-26` |
| AMB-LOGIN-07 | Email **tồn tại** + sai mật khẩu có trả đúng "Invalid email or password" như email không tồn tại không? | Nếu khác → lộ tồn tại tài khoản qua form Login | 🟡 | Giống nhau (thông báo chung) | ✅ Đã trả lời 03-10-2026 | Trùng giả định → REQ-LOGIN-10 🟡 bổ sung ca (b) |
| AMB-LOGIN-08 | Quên mật khẩu bỏ trống Email trả "Email not found" thay vì "The Email Address field is required." — cố ý? | Thông báo sai nghĩa với người dùng | 🟡 | Ghi nhận đúng hành vi hiện tại | ✅ Đã trả lời 03-10-2026 | Đợt 2: trùng giả định — chấp nhận hành vi hiện tại. ⚠️ **Đợt 3 thay kết luận** (qua `AMB-LOGIN-26`): bỏ trống phải báo "The Email Address field is required." → REQ-LOGIN-34/53 🟡 |
| AMB-LOGIN-09 | Sai thông tin: admin "Invalid email or password" (khối cố định) vs cổng "Invalid username or password" (toast tự ẩn 3,5 s; cổng không có khái niệm username) — cố ý? | Thông báo thiếu nhất quán; toast tự ẩn dễ bị bỏ lỡ | 🟡 | Ghi đúng từng nền tảng như hiện tại | ✅ Đã trả lời 03-10-2026 | Trùng giả định — chấp nhận khác biệt, không đổi REQ |
| AMB-LOGIN-10 | Ô Email admin `type=email` (trình duyệt chặn thiếu `@`) còn cổng `type=text` (server chặn) — cố ý? | Hai form cùng chức năng xử lý khác nhau | 🟡 | Ghi đúng từng form | ✅ Đã trả lời 03-10-2026 | Trùng giả định — không đổi REQ |
| AMB-LOGIN-11 | Sau đăng nhập hệ thống luôn vào Dashboard, **không** quay về trang người dùng định mở (deep link) — cố ý? | Người dùng mở link từ email/thông báo phải tự điều hướng lại | 🟡 | Hành vi hiện tại đúng thiết kế | ✅ Đã trả lời 03-10-2026 | Trùng giả định — đúng thiết kế. REQ-LOGIN-18 biên tập mô tả |
| AMB-LOGIN-12 | Logout **không xoá** cookie `autologin` khỏi trình duyệt (giá trị + hạn giữ nguyên), dù server đã vô hiệu nó — có cần xoá? | Cookie chết tồn tại 62 ngày; kết hợp `AMB-LOGIN-05` làm lộ `user_id` | 🟡 | Không ảnh hưởng chức năng (REQ-LOGIN-31) | ✅ Đã trả lời 03-10-2026 | Trùng giả định — không cần xoá, không đổi REQ |
| AMB-LOGIN-13 | `GET /admin/authentication/reset_password` không tham số trả **HTTP 500** thay vì 404/chuyển hướng | Lỗi máy chủ ở luồng người dùng chạm tới (link reset hỏng/cắt cụt) | 🟡 | Là lỗi — chuyển đội Dev | ✅ Đã trả lời 03-10-2026 | **Lỗi** — trùng giả định → sinh **REQ-LOGIN-64** (hiện vi phạm). Hành vi đúng → `AMB-LOGIN-28` |
| AMB-LOGIN-14 | Trang Quên mật khẩu **quản trị** không có lối quay lại Login (chỉ logo về trang chủ cổng); cổng thì có nút Login ở header — cố ý? | Người dùng bấm nhầm bị kẹt | 🟡 | Ghi nhận như hiện tại | ✅ Đã trả lời 03-10-2026 | Trùng giả định — không đổi REQ |
| AMB-LOGIN-15 | Đăng nhập có **cấp session id mới** (chống session fixation) không? Chưa đo được — công cụ không giữ được giá trị cookie giữa thao tác người dùng | Lỗ session fixation không được phát hiện | 🟡 | Có cấp mới (logout có cấp mới — REQ-LOGIN-29) | ✅ Đã trả lời 03-10-2026 | Trùng giả định → sinh **REQ-LOGIN-65** (chưa kiểm chứng) |
| AMB-LOGIN-16 | Phiên hết hạn sau bao lâu không hoạt động? Cookie `sp_session` có hạn = thời điểm phản hồi + 8 giờ — là hạn trượt hay tuyệt đối? | Không viết được TC hết phiên | 🟡 | Hạn trượt 8 giờ | ✅ Đã trả lời 03-10-2026 | Trùng giả định → sinh **REQ-LOGIN-66** (gia hạn) + **REQ-LOGIN-67** (hết hạn sau 8 giờ) |
| AMB-LOGIN-17 | Menu Logout bản mobile (`#mobile-collapse`) ẩn ở viewport `1600×750` — cần recon riêng ở viewport mobile | Không kết luận được lối đăng xuất trên màn hình nhỏ | 🟡 | Hoạt động giống desktop | ✅ Đã trả lời 03-10-2026 | Trùng giả định → sinh **REQ-LOGIN-68** (chưa kiểm chứng) |
| AMB-LOGIN-18 | Đăng xuất bằng **GET** `/admin/authentication/logout` → trang khác có thể ép người dùng đăng xuất (logout CSRF) — chấp nhận? | Phiền toái, mức thấp | 🟢 | Chấp nhận | ✅ Đã trả lời 03-10-2026 | Trùng giả định — chấp nhận, không đổi REQ |
| AMB-LOGIN-19 | CSRF token (`csrf_cookie_name`) **không đổi** khi đăng xuất | Token sống qua nhiều phiên | 🟢 | Chấp nhận (token hết hạn sau ~1 giờ) | ✅ Đã trả lời 03-10-2026 | Trùng giả định — chấp nhận, không đổi REQ |
| AMB-LOGIN-20 | Trang Quên mật khẩu quản trị có `document.title` "Perfex CRM \| Anh Tester Demo - Login" giống trang Login | Tab trình duyệt hiển thị sai tên trang | 🟢 | Lỗi hiển thị nhỏ | ✅ Đã trả lời 03-10-2026 | **Lỗi** — trùng giả định → sinh **REQ-LOGIN-69** (hiện vi phạm). Nguyên văn đúng → `AMB-LOGIN-29` |
| AMB-LOGIN-21 | Ô Email/Password thiếu `autocomplete` (Chrome gợi ý `current-password`) | Trình quản lý mật khẩu điền kém chính xác | 🟢 | Lỗi nhỏ | ✅ Đã trả lời 03-10-2026 | **Lỗi** — trùng giả định → sinh **REQ-LOGIN-70** (form Login quản trị, hiện vi phạm). Phạm vi cổng + giá trị cho Email → `AMB-LOGIN-27` |
| AMB-LOGIN-22 | Bỏ trống cả hai: admin hiện "Password" **trước** "Email" (ngược thứ tự ô), cổng hiện đúng thứ tự | Không nhất quán | 🟢 | TC không assert thứ tự | ✅ Đã trả lời 03-10-2026 | Trùng giả định — REQ-LOGIN-07 giữ nguyên |
| AMB-LOGIN-23 | Popup cảnh báo timer khi Logout (REQ-LOGIN-28) chưa kích hoạt thực tế — phải bật timer task (ghi timesheet lên môi trường dùng chung) | REQ mới dựa trên đọc mã | 🟢 | Popup hoạt động như mã | ✅ Đã trả lời 03-10-2026 | Trùng giả định — PO xác nhận là hành vi mong đợi. REQ-LOGIN-28 vẫn cần kiểm thực tế trên task do mình tạo |
| AMB-LOGIN-24 | Email đăng nhập có phân biệt hoa thường / có bỏ khoảng trắng đầu cuối không? | Thiếu ca biên | 🟢 | Không phân biệt hoa thường; khoảng trắng bị trình duyệt cắt với `type=email` | ✅ Đã trả lời 03-10-2026 | Trùng giả định → sinh **REQ-LOGIN-71** (hoa thường) + **REQ-LOGIN-72** (khoảng trắng) |
| AMB-LOGIN-25 | Chuyển hướng xác thực trả **307** (`GET /admin/ → 307`, logout → 307, đổi ngôn ngữ → 307) thay vì 302/303 thông thường — cấu hình máy chủ hay do công cụ đo? | Assert sai mã chuyển hướng gây FAIL giả | 🟢 | AC chỉ assert URL cuối, không assert mã | ✅ Đã trả lời 03-10-2026 | Trùng giả định — AC tiếp tục chỉ assert URL cuối |
| AMB-LOGIN-26 | Sau khi sửa lỗi AMB-LOGIN-06: thông báo của Quên mật khẩu (admin + cổng) khi email không tồn tại là gì — **nguyên văn**? Có dùng chung cho ca bỏ trống Email (REQ-LOGIN-34/53, hiện "Email not found") không? | TC của REQ-LOGIN-36/55 chỉ assert được "không lộ" — không assert được nội dung đúng | 🟡 | TC chỉ assert **không** hiện "Email not found" / không thông báo nào khẳng định email không tồn tại; REQ-LOGIN-34/53 giữ nguyên | ✅ Đã trả lời 03-10-2026 (đợt 3) | ⚠️ **Khác giả định ở cả hai vế.** (1) **Một câu chung** cho email tồn tại và không tồn tại, nguyên văn **tiếng Việt** "Vui lòng nhập địa chỉ email hoặc mật khẩu chính xác" → REQ-LOGIN-36/55 🟡. (2) Ca bỏ trống **không** dùng câu này mà báo "The Email Address field is required." → REQ-LOGIN-34/53 🟡. Tiếng Anh + câu chữ → `AMB-LOGIN-30`, `31`. ⚠️ Câu chữ thay ở `AMB-LOGIN-31` |
| AMB-LOGIN-27 | Yêu cầu `autocomplete` (REQ-LOGIN-70) có áp cho form Login **cổng khách hàng** không? Giá trị mong đợi cho ô Email (`username` hay `email`)? | Sửa lỗi không đồng bộ giữa hai form | 🟢 | Chỉ áp form Login quản trị; ô Email chỉ cần có `autocomplete` khác rỗng | ✅ Đã trả lời 03-10-2026 (đợt 3) | **Không** áp cho cổng — trùng giả định. Giá trị ô Email PO không chỉ định → giữ AC "khác rỗng". REQ-LOGIN-70 biên tập |
| AMB-LOGIN-28 | Mở link đặt lại mật khẩu thiếu/sai tham số (REQ-LOGIN-64): hành vi đúng là trang 404 hay chuyển về Login kèm thông báo? | Không assert được hành vi đúng sau khi Dev sửa | 🟢 | TC chỉ assert không trả `5xx` | ✅ Đã trả lời 03-10-2026 (đợt 3) | **Trang 404** — cụ thể hơn giả định → REQ-LOGIN-64 🟡: assert mã `404`; thêm ca (b) tham số sai (chưa chạy được) |
| AMB-LOGIN-29 | Tiêu đề tab đúng của trang Quên mật khẩu quản trị (REQ-LOGIN-69) — nguyên văn? (cổng đang là "Forgot Password?") | Không assert được giá trị đúng | 🟢 | TC chỉ assert `document.title` **không** chứa "Login" | ✅ Đã trả lời 03-10-2026 (đợt 3) | **"Forgot Password?"** — giống cổng (REQ-LOGIN-51) → REQ-LOGIN-69 🟡: assert `document.title` = "Forgot Password?" |
| AMB-LOGIN-30 | Thông báo chung của Quên mật khẩu (REQ-LOGIN-36/55) mới có nguyên văn **tiếng Việt**. (1) Nguyên văn **tiếng Anh** là gì? Giao diện mặc định đang là tiếng Anh. (2) Khu quản trị khi **chưa đăng nhập** không có lựa chọn ngôn ngữ — làm sao kiểm bản tiếng Việt ở REQ-LOGIN-36 (đổi ngôn ngữ mặc định hệ thống? — đang Access denied, `AMB-SYS-01`) | TC của REQ-LOGIN-36 không chạy được bằng nguyên văn; ở giao diện tiếng Anh chỉ kiểm được "hai ca giống nhau" | 🟡 | Ở giao diện tiếng Anh: TC chỉ assert **không** hiện "Email not found" (ca b chưa chạy được). Nguyên văn tiếng Việt kiểm ở cổng (REQ-LOGIN-55) sau khi chọn Vietnamese | ✅ Đã trả lời 03-10-2026 (đợt 4) | Bản tiếng Anh: "If this email address exists in our system, we have sent password reset instructions." (agent dịch theo yêu cầu PO, từ câu đã chỉnh ở AMB-LOGIN-31). Khu quản trị kiểm bằng bản tiếng Anh; bản tiếng Việt kiểm ở cổng (REQ-LOGIN-55) → REQ-LOGIN-36 chạy được bằng nguyên văn |
| AMB-LOGIN-31 | Câu chung "Vui lòng nhập địa chỉ email hoặc mật khẩu chính xác" nhắc tới **mật khẩu** trong khi trang chỉ có ô Email, và cũng hiện cho người nhập **đúng** email (đã được gửi email đặt lại) — có muốn chỉnh câu chữ (VD "Nếu email có trong hệ thống, hướng dẫn đặt lại mật khẩu đã được gửi") không? | Người dùng hợp lệ tưởng mình nhập sai, nhập lại nhiều lần → nhận nhiều email đặt lại | 🟢 | Giữ đúng nguyên văn PO đưa | ✅ Đã trả lời 03-10-2026 (đợt 4) | **Có chỉnh** — khác giả định. Câu mới (agent soạn theo yêu cầu PO): VI "Nếu địa chỉ email này có trong hệ thống, chúng tôi đã gửi hướng dẫn đặt lại mật khẩu." · EN "If this email address exists in our system, we have sent password reset instructions." → REQ-LOGIN-36, 55 🟡 |

### 6.2. Risks

| Mã | Rủi ro | Mô tả | Mitigation |
|---|---|---|---|
| RISK-LOGIN-01 | Tài khoản dùng chung bị khoá/đổi | Thử sai mật khẩu nhiều lần, đổi mật khẩu hay reset mật khẩu trên tài khoản `.env` sẽ chặn cả lớp (`RISK-SYS-02`). PO chốt **không** cấp tài khoản riêng (`AMB-LOGIN-01`) | Ca lỗi ưu tiên email **không tồn tại** `auto_login_<timestamp>@auto.test`. Ca bắt buộc dùng email tồn tại dựa trên xác nhận *không có khoá* (`AMB-LOGIN-04`): REQ-LOGIN-10 ca (b) chỉ **1** lần sai · REQ-LOGIN-62 tối đa **5** lần sai, ngay sau đó đăng nhập đúng, **không** chạy song song, tránh giờ lớp đang dùng. Bị khoá → dừng suite, báo người quản trị môi trường + mở bug. Đặt lại mật khẩu (REQ-LOGIN-38/39) vẫn **không** chạy |
| RISK-LOGIN-02 | Agent không nhập mật khẩu thật | Host là public, agent không tự đăng nhập; mọi bước đăng nhập thành công khi recon/thực thi bởi agent cần người nhập tay | Script automation đọc `.env` trên máy QA; khi thực thi tay bằng agent, chừa bước đăng nhập cho người |
| RISK-LOGIN-03 | Không kiểm được email đặt lại mật khẩu | QA không có quyền tầng tích hợp / hộp thư | REQ-LOGIN-38/39/57/58 để ⚪; xin hộp thư test hoặc đội Dev xác minh |
| RISK-LOGIN-04 | Toast cổng tự ẩn sau 3,5 s | Automation chờ chậm sẽ không thấy toast → FAIL giả | Assert `.float-alert` ngay sau điều hướng, không chờ theo thời gian cố định |
| RISK-LOGIN-05 | Trạng thái cookie đan xen giữa các test | `autologin` còn sót sau logout, ngôn ngữ cổng lưu bằng cookie | Mỗi test bắt đầu bằng xoá cookie (context mới); không phụ thuộc thứ tự chạy |
| RISK-LOGIN-06 | Thông báo HTML5 phụ thuộc trình duyệt | Chuỗi "Please include an '@'…" đổi theo trình duyệt/ngôn ngữ | Chỉ assert `checkValidity() === false` và không có request POST |
| RISK-LOGIN-07 | TC của lỗi đã xác nhận FAIL có chủ đích | 8 REQ **hiện vi phạm** (REQ-LOGIN-34, 36, 53, 55, 63, 64, 69, 70) sẽ đỏ tới khi Dev sửa — dễ bị hiểu là lỗi script, hoặc bị hạ AC cho xanh | TC giữ AC đúng yêu cầu, gắn nhãn lỗi đã biết + link bug report (`/create-bug-report`); execution report tách nhóm "FAIL — lỗi đã biết"; **cấm** hạ AC hay assert theo hành vi hiện tại |

---

## 7. Phân rã Epic / Story

> Bắt buộc vì file web có 72 REQ (ngưỡng 25–80 — mục 5.1 của skill). Không tách file story: 72 < 80 và không thoả điều kiện tách 5.2 (không có entity nghiệp vụ có vòng đời riêng, không có ma trận trạng thái). ⚠️ Còn 8 REQ là chạm ngưỡng 80 — đợt cập nhật sau cần cân nhắc tách `web/stories/`.

| Story ID | Tên Story | REQ bao phủ | Số REQ | AMB / RISK liên quan | Ghi chú phạm vi |
|---|---|---|---|---|---|
| STORY-LOGIN-01 | Đăng nhập khu quản trị — form & validation | REQ-LOGIN-01 → 16 · 59 · 61 · 62 · 70 · 71 · 72 | 22 | AMB-LOGIN-04, 07, 10, 21, 22, 24, 25, 27 · RISK-LOGIN-01, 06 | Hiển thị form, validation server + HTML5, flash lỗi, CSRF, autocomplete, chuẩn hoá email, không khoá/CAPTCHA, chặn contact |
| STORY-LOGIN-02 | Phiên đăng nhập & chuyển hướng | REQ-LOGIN-17 → 20 · 65 · 66 · 67 | 7 | AMB-LOGIN-11, 15, 16 · RISK-LOGIN-02 | Đăng nhập thành công, deep link, cookie phiên, session id mới, hạn phiên |
| STORY-LOGIN-03 | Ghi nhớ đăng nhập (Remember me) | REQ-LOGIN-21 → 25 · 63 | 6 | AMB-LOGIN-05 · RISK-LOGIN-05 | Tác tạo `autologin`: tạo · dùng được · gia hạn · HttpOnly |
| STORY-LOGIN-04 | Đăng xuất | REQ-LOGIN-26 → 31 · 68 | 7 | AMB-LOGIN-12, 17, 18, 19, 23 | Menu, popup timer, session id, Back, vô hiệu `autologin`, viewport mobile |
| STORY-LOGIN-05 | Quên mật khẩu — khu quản trị | REQ-LOGIN-32 → 39 · 64 · 69 | 10 | AMB-LOGIN-06, 08, 13, 14, 20, 26, 28, 29, 30, 31 · RISK-LOGIN-03 | 2 REQ ⚪ (gửi email · link dùng được); 4 REQ hiện vi phạm (34 · 36 · 64 · 69) |
| STORY-LOGIN-06 | Cổng khách hàng — Đăng nhập | REQ-LOGIN-40 → 50 · 60 | 12 | AMB-LOGIN-02, 03, 09 · RISK-LOGIN-04 | Ngôn ngữ, register tắt, chặn Staff; 1 REQ ⚪ (đăng nhập thành công) |
| STORY-LOGIN-07 | Cổng khách hàng — Quên mật khẩu | REQ-LOGIN-51 → 58 | 8 | — (dùng chung AMB-LOGIN-06, 08, 26, 30, 31 của STORY-LOGIN-05) | 2 REQ ⚪; 2 REQ hiện vi phạm (53 · 55) |

**Tổng: 7 Story / 72 REQ — mọi REQ thuộc đúng một Story, không mồ côi, không trùng.** `22 + 7 + 6 + 7 + 10 + 12 + 8 = 72 ✔`

**Đối chiếu AMB/RISK** — mỗi mã nằm đúng một chỗ:

| Nhóm | Mã | Nằm ở đâu |
|---|---|---|
| AMB thuộc Story | AMB-LOGIN-02 → 31 (trừ 01) | phân bổ ở bảng Story (AMB-LOGIN-06, 08, 26, 30, 31 gắn STORY-LOGIN-05, áp chung cho STORY-LOGIN-07) |
| AMB cấp Epic | AMB-LOGIN-01 | Ma trận Phân quyền — cột Admin cắt ngang mọi Story |
| RISK thuộc Story | RISK-LOGIN-01 → 06 | phân bổ ở bảng Story |
| RISK cấp Epic | RISK-LOGIN-07 | Lỗi đã xác nhận nằm ở 4 Story (01 · 03 · 05 · 07) |

Kiểm đếm AMB: Story 01 (8) + 02 (3) + 03 (1) + 04 (5) + 05 (10) + 06 (3) = 30 + cấp Epic 1 = **31 ✔** · RISK: 2 + 1 + 1 + 1 + 1 = 6 + cấp Epic 1 = **7 ✔**

**Hạng mục cấp Epic** (cố ý không gán Story):

| Hạng mục | Lý do |
|---|---|
| Ma trận Phân quyền (mục 3) + AMB-LOGIN-01 | Cắt ngang khu quản trị lẫn cổng khách hàng |
| Yêu cầu phi chức năng (mục 5) | Áp cho toàn module |
| RISK-LOGIN-07 | Cách xử lý TC của lỗi đã xác nhận — áp cho mọi Story có REQ hiện vi phạm |

**Thứ tự triển khai đề xuất** (theo phụ thuộc và rủi ro):

| Thứ tự | Story | Lý do | Trạng thái |
|---|---|---|---|
| 1 | STORY-LOGIN-01 | Cửa vào; validation chạy được không cần mật khẩu thật | Sẵn sàng — REQ-LOGIN-62 theo điều kiện `RISK-LOGIN-01`; ⚠️ REQ-LOGIN-59 BLOCKED chờ tài khoản contact (`AMB-SYS-03`) |
| 2 | STORY-LOGIN-02 | Cần đăng nhập thật — dùng `.env` trên máy QA | Sẵn sàng (RISK-LOGIN-02) — REQ-LOGIN-67 là TC dài 8 giờ |
| 3 | STORY-LOGIN-04 | Phụ thuộc 02 | Sẵn sàng (REQ-LOGIN-28 dựa trên mã — AMB-LOGIN-23 ✅) |
| 4 | STORY-LOGIN-03 | Phụ thuộc 02 + 04; cần thao tác cookie | Sẵn sàng — REQ-LOGIN-63 sẽ FAIL tới khi Dev sửa (RISK-LOGIN-07) |
| 5 | STORY-LOGIN-05 | Ca lỗi chạy được ngay | ⚠️ Một phần BLOCKED — REQ-LOGIN-38/39 + ca (b) của REQ-LOGIN-36, 64 chờ hộp thư (RISK-LOGIN-03); |
| 6 | STORY-LOGIN-06 | Ca lỗi + ngôn ngữ chạy được ngay | ⚠️ Một phần BLOCKED — REQ-LOGIN-50 chờ `AMB-SYS-03` |
| 7 | STORY-LOGIN-07 | Ca lỗi chạy được ngay | ⚠️ Một phần BLOCKED — REQ-LOGIN-57/58 + ca (b) của REQ-LOGIN-55 chờ `AMB-SYS-03` |

---

## 8. Nhật ký thay đổi

| Ngày | Nguồn | REQ ảnh hưởng | Loại | Tóm tắt thay đổi | TC cần xử lý |
|---|---|---|---|---|---|
| 03-10-2026 | PO-REPLY-LOGIN-20261003-03 | REQ-LOGIN-36, 55 | 🟡 Sửa | AMB-LOGIN-31: đổi câu chữ thông báo chung (bỏ chữ "mật khẩu", không còn khiến người nhập đúng tưởng mình sai) · AMB-LOGIN-30: thêm nguyên văn tiếng Anh → REQ-LOGIN-36 kiểm bằng tiếng Anh, REQ-LOGIN-55 kiểm cả hai ngôn ngữ. Câu do agent soạn + dịch theo yêu cầu PO | — (chưa có TC — viết theo nguyên văn mới) |
| 03-10-2026 | PO-REPLY-LOGIN-20261003-03 | — | ✏️ Biên tập | AMB-LOGIN-30, 31 ❓ → ✅ — module **hết AMB treo**. Ghi chú câu chữ mới vào kết luận AMB-LOGIN-26 | — |
| 03-10-2026 | PO-REPLY-LOGIN-20261003-02 | REQ-LOGIN-36, 55 | 🟡 Sửa | AMB-LOGIN-26 (khác giả định): email tồn tại hay không đều hiện **cùng** một câu, nguyên văn tiếng Việt "Vui lòng nhập địa chỉ email hoặc mật khẩu chính xác". Đổi tên REQ, AC 2 ca; ca (b) chưa chạy được | — (chưa có TC — viết theo AC mới) |
| 03-10-2026 | PO-REPLY-LOGIN-20261003-02 | REQ-LOGIN-34, 53 | 🟡 Sửa | AMB-LOGIN-26 thay kết luận AMB-LOGIN-08: bỏ trống Email báo "The Email Address field is required." thay vì "Email not found" — hiện vi phạm | — (chưa có TC — **không** assert "Email not found") |
| 03-10-2026 | PO-REPLY-LOGIN-20261003-02 | REQ-LOGIN-64 | 🟡 Sửa | AMB-LOGIN-28: liên kết đặt lại không hợp lệ → trang **404** (trước chỉ "không 5xx"); thêm ca (b) tham số sai, chưa chạy được | — (chưa có TC) |
| 03-10-2026 | PO-REPLY-LOGIN-20261003-02 | REQ-LOGIN-69 | 🟡 Sửa | AMB-LOGIN-29: tiêu đề tab = "Forgot Password?" (trước chỉ "không chứa Login") | — (chưa có TC) |
| 03-10-2026 | PO-REPLY-LOGIN-20261003-02 | REQ-LOGIN-70 | ✏️ Biên tập | AMB-LOGIN-27: ghi rõ không áp cho cổng, PO không chỉ định giá trị ô Email — hành vi không đổi | — |
| 03-10-2026 | PO-REPLY-LOGIN-20261003-02 | — | ✏️ Biên tập | AMB-LOGIN-26 → 29 ❓ → ✅; sửa kết luận AMB-LOGIN-08. Thêm AMB-LOGIN-30 (🟡 tiếng Anh + cách kiểm khu quản trị) · 31 (🟢 câu chữ). RISK-LOGIN-07: 6 → 8 REQ hiện vi phạm | — |
| 03-10-2026 | PO-REPLY-LOGIN-20261003 | REQ-LOGIN-59 → 72 | 🟢 Thêm | 14 REQ mới từ 14 AMB được chốt: 59 (AMB-02) · 60 (03) · 61, 62 (04) · 63 (05, tách từ REQ-23) · 64 (13) · 65 (15) · 66, 67 (16) · 68 (17) · 69 (20) · 70 (21) · 71, 72 (24). 13 🟢 · 1 ⚪ (59). REQ-LOGIN-63, 64, 69, 70 **hiện vi phạm** | — (chưa có TC — viết mới) |
| 03-10-2026 | PO-REPLY-LOGIN-20261003 | REQ-LOGIN-36, 55 | 🟡 Sửa | AMB-LOGIN-06 kết luận **khác giả định** — "Email not found" là lỗi lộ tồn tại tài khoản. Đổi tên + AC: thông báo không được lộ email có tồn tại hay không; hiện vi phạm | — (chưa có TC — viết theo AC mới, **không** assert "Email not found") |
| 03-10-2026 | PO-REPLY-LOGIN-20261003 | REQ-LOGIN-10 | 🟡 Sửa | AMB-LOGIN-07: bổ sung ca (b) email tồn tại + sai mật khẩu cùng thông báo "Invalid email or password" | — (chưa có TC — viết đủ 2 ca) |
| 03-10-2026 | PO-REPLY-LOGIN-20261003 | REQ-LOGIN-23 | 🟡 Sửa | Tách cờ `HttpOnly` sang REQ-LOGIN-63 (AMB-LOGIN-05 — lỗi đã xác nhận). AC REQ-23 không còn assert `HttpOnly = false` | — (chưa có TC — không assert HttpOnly ở TC của REQ-23) |
| 03-10-2026 | PO-REPLY-LOGIN-20261003 | REQ-LOGIN-18, 20, 26, 34, 39, 50, 53, 57, 58 | ✏️ Biên tập | Cập nhật tham chiếu sau khi AMB đóng: 18 "đúng thiết kế" (AMB-11) · 20 → REQ-66/67 · 26 → REQ-68 · 34/53 "PO chấp nhận" (AMB-08) + trỏ AMB-26 · 39 → REQ-64 · 50/57/58 trỏ `AMB-SYS-03` thay AMB-02. Hành vi không đổi | — |
| 03-10-2026 | PO-REPLY-LOGIN-20261003 | — | ✏️ Biên tập | AMB-LOGIN-01 → 25 ❓ → ✅ (AMB-06 khác giả định, 24 AMB trùng giả định). Thêm AMB-LOGIN-26 → 29 (🟡 1 · 🟢 3). Thêm RISK-LOGIN-07; sửa mitigation RISK-LOGIN-01 (không có tài khoản riêng). Ma trận phân quyền: cột Admin → ngoài phạm vi, 3 ô ❔ → ⚠️ theo PO. Story 01/02/03/04/05/06 nhận REQ mới | — |
| 03-10-2026 | UI recon | REQ-LOGIN-01 → 58 | 🟢 Thêm | Khởi tạo tài liệu từ khảo sát UI thực tế (khu quản trị + cổng khách hàng) — 53 🟢 · 5 ⚪ · 25 AMB · 6 RISK · 7 Story | — (viết TC mới) |
