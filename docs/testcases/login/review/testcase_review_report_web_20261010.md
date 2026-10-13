# Báo Cáo Review Test Cases — Đăng nhập (`LOGIN`) · Web

> ← Index bộ TC: [`../TEST_CASES_LOGIN_SUMMARY.md`](../TEST_CASES_LOGIN_SUMMARY.md) · Requirements: [`../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md`](../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md)
> Workflow: `/review-testcases` — **Mode FIX** · Ngày review: 10-10-2026 · **Vòng 2** — chấm lại bộ TC sau FIX vòng 1.
> Báo cáo vòng 1 (Mode REVIEW trên 79 TC + kết quả FIX vòng 1) được thay thế tại chỗ — xem lại bằng `git show 213d312:docs/testcases/login/review/testcase_review_report_web_20261010.md`

## Tổng quan

- **Nguồn:** [`web/parts/part_01_web_smoke.md`](../web/parts/part_01_web_smoke.md) · [`part_02_web_quan_tri.md`](../web/parts/part_02_web_quan_tri.md) · [`part_03_web_quen_mat_khau_cong.md`](../web/parts/part_03_web_quen_mat_khau_cong.md) · [`part_04_web_ky_thuat_phi_chuc_nang.md`](../web/parts/part_04_web_ky_thuat_phi_chuc_nang.md) — đọc qua index theo `## Bản đồ tài liệu`
- **Đối chiếu với:** `REQUIREMENTS_LOGIN_SUMMARY.md` + `web/requirements_login_web.md` (đợt 4, 72 REQ) · bảng *Năng lực kiểm thử của QA* ở `docs/requirements/README.md` · báo cáo vòng 1
- **Số TC review:** 88 (145 case kiểm) · `@Deprecated`: 0
- **Kết quả:** 🟢 88 tốt | 🟡 0 cần sửa | 🔴 0 nên viết lại
- **Điểm trung bình:** 11,6/12 — 66 TC 12/12 · 11 TC 11/12 · 11 TC 10/12
- **Loại trừ theo requirements:** cột Admin của Ma trận Phân quyền (`AMB-LOGIN-01`) · đổi mật khẩu khi đã đăng nhập, 2FA (`PROFILE`) · cổng khách hàng sau đăng nhập (`PORTAL`) · cấu hình bảo mật đăng nhập (`SETTING`) · thứ tự hai thông báo bắt buộc (`AMB-LOGIN-22`) · mã chuyển hướng `307` (`AMB-LOGIN-25`) · cookie `autologin` còn lại sau Logout (`AMB-LOGIN-12`) · Logout bằng GET (`AMB-LOGIN-18`) · mã chống giả mạo không đổi sau Logout (`AMB-LOGIN-19`) · thiếu lối về Login ở Quên mật khẩu quản trị (`AMB-LOGIN-14`) · `autocomplete` ở form Login cổng (`AMB-LOGIN-27`) · câu chữ bong bóng trình duyệt (`RISK-LOGIN-06`)
- **Đối chiếu kết quả chạy:** chưa có `docs/executions/login/` — không có ghi chú `⚠️ chưa có evidence` / `@NeedsVerify` nào đã được giải quyết để dọn
- **Treo từ vòng 1:** gap #3 · #4 · #8 (chờ chốt Expected) · 4 ô `⏭️` thiếu người quyết + danh sách kích thước / trình duyệt (`ASM-13`, `ASM-14`) · ghi chú *smoke có 1 BLOCKED* (`_011`) chưa vào index

> **Nhận định chung:** FIX vòng 1 đã xử lý xong 4 nhóm vấn đề hệ thống cũ — không còn TC phụ thuộc TC khác, không còn thao tác DevTools ở phần chính của TC có hệ quả nhìn thấy được, bộ smoke sạch `@NeedsVerify`. Không TC nào dưới 10 điểm. Vòng 2 bắt được **3 lỗi vòng 1 bỏ sót**, cả ba có thể gây **FAIL giả** khi chạy thật:
>
> 1. **6 TC có bước `Logout`** mà Pre-Condition không loại trừ bộ đếm giờ công việc đang chạy. `EMAIL_ADMIN` là tài khoản **cả lớp dùng chung** — chỉ cần người khác bật bộ đếm giờ là bấm `Logout` hiện popup cảnh báo (REQ-LOGIN-28) thay vì về trang Login. `_010` đã có điều kiện này, 6 TC còn lại thì không
> 2. **8 TC mở đầu bằng `Đăng nhập đúng`** mà không nói đăng nhập ở trang nào. Module có **hai** trang đăng nhập, và đăng nhập `EMAIL_ADMIN` ở trang cổng khách hàng là **bị từ chối** (REQ-LOGIN-60)
> 3. **`_052` bước 4** *"Đóng tab, mở tab mới trong cùng cửa sổ ẩn danh"* — cửa sổ ẩn danh mới chỉ có 1 tab; đóng tab đó là đóng cửa sổ, mất luôn cookie ngôn ngữ → Expected 4 FAIL dù hệ thống đúng
>
> Ở mức bộ: nhánh **State Transition** thiếu chuyển *đã đăng xuất → đăng nhập lại* — ô đó ở index trỏ vào `_020 bước 6`, tham chiếu đã sai từ khi FIX vòng 1 đánh lại số bước của `_020`.

---

## Chi tiết từng TC — ma trận điểm

> C1 Rõ ràng · C2 Kết quả đo được · C3 Độc lập · C4 Dữ liệu cụ thể · C5 Truy vết · C6 Đúng trọng tâm. Thang 0–2 mỗi tiêu chí. Chấm trên bản sau FIX vòng 1 (mốc `213d312`).

| TC ID | C1 | C2 | C3 | C4 | C5 | C6 | Điểm | Xếp loại |
|---|---|---|---|---|---|---|---|---|
| CRM_LOGIN_TC_001 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_002 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_003 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_004 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_005 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_006 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_007 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_008 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_009 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_010 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_011 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_012 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_013 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_014 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_015 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_016 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_017 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_018 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_019 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_020 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_021 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_022 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_023 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_024 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_025 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_026 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_027 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_028 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_029 | 1 | 1 | 2 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_030 | 1 | 2 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_031 | 1 | 2 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_032 | 1 | 2 | 1 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_033 | 1 | 2 | 1 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_034 | 1 | 2 | 1 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_035 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_036 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_037 | 1 | 2 | 1 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_038 | 1 | 2 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_039 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_040 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_041 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_042 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_043 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_044 | 1 | 1 | 2 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_045 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_046 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_047 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_048 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_049 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_050 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_051 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_052 | 1 | 2 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_053 | 1 | 2 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_054 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_055 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_056 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_057 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_058 | 1 | 2 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_059 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_060 | 1 | 1 | 2 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_061 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_062 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_063 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_064 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_065 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_066 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_067 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_068 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_069 | 1 | 1 | 2 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_070 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_071 | 1 | 1 | 2 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_072 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_073 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_074 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_075 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_076 | 2 | 1 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_077 | 2 | 2 | 1 | 1 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_078 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_079 | 1 | 2 | 1 | 2 | 2 | 2 | 10 | 🟢 |
| CRM_LOGIN_TC_080 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_081 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_082 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_083 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_084 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_085 | 1 | 2 | 2 | 2 | 2 | 2 | 11 | 🟢 |
| CRM_LOGIN_TC_086 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_087 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |
| CRM_LOGIN_TC_088 | 2 | 2 | 2 | 2 | 2 | 2 | 12 | 🟢 |

---

## Vấn đề & đề xuất sửa — các TC bị trừ điểm

| TC ID | Điểm | Vấn đề chính (trích nguyên văn) | Đề xuất sửa |
|---|---|---|---|
| CRM_LOGIN_TC_032 · 033 · 034 · 037 | 10/12 | **C3** — Pre-Condition chỉ *"Cửa sổ ẩn danh mới"* (`_032`, `_034` thêm *"DevTools đang mở"*), bước *"Bấm ảnh đại diện → `Logout`"* kỳ vọng *"Về trang Login quản trị"*. Pre-Condition và Test Data không nêu điều kiện *không có bộ đếm giờ đang chạy* — trên tài khoản dùng chung, có bộ đếm giờ thì `Logout` hiện popup (REQ-LOGIN-28). **C1** — bước 1 *"Đăng nhập đúng"* (hệ thống F) | Pre-Condition thêm *"tài khoản `EMAIL_ADMIN` không có bộ đếm giờ công việc nào đang chạy (có thì `Logout` hiện popup cảnh báo — REQ-LOGIN-28)"* — cùng điều kiện `_010` đã ghi. Bước 1 → *"Đăng nhập đúng tại trang Login quản trị `https://crm.anhtester.com/admin/authentication`"* |
| CRM_LOGIN_TC_079 | 10/12 | **C3** như trên (bước 3 `Logout`) · **C1** Expected 1 *"Trang Login hiện đủ thành phần như `CRM_LOGIN_TC_001`"* — người chạy trên Firefox / Safari / Edge phải mở TC khác mới biết đối chiếu gì | Pre-Condition thêm điều kiện bộ đếm giờ. Expected 1: *"đủ logo, tiêu đề `Login`, ô `Email Address`, ô `Password`, ô tích `Remember me`, nút `Login`, liên kết `Forgot Password?`"* |
| CRM_LOGIN_TC_077 | 10/12 | **C3** Pre-Condition *"Đã đăng nhập"* — không nêu cửa sổ, không loại trừ bộ đếm giờ dù bước 4 bấm `Logout` · **C4** không nêu tài khoản (Test Data chỉ có kích thước) | Pre-Condition: *"Cửa sổ ẩn danh mới · đã đăng nhập khu quản trị bằng `EMAIL_ADMIN` / `PASSWORD_ADMIN` (người chạy tự nhập), không tích `Remember me` · tài khoản không có bộ đếm giờ công việc nào đang chạy · DevTools → bật chế độ thiết bị (Toggle device toolbar)"* |
| CRM_LOGIN_TC_029 · 069 · 071 | 10/12 | **C1** bước 1 *"Đăng nhập đúng…"* — không nói trang nào (hệ thống F) · **C2** trần Mẫu B (xem dưới) | Bước 1 → *"Đăng nhập đúng tại trang Login quản trị `…`"*, giữ phần *"không tích / có tích `Remember me`"*. C2 giữ trần |
| CRM_LOGIN_TC_030 | 11/12 | **C1** bước 1 *"Đăng nhập đúng, **không** tích `Remember me`"* | Như trên |
| CRM_LOGIN_TC_052 | 11/12 | **C1** bước 4 *"Đóng tab, mở tab mới **trong cùng cửa sổ ẩn danh**, mở lại `…/authentication/login`"* — Pre-Condition là cửa sổ ẩn danh mới (1 tab). Làm đúng thứ tự chữ thì đóng tab = đóng cửa sổ ẩn danh = mất cookie ngôn ngữ → Expected 4 *"Vẫn tiếng Việt"* FAIL dù hệ thống đúng. Bước 4 cũng gộp 3 hành động | Tách: *"4. Mở tab mới trong cùng cửa sổ ẩn danh (Ctrl+T / Cmd+T) — **giữ** tab cũ"* · *"5. Ở tab mới, mở `https://crm.anhtester.com/authentication/login`"* · *"6. Chọn lại `English`"*. Expected 5 *"Vẫn tiếng Việt"* · 6 *"Trang về tiếng Anh, tiêu đề `Please login`"* |
| CRM_LOGIN_TC_053 | 11/12 | **C1** bước 1 *"Mở `…`, chọn Language `Vietnamese`"* · bước 3 *"Quay lại trang Login cổng, chọn `English`"* — mỗi bước 2 hành động | Tách 5 bước: mở Login cổng · chọn `Vietnamese` · mở Login quản trị · mở lại Login cổng · chọn `English`. Expected đánh lại số 2 · 3 · 5 |
| CRM_LOGIN_TC_058 | 11/12 | **C1** bước 1 *"Mở `…`, đặt Language theo biến thể"* (2 hành động) · bước 6 *"Đặt lại Language = `English`"* — trang Quên mật khẩu cổng **không** có ô Language, người chạy không biết làm ở đâu | Tách bước 1 thành mở trang + chọn Language. Bước dọn tách 2 bước: *"Mở `…/authentication/login`"* · *"Chọn Language `English` (chỉ cần với biến thể `b`)"*. Bước *"Bấm nút gửi"* ghi thêm *"(`Submit` ở biến thể `a`)"* |
| CRM_LOGIN_TC_038 | 11/12 | **C1** bước 6 *"Mở lại `…`, nhập lại Email và Password, bấm `Login`"* — 4 hành động một số | Tách: *"6. Mở lại `…`"* · *"7. Nhập lại Email"* · *"8. Nhập lại Password"* · *"9. Bấm `Login`"*. Expected 6 → 9 |
| CRM_LOGIN_TC_031 | 11/12 | **C1** bước 9 *"Mở task vừa tạo, dừng bộ đếm giờ"* (2 hành động — vòng 1 đề xuất tách, mới tách bước 1–4) · bước 8 *"Đăng nhập lại"* không có Expected | Tách bước 9: *"Mở task `auto_login_timer_1791100008`"* + *"Dừng bộ đếm giờ của task"*. Thêm Expected 8 *"Vào Dashboard"* |
| CRM_LOGIN_TC_085 | 11/12 | **C1** Expected 6 *"Hiện popup cảnh báo bộ đếm giờ như `CRM_LOGIN_TC_031` bước 6"* — trỏ sang TC khác thay vì ghi ra | Expected 6: *"**Chưa** đăng xuất; hiện popup nội dung `Started tasks timers found!` `Are you sure you want to logout without stopping the timers?` kèm nút `Logout`"* |
| CRM_LOGIN_TC_044 · 060 | 10/12 | Phần còn lại của nhận xét vòng 1: bước 3 *"Điền mật khẩu mới vào biểu mẫu đặt lại ⚠️ biểu mẫu chưa quan sát"* · Expected 4 *"Hệ thống xác nhận đã đổi mật khẩu ⚠️"* | **Không sửa được ở vòng này** — chỉ viết đích danh được sau khi recon trang đặt lại (chờ hộp thư test `RISK-LOGIN-03` / tài khoản contact `AMB-SYS-03`) |
| CRM_LOGIN_TC_026 · 028 · 070 · 076 | 11/12 | **C2** trần Mẫu B | Giữ nguyên |

**Mẫu B — chuyển thêm 3 TC.** Vòng 1 xếp `_026`, `_028`, `_029` vào Mẫu A (có hệ quả nghiệp vụ). Sau khi sửa, Expected chính của ba TC này là *"Hệ thống đã lưu dấu ghi nhớ đăng nhập, hạn 62 ngày (chi tiết ở 🔧)"* · *"T2 muộn hơn T1 (chi tiết ở 🔧)"* — che dòng `🔧` thì không chấm được. Không có cách quan sát bằng mắt: hạn cookie chính là thứ REQ-LOGIN-23, 25, 66 đặc tả, và cửa sổ ẩn danh mất cookie khi đóng nên không thử được kiểu *"đóng trình duyệt mở lại"*. → Chuyển sang **Mẫu B**, chấp nhận trần 11/12 cùng `_069`–`_071`, `_076`. Hệ quả nhìn thấy được của dấu ghi nhớ đã có ở `_027` (REQ-24) và `_034` (REQ-31).

**TC 12/12 có ghi chú nhỏ (không trừ điểm):**

- `_072` — Pre-Condition *"DevTools: xoá sạch cookie và xác nhận không còn cookie nào"* trong khi TC không có `@TechCheck`, và *"Cửa sổ ẩn danh mới"* (quy ước chung, part 01) đã bảo đảm không có cookie. Đề xuất: *"Cửa sổ ẩn danh mới (chưa có cookie nào của `crm.anhtester.com`)"*, bỏ phần DevTools
- `_038` — cách ngắt mạng thay thế *"(hoặc DevTools → Network: chọn `Offline`)"* nằm ở bước chính; giữ vì là lựa chọn phụ, cách chính đã là tắt Wi-Fi / rút cáp

---

## Vấn đề mang tính hệ thống

| # | Vấn đề | TC ảnh hưởng | Đề xuất |
|---|---|---|---|
| E | Bước `Logout` trên tài khoản dùng chung mà Pre-Condition không loại trừ bộ đếm giờ đang chạy → popup REQ-LOGIN-28 chặn đường về trang Login, FAIL giả | 6 TC: `_032` · `_033` · `_034` · `_037` · `_077` · `_079` | Pre-Condition thêm điều kiện như `_010` |
| F | Bước *"Đăng nhập đúng"* không nêu trang đăng nhập — module có 2 trang, `EMAIL_ADMIN` ở trang cổng bị từ chối (REQ-LOGIN-60) | 8 TC: `_029` · `_030` · `_032` · `_033` · `_034` · `_037` · `_069` · `_071` | Ghi *"tại trang Login quản trị `https://crm.anhtester.com/admin/authentication`"* — không đổi số bước, không đụng tham chiếu `🔧 bước N` |
| G | Bước gộp nhiều hành động / Expected trỏ sang TC khác thay vì ghi ra | 7 TC: `_031` · `_038` · `_052` · `_053` · `_058` · `_079` · `_085` | Tách bước; ghi nguyên văn thay cho *"như `_0xx`"* |
| C | *(treo từ vòng 1)* 4 ô `⏭️` ở *Đối soát Field-Level* ghi *"agent đề xuất, chờ QA lead duyệt 04-10-2026"* · danh sách kích thước (`ASM-13`) và trình duyệt (`ASM-14`) do agent chọn | Index | Cần người có thẩm quyền chốt — ghi *"Quyết định: <ai>, <ngày>"* + điều kiện rà lại |
| D | *(treo từ vòng 1)* `_011` (`Critical` `@Smoke`) luôn BLOCKED tới khi có tài khoản contact — index chưa ghi | Index | Ghi vào dòng *Tổng hợp nhanh* của index |

---

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | `_001`–`_004` (4 TC · 15 mục kiểm) — nhãn nguyên văn, thứ tự, mặc định, con trỏ, tiêu đề tab của cả 4 màn hình |
| 1 | Open form | ✅ | `_005`–`_007` · `_080` (4 TC · 5 case) — `_006` đã sạch `@NeedsVerify` |
| 1 | Display | ➖ | Đồng ý — module không hiển thị dữ liệu nghiệp vụ |
| 1 | Input valid data · Save | ✅ | `_008` · `_011` (`_011` ⏸️ chờ `AMB-SYS-03`) |
| 1 | Verify data | ✅ | `_009` · `_010` |
| 2 | UI Behavior | ✅ | `_023` · `_051` · `_084` · `_085` (4 TC · 10 case) — gap #5, #6 vòng 1 đã lấp |
| 2 | Required | ✅ | 7 TC · 11 case — từng ô bắt buộc + bỏ trống tất cả ở cả 4 form |
| 2 | Validation | 🟡 Nông | 11 TC · 29 case. Còn treo: gap #3 (khoảng trắng ở Email cổng) · gap #4 (Max length phía vượt) · 4 ô `⏭️` thiếu người quyết (hệ thống C). Index đang chấm ✅ *"Dropdown 5/5"* trong khi mục *Tất cả option* còn `⏭️` chờ duyệt |
| 2 | Equivalence Partitioning | ✅ | `_008` · `_015` · `_016` · `_019` · `_020` — `_016-b` đã đổi sang lớp hai dấu chấm liền nhau |
| 2 | Boundary Value Analysis | ➖ | Đồng ý — Field Spec mục 3 không khai `min` / `max` |
| 2 | Business Rule | ✅ | 14 TC · 15 case |
| 2 | Decision Table | ✅ | 8 rule ↔ 8 TC — khớp bảng ở index |
| 2 | State Transition | 🟡 Nông | Chuyển *Đã đăng xuất → Đăng nhập* **không có TC nào chấm**: index trỏ `_020 bước 6` (sau FIX vòng 1 bước 6 của `_020` là *"Nhập Password sai"*, và `_020` không đi qua trạng thái đã đăng xuất) · `_079` chỉ đăng nhập rồi đăng xuất · `_031` bước 8 đăng nhập lại nhưng không có Expected (gap #9) |
| 2 | Dependency | ✅ | `_051`–`_053` |
| 2 | Use Case / Scenario | ✅ | `_024` · `_043`→`_044` · `_059`→`_060` (2/3 luồng ⏸️ chờ hộp thư — `RISK-LOGIN-03`) |
| 2 | Save / Edit / Delete | ➖ | Đồng ý — không có CRUD |
| 2 | Error Guessing | ✅ | `_035`–`_038`. Gap #8 vẫn treo |
| 3 | Permission | ✅ | `_061` · `_062` · `_005` — đủ các ô trong phạm vi của Ma trận Phân quyền |
| 3 | Security | ✅ | `_063`–`_073` (11 TC · 18 case) + `_020` · `_032`–`_034` · `_054` |
| 3 | API · Database · Integration | ➖ | Khớp *Năng lực kiểm thử của QA* (chốt 03-10-2026) — đội Dev xác minh |
| 3 | Logging / Audit | ➖ | Requirements không có yêu cầu audit + tài khoản QA bị chặn nhật ký hoạt động |
| 4 | Compatibility | ✅ | `_079` (3 trình duyệt) — danh sách vẫn là giả định `ASM-14`, chưa có người chốt (hệ thống C) |
| 4 | Responsive / UI Stability | ✅ | `_077` · `_078` · `_081`–`_083` (5 TC · 11 case) — gap #1, #2 vòng 1 đã lấp. Danh sách kích thước chưa có người chốt (`ASM-13`) |
| 4 | Accessibility | ✅ | `_074`–`_076` · `_086`–`_088` (6 TC · 6 case) + `_001` 🔧 — gap #7 vòng 1 đã lấp |
| 4 | Performance | ➖ | Đồng ý — không có ngưỡng cam kết, đội Hạ tầng |
| 4 | Regression | ➖ | Đồng ý — chưa có `docs/bugs/login/` |
| 4 | E2E | ➖ | Đồng ý — chuyển `/generate-cross-module-test-plan` khi `DASH` có requirements |

> Không có `➖` nào thực chất là `⏭️`. Lỗi ghi nhãn còn lại vẫn là 4 ô `⏭️` thiếu người quyết (hệ thống C).

---

## Đối soát bảng 15 loại field

| Field (form) | Loại | Kết quả | Mục thiếu / ghi chú |
|---|---|---|---|
| Email Address — Login quản trị | Email | ✅ 9/9 mục có TC | Max length mới thăm dò **phía hợp lệ** (`_019-b`, 64 ký tự) — phía vượt là gap #4 (treo) |
| Password — Login quản trị | Password | ✅ 7/7 | `➖` hoa/thường · số · xác nhận mật khẩu — đúng là không áp cho form đăng nhập |
| Remember me — Login quản trị | Checkbox | ✅ 4/4 | — |
| Email Address — Quên MK quản trị | Email | 🟡 | Nhiều `@` · Max length: `⏭️` thiếu người quyết · Case sensitivity ⏸️ `RISK-LOGIN-03` |
| Language — Login cổng | Dropdown | 🟡 | Tất cả option: `⏭️` thiếu người quyết (24/26 ngôn ngữ chỉ đếm số lượng ở `_003`) |
| Email Address — Login cổng (ô chữ tự do) | Email | 🟡 | Khoảng trắng đầu/cuối · chỉ dấu cách — gap #3 (treo, cần chốt Expected) · Max length `⏭️` thiếu người quyết · Format hợp lệ / Case ⏸️ chờ contact |
| Password — Login cổng | Password | 🟡 | Required ✅ `_049`; còn lại `⏭️` thiếu người quyết |
| Remember me — Login cổng | Checkbox | ✅ | Mặc định `_003` · Check / Uncheck `_084` (gap #5 vòng 1 đã lấp) · Tác dụng ⏸️ chờ contact |
| Email Address — Quên MK cổng | Email | 🟡 | Nhiều `@` · Max length `⏭️` thiếu người quyết |

---

## Coverage Gaps (TC còn thiếu)

> TC mới cấp số **nối tiếp** từ `CRM_LOGIN_TC_089`. Gap #1, #2, #5, #6, #7 của vòng 1 đã lấp (`_078-d`, `_081`–`_088`). Gap ghi *"cần chốt Expected"* là hành vi chưa có trong REQ — chốt qua `/update-requirements-from-ticket` (mở AMB) **trước** khi viết TC.

| # | Kịch bản thiếu | Vòng / Nhánh | Priority đề xuất | Trạng thái |
|---|---|---|---|---|
| 9 | Đăng xuất rồi đăng nhập lại ngay trong cùng cửa sổ: sau `Logout` về trang Login, nhập lại `EMAIL_ADMIN` / `PASSWORD_ADMIN`, bấm `Login` → vào Dashboard `https://crm.anhtester.com/admin/`, không hiện trang lỗi / trang báo thao tác không hợp lệ. Lấp ô *Đã đăng xuất × Đăng nhập* của bảng State Transition — luồng người dùng thật đi nhiều nhất sau đăng nhập lần đầu | V2 · State Transition | Medium | **Mới** — đề xuất TC `_089` (REQ-LOGIN-27, 17) |
| 3 | Email Login cổng: khoảng trắng đầu/cuối · chỉ gồm dấu cách. Ô chữ tự do nên chuỗi tới máy chủ nguyên vẹn — chưa biết máy chủ báo lỗi định dạng hay sai thông tin → **cần chốt Expected** | V2 · Validation | Medium | Treo từ vòng 1 |
| 4 | Email Login quản trị: phần trước `@` dài **65** ký tự. Gợi ý cho lượt chạy thử: 65 vượt giới hạn 64 ký tự của chuẩn địa chỉ email — nếu máy chủ trả `The Email Address field must contain a valid email address.` thì đây là biến thể `c` của `_016` (cùng loại phản hồi), **không** phải `_019` | V2 · Validation (Max length) | Low | Treo từ vòng 1 — chạy thử, ghi ASM |
| 8 | Để trang Login quản trị mở quá hạn mã chống giả mạo (~1 giờ) rồi mới bấm `Login` → **cần chốt Expected** | V2 · Error Guessing | Low | Treo từ vòng 1 |

---

## TC trùng lặp — đề xuất merge

Không có cặp TC nào trùng hành vi cần merge. Ghi chú: `_048-b` và `_049-b` cùng một lần thao tác (bỏ trống cả hai ô ở Login cổng), khác ô quan sát và khác REQ (42 / 43) → giữ; người chạy làm một lần, chấm hai TC.

---

## Index — chỗ lệch cần đồng bộ

| # | Chỗ | Hiện tại | Đề xuất |
|---|---|---|---|
| I1 | *State Transition*, ô *Đã đăng xuất × Đăng nhập (không ghi nhớ)* | `_020` bước 6 · `_079` — cả hai không phải chuyển này | `_089` (nếu duyệt gap #9) · `_031` bước 8 |
| I2 | Dòng *Tổng hợp nhanh* | *"11 TC `@Smoke`"* | Thêm *"`_011` BLOCKED có chủ đích tới khi có tài khoản contact (`AMB-SYS-03`) — smoke đạt = 10 PASS + 1 BLOCKED"* (hệ thống D vòng 1) |
| I3 | *Đối soát 4 vòng* — V2 Validation · State Transition | ✅ · ✅ | Validation 🟡 kèm lý do treo · State Transition ✅ sau khi có `_089` |

---

## Ưu tiên (Priority)

Không đổi so với vòng 1: 4 `Critical` là 4 luồng sống còn, TC `@Security` đều `High`, TC bổ sung vòng 1 (`_080`–`_088`) nhận `Medium` / `Low` hợp với rủi ro. `_089` đề xuất `Medium`.

---

## Kết luận & Khuyến nghị

1. **Sửa hệ thống E (6 TC)** — rủi ro FAIL giả cao nhất trên môi trường dùng chung, chỉ thêm một mệnh đề vào Pre-Condition
2. **Sửa hệ thống F + `_052`** — 9 TC, chỉ đổi chữ, không đổi số bước
3. **Thêm `_089`** lấp State Transition, sửa tham chiếu `_020 bước 6` ở index
4. **Chốt 4 ô `⏭️` + `ASM-13` / `ASM-14`** — treo từ 04-10-2026, quyết định phạm vi vẫn chưa có người chịu trách nhiệm
5. **Gap #3, #8** mở AMB qua `/update-requirements-from-ticket`; **gap #4** chạy thử 1 lần rồi ghi ASM; **`_044`, `_060`** viết lại khi có hộp thư test

---

## Kết quả Mode FIX vòng 2 — 10-10-2026

> Sửa **tại chỗ** trong `part_02_web_quan_tri.md`, `part_04_web_ky_thuat_phi_chuc_nang.md` và index. Mốc git trước khi sửa: `213d312` — xem bản cũ bằng `git show 213d312:docs/testcases/login/web/parts/<file>`. Không TC ID nào bị đổi, không thêm / bỏ TC, không đổi số bước nào (tham chiếu `🔧 bước N` giữ nguyên).
>
> User duyệt: nhóm **E + F** (10 TC — 6 + 8, trùng 4 TC; câu hỏi checkpoint ghi nhầm 12) và **chốt hệ thống C**.

| Hạng mục | TC | Đã làm |
|---|---|---|
| Hệ thống E — điều kiện bộ đếm giờ | `_032` · `_033` · `_034` · `_037` · `_077` · `_079` | Pre-Condition thêm *"tài khoản `EMAIL_ADMIN` **không** có bộ đếm giờ công việc nào đang chạy (… có thì `Logout` sẽ hiện popup cảnh báo, REQ-LOGIN-28)"* |
| `_077` — Pre-Condition đầy đủ | `_077` | *"Cửa sổ ẩn danh mới · đã đăng nhập khu quản trị bằng `EMAIL_ADMIN` / `PASSWORD_ADMIN` (người chạy tự nhập), **không** tích `Remember me` · …"* |
| Hệ thống F — trang đăng nhập | `_029` · `_030` · `_032` · `_033` · `_034` · `_037` · `_069` · `_071` | Bước 1 → *"Đăng nhập đúng tại trang Login quản trị `https://crm.anhtester.com/admin/authentication`"*, giữ phần tích / không tích `Remember me` |
| Hệ thống C — chốt phạm vi | Index | 4 ô `⏭️` ở *Đối soát Field-Level* · `ASM-13` · `ASM-14` → *"Quyết định: QA lead — user xác nhận trong phiên review 10-10-2026"* + điều kiện rà lại (giữ nguyên điều kiện cũ; `ASM-13` / `ASM-14` rà lại khi PO cam kết danh sách thiết bị / trình duyệt hỗ trợ). Dòng V4 Compatibility · Responsive của *Đối soát 4 vòng* ghi người chốt |
| Nhật ký | Index · `docs/testcases/README.md` | 1 dòng Nhật ký thay đổi ở index (mốc `213d312`) · 1 dòng Nhật ký danh mục |

**Điểm sau FIX:** 72 TC 12/12 · 14 TC 11/12 · 2 TC 10/12 — trung bình **11,8/12** (trước FIX 11,6). Phép thử 6b không cần chạy lại — không đổi REQ ID của TC nào.

**Chưa làm — user chưa duyệt ở vòng này:**

- Nhóm G — `_031`, `_038`, `_052`, `_053`, `_058`, Expected 1 của `_079`, `_085`. ⚠️ Riêng **`_052` bước 4** vẫn có thể FAIL giả (đóng tab cuối của cửa sổ ẩn danh) — nên sửa trước lần chạy đầu tiên
- `_072` — bỏ phần DevTools thừa ở Pre-Condition
- TC mới `_089` (gap #9 — đăng nhập lại sau Logout) · sửa tham chiếu `_020` bước 6 ở bảng State Transition của index · ghi chú *smoke có 1 BLOCKED* (`_011`) ở *Tổng hợp nhanh* · chấm lại V2 Validation 🟡 ở index
- Gap #3, #8 (chờ mở AMB qua `/update-requirements-from-ticket`) · gap #4 (chờ chạy thử) · `_044`, `_060` (chờ hộp thư test / tài khoản contact)
