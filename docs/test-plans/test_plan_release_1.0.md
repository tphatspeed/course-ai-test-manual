# Master Test Plan — Perfex CRM · Release 1.0

## Kiểm soát tài liệu

### Thông tin tài liệu

| | |
|---|---|
| Mã tài liệu | `test_plan_release_1.0` |
| Phiên bản tài liệu | v1.0 |
| Trạng thái | 🟨 Draft — còn 25 ô chờ thông tin |
| Mức phân loại | ❓ Chờ QA Lead chọn (công khai · nội bộ · mật) |
| Ngày lập | 04-10-2026 |
| Ngày hiệu lực | — (chưa duyệt) |
| Người lập | Anh Tester — trưởng nhóm QA (agent hỗ trợ) |
| Người review | ❓ Chờ QA Lead chỉ định |
| Người phê duyệt | Anh Tester — QA Lead · ❓ (Product Owner, chưa có tên) — chữ ký ở mục 11 |
| Hệ thống · Build | Perfex CRM · v1.0.0 (theo phiếu — môi trường đã khảo sát chạy `app.version = 316`, xem ô treo 22) |
| Phiếu đầu vào | `docs/test-plans/test_plan_release_1.0.input.yaml` (bản lưu của `test_plan.config.yaml`) |
| Cấu trúc tài liệu | Biên soạn **theo cấu trúc** ISO/IEC/IEEE 29119-3 — Test Plan · phủ đủ nội dung điển hình của ISTQB CTFL v4.0 mục 5.1.1 · ánh xạ ở mục 12 |

> **Trạng thái hợp lệ:** 🟨 Draft (đang soạn / còn ô treo) → 🟦 Chờ duyệt (đã review, không còn ô treo chặn) → 🟩 Đã duyệt (đủ chữ ký mục 11) → ⬛ Hết hiệu lực (có bản mới thay thế). Sửa nội dung bản 🟩 → quay về 🟨, tăng phiên bản.

> **Ô còn treo (25)** — gom theo người trả lời:
>
> **QA Lead — Anh Tester**
> - **1.** Mức phân loại tài liệu (Kiểm soát tài liệu)
> - **2.** Người review plan (Kiểm soát tài liệu)
> - **3.** Duyệt 5 mục tiêu kiểm thử do agent đề xuất (1.1)
> - **4.** Mốc mới cho *Duyệt kế hoạch* và *Hoàn tất viết TC* — cả hai ngày trong phiếu (21-09-2026, 02-10-2026) đã qua, repo chưa có TC nào (7.1)
> - **5.** Ước lượng lại, bổ sung hạng mục khảo sát requirements cho `CUST` và `PRJ` (7.2)
> - **6.** Kiểm thử tương thích — có làm không; phiếu khai Firefox nhưng ô *Tương thích* để trống (3.2.1)
> - **7.** Kiểm thử khả năng truy cập — có làm không (3.2.1)
> - **8.** Kiểm thử khả dụng — có làm không (3.2.1)
> - **9.** Kiểm thử độ tin cậy & phục hồi — có làm không (3.2.1)
> - **10.** Chiến lược tự động hoá: mục tiêu · tầng kiểm thử · tiêu chí chọn TC · phần không tự động (3.7)
> - **11.** Chiến lược tự động hoá: kích hoạt chạy · người bảo trì (3.7)
> - **12.** Nhu cầu đào tạo của nhóm (6)
> - **13.** Đối chiếu cột tên mục 29119-3 ở 12.1 với bản chuẩn trước khi đem đi audit (12.1)
>
> **QA Lead + DevOps**
> - **14.** Hệ thống CI — phiếu ghi "GitLab Actions CI", không có công cụ tên như vậy; user chốt để ❓ (3.7 · 5.3)
>
> **QA Lead + Dev Lead**
> - **15.** Quy trình trạng thái lỗi: mặc định hay workflow Jira riêng (9.1)
> - **16.** Thang Severity / Priority: mặc định hay riêng (9.2 · 9.3)
> - **17.** Người phân loại lỗi và tần suất họp phân loại (9.4)
> - **18.** Thời hạn phản hồi / sửa xong theo Severity (9.4)
>
> **Đội DEV**
> - **19.** Dữ liệu nền · nguồn dữ liệu · người cung cấp (5.2)
> - **20.** Môi trường riêng có dữ liệu thật của khách hàng không (5.2)
> - **21.** Dọn dữ liệu sau khi chạy · làm mới dữ liệu nền (5.2)
> - **22.** Build v1.0.0 trên môi trường riêng ứng với phiên bản Perfex nào — tài liệu `LOGIN` khảo sát trên `app.version = 316` (5.1)
> - **23.** Tài khoản test trên môi trường riêng: tài khoản Staff cho QA · tài khoản contact (`AMB-SYS-03`) · hộp thư test (`RISK-LOGIN-03`) (4.1 #3)
>
> **Product Owner**
> - **24.** Tên Product Owner ở bảng bên liên quan và bảng phê duyệt (2.4 · 11)
> - **25.** Cấp tài khoản Admin, hoặc chấp nhận để treo `AMB-SYS-01` cho `CUST` · `PRJ` — ma trận phân quyền cột Admin (4.1 #7)

### Lịch sử thay đổi

| Phiên bản | Ngày | Người sửa | Mục thay đổi | Nội dung | Người duyệt |
|---|---|---|---|---|---|
| v1.0 | 04-10-2026 | Anh Tester (agent hỗ trợ) | Toàn bộ | Lập mới. Trước khi lập đã sửa phiếu theo xác nhận của user ngày 04-10-2026: danh sách 23 module ngoài phạm vi lấy theo danh mục · bỏ dòng Book API (repo không có) · làm rõ *Cổng khách hàng* = module `PORTAL`, trang login cổng thuộc `LOGIN` · sửa giả định ước lượng theo số liệu repo · bỏ "GitLab Actions CI" | ❓ |

---

## 1. Mục tiêu & Cơ sở kiểm thử

### 1.1 Mục tiêu kiểm thử

> ⚠️ Phiếu để trống ô `Mục tiêu` → 5 mục tiêu dưới đây do **agent đề xuất**, **chờ QA Lead / PO duyệt** (ô treo 3).

| # | Mục tiêu | Đo bằng |
|---|---|---|
| O1 | Xác nhận đăng nhập, phiên, Remember me, đăng xuất, quên mật khẩu của khu quản trị và trang đăng nhập cổng khách hàng (`LOGIN`) đúng 72 REQ trên web | Tiêu chí exit #3, #4, #6, #7 |
| O2 | Xác nhận quản lý khách hàng (`CUST`) và dự án (`PRJ`) trên web đúng requirements được khảo sát trong đợt | Tiêu chí exit #3, #4, #6, #7 |
| O3 | Không còn bug Critical đang mở, và bug Major nào còn mở đều có workaround được PM chấp nhận, ở 3 module trước ngày phát hành 15-12-2026 | Tiêu chí exit #1, #2 |
| O4 | Theo dõi 8 REQ `LOGIN` đang vi phạm (`RISK-LOGIN-07`) tới khi Dev sửa xong hoặc người có quyền ở 4.2 chấp nhận rủi ro | Tiêu chí exit #1, #2 · chỉ số *Lỗi* ở 3.6 |
| O5 | Có bộ Smoke và regression tự động cho `LOGIN` · `CUST` · `PRJ` × web, chạy được trước vòng hồi quy 20-11-2026 | Chỉ số *Tự động hoá* ở 3.6 |

### 1.2 Cơ sở kiểm thử (Test basis)

| Tài liệu | Phiên bản / ngày cập nhật | Module | Ghi chú |
|---|---|---|---|
| [`docs/requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md`](../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) + [`web/requirements_login_web.md`](../requirements/login/web/requirements_login_web.md) | Nhật ký thay đổi 03-10-2026 (đợt 4 — `PO-REPLY-LOGIN-20261003-03`) | `LOGIN` | 72 REQ (🟢 58 · 🟡 8 · 🔴 0 · ⚪ 6) · 0 AMB treo · 7 RISK · khảo sát UI, không có tài liệu gốc |
| [`docs/requirements/_discovery/modules/module_03_khach_hang.md`](../requirements/_discovery/modules/module_03_khach_hang.md) | Khảo sát 03-10-2026 | `CUST` | **Chỉ tầng khám phá** — chưa có REQ (ước ~60) |
| [`docs/requirements/_discovery/modules/module_04_du_an.md`](../requirements/_discovery/modules/module_04_du_an.md) | Khảo sát 03-10-2026 | `PRJ` | **Chỉ tầng khám phá** — chưa có REQ (ước ~70) |
| [`docs/requirements/_discovery/system_map.md`](../requirements/_discovery/system_map.md) | 03-10-2026 | Cả hệ thống | Phụ thuộc giữa module · AMB-SYS / RISK-SYS |

> Cơ sở kiểm thử **đổi giữa đợt** (ticket sửa yêu cầu) → cập nhật bằng `/update-requirements-from-ticket` rồi tăng phiên bản plan — TC viết theo cơ sở cũ là TC sai. Requirements `CUST` · `PRJ` sinh trong đợt → bổ sung dòng vào bảng này, tăng phiên bản plan.

## 2. Phạm vi

### 2.1 Trong phạm vi

> Mỗi dòng là **một module × một nền tảng** — đơn vị báo cáo tiến độ theo dõi và báo cáo tổng hợp chấm tiêu chí exit #7. Hệ thống: Perfex CRM (repo chỉ có một hệ thống). Số liệu đọc ngày 04-10-2026.

| Module | Prefix | Nền tảng | Số REQ | Số TC hiện có | Đã từng chạy? | Ghi chú |
|---|---|---|---|---|---|---|
| Đăng nhập | `LOGIN` | Web | 72 (🟢 58 · 🟡 8 · ⚪ 6) | 0 | Chưa | Gồm khu quản trị **và** trang Login / Quên mật khẩu của cổng khách hàng (STORY-LOGIN-06, 07). 8 REQ đang vi phạm — TC sẽ FAIL có chủ đích (`RISK-LOGIN-07`) |
| Customers | `CUST` | Web | 0 — ⬜ chưa khảo sát | 0 | Chưa | Ước ~60 REQ · 9 module phụ thuộc · phải chạy `/generate-requirements-from-website customers` trước khi viết TC |
| Projects | `PRJ` | Web | 0 — ⬜ chưa khảo sát | 0 | Chưa | Ước ~70 REQ · 12 tab, status flow 5 trạng thái · phải chạy `/generate-requirements-from-website projects` trước khi viết TC |

**REQ cần quyết định lại** — REQ ⚪ từng bị loại vì môi trường dùng chung:

| REQ | Nội dung | Lý do bị loại trước đây | Quyết định theo phiếu |
|---|---|---|---|
| REQ-LOGIN-38 | Email tồn tại thì hệ thống gửi email đặt lại mật khẩu | Gửi email đặt lại tới tài khoản dùng chung · QA không xem được hộp thư (`RISK-LOGIN-01`, `RISK-LOGIN-03`) | **Đưa lại** vào phạm vi (`Đưa lại REQ bị loại vì môi trường: có`). Vẫn cần hộp thư test (ô treo 23), chưa có thì TC BLOCKED |
| REQ-LOGIN-39 | Liên kết trong email đặt lại mật khẩu dùng được để đặt mật khẩu mới | Như REQ-LOGIN-38 | **Đưa lại** — cùng điều kiện như REQ-LOGIN-38 |

> 4 REQ ⚪ còn lại (REQ-LOGIN-50, 57, 58, 59) bị loại vì **thiếu tài khoản contact** (`AMB-SYS-03`), không phải vì môi trường. Chúng vẫn nằm trong phạm vi và sẽ BLOCKED tới khi có tài khoản (ô treo 23).

### 2.2 NGOÀI phạm vi (out of scope)

| Không kiểm thử | Lý do | Ai chịu trách nhiệm | Nguồn quyết định |
|---|---|---|---|
| 23 module CRM còn lại: `PROFILE` · `NAV` · `DASH` · `TASK` · `ITEM` · `PROP` · `EST` · `INV` · `PAY` · `CRN` · `SUB` · `CTR` · `EXP` · `LEAD` · `ESTREQ` · `TICKET` · `KBASE` · `UTIL` · `CAL` · `NEWS` · `RPT` · `STAFF` · `SETTING` | Không thuộc Release 1.0 | Đợt sau | Phiếu — Anh Tester (QA Lead), 17-09-2026 |
| Cổng khách hàng **sau đăng nhập** — module `PORTAL` | Loại khỏi phạm vi Release 1.0 | Chưa xếp đợt — chờ PO chốt phạm vi cổng khách hàng | Phiếu — Anh Tester (QA Lead), 17-09-2026 · user làm rõ ranh giới với `LOGIN` ngày 04-10-2026 |
| Cột **Admin** của ma trận phân quyền `LOGIN` | PO chốt chỉ test bằng tài khoản `EMAIL_ADMIN` (đo được là Staff) | PO | `AMB-LOGIN-01` ✅ — `REQUIREMENTS_LOGIN_SUMMARY.md` mục 3 |
| Đổi mật khẩu khi đã đăng nhập · xác thực 2 lớp | Thuộc `PROFILE` — ngoài phạm vi đợt | Đợt sau | `REQUIREMENTS_LOGIN_SUMMARY.md` mục 1 |
| Cấu hình bảo mật đăng nhập trong Setup | Thuộc `SETTING` — Access denied (`AMB-SYS-01`) | Đợt sau | `REQUIREMENTS_LOGIN_SUMMARY.md` mục 1 |
| Nền tảng **mobile** | Hệ thống chỉ có mặt Web · phiếu `Cách chạy mobile: ngoài phạm vi` | — | `system_map.md` mục 1 · phiếu |
| Nền tảng **API** — kiểm thử trực tiếp | QA không có quyền gọi API · phiếu `Cách chạy API: ngoài phạm vi` | Đội Dev | `docs/requirements/README.md` — Năng lực kiểm thử của QA · phiếu |
| Vòng 3 kỹ thuật: truy vấn CSDL · tầng tích hợp · nhật ký hoạt động | QA không có quyền (nhật ký → Access denied) | Đội Dev xác minh | `docs/requirements/README.md` — Năng lực kiểm thử của QA · `RISK-SYS-04` |
| Kiểm thử hiệu năng | Phiếu `Hiệu năng: không` | ❓ Phiếu không nêu ai chịu | Phiếu |
| Kiểm thử bảo mật chuyên sâu (pentest) | Phiếu `Bảo mật chuyên sâu: không`. REQ bảo mật đã có trong `LOGIN` (cookie, CSRF, không lộ email tồn tại) **vẫn** được kiểm như yêu cầu chức năng | ❓ Phiếu không nêu ai chịu | Phiếu |
| Cấp độ Component · Component integration | Thuộc quy trình Dev | Đội Dev | Phiếu `Cấp độ kiểm thử` |

> Mục này đã được thống nhất với ❓ (người duyệt — mục 11) ngày ❓. Thay đổi phạm vi phải cập nhật tài liệu, tăng phiên bản và thông báo lại.

### 2.3 Giả định & Ràng buộc

| Loại | Nội dung | Ảnh hưởng tới kiểm thử | Nguồn |
|---|---|---|---|
| Ràng buộc | Đợt này chạy trên **môi trường test riêng**, khác môi trường **dùng chung** lúc khảo sát `LOGIN` | Được thao tác phá huỷ trên bản ghi do test tạo. Các ràng buộc tài liệu `LOGIN` đặt ra vì môi trường dùng chung (ca lỗi tránh email thật, không đặt lại mật khẩu, không chạy song song REQ-LOGIN-62) nới được, nhưng phải chạy lại để kiểm tài liệu có lệch không — rủi ro R4 | Phiếu `Dùng chung với đội khác: không` · README `Môi trường dùng chung: Có` |
| Ràng buộc | Chỉ dùng tài khoản Staff (`EMAIL_ADMIN`). Không có tài khoản Admin, không có tài khoản contact | REQ-LOGIN-50, 57, 58, 59 BLOCKED. Phân quyền `CUST` · `PRJ` chỉ kiểm được vai trò Staff | `AMB-LOGIN-01` · `AMB-SYS-01` · `AMB-SYS-03` |
| Ràng buộc | QA không có quyền gọi API, truy vấn CSDL, kiểm tích hợp, xem nhật ký hoạt động. Có DevTools | Vòng 3 kỹ thuật do Dev xác minh. Không kiểm được email gửi đi | README — Năng lực kiểm thử của QA |
| Ràng buộc | Agent không nhập mật khẩu thật | Khi agent thực thi manual, người phải tự nhập bước đăng nhập. Automation đọc `.env` trên máy QA | `RISK-LOGIN-02` |
| Ràng buộc | REQ-LOGIN-67 (hạn phiên trượt 8 giờ) là TC kéo dài 8 giờ | Phải lên lịch riêng, không chạy chung lượt | `REQUIREMENTS_LOGIN_SUMMARY.md` mục 7 |
| Giả định | Ước lượng **không** tính công sức PO/BA làm UAT, **không** tính thời gian Dev sửa bug | Hai việc này trễ thì lịch trễ, nằm ngoài con số 7.2 | Phiếu `Ước lượng → Giả định` |
| Giả định | Ước lượng 7.2 **chưa** gồm công khảo sát requirements `CUST` · `PRJ` | Ước lượng thấp hơn thực tế — rủi ro R3 | Phiếu (đã sửa theo repo 04-10-2026) |
| Ràng buộc | Không có ngân sách riêng cho kiểm thử | — | Phiếu |

### 2.4 Các bên liên quan & Giao tiếp

| Bên | Vai trò trong đợt | Liên quan tới kiểm thử | Nhận gì | Tần suất | Kênh |
|---|---|---|---|---|---|
| Anh Tester | QA Lead — lập và duyệt plan · tự động hoá | Chịu trách nhiệm toàn đợt | Kế hoạch · báo cáo tiến độ · báo cáo tổng hợp | Hằng tuần thứ Sáu · cuối đợt | Jira · repo |
| ❓ (chưa có tên) | Product Owner / BA — duyệt plan · thực hiện UAT | Trả lời AMB · chấp nhận rủi ro · nghiệm thu | Kế hoạch · báo cáo tiến độ · báo cáo tổng hợp | Hằng tuần thứ Sáu · cuối đợt | Email · Jira |
| Đội DEV | Sửa bug · dựng và duy trì môi trường test riêng | Tiêu chí vào #2, #9 · Vòng 3 kỹ thuật | Báo cáo lỗi | Khi phát sinh | Jira |
| Hồng · Lan · Huệ | Kiểm thử viên (mục 6) | Viết TC · thực thi | *(phiếu không khai — nội bộ nhóm QA)* | — | — |

**Mẫu tài liệu dùng trong đợt** (phiếu `Dùng mẫu có sẵn của repo: có`):

| Tài liệu | Mẫu | Workflow sinh |
|---|---|---|
| Test case | Mẫu của `skills-rbt-manual-testing` | `/generate-testcases-manual-rbt` |
| Execution report | Mẫu của `skills-manual-test-executor` | `/execute-test-cases` |
| Bug report | Mẫu của `skills-bug-reporter` | `/create-bug-report` |
| Báo cáo tiến độ | Mẫu của `skills-test-progress-reporter` | `/generate-test-progress-report` |
| Báo cáo tổng hợp | Mẫu của `skills-test-summary-reporter` | `/generate-test-summary-report` |

## 3. Chiến lược kiểm thử (Test approach)

### 3.1 Cấp độ kiểm thử

| Cấp độ | Trong đợt? | Ai thực hiện | Tiêu chí vào/ra |
|---|---|---|---|
| Component (unit) | ❌ ngoài phạm vi QA | Đội Dev | Theo quy trình Dev |
| Component integration | ❌ ngoài phạm vi QA | Đội Dev | Theo quy trình Dev |
| System | ✅ | QA | Bộ chung mục 4 |
| System integration | ✅ | QA | Bộ chung mục 4 — phụ thuộc `CUST` ↔ `PRJ` (Project gắn Customer) |
| Acceptance (UAT) | ✅ 30-11-2026 → 04-12-2026 | PO/BA nội bộ, QA hỗ trợ | Bộ chung mục 4 (phiếu không khai tiêu chí riêng) |

### 3.2 Loại kiểm thử

| Loại test | Nền tảng | Có làm? | Cách làm | Ghi chú |
|---|---|---|---|---|
| Kiểm thử chức năng | Web | ✅ | Manual theo TC — `/execute-test-cases` | Toàn bộ TC của 3 module |
| Kiểm thử chức năng | Mobile | ❌ | — | Ngoài phạm vi (2.2) |
| Kiểm thử chức năng | API | ❌ | — | Ngoài phạm vi (2.2) |
| Regression | Web | ✅ 20-11 → 25-11-2026 | Bộ regression tự động + manual | Sau đóng băng mã nguồn |
| Retest bug | Web | ✅ | `/retest-fixed-bugs` | Critical/Major chạy mode FULL |
| Tích hợp liên module | Web | ✅ | `/generate-cross-module-test-plan` | Luồng `CUST` → `PRJ` |
| Automation | Web | ✅ | `/generate-automation-framework` → `/generate-automation-web` | Chi tiết 3.7 |
| Nghiệm thu (UAT) | Web | ✅ | PO/BA nội bộ thực hiện, QA hỗ trợ | |
| Vòng 3 — kỹ thuật | Web | Một phần | DevTools (cookie, network, header) ✅ · API / CSDL / tích hợp / nhật ký ❌ → đội Dev xác minh | Không phải vùng trắng |
| Hiệu năng · Bảo mật chuyên sâu | — | ❌ | — | Ngoài phạm vi (2.2) |

**Tỷ trọng manual/automation:** Chạy manual toàn bộ TC trên web · automate bộ Smoke và regression của `LOGIN`, `CUST`, `PRJ` bằng Playwright (theo phiếu) — chi tiết ở 3.7.

**Thứ tự ưu tiên thực thi:** `LOGIN × web` chạy trước (cửa vào của mọi module, đã có requirements) → `CUST × web` (9 module phụ thuộc, `PRJ` gắn vào Customer) → `PRJ × web`. Trong `LOGIN` theo thứ tự Story đã đề xuất ở `REQUIREMENTS_LOGIN_SUMMARY.md` mục 7.

#### 3.2.1 Kiểm thử phi chức năng

| Loại | Có làm? | Mục tiêu đo | Ngưỡng chấp nhận | Cách làm · công cụ | Môi trường | Ai thực hiện | TC đã có |
|---|---|---|---|---|---|---|---|
| Hiệu năng | ❌ | — | — | — | — | — | 0 — ngoài phạm vi (2.2) |
| Bảo mật | ❌ chuyên sâu | — | — | — | — | — | 0 — ngoài phạm vi (2.2). REQ bảo mật của `LOGIN` (cookie `HttpOnly`/`Secure`, CSRF, chống dò email) kiểm như chức năng |
| Tương thích | ❓ | ❓ | ❓ | ❓ — phiếu khai trình duyệt Google Chrome (chính) + Firefox | Môi trường test riêng | ❓ | 0 |
| Khả năng truy cập | ❓ | ❓ | ❓ | ❓ | ❓ | ❓ | 0 — có REQ-LOGIN-70 (thiếu `autocomplete`) liên quan |
| Khả dụng | ❓ | ❓ | ❓ | ❓ | ❓ | ❓ | 0 |
| Độ tin cậy & phục hồi | ❓ | ❓ | ❓ | ❓ | ❓ | ❓ | 0 |

### 3.3 Kỹ thuật thiết kế test

> Repo chưa có bộ TC nào cho 3 module trong phạm vi → kỹ thuật **xác định khi sinh TC** (`/generate-testcases-manual-rbt`, khung 4 vòng), cập nhật bảng này ở phiên bản plan kế tiếp.

| Kỹ thuật | Áp dụng ở đâu |
|---|---|
| Xác định khi sinh TC | `LOGIN` · `CUST` · `PRJ` × web |

### 3.4 Mức độc lập của kiểm thử

| | |
|---|---|
| Mức độ | Đội QA riêng trong tổ chức |
| Thể hiện ở đâu | Phiếu không mô tả thêm (ô `Ghi chú` để trống) |
| Giới hạn | UAT do PO/BA **nội bộ** thực hiện, không có bên ngoài tổ chức. QA Lead vừa lập vừa duyệt plan — chữ ký PO ở mục 11 là lớp duyệt độc lập duy nhất |

### 3.5 Retest & Regression

- Bug đã fix → `/retest-fixed-bugs`: Critical/Major chạy **mode FULL**, Minor/Trivial chạy mode RETEST
- Mỗi build mới → chạy bộ Smoke trước khi thực thi tiếp
- Regression trước release (20-11 → 25-11-2026) → bộ regression tự động `LOGIN` · `CUST` · `PRJ` + manual phần chưa tự động
- 8 REQ `LOGIN` đang vi phạm: TC giữ đúng AC, gắn nhãn lỗi đã biết, execution report tách nhóm "FAIL — lỗi đã biết" — **cấm** hạ AC cho xanh (`RISK-LOGIN-07`)

### 3.6 Chỉ số theo dõi

> Nhóm theo ISTQB CTFL v4.0 mục 5.3.1. `/generate-test-progress-report` báo cáo **đúng các chỉ số này** mỗi kỳ (hằng tuần, thứ Sáu).

| Nhóm | Chỉ số | Nguồn | Dùng để |
|---|---|---|---|
| Tiến độ kiểm thử | REQ đã khảo sát (`CUST`, `PRJ`) · TC đã viết / đã review · TC đã chạy / chưa chạy · PASS · FAIL · BLOCKED | `docs/requirements/` · `docs/testcases/` · `execution_report.md` | Báo cáo tiến độ · tiêu chí exit #3, #4, #5, #7 |
| Tiến độ dự án | Công sức thực tế so với ước lượng 7.2 | Báo cáo tiến độ | Phát hiện trễ sớm |
| Lỗi | Bug mới / đã fix / đang mở theo Severity · regression phát sinh · 8 lỗi đã biết của `LOGIN` | Jira (nguồn chính) · `docs/bugs/` | Tiêu chí exit #1, #2 |
| Độ phủ | REQ có TC · REQ Critical có TC PASS | `traceability_matrix.md` | Tiêu chí exit #6 |
| Tự động hoá | Số TC đã tự động / tổng TC chọn tự động · tỷ lệ PASS bộ Smoke, regression — **báo riêng**, không cộng vào pass rate manual | `reports/` (Allure) | Mục tiêu O5 |
| Rủi ro | Trạng thái từng rủi ro ở 8.1 | Báo cáo tiến độ | Kiểm soát rủi ro |

### 3.7 Chiến lược tự động hoá

| | |
|---|---|
| Mục tiêu | ❓ Phiếu để trống (ô treo 10). Gợi ý từ tỷ trọng phiếu: chạy Smoke mỗi build và rút thời gian regression của 3 module |
| Hiện trạng | **Đọc từ repo 04-10-2026:** chưa có project automation (không có `package.json` / `pom.xml` / `pyproject.toml`) · 0 script · chưa có file CI (`.github/workflows/` · `.gitlab-ci.yml` · `Jenkinsfile`) |
| Tầng kiểm thử (kim tự tháp) | ❓ (ô treo 10) — ISTQB CTFL v4.0 mục 5.1.6: càng lên tầng UI, test càng ít, chậm và dễ vỡ. QA không có quyền gọi API → đợt này chỉ tự động được tầng UI |
| Tiêu chí chọn TC để tự động | ❓ (ô treo 10) |
| Framework · report | Playwright + TypeScript + Allure report · output trong `reports/` |
| Hệ thống CI | ❓ (ô treo 14) |
| Người bảo trì | ❓ (ô treo 11) — mục 6 có Anh Tester vai trò *tự động hoá* |

**Phạm vi:**

| Tự động | Không tự động | Lý do không tự động |
|---|---|---|
| Bộ Smoke `LOGIN` · `CUST` · `PRJ` × web (theo phiếu) | ❓ (ô treo 10) | ❓ |
| Bộ regression `LOGIN` · `CUST` · `PRJ` × web (theo phiếu) | *Gợi ý từ tài liệu, chờ duyệt:* REQ-LOGIN-67 (chờ 8 giờ) · REQ-LOGIN-38, 39, 57, 58 (cần mở hộp thư) | Thời gian chạy quá dài · cần hệ thống ngoài QA không truy cập được |

**Kích hoạt chạy:**

| Bộ chạy | Khi nào | Môi trường | Ai xem kết quả | Fail thì |
|---|---|---|---|---|
| Smoke | ❓ (ô treo 11) | Môi trường test riêng | ❓ | Chặn thực thi manual — tiêu chí tạm dừng 4.3 |
| Regression | ❓ (ô treo 11) — phải chạy xong **trước 25-11-2026** (`Hồi quy đến`) | Môi trường test riêng | ❓ | Phân loại bằng `/run-and-fix-tests` — **không** sửa test để né bug |

**Nguyên tắc:**
- Script chỉ tính là xong khi đạt Definition of Done của `CLAUDE.md` — PASS ổn định ≥ 2 lần liên tiếp, đủ Allure metadata và screenshot
- Kết quả automation **báo riêng**, không cộng vào pass rate manual của tiêu chí exit #3, #4
- Test chập chờn → `/analyze-flaky-tests`, **không** chạy lại tới khi xanh · UI đổi → `/heal-locators` · yêu cầu đổi → `/update-automation-from-impact`

## 4. Tiêu chí Vào / Ra

### 4.1 Tiêu chí VÀO (Entry) — chưa đủ thì CHƯA bắt đầu test

> Nhóm theo ISTQB CTFL v4.0 mục 5.1.3: nguồn lực · testware · chất lượng ban đầu của đối tượng kiểm thử. Trạng thái tại ngày lập plan **04-10-2026**.

| # | Nhóm | Điều kiện | Trạng thái |
|---|---|---|---|
| 1 | Nguồn lực | Nhân lực ở mục 6 đã được phân công, đủ người cho mọi cặp module × nền tảng | ✅ Đạt — `LOGIN` Hồng · `CUST` Lan · `PRJ` Huệ · Anh Tester toàn bộ |
| 2 | Nguồn lực | Môi trường test sẵn sàng, có dữ liệu nền | ❓ Dự kiến 05-10-2026 (Đội DEV) · dữ liệu nền chưa mô tả (ô treo 19) |
| 3 | Nguồn lực | Tài khoản test đủ mọi vai trò trong phạm vi | ❌ Chưa — thiếu contact (`AMB-SYS-03`) và hộp thư test (`RISK-LOGIN-03`). Tài khoản Staff trên môi trường riêng: ❓ (ô treo 23) |
| 4 | Nguồn lực | Công cụ sẵn sàng: quản lý bug · quản lý kết quả · automation | 🟨 Một phần — Jira + repo có · project automation chưa có · CI ❓ |
| 5 | Nguồn lực | Ngân sách đã duyệt *(khi có ngân sách riêng)* | Không áp dụng — không có ngân sách riêng |
| 6 | Testware | Tài liệu requirements của mọi module × nền tảng trong phạm vi đã có | ❌ Chưa — `LOGIN` ✅ · `CUST` ⬜ · `PRJ` ⬜ |
| 7 | Testware | AMB 🔴 đã được giải đáp hoặc người duyệt chấp nhận treo | ❌ Chưa — `LOGIN` 0 AMB treo ✅ · `AMB-SYS-01` (Admin) và `AMB-SYS-03` (contact) 🔴 còn treo, ảnh hưởng 3 module (ô treo 25) |
| 8 | Testware | Test case đã viết và đã review — đủ từng nền tảng | ❌ Chưa — 0 TC (mốc 02-10-2026 đã qua) |
| 9 | Chất lượng ban đầu | Build đã deploy và truy cập được | ❓ Dự kiến 05-10-2026 |
| 10 | Chất lượng ban đầu | Smoke test đã pass | ❓ Chưa có bộ Smoke |
| 11 | Chất lượng ban đầu | *(có mobile)* Bản build app cài được trên thiết bị · app Flutter đã bật semantics | Không áp dụng |
| 12 | Chất lượng ban đầu | *(có API)* Snapshot spec khớp build đang test · có quyền gọi thử | Không áp dụng |

> ⚠️ Bắt đầu test khi chưa đạt tiêu chí vào là nguyên nhân số một khiến kết quả kiểm thử không dùng được — BLOCKED tràn lan, phải chạy lại từ đầu. Tại ngày lập, `CUST` · `PRJ` **chưa đạt** #6 và #8 → không thể bắt đầu thực thi hai module này ngày 06-10-2026 (rủi ro R1, R2).

### 4.2 Tiêu chí RA (Exit)

> Bảng mặc định lấy **nguyên văn** từ `skills-test-summary-reporter`. `/generate-test-summary-report` chấm lại **cả bảng mặc định lẫn bảng bổ sung**.

| # | Tiêu chí | Ngưỡng |
|---|---|---|
| 1 | Bug **Critical** đang mở | **0** |
| 2 | Bug **Major** đang mở | 0, hoặc có workaround được PM chấp nhận bằng văn bản |
| 3 | Pass rate TC **Priority High** | **≥ 95%** |
| 4 | Pass rate toàn bộ TC đã chạy | ≥ 90% |
| 5 | Tỷ lệ **BLOCKED** | ≤ 5% |
| 6 | REQ mức Critical có ít nhất 1 TC **PASS** | 100% |
| 7 | Module trong phạm vi release đã có TC và đã chạy — tính trên **từng cặp module × nền tảng** trong phạm vi | 100% |

**Tiêu chí bổ sung của dự án:** không có — phiếu `Tiêu chí ra bổ sung` để trống.

☑ Bộ mặc định  ☐ Bộ mặc định + bổ sung  ☐ Bộ tiêu chí riêng của dự án

> Gợi ý ISTQB CTFL v4.0 mục 5.1.3 cho bảng bổ sung nếu dự án muốn thêm: mật độ lỗi · đã thực hiện kiểm thử tĩnh (review requirements/TC) · mọi lỗi tìm thấy đã được báo cáo · toàn bộ regression đã được automate.

> ⚠️ **Dừng kiểm thử khi hết thời gian hoặc ngân sách** (ISTQB CTFL v4.0 mục 5.1.3): được coi là hợp lệ **chỉ khi** **Product Owner cùng Anh Tester — QA Lead** đã xem xét và **chấp nhận bằng văn bản** rủi ro phát hành mà chưa đạt đủ tiêu chí. Báo cáo tổng hợp khi đó ghi rõ tiêu chí nào chưa đạt và ai chấp nhận — **không** chấm lại thành "Đạt".

### 4.3 Tiêu chí TẠM DỪNG (Suspension) & tiếp tục

**Tạm dừng kiểm thử khi:** môi trường sập > 4 giờ · build lỗi không đăng nhập được · > 30% TC BLOCKED cùng một nguyên nhân · phát hiện bug Critical chặn luồng chính.

**Tiếp tục khi:** nguyên nhân đã xử lý, có build mới, và đã chạy lại smoke.

> Ngưỡng trên do agent đề xuất — **đã xác nhận** theo phiếu (`Dùng ngưỡng tạm dừng đề xuất: có`). Bỏ ngưỡng mobile vì đợt không có nền tảng mobile.

## 5. Môi trường, Dữ liệu & Công cụ

### 5.1 Môi trường kiểm thử

| | |
|---|---|
| Môi trường | Môi trường test riêng cho Release 1.0 — URL lưu ở `.env`, **không** ghi vào tài liệu này |
| Dùng chung với đội khác? | Không — TC phá huỷ được phép trên bản ghi do test tạo; dữ liệu test vẫn sinh ngẫu nhiên + truy vết được |
| Khác môi trường đã khảo sát? | **Có** — tài liệu `LOGIN` khảo sát trên môi trường **dùng chung**, Perfex `app.version = 316`. Phiên bản, cấu hình (ngôn ngữ, định dạng, `company_is_required`…) và dữ liệu trên môi trường riêng có thể khác → rủi ro R4 |
| Web — trình duyệt | Google Chrome (chính) · Firefox — theo phiếu. Tài liệu `LOGIN` khảo sát trên Google Chrome 154 / macOS, viewport `1600×750` |
| Người dựng · ngày sẵn sàng | Đội DEV · 05-10-2026 (Thứ Hai) |

### 5.2 Quản lý dữ liệu kiểm thử

| | |
|---|---|
| Dữ liệu nền | ❓ Chờ Đội DEV (ô treo 19) |
| Nguồn dữ liệu | ❓ (ô treo 19) |
| Tài khoản test | Vai trò Staff (`EMAIL_ADMIN` — mật khẩu ở `.env`). Trên môi trường riêng: ❓ (ô treo 23). Contact: chưa có (`AMB-SYS-03`) |
| Dữ liệu thật của khách hàng | ❓ (ô treo 20) — chưa xác nhận thì mọi ảnh evidence phải che phần thân bảng như lúc khảo sát |
| Quy tắc sinh dữ liệu | Random + traceable theo `CLAUDE.md` mục 7 — nhìn bản ghi biết test nào tạo (VD `auto_login_<timestamp>@auto.test`) |
| Dọn dữ liệu sau khi chạy | ❓ (ô treo 21) |
| Làm mới dữ liệu nền | ❓ (ô treo 21) |
| Người cung cấp | ❓ (ô treo 19) |

> 🔒 Dữ liệu thật của khách hàng lọt vào evidence (ảnh chụp, bug report) là rủi ro lộ dữ liệu. Chưa trả lời ô treo 20 thì coi như **có**.

### 5.3 Công cụ

| Mục đích | Công cụ | Ghi chú |
|---|---|---|
| Quản lý lỗi | Jira · file markdown trong repo (`docs/bugs/`) | **Nguồn chính khi lệch:** Jira cho trạng thái bug |
| Quản lý kết quả kiểm thử | Jira · file markdown trong repo (`docs/executions/`) | **Nguồn chính khi lệch:** repo cho execution report |
| Tự động hoá | Playwright · TypeScript · Allure report | Chi tiết ở 3.7 — chưa có project |
| CI | ❓ (ô treo 14) | Repo chưa có file CI |
| Khác | Playwright MCP — thực thi manual qua `/execute-test-cases` · trang xem kết quả `scripts/execution-viewer/bundle.html` · `scripts/bugs-viewer/bundle.html` | Có sẵn trong repo |

## 6. Nhân lực & Phân công

| Vai trò | Người | Module × nền tảng phụ trách | Ghi chú |
|---|---|---|---|
| Trưởng nhóm QA | Anh Tester | Toàn bộ | Lập plan · duyệt plan · báo cáo |
| Tự động hoá | Anh Tester | `LOGIN × web` · `CUST × web` · `PRJ × web` | Kiêm cùng vai trò QA Lead — rủi ro R7 |
| Kiểm thử viên | Hồng | `LOGIN × web` | |
| Kiểm thử viên | Lan | `CUST × web` | Cần khảo sát requirements trước khi viết TC |
| Kiểm thử viên | Huệ | `PRJ × web` | Cần khảo sát requirements trước khi viết TC |

**Nhu cầu đào tạo:** ❓ (ô treo 12) · **Nhu cầu tuyển thêm:** Không.

## 7. Lịch trình, Ước lượng & Ngân sách

### 7.1 Lịch trình & Mốc

| Mốc | Ngày | Điều kiện hoàn thành |
|---|---|---|
| Duyệt plan | 21-09-2026 (Thứ Hai) — ⚠️ **đã qua**, plan lập 04-10-2026 → cần ngày mới (ô treo 4) | Mục 11 có chữ ký |
| Hoàn tất viết & review TC | 02-10-2026 (Thứ Sáu) — ⚠️ **đã qua**, repo 0 TC → cần ngày mới (ô treo 4) | Đủ TC cho 3 cặp module × web |
| Môi trường sẵn sàng | 05-10-2026 (Thứ Hai) | Tiêu chí vào #2, #3 |
| Bắt đầu thực thi | 06-10-2026 (Thứ Ba) | Đạt toàn bộ tiêu chí vào 4.1 |
| Báo cáo tiến độ | Hằng tuần · thứ Sáu — kỳ đầu 09-10-2026 | `/generate-test-progress-report` — slug `release_1.0` |
| Code freeze | 19-11-2026 (Thứ Năm) | |
| Regression | 20-11-2026 (Thứ Sáu) → 25-11-2026 (Thứ Tư) | Bộ regression tự động chạy xong trong khoảng này |
| UAT | 30-11-2026 (Thứ Hai) → 04-12-2026 (Thứ Sáu) | PO/BA nội bộ |
| Báo cáo tổng hợp | 10-12-2026 (Thứ Năm) | `/generate-test-summary-report` — slug `release_1.0` |
| Release | 15-12-2026 (Thứ Ba) | |

> Không mốc nào rơi vào cuối tuần. Thứ tự các mốc hợp lệ.

### 7.2 Ước lượng công sức

| | |
|---|---|
| Kỹ thuật (ISTQB CTFL v4.0 mục 5.1.4) | Three-point estimation (ước lượng ba điểm) |
| Giả định của ước lượng | `LOGIN` đã có tài liệu 72 REQ, chưa có TC · `CUST` và `PRJ` chưa khảo sát requirements (0 REQ) — **ước lượng chưa gồm công khảo sát**, chờ QA Lead ước lượng lại (ô treo 5) · không tính công sức PO/BA làm UAT · không tính thời gian Dev sửa bug |

Công thức cho từng hạng mục: **E = (a + 4m + b) / 6** · **SD = (b − a) / 6**. Tổng: E tổng = cộng E từng hạng mục · SD tổng = cộng SD từng hạng mục (cách cộng thận trọng, cho khoảng rộng hơn cộng căn bậc hai). Đơn vị người-ngày, làm tròn 1 chữ số thập phân.

| Hạng mục | a (lạc quan) | m (khả năng nhất) | b (bi quan) | E = (a+4m+b)/6 | SD = (b−a)/6 |
|---|---|---|---|---|---|
| Viết & review TC cho `LOGIN` · `CUST` · `PRJ` × web | 7 | 8 | 9 | 8.0 | 0.3 |
| Thực thi manual trên Web | 5 | 6 | 7 | 6.0 | 0.3 |
| Retest bug và regression | 2 | 3 | 4 | 3.0 | 0.3 |
| Hỗ trợ UAT, lập báo cáo và quản lý đợt | 1 | 2 | 3 | 2.0 | 0.3 |
| **Tổng** | | | | **19.0 người-ngày** | **±1.3** |

**Đối chiếu năng lực:** 4 người (Anh Tester · Hồng · Lan · Huệ) × 33 ngày làm việc từ 06-10-2026 tới 19-11-2026 = **132 người-ngày** nếu toàn thời gian (phiếu không ghi tỷ lệ phân bổ) → **đủ** về con số so với 19.0 ± 1.3 người-ngày.

> ⚠️ Năng lực dư nhiều so với ước lượng **không** có nghĩa đợt an toàn: ước lượng thiếu hạng mục khảo sát `CUST` · `PRJ` (~130 REQ theo `system_map.md`), và việc viết TC (8 người-ngày) đáng lẽ xong trước 02-10-2026 nay phải dời vào cửa sổ thực thi (rủi ro R2, R3). Số trên giữ nguyên theo phiếu, không tự sửa.

### 7.3 Ngân sách

Không có ngân sách riêng — chi phí nằm trong ngân sách dự án (phiếu `Có ngân sách riêng: không`).

## 8. Rủi ro

### 8.1 Rủi ro DỰ ÁN & biện pháp

> Rủi ro **của việc kiểm thử** — nhóm theo ISTQB CTFL v4.0 mục 5.2.2: tổ chức · con người · kỹ thuật · nhà cung cấp. Khả năng / Ảnh hưởng là **đề xuất của agent** — người duyệt xác nhận. `/generate-test-progress-report` theo dõi trạng thái từng dòng.

| # | Nhóm | Rủi ro | Khả năng | Ảnh hưởng | Biện pháp | Nguồn phát hiện |
|---|---|---|---|---|---|---|
| R1 | Tổ chức | `CUST` · `PRJ` chưa khảo sát requirements trong khi thực thi bắt đầu 06-10-2026 → không có cơ sở viết TC, tiêu chí vào #6 không đạt | Cao | Cao | Chạy `/generate-requirements-from-website customers` rồi `projects` ngay khi môi trường riêng sẵn sàng · `LOGIN` bắt đầu thực thi trước · chốt lại ngày bắt đầu thực thi của `CUST` · `PRJ` | README danh mục — `CUST`, `PRJ` ⬜ |
| R2 | Tổ chức | Mốc *Hoàn tất viết TC* 02-10-2026 đã qua, repo 0 TC → thực thi 06-10-2026 không có TC để chạy | Cao | Cao | Đặt mốc mới (ô treo 4) · viết TC `LOGIN` trước (đã đủ requirements, 0 AMB treo) | Repo — không có `docs/testcases/` |
| R3 | Tổ chức | Ước lượng 19.0 người-ngày dựa trên giả định cũ, thiếu công khảo sát ~130 REQ → lịch thực tế dài hơn kế hoạch | Cao | Trung bình | QA Lead ước lượng lại (ô treo 5) · theo dõi công sức thực tế từng kỳ ở báo cáo tiến độ | Phiếu ↔ repo lệch, user xác nhận 04-10-2026 |
| R4 | Kỹ thuật | Môi trường riêng khác môi trường đã khảo sát (dùng chung, `app.version = 316`) → tài liệu `LOGIN` lệch, TC sai | Trung bình | Trung bình | Chạy smoke + đối chiếu nhanh REQ `LOGIN` khi môi trường sẵn sàng · lệch thì `/update-requirements-from-ticket` | Phiếu `Dùng chung: không` ↔ README `Có` |
| R5 | Nhà cung cấp | Thiếu tài khoản contact, hộp thư test, tài khoản Admin → REQ-LOGIN-38, 39, 50, 57, 58, 59 BLOCKED · phân quyền `CUST`/`PRJ` chỉ kiểm được Staff | Cao | Trung bình | Xin cấp trước 06-10-2026 (ô treo 23, 25) · không có thì PO chấp nhận treo bằng văn bản | `AMB-SYS-01` · `AMB-SYS-03` · `RISK-LOGIN-03` |
| R6 | Nhà cung cấp | Môi trường sẵn sàng 05-10-2026 nhưng dữ liệu nền, nguồn dữ liệu, người cung cấp chưa chốt → BLOCKED đầu đợt | Trung bình | Cao | Đội DEV trả lời ô treo 19–21 trước 05-10-2026 | Phiếu `Dữ liệu kiểm thử` trống |
| R7 | Con người | Anh Tester kiêm QA Lead, tự động hoá, phụ trách toàn bộ — một người nghẽn thì cả đợt nghẽn | Trung bình | Trung bình | Chốt người bảo trì automation (ô treo 11) · phân tỷ lệ thời gian rõ ràng | Phiếu `Nhân lực` |
| R8 | Kỹ thuật | Automation chưa có project, chưa có CI, chiến lược chưa chốt → bộ regression có thể không kịp trước 25-11-2026 | Trung bình | Trung bình | Dựng framework bằng `/generate-automation-framework` sớm · chốt CI (ô treo 14) · không kịp thì regression chạy manual | Repo — không có `package.json`, file CI |
| R9 | Tổ chức | Plan chưa có tên PO để duyệt, mốc duyệt 21-09-2026 đã qua → thực thi bắt đầu khi plan chưa được duyệt | Cao | Trung bình | Bổ sung tên PO (ô treo 24) · gửi duyệt trước 06-10-2026 | Phiếu `Phê duyệt` |
| R10 | Kỹ thuật | QA không có quyền API / CSDL / tích hợp / nhật ký → lỗi ở tầng dưới UI không được QA phát hiện | Trung bình | Trung bình | Đội Dev xác minh nhánh Vòng 3 · ghi rõ trong TC phần do Dev kiểm | README — Năng lực kiểm thử của QA |
| R11 | Tổ chức | 4 loại phi chức năng chưa chốt, phiếu khai Firefox nhưng *Tương thích* trống → phạm vi trình duyệt mơ hồ, dễ cãi nhau lúc nghiệm thu | Thấp | Trung bình | QA Lead chốt ô treo 6–9 trước khi viết TC | Phiếu `Loại kiểm thử` |

### 8.2 Rủi ro SẢN PHẨM — tóm tắt

> **Nguồn chính** vẫn là tài liệu requirements (`RISK-<MODULE>-xx`) và tài liệu test case (đánh giá RBT) của từng module — bảng này chỉ **tóm tắt** để người duyệt plan thấy ngay. Sửa rủi ro ở tài liệu nguồn, **không** sửa ở đây. `CUST` · `PRJ` chưa có tài liệu module → lấy mức rủi ro cấp module ở `system_map.md`.

| Module | Rủi ro | Mức | Kiểm soát bằng | Nguồn |
|---|---|---|---|---|
| `LOGIN` | 8 REQ đang vi phạm (34, 36, 53, 55, 63, 64, 69, 70): lộ email tồn tại qua Quên mật khẩu · cookie `autologin` thiếu `HttpOnly` · thiếu `autocomplete`… | Nguồn không ghi mức (module 🔴 theo `system_map.md`) | TC giữ đúng AC, gắn nhãn lỗi đã biết · tiêu chí exit #1, #2 | [`RISK-LOGIN-07`](../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) |
| `LOGIN` | Không kiểm được email đặt lại mật khẩu | Nguồn không ghi mức | Hộp thư test hoặc Dev xác minh | [`RISK-LOGIN-03`](../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) |
| `LOGIN` | Trạng thái cookie đan xen giữa các test · toast cổng tự ẩn sau 3,5 s → FAIL giả | Nguồn không ghi mức | Mỗi test một context mới · assert ngay sau điều hướng | [`RISK-LOGIN-04`, `RISK-LOGIN-05`](../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) |
| `CUST` | Dữ liệu cá nhân khách hàng (email, điện thoại, địa chỉ) · 9 module phụ thuộc · Import, Bulk Actions · Vault lưu thông tin nhạy cảm | 🔴 | Khảo sát requirements + RBT trong đợt | [`module_03_khach_hang.md`](../requirements/_discovery/modules/module_03_khach_hang.md) |
| `PRJ` | 12 tab · status flow 5 trạng thái · Dashboard và Reports đọc số liệu từ đây · số Summary (191) lệch số bảng (184) chưa giải thích | 🔴 | Khảo sát requirements + RBT trong đợt · mở AMB nếu lệch vẫn còn | [`module_04_du_an.md`](../requirements/_discovery/modules/module_04_du_an.md) |
| Cả hệ thống | CSRF token nằm trong query string của request GET | 🟡 | Chuyển đội Dev | [`AMB-SYS-06`](../requirements/_discovery/system_map.md#8-ambiguity--risk-cấp-hệ-thống) |

## 9. Quản lý lỗi

> Nội dung theo ISTQB CTFL v4.0 mục 5.5. Mục này là **phần mở rộng** so với khung 29119-3 — xem 12.1.
>
> ⚠️ Phiếu để trống `Quy trình trạng thái`, `Thang Severity`, `Thang Priority` → quy trình và thang dưới đây là **mặc định, chờ QA Lead + Dev Lead xác nhận** (ô treo 15, 16). Jira là nguồn chính cho trạng thái bug — workflow Jira khác sơ đồ này thì thay theo Jira.

### 9.1 Quy trình trạng thái lỗi

```text
TC FAIL ──/create-bug-report──→ 🔴 Đang mở ──Dev sửa──→ 🟡 Đã fix — chờ retest
                                    ▲                           │
                                    │                   /retest-fixed-bugs
                                    │                           │
                     NOT_FIXED ─────┤           ┌───────────────┼────────────────┐
                     PARTIAL   ─────┘         FIXED                      CANNOT_VERIFY
                                                │                   (giữ trạng thái, ghi lý do)
                                                ▼
                                            ⬛ Đóng
```

| Trạng thái | Ai chuyển | Điều kiện |
|---|---|---|
| 🔴 Đang mở | Tester | Bug report đủ Build/Version · TC ID · REQ ID · evidence |
| 🟡 Đã fix — chờ retest | Dev | Có build chứa bản sửa |
| ⬛ Đóng | Tester | Retest `FIXED` — lặp ≥ 2 lần theo Steps gốc |
| 🔴 Mở lại | Tester | Retest `NOT_FIXED` hoặc `PARTIAL` — **không** tạo bug trùng |

> Bug *Hoãn* (nếu workflow Jira có) vẫn tính là **đang mở** khi chấm tiêu chí exit #1, #2 — trừ khi người có quyền ở 4.2 chấp nhận bằng văn bản.

### 9.2 Thang Severity

> Mặc định chép **nguyên văn** `skills-bug-reporter` — *Severity & Priority Guide*. Tiêu chí exit #1, #2 đếm theo thang này.

| Severity | Định nghĩa | Ví dụ |
|---|---|---|
| 🔴 **Critical** | Chặn luồng chính, mất data, crash, security | Không login được, thanh toán sai tiền |
| 🟠 **Major** | Chức năng chính sai nhưng có workaround | Filter sai kết quả, export thiếu cột |
| 🟡 **Minor** | Chức năng phụ sai, UI lệch ảnh hưởng sử dụng | Validation message sai, sort không đúng |
| 🟢 **Trivial** | Lỗi hiển thị nhỏ, không ảnh hưởng chức năng | Sai chính tả, lệch margin |

### 9.3 Thang Priority

| Priority | Định nghĩa |
|---|---|
| **P1** | Fix ngay trong sprint hiện tại / hotfix |
| **P2** | Fix trong sprint kế tiếp |
| **P3** | Fix khi có thời gian (backlog) |

> Severity đánh giá theo **mức ảnh hưởng kỹ thuật**; Priority theo **mức khẩn cấp business**. Hai giá trị độc lập nhau. Tester đề xuất Severity; **Priority do người phân loại lỗi chốt**.

### 9.4 Phân loại lỗi & thời hạn xử lý

| | |
|---|---|
| Người phân loại lỗi (triage) | ❓ (ô treo 17) |
| Họp phân loại lỗi | ❓ (ô treo 17) |
| Bug đang mở tại ngày lập | Critical 0 · Major 0 · Minor 0 · Trivial 0 — repo chưa có `docs/bugs/`. **Chưa đối chiếu Jira** (nguồn chính). 8 REQ `LOGIN` đang vi phạm **chưa** có bug report — mở bằng `/create-bug-report` khi TC FAIL |

| Severity | Thời hạn phản hồi | Thời hạn sửa xong |
|---|---|---|
| Critical | ❓ | ❓ |
| Major | ❓ | ❓ |
| Minor | ❓ | ❓ |
| Trivial | ❓ | ❓ |

> Thời hạn **không có mặc định** (ô treo 18). Bug quá hạn là dữ liệu cho mục *trở ngại* của báo cáo tiến độ.

## 10. Sản phẩm bàn giao

| Sản phẩm | Nơi lưu | Workflow sinh ra |
|---|---|---|
| Master Test Plan + bản lưu phiếu | `docs/test-plans/test_plan_release_1.0.md` · `test_plan_release_1.0.input.yaml` | `/generate-master-test-plan` |
| Tài liệu requirements | `docs/requirements/<module>/` — tầng `web/` | `/generate-requirements-from-website` |
| Test cases | `docs/testcases/<module>/` — tầng `web/` | `/generate-testcases-manual-rbt` |
| Execution report | `docs/executions/<module>/web/run_*/` | `/execute-test-cases` |
| Retest report | `docs/executions/<module>/web/retest_*/` | `/retest-fixed-bugs` |
| Bug report | `docs/bugs/<module>/web/` (+ Jira) | `/create-bug-report` |
| Automation script + report | Project automation · `reports/` | `/generate-automation-framework` · `/generate-automation-web` |
| Ma trận truy vết | `traceability_matrix.md` | `/generate-traceability-matrix` |
| Kế hoạch tích hợp liên module | Theo workflow | `/generate-cross-module-test-plan` |
| **Báo cáo tiến độ** | `docs/executions/test_progress_release_1.0_<YYYYMMDD>.md` | `/generate-test-progress-report` |
| **Báo cáo tổng hợp** | `docs/executions/test_summary_release_1.0_*.md` | `/generate-test-summary-report` |

## 11. Phê duyệt

| Vai trò | Tên | Phiên bản duyệt | Ngày | Ý kiến |
|---|---|---|---|---|
| QA Lead | Anh Tester | v1.0 | ❓ | |
| Product Owner | ❓ (ô treo 24) | v1.0 | ❓ | |

## 12. Ánh xạ chuẩn tài liệu

### 12.1 Đối chiếu mục

> Tài liệu này biên soạn **theo cấu trúc** ISO/IEC/IEEE 29119-3 — Test Plan và phủ đủ nội dung điển hình của test plan theo **ISTQB CTFL v4.0 mục 5.1.1**. Cột IEEE 829 theo **khung Test Plan bản 1998**. Bảng dưới để người duyệt đối chiếu; **không** phải tuyên bố đã được đánh giá tuân thủ. Cột 29119-3 ghi theo khung Test Plan của chuẩn, **chưa** đối chiếu với bản chuẩn (ô treo 13).

| Mục | ISO/IEC/IEEE 29119-3 — Test Plan | ISTQB CTFL v4.0 — 5.1.1 | IEEE 829-1998 — Test Plan |
|---|---|---|---|
| Kiểm soát tài liệu | Document-specific information — Unique identification · Issuing organization · Approval authority · Change history | — | Test plan identifier |
| 1.1 | Introduction — Scope · Context of the testing — Project/test sub-process | Context of testing — test objectives | Introduction |
| 1.2 | Context of the testing — Test item(s) | Context of testing — test basis | Introduction |
| 2.1 | Context of the testing — Test item(s) · Test scope | Context of testing — scope | Test items · Features to be tested |
| 2.2 | Context of the testing — Test scope (phần loại trừ) | Context of testing — scope | Features not to be tested |
| 2.3 | Context of the testing — Assumptions and constraints | Assumptions and constraints of the test project · Context of testing — constraints | — |
| 2.4 | Context of the testing — Stakeholders · Testing communication | Stakeholders — roles, relevance to testing · Communication — forms and frequency of communication, documentation templates | — |
| 3.1 | Test strategy — Test sub-processes | Test approach — test levels | Approach |
| 3.2 | Test strategy — Test sub-processes | Test approach — test types | Approach |
| 3.2.1 | Test strategy — Test sub-processes · Test design techniques | Test approach — test types | Approach |
| 3.3 | Test strategy — Test design techniques | Test approach — test techniques | Approach |
| 3.4 | Staffing — Roles, activities, and responsibilities | Test approach — independence of testing | Responsibilities |
| 3.5 | Test strategy — Retesting and regression testing | Test approach — test types | Approach |
| 3.6 | Test strategy — Metrics to be collected | Test approach — metrics to be collected | — |
| 3.7 | Test strategy — Test sub-processes | Test approach — test types *(kim tự tháp kiểm thử: CTFL v4.0 mục 5.1.6)* | Approach |
| 4.1 | Test strategy | Test approach — entry criteria | — |
| 4.2 | Test strategy — Test completion criteria | Test approach — exit criteria | Item pass/fail criteria |
| 4.3 | Test strategy — Suspension and resumption criteria | — | Suspension criteria and resumption requirements |
| 5.1 | Test strategy — Test environment requirements | Test approach — test environment requirements | Environmental needs |
| 5.2 | Test strategy — Test data requirements | Test approach — test data requirements | Environmental needs |
| 5.3 | Test strategy — Test environment requirements | — *(công cụ: CTFL v4.0 chương 6)* | Environmental needs |
| 6 | Staffing — Roles, activities, and responsibilities · Hiring needs · Training needs | Stakeholders — responsibilities, hiring and training needs | Responsibilities · Staffing and training needs |
| 7.1 | Schedule | Budget and schedule | Schedule |
| 7.2 | Testing activities and estimates | Budget and schedule | Testing tasks |
| 7.3 | Testing activities and estimates | Budget and schedule | — |
| 8.1 | Risk register — Project risks | Risk register — project risks | Risks and contingencies |
| 8.2 | Risk register — Product risks | Risk register — product risks | Risks and contingencies |
| 9 | — *(không có mục riêng trong Test Plan; báo cáo sự cố là tài liệu riêng — Incident Report)* | — *(quản lý lỗi: CTFL v4.0 mục 5.5)* | — *(Test incident report là tài liệu riêng)* |
| 10 | Test strategy — Test deliverables | Test approach — test deliverables | Test deliverables |
| 11 | Document-specific information — Approval authority | — | Approvals |
| 12.2 | Test strategy — Deviations from the Organizational Test Strategy | Test approach — deviations from the organizational test policy and test strategy | — |

**Rủi ro sản phẩm:** plan chỉ tóm tắt ở 8.2 — nguồn chính là tài liệu requirements và test case của từng module.

**Phần mở rộng ngoài khung chuẩn:** 3.7 Chiến lược tự động hoá · 9 Quản lý lỗi.

### 12.2 Điểm làm khác chính sách & chiến lược kiểm thử chung (Deviations)

Không áp dụng — tổ chức chưa có Test Policy và Test Strategy (phiếu `Có chính sách kiểm thử (Test Policy): không` · `Có chiến lược kiểm thử (Test Strategy): không`).
