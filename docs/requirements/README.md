# Danh mục Requirements — Perfex CRM (bản demo Anh Tester)

> Điểm vào cấp hệ thống. **Đọc file này đầu tiên** trước mọi tác vụ đụng `docs/requirements/`.
> Bản đồ hệ thống chi tiết: [`_discovery/system_map.md`](_discovery/system_map.md)

## Thuộc tính dự án

| Thuộc tính | Giá trị |
|---|---|
| **Hệ thống** | Perfex CRM — bản demo của Anh Tester · `app.version = 316` (đọc từ biến `app.version` trong trang) |
| **URL · tài khoản** | Lưu ở `.env` (`BASE_URL`, `EMAIL_ADMIN`, `PASSWORD_ADMIN`) — **không** ghi vào `docs/` |
| **Mặt hệ thống** | Web — khu vực quản trị `/admin/*` + cổng khách hàng `/` |
| **Tiền tố TC ID** | `CRM_` → `CRM_<MODULE>_TC_<3 số>` (VD `CRM_LOGIN_TC_001`) — chốt 03-10-2026 |
| **Môi trường dùng chung** | ✅ **Có** — chốt 03-10-2026. Bật quy tắc: khảo sát chỉ đọc · dữ liệu test phải sinh động, traceable và tự dọn · **cấm** thao tác phá huỷ lên bản ghi không do mình tạo · không assert số đếm tuyệt đối (dữ liệu do nhiều người cùng ghi) |
| **Tài khoản đang có** | 1 tài khoản (`EMAIL_ADMIN`) — đo 03-10-2026: `app.user_is_admin = ""`, `app.user_is_staff_member = "1"` → **là Staff, KHÔNG phải Admin** (xem `AMB-SYS-01`). Tài khoản role khác: user sẽ bổ sung `.env` sau |
| **Năng lực kiểm thử của QA** | Chốt 03-10-2026 — dùng cho nhánh Vòng 3 của **mọi** bộ TC:<br>• Gọi API: ❌ không có quyền — đội Dev xác minh<br>• Truy vấn CSDL: ❌ không có quyền — đội Dev xác minh<br>• Kiểm tầng tích hợp: ❌ không có quyền — đội Dev xác minh<br>• Xem nhật ký hoạt động: ❌ tài khoản bị chặn (`/admin/utilities/activity_log` → chuyển `/admin/access_denied`, toast "Access denied") — cần tài khoản Admin, đề nghị PO cấp<br>• DevTools trình duyệt: ✅ có |
| **Định dạng hệ thống** | Ngày `d-m-Y` · giờ 24h · múi giờ `Asia/Ho_Chi_Minh` · thập phân `.` · hàng nghìn `,` · 2 chữ số thập phân · phân trang mặc định 25 · file cho phép `.png,.jpg,.pdf,.doc,.docx,.xls,.xlsx,.ppt,.pptx,.zip` (đọc từ `app.options`) |
| **Ngôn ngữ giao diện khảo sát** | English (hệ thống có 26 ngôn ngữ + System Default) |

---

## 1. Bảng danh mục module

> 27 module · 27 prefix — do `/discover-system` phát hiện 03-10-2026. Mode **UI**, không có tài liệu → mọi module `Mức phủ tài liệu` = ⬜ Trắng.

| Module | Prefix | Nền tảng | Trạng thái recon | Mức phủ tài liệu | Tài liệu | REQ đã dùng | Mã kế tiếp | AMB treo | Story | Cập nhật |
|---|---|---|---|---|---|---|---|---|---|---|
| Đăng nhập | `LOGIN` | Web ✅ | ✅ Đã có tài liệu | ⬜ Trắng | [login/REQUIREMENTS_LOGIN_SUMMARY.md](login/REQUIREMENTS_LOGIN_SUMMARY.md) | `REQ-LOGIN-01` → `72` | `REQ-LOGIN-73` · `AMB-LOGIN-32` · `RISK-LOGIN-08` | 0 | 7 | 03-10-2026 |
| Hồ sơ cá nhân | `PROFILE` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PROFILE-01` | — | — | 03-10-2026 |
| Khung điều hướng | `NAV` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-NAV-01` | — | — | 03-10-2026 |
| Dashboard | `DASH` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-DASH-01` | — | — | 03-10-2026 |
| Customers | `CUST` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CUST-01` | — | — | 03-10-2026 |
| Projects | `PRJ` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PRJ-01` | — | — | 03-10-2026 |
| Tasks | `TASK` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TASK-01` | — | — | 03-10-2026 |
| Items | `ITEM` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-ITEM-01` | — | — | 03-10-2026 |
| Proposals | `PROP` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PROP-01` | — | — | 03-10-2026 |
| Estimates | `EST` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-EST-01` | — | — | 03-10-2026 |
| Invoices | `INV` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-INV-01` | — | — | 03-10-2026 |
| Payments | `PAY` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PAY-01` | — | — | 03-10-2026 |
| Credit Notes | `CRN` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CRN-01` | — | — | 03-10-2026 |
| Subscriptions | `SUB` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-SUB-01` | — | — | 03-10-2026 |
| Contracts | `CTR` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CTR-01` | — | — | 03-10-2026 |
| Expenses | `EXP` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-EXP-01` | — | — | 03-10-2026 |
| Leads | `LEAD` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-LEAD-01` | — | — | 03-10-2026 |
| Estimate Request | `ESTREQ` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-ESTREQ-01` | — | — | 03-10-2026 |
| Support (Tickets) | `TICKET` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TICKET-01` | — | — | 03-10-2026 |
| Knowledge Base | `KBASE` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-KBASE-01` | — | — | 03-10-2026 |
| Utilities (Media · Bulk PDF Export) | `UTIL` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-UTIL-01` | — | — | 03-10-2026 |
| Calendar | `CAL` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CAL-01` | — | — | 03-10-2026 |
| Bảng tin & Thông báo | `NEWS` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-NEWS-01` | — | — | 03-10-2026 |
| Reports | `RPT` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-RPT-01` | — | — | 03-10-2026 |
| Cổng khách hàng | `PORTAL` | Web ⬜ | ⬜ Chưa khảo sát — **BLOCKED** thiếu tài khoản contact (`AMB-SYS-03`) | ⬜ Trắng | — | — | `REQ-PORTAL-01` | AMB-SYS-03 | — | 03-10-2026 |
| Nhân viên & Phân quyền | `STAFF` | Web ⬜ | ⬜ Chưa khảo sát — **BLOCKED** Access denied (`AMB-SYS-01`) | ⬜ Trắng | — | — | `REQ-STAFF-01` | AMB-SYS-01 | — | 03-10-2026 |
| Cấu hình hệ thống | `SETTING` | Web ⬜ | ⬜ Chưa khảo sát — **BLOCKED** Access denied (`AMB-SYS-01`) | ⬜ Trắng | — | — | `REQ-SETTING-01` | AMB-SYS-01 | — | 03-10-2026 |

**Danh sách prefix đã chiếm** (module mới **phải** chọn prefix khác):
`LOGIN` · `PROFILE` · `NAV` · `DASH` · `CUST` · `PRJ` · `TASK` · `ITEM` · `PROP` · `EST` · `INV` · `PAY` · `CRN` · `SUB` · `CTR` · `EXP` · `LEAD` · `ESTREQ` · `TICKET` · `KBASE` · `UTIL` · `CAL` · `NEWS` · `RPT` · `PORTAL` · `STAFF` · `SETTING` · **`SYS`** (dành riêng cho AMB/RISK cấp hệ thống — không cấp cho module)

**Bảng mã trạng thái recon:** ⬜ Chưa khảo sát · 🟨 Đang khảo sát · ✅ Đã có tài liệu · ⏸️ Hoãn · ⚪ Chưa implement

---

## 2. Trạng thái REQ toàn hệ thống

| Module | 🟢 Active | 🟡 Changed | 🔴 Deprecated | ⚪ Chưa implement / chưa kiểm chứng | Tổng |
|---|---|---|---|---|---|
| `LOGIN` | 58 | 8 | 0 | 6 | 72 |
| **Tổng** | **58** | **8** | **0** | **6** | **72** |

---

## 3. Ambiguity 🔴 High còn treo

| Mã | Phạm vi | Nội dung | Chặn module | Cần ai trả lời |
|---|---|---|---|---|
| `AMB-SYS-01` | Hệ thống | Tài khoản `EMAIL_ADMIN` tên hiển thị "Admin Example" nhưng **không phải Admin** (`is_admin` rỗng). Menu Setup rỗng, 21 URL cấu hình → Access denied. Xin tài khoản Admin thật | `STAFF` · `SETTING` · ma trận phân quyền của **mọi** module | PO / người quản trị môi trường |
| `AMB-SYS-02` | Hệ thống | Tài khoản Staff thấy 2.129 Customers nhưng chỉ 6 Invoices · 5 Proposals · 3 Estimates · 0 Expenses/Leads/Tickets → nghi quyền "chỉ xem của mình" ở nhóm Sales/Leads/Support. Xin bảng quyền của role đang gán cho tài khoản | `INV` · `PROP` · `EST` · `PAY` · `CRN` · `SUB` · `EXP` · `LEAD` · `TICKET` · `KBASE` | PO |
| `AMB-SYS-03` | Hệ thống | Chưa có tài khoản contact (khách hàng) → không đăng nhập được cổng khách hàng | `PORTAL` · phần tương tác khách hàng của `CTR` · `PROP` · `EST` · `INV` · `TICKET` | User (sẽ bổ sung `.env`) |

> `LOGIN`: 6 AMB 🔴 (AMB-LOGIN-01 → 06) đã được PO trả lời 03-10-2026 — gỡ khỏi bảng. Module **hết AMB treo** (AMB-LOGIN-30, 31 trả lời ở đợt 4). Chi tiết: [`login/impact/impact_PO-REPLY-LOGIN-20261003.md`](login/impact/impact_PO-REPLY-LOGIN-20261003.md) · [`login/impact/impact_PO-REPLY-LOGIN-20261003-02.md`](login/impact/impact_PO-REPLY-LOGIN-20261003-02.md)

Chi tiết + các AMB/RISK mức 🟡/🟢: [`_discovery/system_map.md` mục 8](_discovery/system_map.md#8-ambiguity--risk-cấp-hệ-thống).

---

## 4. Cấu trúc thư mục chuẩn

```
docs/requirements/
├── README.md                                   ← DANH MỤC (file này)
├── _discovery/                                 ← TẦNG KHÁM PHÁ — không chứa mã REQ
│   ├── system_map.md                           ← INDEX bản đồ hệ thống — TÊN BẤT BIẾN
│   ├── modules/module_NN_<slug>.md             ← chi tiết từng nhóm module
│   └── evidence/*.png                          ← 1 ảnh tổng quan mỗi module
└── <module>/                                   ← TẦNG MODULE — sinh bởi /generate-requirements-from-website
    ├── REQUIREMENTS_<TÊN_MODULE>_SUMMARY.md    ← INDEX module — TÊN BẤT BIẾN
    ├── web/
    │   ├── requirements_<module>_web.md
    │   ├── evidence/*.png
    │   └── stories/story_NN_<slug>.md          ← chỉ khi vượt ngưỡng tách
    ├── analysis/analysis_<TICKET-ID>.md
    └── impact/impact_<TICKET-ID>.md
```

Tầng nền tảng chỉ nhận `web` · `mobile` · `api`. Một nghiệp vụ = một prefix trên mọi nền tảng.

---

## 5. Quy trình sử dụng

| Tình huống | Workflow | Ghi vào đâu |
|---|---|---|
| Khảo sát chi tiết một module trên web | `/generate-requirements-from-website <module>` | `<module>/REQUIREMENTS_<TÊN_MODULE>_SUMMARY.md` + `<module>/web/` · cập nhật dòng module ở mục 1 |
| Phát hiện module bị sót / hệ thống vừa deploy tính năng mới | `/discover-system` (mode ADD / DELTA) | `_discovery/` + thêm dòng ở mục 1 |
| Có ticket thay đổi cho module đã có tài liệu | `/update-requirements-from-ticket` | `<module>/impact/impact_<TICKET-ID>.md` |
| Có tài liệu (Jira / .docx / .pdf) cần phân tích | `/analyze-requirement-document` | `<module>/analysis/analysis_<TICKET-ID>.md` |
| Sinh test case từ requirements | `/generate-testcases-manual-rbt` · `/generate-testcases-from-requirements` | `docs/testcases/<module>/` |

**Lệnh kế tiếp theo thứ tự đã chốt:** `/generate-requirements-from-website profile`

---

## 6. Nhật ký danh mục

| Ngày | Thay đổi | Người/Workflow |
|---|---|---|
| 03-10-2026 | Khởi tạo danh mục. Mode UI · 1 mặt Web. Cấp 27 prefix cho 27 module (user chốt ở checkpoint). Ghi thuộc tính dự án: tiền tố TC `CRM_`, môi trường dùng chung, năng lực kiểm thử QA (đã đo nhật ký hoạt động → Access denied). Mở `AMB-SYS-01` → `AMB-SYS-03` 🔴 | `/discover-system` |
| 03-10-2026 | `LOGIN` → ✅ Đã có tài liệu: 58 REQ (53 🟢 · 5 ⚪) · 25 AMB (6 🔴 đưa lên mục 3) · 6 RISK · 7 Story. Cập nhật mục 1, 2, 3, lệnh kế tiếp. Đối chiếu danh mục ↔ thư mục: khớp | `/generate-requirements-from-website login` |
| 03-10-2026 | `LOGIN` cập nhật theo PO trả lời 25 AMB (`PO-REPLY-LOGIN-20261003`): 72 REQ (62 🟢 · 4 🟡 · 6 ⚪) · AMB 29 (treo 4, 🔴 0) · RISK 7. Gỡ AMB-LOGIN-01 → 06 khỏi mục 3. Mục 2: sửa tên cột theo bảng trạng thái REQ của skill (🟢 Active · 🟡 Changed) — trước đây ghi nhầm "Đã/Chưa kiểm chứng" | `/update-requirements-from-ticket` |
| 03-10-2026 | `LOGIN` cập nhật theo PO trả lời AMB-LOGIN-26 → 29 (`PO-REPLY-LOGIN-20261003-02`): sửa 6 REQ (34 · 36 · 53 · 55 · 64 · 69) → 72 REQ (58 🟢 · 8 🟡 · 6 ⚪) · AMB 31 (treo 2, 🔴 0) | `/update-requirements-from-ticket` |
| 03-10-2026 | `LOGIN` cập nhật theo PO trả lời AMB-LOGIN-30, 31 (`PO-REPLY-LOGIN-20261003-03`): đổi câu chữ + thêm bản tiếng Anh cho REQ-LOGIN-36 · 55. Trạng thái REQ không đổi (58 🟢 · 8 🟡 · 6 ⚪) · AMB 31, treo 0 | `/update-requirements-from-ticket` |
