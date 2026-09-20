# Bản đồ hệ thống — Perfex CRM (Anh Tester Demo)

> **Tầng khám phá — KHÔNG chứa mã `REQ-XXX-NN`.** Tầng này chỉ cấp **prefix**; số REQ do `/generate-requirements-from-website` cấp ở tầng module.
>
> Danh mục và trạng thái recon: [`docs/requirements/README.md`](../README.md) — file này **không** nhân bản trạng thái.

---

## 1. Bối cảnh khảo sát

| Mục | Giá trị |
|---|---|
| Ngày khảo sát | **20-09-2026** |
| Mode | **UI** — user chỉ định; không có tài liệu nào được cung cấp |
| Mặt hệ thống | **Web** duy nhất. Không có app mobile, **không có mặt API** (xem mục 1b) |
| Hệ thống | Perfex CRM — tiêu đề trang: `Perfex CRM \| Anh Tester Demo` |
| URL · tài khoản | Lưu ở `.env` (đã `.gitignore`). **Không ghi vào `docs/`** |
| Role đã dùng | **1 account duy nhất** — nhân viên id `2` (`body.className` chứa `user-id-2`), không có quyền khu Setup |
| Trình duyệt khảo sát | Chromium qua Playwright MCP · viewport **1600×750** (đo: `innerWidth/innerHeight`) |
| Giao diện | Tiếng Anh. Hệ thống có **25 ngôn ngữ** + System Default |
| **Môi trường dùng chung** | ✅ **CÓ** — xem mục 1c. Toàn bộ khảo sát chạy **kỷ luật chỉ-đọc** |
| Phạm vi crawl | Khu `/admin` (quản trị). **Cổng khách hàng dùng URL khác — user chốt để đợt sau** |
| Số module | **27** |

### 1b. Tầng network — không có API REST

Bật quan sát thụ động suốt quá trình crawl. Kết quả:

| Phát hiện | Bằng chứng |
|---|---|
| **Mọi bảng dữ liệu** gọi `POST /admin/<module>/table` | `POST /admin/clients/table` khi mở trang · **lần thứ hai** khi gõ vào ô tìm kiếm → DataTables server-side |
| **Không có namespace `/api/`** | Không request nào khớp mẫu `/api/` trong toàn bộ phiên |
| Ghi dữ liệu là form POST thường | Không có payload JSON để rút schema |

> ⚠️ Hệ quả cho tầng recon module: **không rút được schema entity từ network**. Mọi tên trường và mọi validation phải lấy bằng đọc DOM và trigger thủ công từng trường. Khối lượng recon vì thế **nặng hơn** hệ thống có API.
>
> Cũng có nghĩa: dòng *"QA gọi được API không"* trong bảng Năng lực kiểm thử là **không áp dụng** — hệ thống không phơi API nào ra trình duyệt.

### 1c. Bằng chứng môi trường dùng chung

Không hỏi user — **đo được từ dữ liệu**:

| Bằng chứng | Nguồn |
|---|---|
| `AUTO_POM_ADD_PROJECT_1787064837550` · `AUTO_POM_ADD_PROJECT_1785661551151` … (≥ 8 bản ghi cùng mẫu) | Khối Projects trên Dashboard |
| `New Project 3008` ×4 · `Test company` · `1231322323` | Cùng nguồn |
| 172 dự án · 259 công việc trong khi số liệu tiền chỉ `$14.00` | Dữ liệu rác tích luỹ từ nhiều đợt automation của người khác |

→ **Bật quy tắc dọn dữ liệu test** cho mọi TC sinh về sau. Cấm thao tác phá huỷ trên dữ liệu có sẵn.

---

## 2. Sơ đồ điều hướng toàn hệ thống

### Thanh bên (nguyên trạng, tiếng Anh)

```
Dashboard                → /admin/
Customers                → /admin/clients
Projects                 → /admin/projects
Tasks                    → /admin/tasks
Contracts                → /admin/contracts
Sales                    → (nhóm, href="#")
├── Proposals            → /admin/proposals
├── Estimates            → /admin/estimates
├── Invoices             → /admin/invoices
├── Payments             → /admin/payments
├── Credit Notes         → /admin/credit_notes
└── Items                → /admin/invoice_items
Subscriptions            → /admin/subscriptions
Expenses                 → /admin/expenses
Support                  → /admin/tickets
Leads                    → /admin/leads
Estimate Request         → /admin/estimate_request
Knowledge Base           → /admin/knowledge_base
Utilities                → (nhóm, href="#")
├── Media                → /admin/utilities/media
├── Bulk PDF Export      → /admin/utilities/bulk_pdf_exporter
└── Calendar             → /admin/utilities/calendar
Reports                  → (nhóm, href="#")
├── Sales                → /admin/reports/sales
├── Expenses             → /admin/reports/expenses
├── Expenses vs Income   → /admin/reports/expenses_vs_income
├── Leads                → /admin/reports/leads
├── Timesheets overview  → /admin/staff/timesheets?view=all   ← trỏ sang route của TIME
└── KB Articles          → /admin/reports/knowledge_base_articles
```

**29 mục, 15 nhóm cấp 1.** Đã mở từng nhóm cha và cuộn hết thanh bên.

### Thanh đầu trang

```
Ô tìm kiếm toàn cục (có lưu lịch sử tìm)
Nút tạo nhanh [+] → Invoice · Estimate · Proposal · Credit Note · Customer · Subscription
                     · Project · Task · Expense · Contract · Article · Ticket · Event
Biểu tượng chia sẻ   → "Share documents, ideas.." (bảng tin, href="#")
Biểu tượng ✓ [2]     → /admin/todo
Ảnh đại diện         → My Profile · My Timesheets · Edit Profile · Language (25 thứ tiếng) · Logout
Biểu tượng đồng hồ   → bộ đếm giờ (a.top-timers)
Biểu tượng chuông    → thông báo · "View all notifications" → /admin/profile?notifications=true
```

> 🚫 **Không có biểu tượng Setup.** `#setup-menu-wrapper` có trong DOM nhưng danh sách bên trong rỗng — xem mục 5.

### Route ngoài menu (đã xác minh mở được)

| Route | Ghi nhận |
|---|---|
| `/admin/announcements` | ✅ mở được — chỉ đọc, 0 bản ghi → module `ANN` |
| `/admin/knowledge_base/manage_groups` | ✅ mở được — vào từ nút `Groups` trên thanh công cụ KB |
| `/admin/misc/reminders` | ✅ mở được — vào từ khối Reminders trên Dashboard |
| `/admin/todo` · `/admin/profile` · `/admin/staff/timesheets` · `/admin/staff/edit_profile` | ✅ vào từ thanh đầu trang |
| `/admin/projects/gantt` · `/admin/tasks/detailed_overview` · `/admin/invoices/recurring` · `/admin/estimates/pipeline/1` · `/admin/proposals/pipeline/1` | ✅ vào từ thanh công cụ của module tương ứng |

---

## 3. Bảng module tổng — 27 module

| # | Module (tên UI) | Prefix | Nền tảng | File khám phá | Loại màn hình | Status flow | Ước REQ | Risk |
|---|---|---|---|---|---|---|---|---|
| 1 | Đăng nhập & Tài khoản | `LOGIN` | Web | [01](modules/module_01_dang_nhap_tai_khoan.md) | Form | — | 15–20 | 🔴 |
| 2 | Khách hàng | `CUST` | Web | [02](modules/module_02_khach_hang.md) | List · Form 2 tab · Detail 22 tab | Active/Inactive | 50–70 | 🔴 |
| 3 | Dự án | `PRJ` | Web | [03](modules/module_03_du_an_cong_viec_cham_cong.md) | List · Detail 18 tab · Gantt | **5 trạng thái** | 50–70 | 🔴 |
| 4 | Công việc | `TASK` | Web | [03](modules/module_03_du_an_cong_viec_cham_cong.md) | List · Kanban · Modal | Status + Priority | 35–45 | 🔴 |
| 5 | Chấm công | `TIME` | Web | [03](modules/module_03_du_an_cong_viec_cham_cong.md) | List + bộ lọc | — | 10–15 | 🟡 |
| 6 | Hoá đơn & Thanh toán | `INV` | Web | [04](modules/module_04_hoa_don_giay_bao_co.md) | List · Form dòng hàng · Định kỳ | **6 trạng thái** | 50–60 | 🔴 |
| 7 | Giấy báo có | `CN` | Web | [04](modules/module_04_hoa_don_giay_bao_co.md) | List · Form dòng hàng | Status + số dư | 20–25 | 🔴 |
| 8 | Dự toán | `EST` | Web | [05](modules/module_05_bao_gia_du_toan_yeu_cau.md) | List · Pipeline · Form | **6 trạng thái** | 30–35 | 🔴 |
| 9 | Đề xuất | `PROP` | Web | [05](modules/module_05_bao_gia_du_toan_yeu_cau.md) | List · Pipeline · Form | **6 trạng thái** | 30–35 | 🔴 |
| 10 | Yêu cầu báo giá | `ESTREQ` | Web | [05](modules/module_05_bao_gia_du_toan_yeu_cau.md) | List · Form builder | Status | 15–20 | 🟡 |
| 11 | Hợp đồng | `CTR` | Web | [06](modules/module_06_hop_dong_dang_ky.md) | List · Form | Signature | 25–30 | 🟡 |
| 12 | Đăng ký định kỳ | `SUB` | Web | [06](modules/module_06_hop_dong_dang_ky.md) | List · Form | Status + chu kỳ | 20–25 | 🔴 |
| 13 | Chi phí | `EXP` | Web | [07](modules/module_07_chi_phi_danh_muc_hang_hoa.md) | List · Form · Import | — | 25–30 | 🟡 |
| 14 | Danh mục hàng hoá | `ITEM` | Web | [07](modules/module_07_chi_phi_danh_muc_hang_hoa.md) | List · Modal · Import | — | 20–25 | 🟡 |
| 15 | Hỗ trợ | `TIC` | Web | [08](modules/module_08_ho_tro_tri_thuc.md) | List · Form hội thoại | Status + Priority | 30–35 | 🟡 |
| 16 | Cơ sở tri thức | `KB` | Web | [08](modules/module_08_ho_tro_tri_thuc.md) | List · Trình soạn thảo | — | 15–20 | 🟢 |
| 17 | Khách hàng tiềm năng | `LEAD` | Web | [09](modules/module_09_khach_hang_tiem_nang.md) | List · Kanban · Biểu đồ | Status + Source | 30–35 | 🔴 |
| 18 | Báo cáo | `RPT` | Web | [10](modules/module_10_bao_cao_bang_dieu_khien.md) | 6 báo cáo (Sales có 8 mục con) | — | 25–30 | 🟡 |
| 19 | Bảng điều khiển | `DASH` | Web | [10](modules/module_10_bao_cao_bang_dieu_khien.md) | Widget kéo thả | — | 8–12 | 🟢 |
| 20 | Tiện ích | `UTIL` | Web | [11](modules/module_11_tien_ich_viec_can_lam.md) | File manager · Form · Lịch | — | 20–25 | 🟢 |
| 21 | Việc cần làm | `TODO` | Web | [11](modules/module_11_tien_ich_viec_can_lam.md) | List · Kanban | Status | 12–15 | 🟢 |
| 22 | Nhân sự | `STAFF` | Web | [12](modules/module_12_nhan_su_phan_quyen_cau_hinh.md) | 🚫 chưa thấy | ❔ | ❔ | 🔴 |
| 23 | Vai trò & Phân quyền | `ROLE` | Web | [12](modules/module_12_nhan_su_phan_quyen_cau_hinh.md) | 🚫 chưa thấy | ❔ | ❔ | 🔴 |
| 24 | Cấu hình hệ thống | `SETTING` | Web | [12](modules/module_12_nhan_su_phan_quyen_cau_hinh.md) | 🚫 chưa thấy | ❔ | ❔ | 🔴 |
| 25 | Mục tiêu | `GOAL` | Web | [13](modules/module_13_muc_tieu_khao_sat_thong_bao.md) | 🚫 chưa thấy | ❔ | ❔ | 🟡 |
| 26 | Khảo sát | `SURVEY` | Web | [13](modules/module_13_muc_tieu_khao_sat_thong_bao.md) | 🚫 chưa thấy | ❔ | ❔ | 🟡 |
| 27 | Thông báo | `ANN` | Web | [13](modules/module_13_muc_tieu_khao_sat_thong_bao.md) | List chỉ đọc (route ẩn) | — | 5–8 | 🟢 |

**Tổng: 27 module · 27 prefix · 13 file khám phá.** Ước tổng REQ khu truy cập được: **~490–580** (21 module ✅). Sáu module 🚫 chưa ước được.

### Quyết định gộp — user chốt 20-09-2026

| Gộp | Vào | Căn cứ đo được |
|---|---|---|
| Contacts | `CUST` | `/admin/clients/all_contacts` chỉ có nút `Export`, không có nút tạo ở cấp toàn cục |
| Payments | `INV` | `/admin/payments` chỉ có nút `Export`; thanh toán ghi nhận từ trong hoá đơn |
| Reminders | `TODO` | Cùng là việc cá nhân, mỗi cái một màn hình nhỏ |
| ❌ Timesheets **KHÔNG** gộp vào `PRJ` | — | User chốt giữ riêng: có màn hình riêng, bộ lọc riêng, báo cáo riêng |

---

## 4. Bản đồ entity & phụ thuộc

```
                        ┌──────────┐
                        │   LEAD   │  (0 bản ghi — chưa quan sát được)
                        └────┬─────┘
                             │ chuyển đổi
                             ▼
   ESTREQ ──yêu cầu──►  ┌──────────┐  ◄── entity GỐC, 11 module tham chiếu
                        │   CUST   │
                        └────┬─────┘
          ┌──────────────────┼──────────────────┬─────────────┐
          ▼                  ▼                  ▼             ▼
     ┌─────────┐      ┌────────────┐      ┌─────────┐   ┌─────────┐
     │   PRJ   │      │ EST · PROP │      │   CTR   │   │   TIC   │
     └────┬────┘      └─────┬──────┘      └────┬────┘   └────┬────┘
          │                 │ chấp nhận        │             │
     ┌────┴────┐            ▼                  │             ▼
     │  TASK   │      ┌──────────┐             │        ┌────────┐
     └────┬────┘      │   INV    │ ◄───────────┘        │   KB   │
          │           └────┬─────┘  ◄── SUB (định kỳ)   └────────┘
          ▼                │  ▲
     ┌─────────┐           │  │ bù trừ
     │  TIME   │           │  └──── CN
     └─────────┘           │
                           │ ◄──── ITEM (dòng hàng) · EXP (tính lại cho khách)
                           ▼
                      ┌─────────┐
                      │   RPT   │ ── DASH
                      └─────────┘

Cắt ngang mọi module:  STAFF · ROLE · SETTING   ← 🚫 CHẶN, chưa khảo sát được
Độc lập:               UTIL · TODO · GOAL · SURVEY · ANN
```

| Phụ thuộc | Hệ quả cho thứ tự khảo sát |
|---|---|
| `CUST` là entity gốc của 11 module | Phải recon trước mọi module nghiệp vụ |
| `SETTING` cấp master data cho `ITEM`(thuế) · `TIC`(phòng ban, dịch vụ) · `CUST`(nhóm) · `EXP`(danh mục) · `LEAD`(nguồn) · `CTR`(loại) | 🚫 chặn → các module này recon được **một phần** |
| `ROLE` quyết định ma trận phân quyền của mọi module | 🚫 chặn → mọi ma trận phân quyền ghi `❔` |
| `EST`/`PROP` chấp nhận → sinh `INV` | Recon `INV` trước để hiểu đích đến |
| `TASK` thuộc `PRJ`, `TIME` ghi vào `TASK` | Recon theo đúng thứ tự này |

---

## 5. Ma trận phân quyền sơ bộ — cấp module

**Chỉ có 1 account.** Áp thang 3 mức bằng chứng; **không suy quyền role B từ role A**.

| Vùng | Nhân viên id 2 (account đang có) | Role khác |
|---|---|---|
| 21 module nghiệp vụ (`LOGIN`→`TODO`, `ANN`) | ✅ **Đã kiểm chứng** — mở được, thấy thanh công cụ đầy đủ | ❔ |
| `STAFF` · `ROLE` · `SETTING` · `GOAL` · `SURVEY` | ✅ **Đã kiểm chứng là BỊ TỪ CHỐI** — `/admin/access_denied` | ❔ |
| Nhật ký hoạt động (`/admin/utilities/activity_log`) | ✅ **Đã kiểm chứng là BỊ TỪ CHỐI** | ❔ |
| Tạo thông báo (`ANN`) | ⚠️ **Suy từ giao diện** — màn hình không có nút tạo; nghi nút nằm ở khu Setup | ❔ |

**Dòng tổng:** Đã kiểm chứng **3 ô** · Suy diễn **1 ô** · Chưa rõ **4 ô** *(cột role khác — toàn bộ)*

> Hệ thống có những role nào: **❔ chưa biết.** Màn hình liệt kê role (`/admin/roles`) bị chặn — đúng thứ lẽ ra dùng để trả lời câu này. → `AMB-SYS-01` 🔴.

---

## 6. Thứ tự khảo sát đã chốt

User chốt 20-09-2026 — nguyên tắc **phụ thuộc trước, risk sau**:

```
 1. LOGIN    2. CUST     3. PRJ      4. TASK     5. INV      6. EST      7. PROP
 8. CTR      9. SUB     10. CN      11. EXP     12. TIC     13. LEAD    14. ITEM
15. ESTREQ  16. RPT     17. KB      18. UTIL    19. TODO    20. TIME    21. DASH
```

Sáu module còn lại **`BLOCKED`**, không xếp vào thứ tự trên:

| Module | Điều kiện gỡ chặn |
|---|---|
| `STAFF` · `ROLE` · `SETTING` · `GOAL` · `SURVEY` | Cần account có quyền khu Setup |
| `ANN` | Vào xem được, nhưng không tạo được → cần account Setup mới recon trọn |

> ⚠️ `ROLE` bị chặn khiến **cả 21 module trong thứ tự trên** đều recon được phần chức năng nhưng **không** recon được ma trận phân quyền. Đây là nợ kỹ thuật phải ghi rõ ở từng tài liệu module, không làm tròn.

---

## 7. Vùng chưa xác minh & ứng viên Ambiguity cấp hệ thống

| Mã | Mức | Nội dung |
|---|---|---|
| `AMB-SYS-01` | 🔴 | **Hệ thống có những role nào — không biết.** Chỉ 1 account, màn hình liệt kê role bị chặn. Xin account từng role để lấp 4 ô `❔` ở mục 5 |
| `AMB-SYS-02` | 🔴 | **Trang từ chối truy cập báo sai bản chất lỗi.** `/admin/access_denied` hiển thị nguyên văn *"Something went wrong. Try again"* thay vì thông báo thiếu quyền → người dùng hiểu nhầm là lỗi hệ thống và thử lại mãi |
| `AMB-SYS-03` | 🟡 | **`Not Sent` là trạng thái hay bộ lọc?** `INV` và `EST` dùng tham số riêng (`filter=not_sent` · `not_sent=1`) trong khi các trạng thái khác dùng `status=`. `PROP` thì cả 6 đều dùng `status=`. Ba module không nhất quán |
| `AMB-SYS-04` | 🟡 | **Hoá đơn có trạng thái `Cancelled`** — chỉ suy ra từ câu *"Cancelled invoices are excluded from the report"* ở báo cáo bán hàng; không xuất hiện trong 6 ô lọc trên Dashboard. Nó ảnh hưởng mẫu số của mọi con số phần trăm |
| `AMB-SYS-05` | 🟡 | **Master data loại hợp đồng ở đâu?** `/admin/contracts/contract_types` trả **404**, không phải `access_denied` → đường dẫn khác, chưa tìm ra |

### Vùng dữ liệu rỗng — không quan sát được luồng nghiệp vụ

| Module | Số bản ghi |
|---|---|
| `LEAD` · `ESTREQ` · `KB` · `ANN` | **0** — `No entries found` |
| `TIC` · `SUB` · `CN` · `EXP` · Payments | **1** |

→ Recon các module này cần **dữ liệu mẫu**, hoặc chấp nhận tạo dữ liệu thử **kèm dọn sau** (môi trường dùng chung).

### Việc không làm được bằng giao diện quản trị

- Tác vụ nền phát hành hoá đơn định kỳ (`SUB`)
- Hết hạn tự động của dự toán (`EST`)
- Luồng ký điện tử và bình luận của khách trên hợp đồng (`CTR`) — diễn ra ở **cổng khách hàng**, URL khác
- Thư tự động gửi đi — không có hộp thư để đối chiếu

---

## 8. Bản đồ tài liệu

| File | Module bao phủ | Prefix |
|---|---|---|
| [modules/module_01_dang_nhap_tai_khoan.md](modules/module_01_dang_nhap_tai_khoan.md) | Đăng nhập & Tài khoản | `LOGIN` |
| [modules/module_02_khach_hang.md](modules/module_02_khach_hang.md) | Khách hàng | `CUST` |
| [modules/module_03_du_an_cong_viec_cham_cong.md](modules/module_03_du_an_cong_viec_cham_cong.md) | Dự án · Công việc · Chấm công | `PRJ` · `TASK` · `TIME` |
| [modules/module_04_hoa_don_giay_bao_co.md](modules/module_04_hoa_don_giay_bao_co.md) | Hoá đơn & Thanh toán · Giấy báo có | `INV` · `CN` |
| [modules/module_05_bao_gia_du_toan_yeu_cau.md](modules/module_05_bao_gia_du_toan_yeu_cau.md) | Dự toán · Đề xuất · Yêu cầu báo giá | `EST` · `PROP` · `ESTREQ` |
| [modules/module_06_hop_dong_dang_ky.md](modules/module_06_hop_dong_dang_ky.md) | Hợp đồng · Đăng ký định kỳ | `CTR` · `SUB` |
| [modules/module_07_chi_phi_danh_muc_hang_hoa.md](modules/module_07_chi_phi_danh_muc_hang_hoa.md) | Chi phí · Danh mục hàng hoá | `EXP` · `ITEM` |
| [modules/module_08_ho_tro_tri_thuc.md](modules/module_08_ho_tro_tri_thuc.md) | Hỗ trợ · Cơ sở tri thức | `TIC` · `KB` |
| [modules/module_09_khach_hang_tiem_nang.md](modules/module_09_khach_hang_tiem_nang.md) | Khách hàng tiềm năng | `LEAD` |
| [modules/module_10_bao_cao_bang_dieu_khien.md](modules/module_10_bao_cao_bang_dieu_khien.md) | Báo cáo · Bảng điều khiển | `RPT` · `DASH` |
| [modules/module_11_tien_ich_viec_can_lam.md](modules/module_11_tien_ich_viec_can_lam.md) | Tiện ích · Việc cần làm | `UTIL` · `TODO` |
| [modules/module_12_nhan_su_phan_quyen_cau_hinh.md](modules/module_12_nhan_su_phan_quyen_cau_hinh.md) | Nhân sự · Vai trò · Cấu hình | `STAFF` · `ROLE` · `SETTING` |
| [modules/module_13_muc_tieu_khao_sat_thong_bao.md](modules/module_13_muc_tieu_khao_sat_thong_bao.md) | Mục tiêu · Khảo sát · Thông báo | `GOAL` · `SURVEY` · `ANN` |

**Kiểm chứng: 1+1+3+2+3+2+2+2+1+2+2+3+3 = 27** — khớp bảng mục 3.

> ⚠️ Gộp file **không** gộp prefix. 27 module chung 13 file khám phá vẫn sinh ra **27 thư mục** và **27 file `requirements_<module>.md`** riêng ở tầng module.

---

## 9. Nhật ký khám phá

| Ngày | Mode · mặt | Nội dung | Nguồn |
|---|---|---|---|
| 20-09-2026 | UI · Web | Khám phá lần đầu. Crawl 29 mục thanh bên + thanh đầu trang + route ngoài menu. Phát hiện **27 module**, cấp 27 prefix. Đo tầng network: **không có API REST**. Xác minh khu Setup bị chặn bằng 11 route chỉ đọc. Phát hiện 2 route ẩn mở được (`announcements`, `knowledge_base/manage_groups`) và 3 route trả 404. Mở 5 `AMB-SYS`. Chốt với user: gộp Contacts→`CUST`, Payments→`INV`, Reminders→`TODO`; giữ riêng `TIME`; tiền tố TC ID `CRM_`; thứ tự khảo sát phụ thuộc-trước-risk-sau | UI thực tế |
| 20-09-2026 | Recon module `LOGIN` | **5 điểm lệch so với bản đồ** — đã sửa ở [`modules/module_01_dang_nhap_tai_khoan.md`](modules/module_01_dang_nhap_tai_khoan.md), chi tiết đầy đủ ở [`../login/requirements_login.md`](../login/requirements_login.md):<br>① Số ngôn ngữ: bản đồ ghi **25**, thực tế **26 + `System Default` = 27 lựa chọn** (đếm `href` duy nhất, đối chiếu `options.length` của `#default_language`)<br>② Trường `email` ở Sửa hồ sơ: bản đồ xếp vào danh sách trường nhập, thực tế **`disabled` và không có thuộc tính `name`** → không sửa được, cũng không gửi lên khi lưu<br>③ Sửa hồ sơ **không phải một form**: có **3 form độc lập**, 3 action riêng (`edit_profile` · `change_password_profile` · `update_two_factor`), 3 nút `Save` riêng → locator phải giới hạn trong từng form<br>④ Ba lựa chọn xác thực 2 lớp (bản đồ để ❔): `off` `Disabled` · `email` `Enable Email Two Factor Authentication` · `google` `Enable Google Authenticator`<br>⑤ Đăng xuất: DOM có **3 phần tử mang chữ "Logout"**, ở viewport `1600×750` chỉ **1** phần tử dùng được (hai phần tử kia bị `div#mobile-collapse` ẩn và là template cảnh báo bấm giờ)<br>Ngoài ra: màn hình Quên mật khẩu (bản đồ để ❔) đã khảo sát được phần giao diện sau khi user cho phép đăng xuất | Recon module `LOGIN` |
