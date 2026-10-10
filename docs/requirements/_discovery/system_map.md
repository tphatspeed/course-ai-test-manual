# Bản đồ hệ thống — Perfex CRM (bản demo Anh Tester)

> **INDEX tầng khám phá — tên file bất biến.** Bản đồ đã tách: chi tiết từng module nằm ở `modules/` (xem [Bản đồ tài liệu](#bản-đồ-tài-liệu)).
> Tầng khám phá **chỉ cấp prefix**, **không** cấp mã REQ. Trạng thái recon của từng module chỉ ghi ở [`../README.md`](../README.md) — file này không nhân bản.

---

## 1. Bối cảnh khảo sát

| Mục | Giá trị |
|---|---|
| Ngày khảo sát | 03-10-2026 |
| Mode | **UI** — chỉ có hệ thống đang chạy, không có tài liệu (user chỉ định) |
| Mặt hệ thống | **Web** (1 mặt) — khu vực quản trị `/admin/*` + cổng khách hàng `/` |
| URL · tài khoản | `.env` (`BASE_URL` · `EMAIL_ADMIN`) — không ghi vào tài liệu |
| Role đã dùng | 1 tài khoản — **Staff** (`app.user_is_admin = ""`, `app.user_is_staff_member = "1"`). Không có tài khoản Admin, không có tài khoản contact |
| Môi trường dùng chung | ✅ Có — toàn bộ khảo sát **chỉ đọc**: chỉ mở trang / form, **không** bấm Save, Delete, Send, không đổi ngôn ngữ, không reset dashboard |
| Phiên bản | `app.version = 316` (Perfex CRM) |
| Trình duyệt · viewport | Google Chrome (Playwright MCP, headed) · viewport đo được `1600×750` (`innerWidth × innerHeight`) |
| Phạm vi crawl | Toàn bộ sidebar (14 mục cấp 1 + 15 mục con) · header (Quick Create 13 mục, menu người dùng, thông báo) · 31 trang danh sách/tiện ích · 6 trang chi tiết bản ghi · 11 form tạo mới (chỉ mở) · 29 URL thử trực tiếp (route ẩn / chỉ admin) · cổng khách hàng chưa đăng nhập |
| Tầng network | Quan sát thụ động bằng `browser_network_requests`. Không gọi API trực tiếp |
| Ảnh evidence | 1 ảnh/module, chụp **viewport** (đối tượng cần chứng minh — thanh công cụ + tiêu đề bảng — nằm trọn trong viewport). Phần thân bảng / danh sách / lịch / input cá nhân được **làm mờ bằng CSS trước khi chụp** vì môi trường dùng chung chứa dữ liệu của người khác và ảnh sẽ được commit |

---

## 2. Sơ đồ điều hướng

### 2.1 Khu vực quản trị — sidebar (tài khoản Staff)

```
Dashboard ................................ /admin/
Customers ................................ /admin/clients
Projects ................................. /admin/projects
Tasks .................................... /admin/tasks
Contracts ................................ /admin/contracts
Sales ▸
  ├─ Proposals ........................... /admin/proposals
  ├─ Estimates ........................... /admin/estimates
  ├─ Invoices ............................ /admin/invoices
  ├─ Payments ............................ /admin/payments
  ├─ Credit Notes ........................ /admin/credit_notes
  └─ Items ............................... /admin/invoice_items
Subscriptions ............................ /admin/subscriptions
Expenses ................................. /admin/expenses
Support .................................. /admin/tickets
Leads .................................... /admin/leads
Estimate Request ......................... /admin/estimate_request
Knowledge Base ........................... /admin/knowledge_base
Utilities ▸
  ├─ Media ............................... /admin/utilities/media
  ├─ Bulk PDF Export ..................... /admin/utilities/bulk_pdf_exporter
  └─ Calendar ............................ /admin/utilities/calendar
Reports ▸
  ├─ Sales ............................... /admin/reports/sales
  ├─ Expenses ............................ /admin/reports/expenses
  ├─ Expenses vs Income .................. /admin/reports/expenses_vs_income
  ├─ Leads ............................... /admin/reports/leads
  ├─ Timesheets overview ................. /admin/staff/timesheets?view=all
  └─ KB Articles ......................... /admin/reports/knowledge_base_articles
Setup (#setup-menu) ...................... khung có trong DOM nhưng RỖNG với tài khoản Staff
```

### 2.2 Khu vực quản trị — header

```
[≡] Thu/mở sidebar
[Search...] Tìm kiếm toàn cục
[+] Quick Create ▸ Invoice · Estimate · Proposal · Credit Note · Customer · Subscription ·
                   Project · Task (mở modal, href="#") · Expense · Contract · Article · Ticket · Event
[Share documents, ideas..] Newsfeed (mở modal)
[✓ badge] To-do ......................... /admin/todo
[Avatar] ▸ My Profile (/admin/profile) · My Timesheets (/admin/staff/timesheets) ·
           Edit Profile (/admin/staff/edit_profile) · Language ▸ System Default + 26 ngôn ngữ
           (/admin/staff/change_language/<ngôn ngữ>) · Logout (/admin/authentication/logout)
[⏱] Timer đang chạy
[🔔] Notifications ▸ View all (/admin/profile?notifications=true)
```

### 2.3 Route không có trên menu nhưng vào được (Staff)

| Route | Tới từ đâu | Thuộc module |
|---|---|---|
| `/admin/announcements` | Không có link ở menu · tab "Announcements" trên Dashboard | `NEWS` |
| `/admin/clients/all_contacts` | Nút "Contacts" ở Customers | `CUST` |
| `/admin/tasks/detailed_overview` | Nút "Tasks Overview" ở Tasks | `TASK` |
| `/admin/invoices/recurring` | Nút "Recurring Invoices" ở Invoices | `INV` |
| `/admin/knowledge_base/manage_groups` | Nút "Groups" ở Knowledge Base | `KBASE` |
| `/admin/misc/reminders` | Dashboard → tab My Reminders | `PROFILE` |
| `/admin/staff/reset_dashboard` | Dashboard Options — **làm đổi trạng thái, KHÔNG mở khi khảo sát** | `DASH` |

### 2.4 Route trả Access denied với Staff (tồn tại — khác 404)

Mọi route dưới đây chuyển về `/admin/access_denied`: toast **"Access denied"** + thân trang **"Something went wrong. Try again"**.

`/admin/settings` · `/admin/staff` · `/admin/roles` · `/admin/departments` · `/admin/utilities/activity_log` · `/admin/goals` · `/admin/custom_fields` · `/admin/emails` · `/admin/gdpr` · `/admin/surveys` · `/admin/clients/groups` · `/admin/leads/sources` · `/admin/leads/statuses` · `/admin/tickets/services` · `/admin/tickets/priorities` · `/admin/contracts/types` · `/admin/expenses/categories` · `/admin/taxes` · `/admin/currencies` · `/admin/paymentmodes` → gom vào `STAFF` / `SETTING`.

Khác: `/admin/modules` → chuyển về Dashboard. `/admin/utilities/ticket_pipe_log` · `/admin/spam_filters` · `/admin/misc/menu_setup` → **404** (không tồn tại ở phiên bản này — không đưa vào danh mục).

### 2.5 Cổng khách hàng (chưa đăng nhập)

```
/                         → chuyển /authentication/login
/authentication/login     Language · Email Address · Password · Remember me · Login · Forgot Password?
/authentication/forgot_password
/authentication/register  → chuyển về /authentication/login (đăng ký khách hàng đang TẮT)
/knowledge-base           Ô tìm bài — chưa có bài viết nào hiển thị công khai
```

---

## 3. Bảng module tổng

> **Tổng: 27 module trong 16 file** — kiểm chứng: cộng cột "Số module" ở [Bản đồ tài liệu](#bản-đồ-tài-liệu) = 27.

| # | Module (tên UI) | Bí danh | Prefix | Nền tảng | File khám phá | Loại màn hình | Risk | Ước REQ |
|---|---|---|---|---|---|---|---|---|
| 1 | Login | Admin login · Client login · Forgot Password | `LOGIN` | Web | [module_01](modules/module_01_dang_nhap_ho_so_ca_nhan.md) | Form | 🔴 | ~20 |
| 2 | My Profile / Edit Profile | To-do · Reminders · Notifications · Language · 2FA | `PROFILE` | Web | [module_01](modules/module_01_dang_nhap_ho_so_ca_nhan.md) | Form + Danh sách | 🟡 | ~30 |
| 3 | (Khung chung) | Sidebar · Header · Global search · Quick Create | `NAV` | Web | [module_02](modules/module_02_khung_dieu_huong_dashboard.md) | Shell | 🟡 | ~20 |
| 4 | Dashboard | Dashboard Options | `DASH` | Web | [module_02](modules/module_02_khung_dieu_huong_dashboard.md) | Dashboard | 🟡 | ~25 |
| 5 | Customers | Clients · Contacts | `CUST` | Web | [module_03](modules/module_03_khach_hang.md) | Danh sách + Form + Chi tiết 19 tab | 🔴 | ~60 |
| 6 | Projects | — | `PRJ` | Web | [module_04](modules/module_04_du_an.md) | Danh sách + Form + Chi tiết 12 tab | 🔴 | ~70 |
| 7 | Tasks | Timesheets · Tasks Overview | `TASK` | Web | [module_05](modules/module_05_cong_viec.md) | Danh sách + Kanban + Modal | 🔴 | ~55 |
| 8 | Items | Invoice Items · Item Groups | `ITEM` | Web | [module_06](modules/module_06_hang_hoa_de_xuat_bao_gia.md) | Danh sách + Modal | 🟡 | ~20 |
| 9 | Proposals | — | `PROP` | Web | [module_06](modules/module_06_hang_hoa_de_xuat_bao_gia.md) | Danh sách + Form chứng từ | 🟡 | ~35 |
| 10 | Estimates | — | `EST` | Web | [module_06](modules/module_06_hang_hoa_de_xuat_bao_gia.md) | Danh sách + Form chứng từ | 🟡 | ~40 |
| 11 | Invoices | Recurring Invoices · Batch Payments | `INV` | Web | [module_07](modules/module_07_hoa_don.md) | Danh sách + Form chứng từ | 🔴 | ~50 |
| 12 | Payments | — | `PAY` | Web | [module_08](modules/module_08_thanh_toan_phieu_ghi_co.md) | Danh sách | 🔴 | ~15 |
| 13 | Credit Notes | — | `CRN` | Web | [module_08](modules/module_08_thanh_toan_phieu_ghi_co.md) | Danh sách + Form chứng từ | 🟡 | ~30 |
| 14 | Subscriptions | Stripe | `SUB` | Web | [module_09](modules/module_09_dang_ky_dinh_ky.md) | Danh sách + Form | 🔴 | ~25 |
| 15 | Contracts | — | `CTR` | Web | [module_10](modules/module_10_hop_dong_chi_phi.md) | Danh sách + Form + Chi tiết 7 tab | 🟡 | ~35 |
| 16 | Expenses | — | `EXP` | Web | [module_10](modules/module_10_hop_dong_chi_phi.md) | Danh sách + Form + Import | 🟡 | ~25 |
| 17 | Leads | — | `LEAD` | Web | [module_11](modules/module_11_khach_tiem_nang_yeu_cau_bao_gia.md) | Danh sách + Kanban + Modal | 🟡 | ~40 |
| 18 | Estimate Request | Form builder | `ESTREQ` | Web | [module_11](modules/module_11_khach_tiem_nang_yeu_cau_bao_gia.md) | Danh sách + Form builder | 🟢 | ~15 |
| 19 | Support | Tickets | `TICKET` | Web | [module_12](modules/module_12_ho_tro_kho_kien_thuc.md) | Danh sách + Form | 🟡 | ~35 |
| 20 | Knowledge Base | KB Articles · KB Groups | `KBASE` | Web | [module_12](modules/module_12_ho_tro_kho_kien_thuc.md) | Danh sách + Form | 🟢 | ~15 |
| 21 | Utilities | Media · Bulk PDF Export | `UTIL` | Web | [module_13](modules/module_13_tien_ich_lich_bang_tin.md) | Công cụ | 🟢 | ~15 |
| 22 | Calendar | Event | `CAL` | Web | [module_13](modules/module_13_tien_ich_lich_bang_tin.md) | Lịch | 🟢 | ~15 |
| 23 | Announcements · Newsfeed | — | `NEWS` | Web | [module_13](modules/module_13_tien_ich_lich_bang_tin.md) | Danh sách + Modal | 🟢 | ~12 |
| 24 | Reports | Sales · Expenses · Expenses vs Income · Leads · Timesheets overview · KB Articles | `RPT` | Web | [module_14](modules/module_14_bao_cao.md) | Báo cáo | 🟡 | ~25 |
| 25 | (Cổng khách hàng) | Customer area · Client portal | `PORTAL` | Web | [module_15](modules/module_15_cong_khach_hang.md) | — chưa vào được | 🔴 | ❔ |
| 26 | (Setup) Staff · Roles · Departments | — | `STAFF` | Web | [module_16](modules/module_16_nhan_vien_cau_hinh.md) | — Access denied | 🔴 | ❔ |
| 27 | (Setup) Settings + danh mục master | Custom Fields · Email Templates · GDPR · Activity Log · Goals · Surveys | `SETTING` | Web | [module_16](modules/module_16_nhan_vien_cau_hinh.md) | — Access denied | 🟡 | ❔ |

**Quyết định tách/gộp đã chốt với user (03-10-2026):** Contacts gộp vào `CUST` · Timesheets gộp vào `TASK` · To-do + Reminders + Notifications gộp vào `PROFILE` · Newsfeed + Announcements gộp thành `NEWS` · Goals, Surveys, Activity Log gộp tạm vào `SETTING` (chưa nhìn thấy được).

---

## 4. Bản đồ entity & phụ thuộc

```
Customer ─┬─ Contact (1-n)
          ├─ Project ─┬─ Task ─── Timesheet
          │           ├─ Milestone · Discussion · File · Gantt · Note
          │           └─ (tab Sales / Tickets / Contracts của project)
          ├─ Proposal ──→ Estimate ──→ Invoice ──→ Payment
          │                               ↑
          │                     Credit Note (áp vào Invoice)
          │                     Subscription (Stripe) ──→ Invoice
          ├─ Contract (comment từ khách hàng qua cổng)
          ├─ Expense (gắn Customer · Project · Invoice)
          ├─ Ticket (gắn Contact · Department · Service)
          ├─ Statement · Vault · Files · Reminders · Map
Lead ──→ (chuyển đổi thành) Customer       Estimate Request ──→ (Status / Assigned)
Item (+ Item Group · Tax) ──→ dòng hàng của Proposal / Estimate / Invoice / Credit Note
KB Article ── KB Group                      Calendar ◀── Event + hạn Task/Project
Task gắn được vào: Project · Customer · Contract · Invoice · Estimate · Proposal (tab "Tasks")
Reminder gắn được vào: Customer · Invoice · Estimate · Proposal (tab "Reminders")
```

**Nguồn:** danh sách tab của trang chi tiết Customer / Project / Contract / Invoice / Proposal / Estimate và cột của các bảng danh sách — đều quan sát trên UI 03-10-2026. Chiều mũi tên chuyển đổi (Proposal → Estimate → Invoice, Lead → Customer) là **nghiệp vụ chuẩn của Perfex, chưa kiểm chứng** trên môi trường này: tài khoản Staff không thấy nút chuyển đổi vì số bản ghi được xem quá ít (`AMB-SYS-02`).

**Module có nhiều module khác phụ thuộc** (khảo sát trước): `CUST` (9 module) · `PRJ` (4) · `ITEM` (4 loại chứng từ) · `INV` (`PAY` · `CRN` · `SUB` · `EXP` · `RPT`).

**Tầng network cấp hệ thống** (quan sát thụ động trên Dashboard):

| Request | Status | Ghi chú |
|---|---|---|
| `GET /admin/utilities/get_calendar_data?csrf_token_name=<32 ký tự hex>&start=…&end=…` | 200 | Dữ liệu lịch. **CSRF token nằm trong query string của GET** — xem `AMB-SYS-06` |
| `POST /admin/tasks/table` | 200 | Bảng DataTables nạp qua AJAX. Mẫu `POST /admin/<module>/table` **mới kiểm chứng ở Tasks** — module khác ❔, xác minh khi recon |

CSRF: token gửi kèm mọi request AJAX qua `$.ajaxSetup` (tên trường `csrf_token_name`). Hết hạn → HTTP `419` + cảnh báo *"Page expired, refresh the page make an action."* (đọc từ mã trang).

---

## 5. Ma trận phân quyền sơ bộ (cấp module)

> Role phát hiện được: **Staff** (tài khoản đang có) · **Admin** (hệ thống có khái niệm `is_admin`) · **Contact** (người dùng cổng khách hàng). Danh sách role tuỳ biến (`/admin/roles`) **chưa đọc được**.
> ✅ thấy/vào được · ❌ Access denied · ❔ chưa có căn cứ · — không thuộc khu vực của role đó.

| Module | Staff (đã kiểm chứng) | Admin | Contact |
|---|---|---|---|
| `LOGIN` | ✅ | ❔ | ❔ (trang login cổng khách hàng hiển thị, chưa đăng nhập được) |
| `PROFILE` · `NAV` · `DASH` · `CUST` · `PRJ` · `TASK` · `ITEM` · `PROP` · `EST` · `INV` · `PAY` · `CRN` · `SUB` · `CTR` · `EXP` · `LEAD` · `ESTREQ` · `TICKET` · `KBASE` · `UTIL` · `CAL` · `RPT` | ✅ (22 module — có trên menu và mở được) | ❔ | — |
| `NEWS` | ✅ (qua URL — không có trên menu) | ❔ | — |
| `PORTAL` | — | — | ❔ |
| `STAFF` | ❌ | ❔ | — |
| `SETTING` | ❌ | ❔ | — |

⚠️ "✅ mở được" ở cột Staff chỉ nghĩa là **vào được trang**. Quyền Create / Edit / Delete và phạm vi dữ liệu (xem tất cả hay chỉ của mình) **chưa kiểm chứng** — xem `AMB-SYS-02`.

**Tổng (81 ô = 27 module × 3 role):** Đã kiểm chứng: 26 ô (Staff: 24 ✅ · 2 ❌) · Suy diễn: 0 ô · Chưa rõ: 28 ô (Admin 26 · Contact 2) · Không áp dụng: 27 ô (Contact 25 · Staff/Admin ở `PORTAL` 2).

---

## 6. Thứ tự khảo sát đã chốt

| Bước | Module | Lệnh | Ghi chú |
|---|---|---|---|
| 1 | `LOGIN` | `/generate-requirements-from-website login` | Cửa vào của mọi module khác |
| 2 | `PROFILE` | `/generate-requirements-from-website profile` | Đổi mật khẩu / 2FA — **không** bấm Save trên tài khoản dùng chung (`RISK-SYS-02`) |
| 3 | `NAV` | `/generate-requirements-from-website nav` | |
| 4 | `DASH` | `/generate-requirements-from-website dashboard` | Không bấm Reset dashboard |
| 5 | `CUST` | `/generate-requirements-from-website customers` | 9 module phụ thuộc |
| 6 | `PRJ` | `/generate-requirements-from-website projects` | |
| 7 | `TASK` | `/generate-requirements-from-website tasks` | |
| 8 | `ITEM` | `/generate-requirements-from-website items` | Dòng hàng cho mọi chứng từ |
| 9–13 | `PROP` → `EST` → `INV` → `PAY` → `CRN` | lần lượt | Phụ thuộc `AMB-SYS-02` |
| 14 | `SUB` | | Tích hợp Stripe — không kiểm được tầng tích hợp |
| 15–16 | `CTR` → `EXP` | | |
| 17–18 | `LEAD` → `ESTREQ` | | |
| 19–20 | `TICKET` → `KBASE` | | |
| 21–23 | `UTIL` → `CAL` → `NEWS` | | |
| 24 | `RPT` | | Làm sau cùng — tổng hợp số liệu của các module trên |
| ⛔ | `PORTAL` | BLOCKED | Chờ tài khoản contact — `AMB-SYS-03` |
| ⛔ | `STAFF` · `SETTING` | BLOCKED | Chờ tài khoản Admin — `AMB-SYS-01` |

---

## 7. Vùng chưa xác minh (cấp hệ thống)

| Vùng | Lý do | Cần gì |
|---|---|---|
| Toàn bộ Setup (`STAFF`, `SETTING`) | Access denied | Tài khoản Admin |
| Cổng khách hàng sau đăng nhập | Không có tài khoản contact | Tài khoản contact |
| Quyền Create/Edit/Delete và phạm vi dữ liệu của Staff | Chỉ quan sát nút trên thanh công cụ, chưa thao tác | Bảng quyền role (`AMB-SYS-02`) |
| Module cài thêm (`/admin/modules`) | Route chuyển về Dashboard | Tài khoản Admin |
| Mẫu network `POST /admin/<module>/table` | Mới thấy ở Tasks | Bật network khi recon từng module |

---

## 8. Ambiguity & Risk cấp hệ thống

| Mã | Mức | Nội dung | Nguồn | Trạng thái |
|---|---|---|---|---|
| `AMB-SYS-01` | 🔴 | Tài khoản trong `.env` (tên hiển thị "Admin Example") **không phải Admin**: `app.user_is_admin = ""`. Menu Setup rỗng, 21 route cấu hình → Access denied. Tên tài khoản gây hiểu nhầm về quyền. Xin tài khoản Admin thật để khảo sát `STAFF` / `SETTING` và kiểm chứng cột Admin của ma trận | UI thực tế · biến `app` trong trang | Treo |
| `AMB-SYS-02` | 🔴 | Phạm vi dữ liệu của Staff không đồng nhất: Customers 2.129 · Projects 184 · Tasks 272 · Contracts 135 · Items 86, nhưng Invoices 6 · Proposals 5 · Estimates 3 · Payments 1 · Credit Notes / Subscriptions / Expenses / Leads / Tickets / KB Articles 0. Nghi quyền "chỉ xem của mình". Xin bảng quyền role đang gán | Số "Showing … entries" của từng bảng | Treo |
| `AMB-SYS-03` | 🔴 | Không có tài khoản contact → không khảo sát được cổng khách hàng, phần khách hàng ký/bình luận Contract, chấp nhận Proposal/Estimate, thanh toán Invoice, gửi Ticket | Thông báo "New comment from customer on contract …" chứng tỏ cổng đang được dùng | Treo — user sẽ bổ sung |
| `AMB-SYS-04` | 🟡 | Trang `/admin/access_denied` chỉ hiện *"Something went wrong. Try again"*. Thông điệp "Access denied" chỉ nằm ở toast tự ẩn. Người dùng không biết mình thiếu quyền hay hệ thống lỗi — cố ý hay lỗi? | UI thực tế | Treo |
| `AMB-SYS-05` | 🟡 | `/admin/announcements` không có trên menu của Staff nhưng mở được trực tiếp — cố ý (Staff chỉ xem qua Dashboard) hay thiếu link? | UI thực tế | Treo |
| `AMB-SYS-06` | 🟡 | CSRF token gửi trong query string của `GET /admin/utilities/get_calendar_data` → token có thể lọt vào log máy chủ / lịch sử proxy | Network | Treo — chuyển đội Dev |
| `AMB-SYS-07` | 🟢 | Trang Reports › Leads có `document.title` = "Perfex CRM \| Anh Tester Demo" — các trang khác có tiêu đề riêng | UI thực tế | Treo |
| `RISK-SYS-01` | 🔴 | **Môi trường dùng chung, dữ liệu rác dày đặc** (tag `htest…`, bản ghi `AUTO_POM_…`, `auto_task_create_…` do nhiều học viên tạo). Số đếm Summary đổi liên tục → TC **không** được assert số tuyệt đối; dữ liệu test phải tự tạo, traceable, tự dọn | UI thực tế | Mở |
| `RISK-SYS-02` | 🔴 | **Một tài khoản dùng chung cho nhiều người**: đổi mật khẩu, bật 2FA, đổi ngôn ngữ, reset dashboard hay khoá tài khoản do đăng nhập sai sẽ chặn mọi người khác. TC loại này phải chạy trên tài khoản riêng | Suy từ môi trường dùng chung + 1 tài khoản | Mở |
| `RISK-SYS-03` | 🟡 | Link xoá task dạng **GET** (`/admin/tasks/delete_task/{id}`) xuất hiện ngay trong bảng Dashboard → crawler / prefetch / bấm nhầm đều có thể xoá dữ liệu. Khảo sát tự động phải **loại** mẫu URL `delete` | DOM Dashboard | Mở |
| `RISK-SYS-04` | 🟡 | Không xem được nhật ký hoạt động → không kiểm được tầng Logging/Audit của mọi module | Đo 03-10-2026 | Mở |

---

## Bản đồ tài liệu

| File | Module bao phủ | Prefix | Số module |
|---|---|---|---|
| [modules/module_01_dang_nhap_ho_so_ca_nhan.md](modules/module_01_dang_nhap_ho_so_ca_nhan.md) | Đăng nhập · Hồ sơ cá nhân | `LOGIN` · `PROFILE` | 2 |
| [modules/module_02_khung_dieu_huong_dashboard.md](modules/module_02_khung_dieu_huong_dashboard.md) | Khung điều hướng · Dashboard | `NAV` · `DASH` | 2 |
| [modules/module_03_khach_hang.md](modules/module_03_khach_hang.md) | Customers | `CUST` | 1 |
| [modules/module_04_du_an.md](modules/module_04_du_an.md) | Projects | `PRJ` | 1 |
| [modules/module_05_cong_viec.md](modules/module_05_cong_viec.md) | Tasks | `TASK` | 1 |
| [modules/module_06_hang_hoa_de_xuat_bao_gia.md](modules/module_06_hang_hoa_de_xuat_bao_gia.md) | Items · Proposals · Estimates | `ITEM` · `PROP` · `EST` | 3 |
| [modules/module_07_hoa_don.md](modules/module_07_hoa_don.md) | Invoices | `INV` | 1 |
| [modules/module_08_thanh_toan_phieu_ghi_co.md](modules/module_08_thanh_toan_phieu_ghi_co.md) | Payments · Credit Notes | `PAY` · `CRN` | 2 |
| [modules/module_09_dang_ky_dinh_ky.md](modules/module_09_dang_ky_dinh_ky.md) | Subscriptions | `SUB` | 1 |
| [modules/module_10_hop_dong_chi_phi.md](modules/module_10_hop_dong_chi_phi.md) | Contracts · Expenses | `CTR` · `EXP` | 2 |
| [modules/module_11_khach_tiem_nang_yeu_cau_bao_gia.md](modules/module_11_khach_tiem_nang_yeu_cau_bao_gia.md) | Leads · Estimate Request | `LEAD` · `ESTREQ` | 2 |
| [modules/module_12_ho_tro_kho_kien_thuc.md](modules/module_12_ho_tro_kho_kien_thuc.md) | Support · Knowledge Base | `TICKET` · `KBASE` | 2 |
| [modules/module_13_tien_ich_lich_bang_tin.md](modules/module_13_tien_ich_lich_bang_tin.md) | Utilities · Calendar · Bảng tin & Thông báo | `UTIL` · `CAL` · `NEWS` | 3 |
| [modules/module_14_bao_cao.md](modules/module_14_bao_cao.md) | Reports | `RPT` | 1 |
| [modules/module_15_cong_khach_hang.md](modules/module_15_cong_khach_hang.md) | Cổng khách hàng | `PORTAL` | 1 |
| [modules/module_16_nhan_vien_cau_hinh.md](modules/module_16_nhan_vien_cau_hinh.md) | Nhân viên & Phân quyền · Cấu hình hệ thống | `STAFF` · `SETTING` | 2 |
| | | **Tổng** | **27** |

Trạng thái recon của từng module: xem cột `Trạng thái recon` ở [`../README.md`](../README.md).

---

## Nhật ký khám phá

| Ngày | Mode · Mặt | Nguồn | Kết quả | Ghi chú |
|---|---|---|---|---|
| 03-10-2026 | UI · Web | UI thực tế (Playwright MCP, tài khoản Staff) | 27 module · 27 prefix · 16 file module · 27 ảnh evidence · 7 AMB-SYS · 4 RISK-SYS | Khảo sát chỉ đọc trên môi trường dùng chung. User tự đăng nhập — agent không nhập mật khẩu. Ảnh `login_overview_fullpage.png` chụp đầu phiên cùng ngày, trước khi đăng nhập |
| 03-10-2026 | UI · Web — recon module `LOGIN` | `/generate-requirements-from-website login` | Lệch so với bản đồ: `LOGIN` lớn hơn dự kiến — **58 REQ** (ước ~20), do gồm cả Remember me (3 REQ tác tạo), đăng xuất (6) và cổng khách hàng (19). Phát hiện route `/admin/authentication/reset_password` (không có trên menu) trả **HTTP 500** khi thiếu tham số. Không cần tách module | Đổi ngôn ngữ **trang Login cổng khách hàng** (chỉ cookie khách `contact_language`, không đụng tài khoản) để kiểm REQ, đã trả về English. Tài liệu: `../login/REQUIREMENTS_LOGIN_SUMMARY.md` |
