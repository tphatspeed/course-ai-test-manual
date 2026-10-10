# Báo Cáo Review Test Cases — Đăng nhập (`LOGIN`) · Web

> ← Index bộ TC: [`../TEST_CASES_LOGIN_SUMMARY.md`](../TEST_CASES_LOGIN_SUMMARY.md) · Requirements: [`../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md`](../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md)
> Workflow: `/review-testcases` — **Mode REVIEW** (chỉ báo cáo, không sửa file TC nào) · Ngày review: 10-10-2026

## Tổng quan

- **Nguồn:** [`web/parts/part_01_web_smoke.md`](../web/parts/part_01_web_smoke.md) · [`part_02_web_quan_tri.md`](../web/parts/part_02_web_quan_tri.md) · [`part_03_web_quen_mat_khau_cong.md`](../web/parts/part_03_web_quen_mat_khau_cong.md) · [`part_04_web_ky_thuat_phi_chuc_nang.md`](../web/parts/part_04_web_ky_thuat_phi_chuc_nang.md) — đọc qua index theo `## Bản đồ tài liệu`
- **Đối chiếu với:** `REQUIREMENTS_LOGIN_SUMMARY.md` + `web/requirements_login_web.md` (đợt 4, 72 REQ) · bảng *Năng lực kiểm thử của QA* ở `docs/requirements/README.md`
- **Số TC review:** 79 (130 case kiểm) · `@Deprecated`: 0
- **Kết quả:** 🟢 77 tốt | 🟡 2 cần sửa | 🔴 0 nên viết lại
- **Điểm trung bình:** 11,6/12 (57 TC đạt 12/12)
- **Loại trừ theo requirements:** cột Admin của Ma trận Phân quyền (`AMB-LOGIN-01`) · đổi mật khẩu khi đã đăng nhập, 2FA (`PROFILE`) · cổng khách hàng sau đăng nhập (`PORTAL`) · cấu hình bảo mật đăng nhập (`SETTING`) · thứ tự hai thông báo bắt buộc (`AMB-LOGIN-22`) · mã chuyển hướng `307` (`AMB-LOGIN-25`) · cookie `autologin` còn lại sau Logout (`AMB-LOGIN-12`) · Logout bằng GET (`AMB-LOGIN-18`) · mã chống giả mạo không đổi sau Logout (`AMB-LOGIN-19`) · thiếu lối về Login ở Quên mật khẩu quản trị (`AMB-LOGIN-14`) · `autocomplete` ở form Login cổng (`AMB-LOGIN-27`) · câu chữ bong bóng trình duyệt (`RISK-LOGIN-06`)
- **Đối chiếu kết quả chạy:** chưa có `docs/executions/login/` — không có ghi chú `⚠️ chưa có evidence` / `@NeedsVerify` nào đã được giải quyết để dọn

> **Nhận định chung:** bộ TC viết rất chặt — nhãn nguyên văn, dữ liệu traceable, mọi REQ có TC, phần kỹ thuật phần lớn đã tách xuống `🔧`, TC `@KnownBug` giữ đúng Expected của PO. Điểm trừ tập trung ở **4 nhóm hệ thống** (mục *Vấn đề mang tính hệ thống*), không phải lỗi rải rác. Hai TC 🟡 đều là TC chưa có evidence (đặt lại mật khẩu qua email), cần recon trước khi viết lại.

---

## Chi tiết từng TC — ma trận điểm

> C1 Rõ ràng · C2 Kết quả đo được · C3 Độc lập · C4 Dữ liệu cụ thể · C5 Truy vết · C6 Đúng trọng tâm. Thang 0–2 mỗi tiêu chí.

| TC ID | C1 | C2 | C3 | C4 | C5 | C6 | Điểm | Xếp loại |
|---|---|---|---|---|---|---|---|---|
| CRM_LOGIN_TC_001 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_002 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_003 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_004 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_005 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_006 | 2 | 2 | 2 | 2 | 2 | 1 | 11 | 🟢 |
| CRM_LOGIN_TC_007 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_008 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_009 | 2 | 2 | 1 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_010 | 2 | 2 | 1 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_011 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_012 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_013 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_014 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_015 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_016 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_017 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_018 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_019 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_020 | 2 | 2 | 1 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_021 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_022 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_023 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_024 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_025 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_026 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_027 | 2 | 1 | 1 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_028 | 2 | 1 | 1 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_029 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_030 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_031 | 1 | 2 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_032 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_033 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_034 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_035 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_036 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_037 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_038 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_039 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_040 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_041 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_042 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_043 | 2 | 2 | 1 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_044 | 1 | 1 | 1 | 2 | 2 | 2 | 9 | 🟡 |
| CRM_LOGIN_TC_045 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_046 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_047 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_048 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_049 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_050 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_051 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_052 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_053 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_054 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_055 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_056 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_057 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_058 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_059 | 2 | 2 | 1 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_060 | 1 | 1 | 1 | 2 | 2 | 2 | 9 | 🟡 |
| CRM_LOGIN_TC_061 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_062 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_063 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_064 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_065 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_066 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_067 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_068 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_069 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_070 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_071 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_072 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_073 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_074 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_075 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_076 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_077 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_078 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_079 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |

---

## Vấn đề & đề xuất sửa — các TC bị trừ điểm

| TC ID | Điểm | Vấn đề chính (trích nguyên văn) | Đề xuất sửa |
|---|---|---|---|
| CRM_LOGIN_TC_044 | 9/12 🟡 | C1 bước 2 *"Nhập mật khẩu mới theo biểu mẫu hiện ra"* — không nêu ô nào · C2 Expected 3 *"Hệ thống xác nhận đã đổi mật khẩu"* — không có nguyên văn · C3 Pre-Condition *"Đã có email đặt lại từ `CRM_LOGIN_TC_043`"* | Khi có hộp thư test (`RISK-LOGIN-03`): recon trang đặt lại rồi viết Steps theo nhãn thật (VD *"2. Nhập `Auto@New_<timestamp>` vào ô `<nhãn thật>`"*), Expected 3 ghi nguyên văn thông báo. Pre-Condition: *"Hộp thư test có email đặt lại vừa gửi cho `EMAIL_STAFF_MAILBOX` (chưa có thì làm bước 1–3 của `_043`)"*. Thêm Expected *"5. Đăng nhập bằng mật khẩu cũ → khung đỏ `Invalid email or password`"* — hệ quả trực tiếp của đổi mật khẩu. Thêm bước dọn: cập nhật mật khẩu mới vào `.env` để lần chạy sau dùng được |
| CRM_LOGIN_TC_060 | 9/12 🟡 | Như `_044`: *"Nhập mật khẩu mới theo biểu mẫu hiện ra"* · *"Hệ thống xác nhận đã đổi mật khẩu"* · Pre-Condition *"Đã có email đặt lại từ `CRM_LOGIN_TC_059`"* · Expected 4 *"Đăng nhập cổng thành công như `CRM_LOGIN_TC_011`"* — trỏ vào một Expected cũng chưa có evidence (`ASM-10`) | Sửa như `_044` (tài khoản contact, `AMB-SYS-03`). Expected 4 viết đích danh trang đích sau khi `_011` được recon; thêm *"5. Đăng nhập cổng bằng mật khẩu cũ → thông báo nổi `Invalid username or password`"* |
| CRM_LOGIN_TC_027 | 10/12 | C3 Pre-Condition *"Đã làm xong `CRM_LOGIN_TC_026`"* — trái `RISK-LOGIN-05` (mỗi test bắt đầu context mới, không phụ thuộc thứ tự) · C2 thao tác DevTools ở Steps chính | Pre-Condition: *"Cửa sổ ẩn danh mới · DevTools đang mở · đã đăng nhập `EMAIL_ADMIN` **có** tích `Remember me`, đang ở Dashboard"*. C2: xem mẫu A |
| CRM_LOGIN_TC_028 | 10/12 | C3 *"Đã làm xong `CRM_LOGIN_TC_026` · ghi lại ngày giờ hết hạn…"* · C2 Expected 4 *"T2 = thời điểm bước 3 + 62 ngày"* ở phần chính | Pre-Condition như `_027`; đưa *"ghi lại hạn cookie `autologin` (T1)"* thành bước 1. C2: xem mẫu A |
| CRM_LOGIN_TC_006 | 11/12 | C6 TC `@Smoke` gồm 2 hành vi (link `Forgot Password?` + logo) và bước 4 *"⚠️ trang đích cuối chưa có evidence (ASM-17)"* — bộ smoke có thể FAIL giả vì một giả định | Bỏ bước 4 khỏi `_006` (giữ REQ-15; REQ-16 vẫn có `_001` chống lưng). Chuyển bước logo sang **TC mới nối tiếp dải** (REQ-LOGIN-16, `@Regression @NeedsVerify`) |
| CRM_LOGIN_TC_009 | 11/12 | C3 Pre-Condition *"Đã đăng nhập theo `CRM_LOGIN_TC_008`"* | *"Cửa sổ ẩn danh mới · đã đăng nhập bằng `EMAIL_ADMIN` / `PASSWORD_ADMIN`, không tích `Remember me` · đang ở Dashboard `https://crm.anhtester.com/admin/`"* |
| CRM_LOGIN_TC_010 | 11/12 | C3 như `_009` — TC `Critical` `@Smoke` nên càng cần chạy độc lập | Pre-Condition như `_009`, giữ phần *"không có bộ đếm giờ… · cửa sổ `1600×770`"* |
| CRM_LOGIN_TC_011 | 11/12 | C2 Expected 4 chỉ toàn phủ định *"không còn form `Please login`, đầu trang không còn nút `Login`, không có thông báo…"* — một trang lỗi máy chủ cũng thoả cả ba | Thêm *"trang đích không phải trang lỗi (không có chữ `404` / `500` / `Error`), thanh địa chỉ không còn `/authentication/login`"*. Lần chạy đầu ghi lại trang đích thật → đóng `ASM-10`, viết Expected khẳng định |
| CRM_LOGIN_TC_020 | 11/12 | C3 bước 5 *"So sánh trang với kết quả `CRM_LOGIN_TC_019-a`"* — cần kết quả của TC khác | Thêm 4 bước đầu trong chính TC: gửi `auto_login_1791100005@auto.test` + `Auto@12345`, ghi lại thông báo; rồi mới gửi email tồn tại + mật khẩu sai. Expected 5: *"Hai lần cùng chữ `Invalid email or password`, cùng khung đỏ, cùng địa chỉ"*. Không tăng số lần sai trên tài khoản dùng chung |
| CRM_LOGIN_TC_043 | 11/12 | C3 bước 4 *"So sánh thông báo với `CRM_LOGIN_TC_042`"* | Như `_020`: gửi `auto_login_<timestamp>@auto.test` trước trong chính TC, rồi gửi email tồn tại, so hai thông báo |
| CRM_LOGIN_TC_059 | 11/12 | C3 bước 4 *"So sánh thông báo với `CRM_LOGIN_TC_058-a`"* | Như `_043` |
| CRM_LOGIN_TC_030 | 11/12 | C2 — **thiết kế không phân biệt được điều REQ cần chứng minh.** Cookie `sp_session` có hạn = lúc phản hồi + 8 giờ (`AMB-LOGIN-16`), nên sau *"8 giờ 5 phút"* trình duyệt **tự xoá cookie** → bước 3 *"Bị đưa về trang Login"* đạt kể cả khi máy chủ không bao giờ huỷ phiên. Đây đúng là điều mà chính ghi chú `🔧` của TC muốn tránh | Thêm vào `🔧`: ngay sau bước 1 sao chép giá trị `sp_session` (chỉ giữ trong clipboard); sau 8 giờ 5 phút thêm lại cookie với giá trị đó rồi mới F5 → vẫn về Login = máy chủ đã hết phiên. Cùng nhận xét áp cho AC của REQ-LOGIN-67 — nếu PO muốn sửa AC thì đi qua `/update-requirements-from-ticket`, không sửa ở đây |
| CRM_LOGIN_TC_031 | 11/12 | C1 bước 1 *"Vào `Tasks` → `New Task`, nhập Subject, bấm `Save`"* và bước 5 *"Đăng nhập lại, dừng bộ đếm giờ, xoá task vừa tạo"* — nhiều hành động trong một bước | Tách: *1. Vào menu `Tasks` · 2. Bấm `New Task` · 3. Nhập Subject · 4. Bấm `Save` …* · dọn: *Đăng nhập lại · Mở task `auto_login_timer_<ts>` · Bấm `Stop Timer` · Xoá task* — mỗi bước một số |
| CRM_LOGIN_TC_025 | 11/12 | C2 bước 3 *"DevTools → Application → Cookies: xoá cookie phiên `sp_session`"*, Expected 3 *"Cookie phiên đã bị xoá"* ở phần chính | Mẫu A |
| CRM_LOGIN_TC_026 | 11/12 | C2 Expected 5 *"Xuất hiện thêm cookie ghi nhớ đăng nhập `autologin`, ngày hết hạn = ngày đăng nhập + 62 ngày"* ở phần chính | Mẫu A — Expected chính bước 5 chỉ còn *"Đã có dấu ghi nhớ đăng nhập (kiểm ở 🔧)"*; tên cookie + hạn 62 ngày xuống `🔧` |
| CRM_LOGIN_TC_029 | 11/12 | C2 Expected 2 / 4 *"T1 ≈ … + 8 giờ"* · *"T2 ≈ … + 8 giờ, muộn hơn T1"* ở phần chính | Mẫu A — Expected chính bước 3: *"Trang Customers mở bình thường, không bị đòi đăng nhập lại"*; T1/T2 xuống `🔧` |
| CRM_LOGIN_TC_032 | 11/12 | C2 bước 2, 4 *"DevTools: sao chép / thay giá trị `sp_session`"* ở Steps chính | Mẫu A |
| CRM_LOGIN_TC_034 | 11/12 | C2 bước 2, 4 thao tác cookie · Expected 1 *"có cookie `autologin`"* ở phần chính | Mẫu A |
| CRM_LOGIN_TC_069 · 070 · 071 · 076 | 11/12 | C2 — TC thuần công cụ: *"DevTools → Application → Cookies"*, *"Xem tab Console / Issues"*; Expected chính *"Cookie phiên được đánh dấu chỉ gửi qua HTTPS…"*, *"thiếu thuộc tính tự điền (`autocomplete`)"* — che dòng `🔧` thì không chấm được | Mẫu B |

**Mẫu A — TC dùng cookie làm công cụ giả lập, có hệ quả nghiệp vụ** (`_025`, `_026`, `_027`, `_028`, `_029`, `_032`, `_034`): Steps chính ghi thao tác người dùng + một bước *"Giả lập phiên mất (thao tác ở 🔧)"*; Expected chính giữ **hệ quả** đã có sẵn (về trang Login · vào thẳng Dashboard · phiên cũ không dùng lại được); toàn bộ tên cookie, cờ, hạn, thao tác Application → Cookies gom xuống `🔧 Ghi chú kỹ thuật (cần DevTools):`.

**Mẫu B — TC không có hệ quả nhìn thấy** (`_069` REQ-20 · `_070` REQ-65 · `_071` REQ-63 · `_076` REQ-70): các REQ này bản chất chỉ kiểm được bằng công cụ. Hai lựa chọn: (1) chấp nhận trần 11/12, giữ nguyên; (2) chuyển thành mục **checklist kỹ thuật cho security review** (`/generate-checklist-test` — ngoại lệ mục 5 của *Quy Tắc Ngôn Ngữ Kiểm Chứng*), TC giữ ID và trỏ sang mục checklist. Không đề xuất xoá — `_071`, `_076` là TC `@KnownBug` đang gắn bug.

**TC 12/12 có ghi chú nhỏ (không trừ điểm):**

- `_016` — biến thể `b` (`@localhost`) cùng lớp tương đương với `a` (`@autotest` — tên miền không có dấu chấm). Đề xuất đổi `b` thành lớp khác cũng lọt qua trình duyệt nhưng máy chủ chặn, VD hai dấu chấm liền nhau ở phần trước `@`: `auto..login_1791100003@auto.test` (gắn `⚠️ ASM-03`, chạy thử trước)
- `_038` — bước 3 *"DevTools → Network: chọn `Offline`"* có thể viết thành *"Tắt Wi-Fi / rút cáp mạng"* cho tester không quen DevTools; giữ DevTools làm cách thay thế
- `_075` — ô `Language` nằm trên ô Email nên chuỗi `Tab` không đi qua; chưa có bước chọn ngôn ngữ bằng bàn phím (xem gap #7)
- `_078`, `_079` — breakpoint và danh sách trình duyệt là giả định của agent (`ASM-13`, `ASM-14`), chưa có người chốt (xem *Vấn đề hệ thống* C)

---

## Vấn đề mang tính hệ thống

| # | Vấn đề | TC ảnh hưởng | Đề xuất |
|---|---|---|---|
| A | Thao tác DevTools (xem / xoá / sửa cookie, Console) nằm ở Steps / Expected **chính** dù TC đã có `🔧` và `@TechCheck` | 11 TC: `_025`–`_029` · `_032` · `_034` · `_069`–`_071` · `_076` | Mẫu A / Mẫu B ở trên |
| B | TC phụ thuộc TC khác (Pre-Condition *"theo / đã làm xong `_0xx`"* hoặc bước *"so sánh với `_0xx`"*) — trái `RISK-LOGIN-05` | 9 TC: `_009` · `_010` · `_020` · `_027` · `_028` · `_043` · `_044` · `_059` · `_060` | Pre-Condition tự mô tả trạng thái; bước so sánh tự tạo mốc trong chính TC |
| C | Ô `⏭️` **thiếu người quyết**: 4 dòng của *Đối soát Field-Level* ghi *"agent đề xuất, chờ QA lead duyệt 04-10-2026"* (Email Quên MK quản trị: Nhiều `@` · Max length · Language: tất cả option · Password cổng). Tương tự, nhánh V4 Responsive / Compatibility chấm ✅ trên breakpoint / trình duyệt do agent tự chọn (`ASM-13`, `ASM-14`) | Index — mục *Đối soát Field-Level*, *Đối soát 4 vòng* | QA lead chốt từng ô: duyệt → ghi *"Quyết định: <tên>, <ngày>"* + điều kiện rà lại; không duyệt → bổ sung biến thể (VD `_039-d` nhiều `@`). Ghi người chốt danh sách breakpoint / trình duyệt vào `ASM-13`, `ASM-14` |
| D | Bộ smoke không sạch: `_006` chứa bước `@NeedsVerify`; `_011` (`Critical` `@Smoke`) ⛔ chờ tài khoản contact → mỗi lần chạy smoke luôn có 1 BLOCKED | `_006` · `_011` | `_006`: xem bảng trên. `_011`: giữ nguyên, ghi chú ở *Bộ chạy đề xuất* của index rằng smoke có 1 BLOCKED có chủ đích tới khi xong `AMB-SYS-03` |

---

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | `_001`–`_004` (4 TC · 15 mục kiểm) — nhãn nguyên văn, thứ tự từ trên xuống, trạng thái mặc định, con trỏ, tiêu đề tab của cả 4 màn hình |
| 1 | Open form | ✅ | `_005`–`_007` (3 TC · 4 case). Lưu ý hệ thống D về `_006` |
| 1 | Display | ➖ | Đồng ý — module không hiển thị dữ liệu nghiệp vụ |
| 1 | Input valid data · Save | ✅ | `_008` · `_011` (`_011` ⏸️ chờ `AMB-SYS-03`) |
| 1 | Verify data | ✅ | `_009` · `_010` |
| 2 | UI Behavior | 🟡 Nông | `_023` · `_051` (2 TC · 6 case). Thiếu: Remember me cổng tích / bỏ tích (gap #5) · đóng popup cảnh báo timer (gap #6) |
| 2 | Required | ✅ | 7 TC · 11 case — đủ từng ô bắt buộc + bỏ trống tất cả ở cả 4 form |
| 2 | Validation | 🟡 Nông | 11 TC · 29 case. Thiếu: Email Login cổng (ô chữ tự do, trình duyệt **không** cắt khoảng trắng) chưa có ca khoảng trắng đầu/cuối và chỉ dấu cách (gap #3) · Max length Email quản trị mới thăm dò phía hợp lệ 64 ký tự (gap #4) · 4 ô `⏭️` thiếu người quyết (hệ thống C) |
| 2 | Equivalence Partitioning | ✅ | `_008` · `_015` · `_016` · `_019` · `_020` — lớp "lọt trình duyệt, máy chủ chặn" mới có 1 lớp thật (ghi chú `_016`) |
| 2 | Boundary Value Analysis | ➖ | Đồng ý — Field Spec mục 3 không khai `min` / `max` |
| 2 | Business Rule | ✅ | 14 TC · 15 case |
| 2 | Decision Table | ✅ | 8 rule ↔ 8 TC — khớp bảng ở index |
| 2 | State Transition | ✅ | 4 trạng thái phiên — transition hợp lệ + bị chặn đều có TC |
| 2 | Dependency | ✅ | `_051`–`_053` |
| 2 | Use Case / Scenario | ✅ | `_024` · `_043`→`_044` · `_059`→`_060` (2/3 luồng ⏸️ chờ hộp thư — `RISK-LOGIN-03`) |
| 2 | Save / Edit / Delete | ➖ | Đồng ý — không có CRUD |
| 2 | Error Guessing | ✅ | `_035`–`_038`. Gợi ý thêm ở gap #8 |
| 3 | Permission | ✅ | `_061` · `_062` · `_005` — đủ các ô trong phạm vi của Ma trận Phân quyền |
| 3 | Security | ✅ | `_063`–`_073` (11 TC · 18 case) + `_020` · `_032`–`_034` · `_054` |
| 3 | API · Database · Integration | ➖ | Khớp *Năng lực kiểm thử của QA* (chốt 03-10-2026) — đội Dev xác minh |
| 3 | Logging / Audit | ➖ | Requirements không có yêu cầu audit + tài khoản QA bị chặn nhật ký hoạt động |
| 4 | Compatibility | ✅ | `_079` (3 trình duyệt) — danh sách là giả định `ASM-14`, chưa có người chốt (hệ thống C); chỉ phủ khu quản trị |
| 4 | Responsive / UI Stability | 🟡 Nông | `_077` · `_078` chỉ phủ **Login quản trị** + menu Logout. Chưa có: Login cổng · Quên mật khẩu cổng · Quên mật khẩu quản trị ở kích thước nhỏ; chưa có phóng to 125% (gap #1, #2) |
| 4 | Accessibility | 🟡 Nông | `_074`–`_076` — bàn phím chỉ ở 2 form Login; thiếu mục *ảnh có văn bản thay thế* (logo) và thao tác bàn phím ở 2 trang Quên mật khẩu + ô Language (gap #7) |
| 4 | Performance | ➖ | Đồng ý — không có ngưỡng cam kết, đã ghi đội Hạ tầng |
| 4 | Regression | ➖ | Đồng ý — `docs/bugs/login/` chưa có bug nào |
| 4 | E2E | ➖ | Đồng ý — chuyển `/generate-cross-module-test-plan` khi `DASH` có requirements |

> Đối soát `⏭️` / `➖` ở index: không có `➖` nào thực chất là `⏭️`. Lỗi ghi nhãn duy nhất là 4 ô `⏭️` thiếu người quyết (hệ thống C).

---

## Đối soát bảng 15 loại field

| Field (form) | Loại | Kết quả | Mục thiếu / ghi chú |
|---|---|---|---|
| Email Address — Login quản trị | Email | ✅ 9/9 mục có TC | Max length mới thăm dò **phía hợp lệ** (`_019-b`, 64 ký tự phần trước `@`) — chưa có phía vượt (gap #4) |
| Password — Login quản trị | Password | ✅ 7/7 | `➖` hoa/thường · số · xác nhận mật khẩu — đúng là không áp cho form đăng nhập |
| Remember me — Login quản trị | Checkbox | ✅ 4/4 | — |
| Email Address — Quên MK quản trị | Email | 🟡 | Nhiều `@` · Max length: `⏭️` thiếu người quyết · Case sensitivity ⏸️ `RISK-LOGIN-03` |
| Language — Login cổng | Dropdown | 🟡 | Tất cả option: `⏭️` thiếu người quyết (24/26 ngôn ngữ chỉ đếm số lượng) |
| Email Address — Login cổng (ô chữ tự do) | Email | 🟡 | **Khoảng trắng đầu/cuối · chỉ gồm dấu cách** — ô `type=text` nên trình duyệt không cắt, khác lớp với form quản trị (gap #3) · Max length `⏭️` thiếu người quyết · Format hợp lệ / Case ⏸️ chờ contact |
| Password — Login cổng | Password | 🟡 | Required ✅ `_049`; còn lại `⏭️` thiếu người quyết |
| Remember me — Login cổng | Checkbox | 🟡 | Mặc định ✅ `_003` · **Check / Uncheck chưa có TC** — không cần tài khoản contact (gap #5) |
| Email Address — Quên MK cổng | Email | 🟡 | Nhiều `@` · Max length `⏭️` thiếu người quyết |

---

## Coverage Gaps (TC còn thiếu)

> TC mới cấp số **nối tiếp** từ `CRM_LOGIN_TC_080` khi chạy Mode FIX, theo thứ tự user duyệt. Gap ghi *"cần chốt Expected"* là hành vi chưa có trong REQ — chốt qua `/update-requirements-from-ticket` (mở AMB) **trước** khi viết TC, không đoán Expected.

| # | Kịch bản thiếu | Vòng / Nhánh | Priority đề xuất |
|---|---|---|---|
| 1 | Login cổng và Quên mật khẩu cổng ở `375×812` · `768×1024`: đủ thành phần như `_003` / `_004`, không thanh cuộn ngang, ô `Language` mở được. Cổng là màn hình của **khách hàng**, khả năng dùng điện thoại cao hơn khu quản trị. Thêm Quên MK quản trị ở `375×812`. Gắn `@NeedsVerify` (`ASM-13`) | V4 · Responsive | Medium |
| 2 | Phóng to trình duyệt 125% ở Login quản trị và Login cổng: đủ thành phần, không thanh cuộn ngang, chữ không đè | V4 · Responsive | Low |
| 3 | Email Login cổng: (a) email đúng định dạng có 2 dấu cách đầu/cuối `  auto_login_<ts>@auto.test  ` · (b) chỉ gồm 3 dấu cách. Ô chữ tự do nên chuỗi tới máy chủ nguyên vẹn — chưa biết máy chủ báo `The Email Address field must contain…` hay `Invalid username or password` → **cần chốt Expected** | V2 · Validation | Medium |
| 4 | Email Login quản trị: phần trước `@` dài **65** ký tự (vượt 64) — chạy thử, ghi kết quả vào ASM rồi chốt Expected; ghép làm biến thể `c` của `_019` nếu cùng loại phản hồi, tách TC nếu khác | V2 · Validation (Max length) | Low |
| 5 | Remember me ở Login cổng: bấm ô vuông → tích · bấm lần nữa → bỏ tích · bấm chữ `Remember me` → tích (như `_023`) | V2 · UI Behavior | Low |
| 6 | Popup cảnh báo timer khi Logout: đóng popup (nút đóng / `Esc` / bấm ra ngoài) → **vẫn** đang đăng nhập, bộ đếm giờ vẫn chạy. REQ-LOGIN-28 nói *"không đăng xuất ngay"*; cách đóng popup chưa quan sát → gộp vào lượt recon `ASM-18`, `@NeedsVerify` | V2 · UI Behavior (Modal / Dialog) | Medium |
| 7 | Bàn phím ở 2 trang Quên mật khẩu (gõ Email → `Enter` gửi biểu mẫu) và chọn `Language` ở Login cổng bằng bàn phím · logo có văn bản thay thế (`🔧`: thuộc tính `alt` chứa `Anh Tester Demo` — AC của REQ-LOGIN-16) | V4 · Accessibility | Low |
| 8 | Để trang Login quản trị mở quá hạn mã chống giả mạo (~1 giờ — `AMB-LOGIN-19`) rồi mới bấm `Login` — người dùng thật hay gặp. Hành vi chưa có trong REQ → **cần chốt Expected** | V2 · Error Guessing | Low |

---

## TC trùng lặp — đề xuất merge

Không có cặp TC nào trùng hành vi cần merge. Các cặp gần nhau đã soát và **giữ riêng** vì khác REQ chống lưng hoặc khác lối vào:

- `_027` ≈ `_028` (bước giống nhau, `_028` đo thêm hạn cookie) — gộp sẽ để REQ-LOGIN-24 và 25 cùng chỉ dựa vào một TC → vi phạm luật CẤM gộp
- `_009` bước 2 ≈ `_036` (cùng REQ-LOGIN-19) — khác lối vào (gõ URL vs nút Back)

Trùng mức **biến thể**: `_016-b` cùng lớp với `_016-a` → đổi giá trị (ghi chú ở mục trên), không merge TC.

---

## Ưu tiên (Priority)

Hợp lý với rủi ro: 4 `Critical` đúng là 4 luồng sống còn (chặn khi chưa đăng nhập · đăng nhập · đăng xuất · đăng nhập cổng); TC `@Security` đều `High`; 3 `Low` (`_046` tiêu đề tab · `_053` ngôn ngữ không lan · `_076` gợi ý tự điền) là lỗi hiển thị / tiện ích. Không đề xuất đổi.

---

## Kết luận & Khuyến nghị

1. **QA lead chốt 4 ô `⏭️` và danh sách breakpoint / trình duyệt** (hệ thống C) — việc nhỏ nhất nhưng đang để quyết định phạm vi trôi nổi từ 04-10-2026
2. **Sửa `_030`** — đúng như TC đang viết, nó PASS kể cả khi máy chủ không bao giờ huỷ phiên. Thêm bước dùng lại cookie cũ ở `🔧`
3. **Làm 9 TC độc lập** (hệ thống B) — ưu tiên `_010` (`Critical` `@Smoke`) và `_027`, `_028` (đang trái `RISK-LOGIN-05`)
4. **Bổ sung gap #1, #3, #6** (Medium) — Responsive cho cổng khách hàng, khoảng trắng ở Email cổng, đóng popup timer; gap #3 và #8 cần mở AMB trước
5. **Viết lại `_044`, `_060`** ngay khi có hộp thư test / tài khoản contact — hai TC 🟡 duy nhất, đều chặn bởi môi trường chứ không phải cách viết

> **Muốn agent sửa luôn (Mode FIX):** `docs/` hiện **chưa được git theo dõi** (`?? docs/`) nên không có mốc git để lấy lại bản trước khi sửa. Cần commit `docs/` trước — agent không tự commit.

---

## Kết quả Mode FIX — 10-10-2026

> Sửa **tại chỗ** trong 4 file part + index. Mốc git trước khi sửa: `d0d0a6d` — xem bản cũ bằng `git show d0d0a6d:docs/testcases/login/web/parts/<file>`. Không TC ID nào bị đổi hay đánh lại.

| Hạng mục | TC | Đã làm |
|---|---|---|
| Hệ thống B — độc lập | `_009` · `_010` · `_027` · `_028` · `_044` · `_060` | Pre-Condition tự mô tả trạng thái (cửa sổ ẩn danh mới · đăng nhập bằng `.env` · có / không tích Remember me) |
| Hệ thống B — tự tạo mốc so sánh | `_020` · `_043` · `_059` | Gửi email không tồn tại trước trong chính TC, ghi mốc, rồi so |
| Hệ thống A — Mẫu A | `_025`–`_029` · `_032` · `_034` | Steps chính ghi thao tác người dùng + *"(thao tác ở 🔧)"*; tên cookie, cờ, hạn, Application → Cookies gom xuống `🔧` |
| Hệ thống A — Mẫu B | `_069`–`_071` · `_076` | **Giữ nguyên** — chấp nhận trần 11/12 |
| Hệ thống D — smoke | `_006` → `_080` | Bỏ bước logo khỏi `_006` (hết `@NeedsVerify` trong TC smoke), chuyển sang TC mới `_080` |
| Thiết kế | `_030` | Thêm bước lưu + khôi phục cookie phiên (ở `🔧`) — chứng minh **máy chủ** huỷ phiên |
| Rõ ràng | `_031` | Tách 5 bước thành 10, mỗi bước một hành động |
| Kết quả đo được | `_011` · `_044` · `_060` | `_011`, `_060` loại trừ trang lỗi; `_044`, `_060` thêm kiểm mật khẩu cũ bị từ chối + bước dọn `.env`. Bước biểu mẫu đặt lại **vẫn** `⚠️` — chỉ viết đích danh được sau khi recon |
| Ghi chú nhỏ | `_016` · `_038` · `_001` | `_016-b` → `auto..login_1791100003@auto.test` · `_038` ngắt mạng bằng Wi-Fi / cáp · `_001` thêm `🔧` văn bản thay thế của logo (gap #7) |
| Gap #1, #2 | `_081` · `_082` · `_083` · `_078-d` | Responsive Login cổng (+125%), Quên MK cổng, Quên MK quản trị; Login quản trị thêm biến thể phóng to 125% |
| Gap #5 | `_084` | Remember me cổng tích / bỏ tích |
| Gap #6 | `_085` | Đóng popup cảnh báo bộ đếm giờ (×, `Esc`, bấm ra ngoài) → vẫn đăng nhập |
| Gap #7 | `_086` · `_087` · `_088` | Bàn phím ở 2 trang Quên mật khẩu + chọn Language bằng bàn phím |

**Chưa làm — chờ quyết định:**

- Gap #3 (khoảng trắng ở Email cổng) · gap #8 (mã chống giả mạo hết hạn) — Expected chưa có trong REQ → chốt qua `/update-requirements-from-ticket` rồi mới viết TC
- Gap #4 (phần trước `@` dài 65 ký tự) — chạy thử trước, ghi ASM, rồi thêm biến thể vào `_019`
- 4 ô `⏭️` thiếu người quyết ở *Đối soát Field-Level* · người chốt danh sách kích thước (`ASM-13`) và trình duyệt (`ASM-14`) — chờ QA lead

**Sau FIX:** 88 TC · 145 case kiểm · `@NeedsVerify` 19 · `@TechCheck` 25 · TC kế tiếp `CRM_LOGIN_TC_089`. Phép thử 6b chạy lại — không vi phạm.
