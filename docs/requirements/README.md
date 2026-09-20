# Danh mục Requirements — Perfex CRM (Anh Tester Demo)

> **Điểm vào cấp hệ thống.** Mọi workflow đụng `docs/` đọc file này **đầu tiên** — để biết module nào đã có tài liệu, **prefix nào đã bị chiếm**, mã REQ kế tiếp bắt đầu từ đâu.
>
> Bối cảnh cấp hệ thống (sơ đồ điều hướng · phụ thuộc · risk): [`_discovery/system_map.md`](_discovery/system_map.md)

---

## Thuộc tính dự án

| Mục | Giá trị |
|---|---|
| Hệ thống | **Perfex CRM** — tiêu đề trang `Perfex CRM \| Anh Tester Demo` |
| Mặt hệ thống | **Web** duy nhất. Không có app mobile · **không có API REST** (đo 20-09-2026) |
| Phạm vi | Khu quản trị `/admin`. **Cổng khách hàng dùng URL khác — user chốt để đợt sau** |
| **Tiền tố TC ID** | **`CRM_`** — chốt 20-09-2026. Mẫu đầy đủ: `CRM_<MODULE>_TC_<3 số>`, VD `CRM_LOGIN_TC_001` |
| Mẫu mã REQ | `REQ-<MODULE>-<2 số>`, VD `REQ-CUST-01` |
| Mẫu mã AMB / RISK | `AMB-<MODULE>-<nn>` · `RISK-<MODULE>-<nn>` · cấp hệ thống dùng `AMB-SYS-<nn>` |
| URL · tài khoản | Ở `.env` (đã `.gitignore`). 🔒 **KHÔNG ghi credentials vào `docs/`** |
| **Môi trường dùng chung** | ✅ **CÓ** — đo được từ dữ liệu rác của automation người khác (`AUTO_POM_ADD_PROJECT_*`, `New Project 3008`, `Test company`). → **Bật quy tắc dọn dữ liệu test · cấm thao tác phá huỷ trên dữ liệu có sẵn** |
| **Năng lực kiểm thử của QA** | Chốt **20-09-2026** — đầu vào cho nhánh **Vòng 3** của **mọi** bộ TC sinh về sau:<br>• **Gọi API: ⚪ Không áp dụng** — hệ thống không phơi API REST. Mọi bảng là `POST /admin/<module>/table` (DataTables server-side), ghi dữ liệu là form POST thường. *(đo 20-09-2026)*<br>• **Truy vấn CSDL: ❔ chưa xác nhận** — cần user trả lời<br>• **Kiểm tầng tích hợp: ❔ chưa xác nhận** — cần user trả lời<br>• **Xem nhật ký hoạt động: ❌ KHÔNG** — `/admin/utilities/activity_log` → `access_denied`. *(đo 20-09-2026)*<br>• **DevTools trình duyệt: ✅ CÓ** *(đo 20-09-2026)* |
| Số module | **27** — 21 truy cập được · 6 bị chặn quyền |

---

## 1. Bảng danh mục module

**Prefix đã chiếm (27):** `LOGIN` `CUST` `PRJ` `TASK` `TIME` `INV` `CN` `EST` `PROP` `ESTREQ` `CTR` `SUB` `EXP` `ITEM` `TIC` `KB` `LEAD` `RPT` `DASH` `UTIL` `TODO` `STAFF` `ROLE` `SETTING` `GOAL` `SURVEY` `ANN`

> ⚠️ Module mới **phải** chọn prefix chưa có trong danh sách trên.

| # | Module | Prefix | Nền tảng | Trạng thái recon | Mức phủ tài liệu | Tài liệu | REQ đã dùng | Mã kế tiếp | AMB treo | Story | Cập nhật |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Đăng nhập & Tài khoản | `LOGIN` | Web ✅ | ✅ **Đã có tài liệu** | ⬜ Trắng | [`login/requirements_login.md`](login/requirements_login.md) | `REQ-LOGIN-01` → `62` | `REQ-LOGIN-63` | **0 treo** *(17/17 ⏭️ theo giả định)* | 9 | 20-09-2026 |
| 2 | Khách hàng | `CUST` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CUST-01` | — | — | 20-09-2026 |
| 3 | Dự án | `PRJ` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PRJ-01` | — | — | 20-09-2026 |
| 4 | Công việc | `TASK` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TASK-01` | — | — | 20-09-2026 |
| 5 | Chấm công | `TIME` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TIME-01` | — | — | 20-09-2026 |
| 6 | Hoá đơn & Thanh toán | `INV` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-INV-01` | — | — | 20-09-2026 |
| 7 | Giấy báo có | `CN` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CN-01` | — | — | 20-09-2026 |
| 8 | Dự toán | `EST` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-EST-01` | — | — | 20-09-2026 |
| 9 | Đề xuất | `PROP` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PROP-01` | — | — | 20-09-2026 |
| 10 | Yêu cầu báo giá | `ESTREQ` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-ESTREQ-01` | — | — | 20-09-2026 |
| 11 | Hợp đồng | `CTR` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CTR-01` | — | — | 20-09-2026 |
| 12 | Đăng ký định kỳ | `SUB` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-SUB-01` | — | — | 20-09-2026 |
| 13 | Chi phí | `EXP` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-EXP-01` | — | — | 20-09-2026 |
| 14 | Danh mục hàng hoá | `ITEM` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-ITEM-01` | — | — | 20-09-2026 |
| 15 | Hỗ trợ | `TIC` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TIC-01` | — | — | 20-09-2026 |
| 16 | Cơ sở tri thức | `KB` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-KB-01` | — | — | 20-09-2026 |
| 17 | Khách hàng tiềm năng | `LEAD` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-LEAD-01` | — | — | 20-09-2026 |
| 18 | Báo cáo | `RPT` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-RPT-01` | — | — | 20-09-2026 |
| 19 | Bảng điều khiển | `DASH` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-DASH-01` | — | — | 20-09-2026 |
| 20 | Tiện ích | `UTIL` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-UTIL-01` | — | — | 20-09-2026 |
| 21 | Việc cần làm | `TODO` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TODO-01` | — | — | 20-09-2026 |
| 22 | Nhân sự | `STAFF` | Web ⬜ | ⏸️ **Hoãn — chặn quyền** | ⬜ Trắng | — | — | `REQ-STAFF-01` | — | — | 20-09-2026 |
| 23 | Vai trò & Phân quyền | `ROLE` | Web ⬜ | ⏸️ **Hoãn — chặn quyền** | ⬜ Trắng | — | — | `REQ-ROLE-01` | — | — | 20-09-2026 |
| 24 | Cấu hình hệ thống | `SETTING` | Web ⬜ | ⏸️ **Hoãn — chặn quyền** | ⬜ Trắng | — | — | `REQ-SETTING-01` | — | — | 20-09-2026 |
| 25 | Mục tiêu | `GOAL` | Web ⬜ | ⏸️ **Hoãn — chặn quyền** | ⬜ Trắng | — | — | `REQ-GOAL-01` | — | — | 20-09-2026 |
| 26 | Khảo sát | `SURVEY` | Web ⬜ | ⏸️ **Hoãn — chặn quyền** | ⬜ Trắng | — | — | `REQ-SURVEY-01` | — | — | 20-09-2026 |
| 27 | Thông báo | `ANN` | Web ⬜ | ⬜ Chưa khảo sát *(xem được, không tạo được)* | ⬜ Trắng | — | — | `REQ-ANN-01` | — | — | 20-09-2026 |

**Bảng mã trạng thái recon:** ⬜ Chưa khảo sát · 🟨 Đang khảo sát · ✅ Đã có tài liệu · ⏸️ Hoãn · ⚪ Chưa implement

> Mức phủ tài liệu toàn bộ là **⬜ Trắng** — mode UI, user không cung cấp tài liệu nào. Mọi module phải recon đầy đủ từ UI thật.

---

## 2. Trạng thái REQ toàn hệ thống

**Đã có 62 REQ / 1 module** *(1/21 module truy cập được — 4,8%)*.

| Module | Tổng | 🟢 Active | 🟡 Changed | 🔴 Deprecated | ⚪ Chưa kiểm chứng | Story |
|---|---|---|---|---|---|---|
| `LOGIN` | 62 | 44 | 8 | 0 | 10 | 9 |
| **Toàn hệ thống** | **62** | **44** | **8** | **0** | **10** | **9** |

> **10 REQ ⚪ của `LOGIN` chia hai loại:** 3 REQ bị chặn bởi phạm vi thao tác *(không lưu dữ liệu thật trên môi trường dùng chung)* · **7 REQ mô tả hành vi theo giả định tạm chưa ai xác nhận** (`DECISION-LOGIN-01`). Test case sinh từ nhóm thứ hai phải gắn nhãn `assumption-based`.

26 module còn lại chưa có REQ nào. Ước tổng REQ cho 20 module truy cập được còn lại: **~440–530** *(điều chỉnh từ ước ban đầu: `LOGIN` thực tế ra 62 REQ)*. Sáu module ⏸️ chưa ước được.

---

## 3. Ambiguity 🔴 High còn treo

| Mã | Module | Nội dung | Cần ai trả lời |
|---|---|---|---|
| `AMB-SYS-01` | Cấp hệ thống | **Hệ thống có những role nào?** Chỉ có 1 account, màn hình liệt kê role (`/admin/roles`) bị chặn — đúng thứ lẽ ra dùng để trả lời. Không có câu trả lời thì **ma trận phân quyền của cả 27 module** chỉ ghi được `❔` | PO / Quản trị hệ thống — xin account từng role |
| `AMB-SYS-02` | Cấp hệ thống | **Trang từ chối truy cập báo sai bản chất lỗi.** `/admin/access_denied` hiển thị nguyên văn *"Something went wrong. Try again"* thay vì thông báo thiếu quyền → người dùng hiểu nhầm là lỗi hệ thống | PO — cố ý hay lỗi? |

Ambiguity 🟡 Medium cấp hệ thống (`AMB-SYS-03` → `05`): xem [`_discovery/system_map.md`](_discovery/system_map.md) mục 7.

### 3.1. Ambiguity đã đóng bằng **giả định tạm** — không phải đã được trả lời

| Module | Số mã | Trạng thái | Chi tiết |
|---|---|---|---|
| `LOGIN` | 17/17 | ⏭️ **Bỏ qua** *(`DECISION-LOGIN-01` · 20-09-2026)* | [`login/requirements_login.md`](login/requirements_login.md) mục 4 · [Impact Report](login/impact/impact_DECISION-LOGIN-01.md) |

> ⚠️ **`⏭️ Bỏ qua` nghĩa là "test theo giả định và chấp nhận rủi ro", KHÔNG phải "đã có câu trả lời".** Trong 17 mã của `LOGIN` có **4 giả định thuộc về bảo mật** (`AMB-LOGIN-02` khoá tài khoản · `05` xác nhận mật khẩu · `10` độ mạnh mật khẩu · `13` chống dò mật khẩu). Nếu một giả định sai thì **test case sai**, không phải hệ thống sai — và bộ kiểm thử vẫn báo xanh.
>
> 📌 **Mẫu lặp lại đáng chú ý:** `AMB-SYS-01` và `AMB-LOGIN-01` cùng một gốc — **thiếu tài khoản của các vai trò khác**. `AMB-LOGIN-01` đã đóng bằng cách **thu hẹp phạm vi** (chỉ vai trò quản trị), nhưng gốc vấn đề vẫn còn ở `AMB-SYS-01`: mọi module recon sau đây sẽ gặp lại đúng nút thắt này và ma trận phân quyền sẽ tiếp tục chỉ điền được một cột. Nên gỡ **trước** khi recon module thứ hai.

---

## 4. Cấu trúc thư mục chuẩn

```
docs/requirements/
├── README.md                              ← FILE NÀY — danh mục toàn hệ thống
├── _discovery/                            ← TẦNG KHÁM PHÁ — cấp hệ thống, KHÔNG có mã REQ
│   ├── system_map.md                      ← INDEX BẤT BIẾN
│   ├── modules/module_NN_<slug>.md        ← 13 file (27 module > 8 nên tách)
│   └── evidence/*.png                     ← 1 ảnh tổng quan mỗi module
└── <module>/                              ← TẦNG MODULE — do /generate-requirements-from-website sinh
    ├── requirements_<module>.md           ← INDEX — TÊN FILE BẤT BIẾN
    ├── web/
    │   ├── requirements_<module>_web.md
    │   └── evidence/*.png
    ├── analysis/analysis_<TICKET-ID>.md
    └── impact/impact_<TICKET-ID>.md
```

> Tầng nền tảng `web/` **luôn có**, kể cả khi hệ thống hiện chỉ có mặt web — để sau này thêm app/API không phải di chuyển file nào.

---

## 5. Quy trình sử dụng

| Tình huống | Workflow | Ghi vào đâu |
|---|---|---|
| Recon một module từ web thật | `/generate-requirements-from-website <module>` | `<module>/web/` + index module |
| Ticket mới sửa requirements đã có | `/update-requirements-from-ticket` | Sửa tại chỗ + `impact/impact_<TICKET-ID>.md` |
| Phân tích tài liệu QA đưa sau | `/analyze-requirement-document` | `<module>/analysis/` |
| Rà lại xem hệ thống có gì mới | `/discover-system` (mode DELTA) | `_discovery/` — ghi thêm Nhật ký, **không** viết đè |
| User chỉ ra module bị sót | `/discover-system` (mode ADD) | Như trên |
| Có account quyền Setup | `/discover-system` (mode ADD) rồi recon 6 module ⏸️ | Như trên |

**Sau mỗi lần recon module xong, cập nhật ngược 2 nơi:**
1. Bảng danh mục ở trên — cột `Trạng thái recon` · `Tài liệu` · `REQ đã dùng` · `Mã kế tiếp` · `AMB treo` · `Cập nhật`
2. Bảng metadata trong `requirements_<module>.md`

---

## 6. Nhật ký danh mục

| Ngày | Thay đổi |
|---|---|
| 20-09-2026 | Khởi tạo danh mục. `/discover-system` mode **UI** · mặt **Web**. Đăng ký **27 module / 27 prefix**. Chốt tiền tố TC ID `CRM_`. Ghi nhận môi trường **dùng chung** và bảng Năng lực kiểm thử của QA. 6 module ⏸️ Hoãn do chặn quyền khu Setup. Mở `AMB-SYS-01` → `AMB-SYS-05` |
| 20-09-2026 | `/generate-requirements-from-website LOGIN` — module đầu tiên có tài liệu. Cấp `REQ-LOGIN-01` → `58` (9 Story), `AMB-LOGIN-01` → `17`, `RISK-LOGIN-01` → `05`. **Thêm cột `Story`** vào bảng danh mục mục 1 theo chuẩn skill 5.7.2 (26 module chưa recon để `—`). Cập nhật mục 2 từ "chưa có REQ nào" sang bảng trạng thái thật. Đưa 6 AMB 🔴 của `LOGIN` lên mục 3 |
| 20-09-2026 | **Đối chiếu danh mục ↔ thư mục:** glob `docs/requirements/*/requirements_*.md` ra đúng 1 module (`login`), khớp với 1 dòng ✅ trong bảng. Không có module mồ côi, không có dòng trỏ tới thư mục không tồn tại, không có prefix trùng. `Mã kế tiếp` của `LOGIN` khớp REQ lớn nhất trong tài liệu module (`58` → kế tiếp `59`) |
| 20-09-2026 | `/update-requirements-from-ticket` · nguồn `DECISION-LOGIN-01`. `LOGIN`: 58 → **62 REQ** (thêm `59`→`62`, sửa 9 REQ), **17/17 AMB đóng bằng giả định tạm** → cột `AMB treo` về `0 treo`. Thêm **mục 3.1** phân biệt rõ "đóng bằng giả định" với "đã được trả lời" — `AMB-SYS-01` vẫn treo vì gốc vấn đề (thiếu tài khoản vai trò khác) chưa được gỡ, `AMB-LOGIN-01` chỉ đóng bằng cách thu hẹp phạm vi |
