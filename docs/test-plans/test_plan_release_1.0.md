# Master Test Plan — Perfex CRM (Anh Tester Demo) · Release 1.0

## Kiểm soát tài liệu

### Thông tin tài liệu

| | |
|---|---|
| Mã tài liệu | `test_plan_release_1.0` |
| Phiên bản tài liệu | v1.0 |
| Trạng thái | 🟨 **Draft** — còn **29 ô** chờ thông tin, trong đó **5 ô chặn** tiêu chí vào |
| Mức phân loại | ❓ Chờ QA Lead |
| Ngày lập | 21-09-2026 |
| Ngày hiệu lực | — *(chưa duyệt)* |
| Người lập | Anh Tester — trưởng nhóm QA *(suy từ `Nhân lực`, phiếu để trống ô `Người lập`)* |
| Người review | ❓ Chờ QA Lead |
| Người phê duyệt | Anh Tester — QA Lead · ❓ *(Product Owner — chưa có tên)* — chữ ký ở mục 11 |
| Hệ thống · Build | Perfex CRM (Anh Tester Demo) · `v1.0.0` |
| Phiếu đầu vào | [`test_plan_release_1.0.input.yaml`](test_plan_release_1.0.input.yaml) *(bản lưu của `plans/master-test-plan/test_plan.config.yaml` tại 21-09-2026)* |
| Cấu trúc tài liệu | Biên soạn **theo cấu trúc** ISO/IEC/IEEE 29119-3 — Test Plan · phủ đủ nội dung điển hình của ISTQB CTFL v4.0 mục 5.1.1 · ánh xạ ở mục 12 |

> **Trạng thái hợp lệ:** 🟨 Draft (đang soạn / còn ô treo) → 🟦 Chờ duyệt (đã review, không còn ô treo chặn) → 🟩 Đã duyệt (đủ chữ ký mục 11) → ⬛ Hết hiệu lực (có bản mới thay thế). Sửa nội dung bản 🟩 → quay về 🟨, tăng phiên bản.

---

### ⛔ Bốn điểm lệch giữa phiếu đầu vào và `docs/` — phải chốt trước khi duyệt

Ghi lại **cả hai phía**, không tự chọn bên nào.

| # | Phiếu khai | `docs/` ghi nhận | Ảnh hưởng |
|---|---|---|---|
| L1 | `Ước lượng` → `Giả định`: *"LOGIN đã có 50 TC"* | **Không có thư mục `docs/testcases/`** — toàn hệ thống **0 test case** | Hạng mục *Viết & review TC* ước 8 người-ngày đang dựa trên giả định sai chiều **có lợi**. Xem 7.2 · rủi ro `RR-03` |
| L2 | `Ước lượng` → `Giả định`: *"CUST (79 REQ) và PRJ (104 REQ)"* | Danh mục: `CUST` và `PRJ` **⬜ Chưa khảo sát · 0 REQ**. `system_map.md` **ước** 50–70 REQ mỗi module | Không có REQ thì không viết được TC. Ước lượng **thiếu hẳn hạng mục recon**. Xem 7.2 · rủi ro `RR-01` |
| L3 | `Phạm vi` → `Ngoài phạm vi bổ sung`: *"20 module CRM còn lại (CONT · LEAD · … · PROF)"* | Danh mục có **27 module**; trừ 3 module trong phạm vi còn **24**. Trong danh sách phiếu có **6 prefix không tồn tại** (`CONT` `PAY` `TICK` `REP` `REM` `PROF`) và **thiếu 8 prefix có thật** (`TIME` `UTIL` `STAFF` `ROLE` `SETTING` `GOAL` `SURVEY` `ANN`) | Mục 2.2 là mục có giá trị pháp lý cao nhất. Bảng ở 2.2 dưới đây đã dựng lại **theo danh mục**; cần QA Lead xác nhận |
| L4 | `Công cụ` → `Tự động hoá`: *"… GitLab Actions CI"* | Repo **không có** `.gitlab-ci.yml`, `.github/workflows/`, `Jenkinsfile`, cũng **không có project automation nào** *(chỉ `scripts/testcases-viewer/package.json` — web viewer, không phải framework test)* | Mục 3.7 hiện là hiện trạng **chưa có gì**. Tên hệ thống CI cần chốt lại. Xem rủi ro `RR-05` |

> Điểm lệch thứ năm — **môi trường** — là lệch **hợp lệ**, không phải sai: phiếu khai môi trường riêng, tài liệu recon làm trên môi trường dùng chung. Đã ghi ở 2.3 và rủi ro `RR-04`.

---

### ❓ Ô còn treo (29)

**Nhóm A — QA Lead trả lời (10)**

- **A1.** Người review tài liệu này *(Kiểm soát tài liệu)*
- **A2.** Mức phân loại tài liệu: công khai · nội bộ · mật *(Kiểm soát tài liệu)*
- **A3.** Tên người giữ vai trò Product Owner / BA *(2.4 · 11)*
- **A4.** Xác nhận bảng *Ngoài phạm vi* dựng lại theo danh mục — điểm lệch `L3` *(2.2)*
- **A5.** Mục tiêu kiểm thử — 4 mục tiêu ở 1.1 là **agent đề xuất**, chờ duyệt *(1.1)*
- **A6.** Bốn loại kiểm thử để trống trong phiếu: Tương thích · Khả năng truy cập · Khả dụng · Độ tin cậy và phục hồi — **để trống nghĩa là chưa quyết định**, không phải "không làm" *(3.2 · 3.2.1)*
- **A7.** Tiêu chí chọn TC để tự động hoá *(3.7)*
- **A8.** Người bảo trì bộ automation *(3.7)*
- **A9.** Mục tiêu và tầng kiểm thử của chiến lược tự động hoá *(3.7)*
- **A10.** Ước lượng cho hạng mục recon `CUST` · `PRJ` — điểm lệch `L1` `L2` *(7.2)*

**Nhóm B — QA Lead + Dev Lead trả lời (5)**

- **B1.** Quy trình trạng thái lỗi: dùng mặc định của repo hay workflow Jira riêng *(9.1)*
- **B2.** Xác nhận thang Severity · Priority mặc định — tiêu chí ra #1, #2 đếm theo thang này *(9.2 · 9.3)*
- **B3.** Người phân loại lỗi (triage) *(9.4)*
- **B4.** Tần suất họp phân loại lỗi *(9.4)*
- **B5.** Thời hạn phản hồi và sửa xong theo từng Severity — **không có giá trị mặc định** *(9.4)*

**Nhóm C — Đội DEV trả lời (7)**

- **C1.** Dữ liệu nền của môi trường mới gồm gì *(5.2)*
- **C2.** Nguồn dữ liệu: tự sinh · nạp sẵn · bản sao production đã che · nhập tay *(5.2)*
- **C3.** Môi trường mới **có chứa dữ liệu thật của khách hàng không**, nếu có thì che bằng cách nào *(5.2)*
- **C4.** Ai dọn dữ liệu test sau khi chạy, dọn khi nào *(5.2)*
- **C5.** Làm mới dữ liệu nền theo nhịp nào *(5.2)*
- **C6.** Ai cung cấp dữ liệu kiểm thử *(5.2)*
- **C7.** ⛔ **Hệ thống CI thật là gì** — điểm lệch `L4` *(3.7 · 5.3)*

**Nhóm D — Product Owner / Quản trị hệ thống trả lời — ⛔ 5 ô CHẶN tiêu chí vào (5)**

- **D1.** ⛔ `AMB-SYS-01` 🔴 — **hệ thống có những vai trò nào?** Không trả lời thì ma trận phân quyền của cả 3 module trong phạm vi chỉ điền được một cột *(4.1 #3 · 4.1 #7 · 8.1 `RR-02`)*
- **D2.** ⛔ **Tài khoản test riêng cho từng người kiểm thử** — hiện chỉ có **1 tài khoản dùng chung** (`RISK-LOGIN-01`) *(4.1 #3 · 5.2)*
- **D3.** ⛔ `AMB-SYS-02` 🔴 — trang từ chối truy cập báo sai bản chất lỗi: cố ý hay lỗi? *(4.1 #7)*
- **D4.** ⛔ Ngưỡng độ dài mật khẩu tối thiểu (`AMB-LOGIN-10`) — giả định tạm chỉ nói *có* ràng buộc, **không nói bao nhiêu** → chưa phủ được giá trị biên *(3.2 · 8.2)*
- **D5.** ⛔ Xác nhận hoặc bác bỏ **4 giả định bảo mật** của `LOGIN` (`AMB-LOGIN-02` · `05` · `10` · `13`) — giả định sai thì **test case sai mà vẫn báo xanh** *(8.2 · 4.1 #7)*

**Nhóm E — người có bản chuẩn trả lời (2)**

- **E1.** Đối chiếu lại cột **ISO/IEC/IEEE 29119-3** ở mục 12.1 trước khi đem đi audit — agent không có bản chuẩn để tra tên mục *(12.1)*
- **E2.** Xác nhận có cần ánh xạ theo **IEEE 829-2008** (dàn mục lồng nhau) thay cho bản 1998 hay không — nếu có, xin file template của khách *(12.1)*

---

### Lịch sử thay đổi

| Phiên bản | Ngày | Người sửa | Mục thay đổi | Nội dung | Người duyệt |
|---|---|---|---|---|---|
| v1.0 | 21-09-2026 | Anh Tester (agent hỗ trợ) | Toàn bộ | Lập mới từ phiếu `test_plan.config.yaml` | ❓ |

---

## 1. Mục tiêu & Cơ sở kiểm thử

### 1.1 Mục tiêu kiểm thử

> ⚠️ Phiếu để trống mục này → **4 mục tiêu dưới đây là đề xuất của agent, chờ QA Lead / PO duyệt** *(ô treo `A5`)*.

| # | Mục tiêu | Đo bằng |
|---|---|---|
| O1 | Xác nhận cổng vào hệ thống (`LOGIN`) hoạt động đúng đặc tả trên web — đăng nhập, bảo vệ route, vòng đời phiên, tài khoản cá nhân | Tiêu chí ra #3, #4 · chỉ số *Tiến độ kiểm thử* mục 3.6 |
| O2 | Phủ được hai module nghiệp vụ gốc `CUST` và `PRJ` bằng tài liệu yêu cầu và test case trước khi thực thi | Tiêu chí ra #7 · tiêu chí vào #6, #8 |
| O3 | Không còn lỗi Critical đang mở ở ba module trong phạm vi trước ngày phát hành | Tiêu chí ra #1 |
| O4 | Thu hẹp vùng chưa kiểm chứng của `LOGIN` — 10 REQ ⚪ hiện có, trong đó 3 REQ bị chặn bởi môi trường cũ | Chỉ số *Độ phủ* mục 3.6 · tiêu chí ra #6 |

> Mục tiêu nào không đo được bằng tiêu chí ra hay chỉ số ở 3.6 thì viết lại hoặc bỏ.

### 1.2 Cơ sở kiểm thử (Test basis)

| Tài liệu | Phiên bản / ngày cập nhật | Module | Ghi chú |
|---|---|---|---|
| [`docs/requirements/login/requirements_login.md`](../requirements/login/requirements_login.md) | Nhật ký thay đổi **20-09-2026** *(2 đợt: UI recon + `DECISION-LOGIN-01`)* | `LOGIN` | **62 REQ** · 9 Story · **0 AMB treo** nhưng **17/17 đóng bằng giả định tạm**, không phải câu trả lời của PO |
| [`docs/requirements/login/web/requirements_login_web.md`](../requirements/login/web/requirements_login_web.md) | 20-09-2026 | `LOGIN` | Field Spec · Validation · 10 ảnh evidence · **Trình duyệt khảo sát: Google Chrome `153.0.0.0`** |
| [`docs/requirements/login/impact/impact_DECISION-LOGIN-01.md`](../requirements/login/impact/impact_DECISION-LOGIN-01.md) | 20-09-2026 | `LOGIN` | Căn cứ của 4 REQ mới `59`→`62` và 9 REQ sửa |
| [`docs/requirements/_discovery/system_map.md`](../requirements/_discovery/system_map.md) | 20-09-2026 | Cấp hệ thống | Bản đồ 27 module · phụ thuộc entity · 5 `AMB-SYS` |
| Tài liệu yêu cầu `CUST` | ❌ **Chưa có** | `CUST` | Phải recon trước khi viết TC — xem `RR-01` |
| Tài liệu yêu cầu `PRJ` | ❌ **Chưa có** | `PRJ` | Phải recon trước khi viết TC — xem `RR-01` |

> Cơ sở kiểm thử **đổi giữa đợt** (ticket sửa yêu cầu) → cập nhật bằng `/update-requirements-from-ticket` rồi tăng phiên bản plan — TC viết theo cơ sở cũ là TC sai.
>
> ⚠️ **Mức phủ tài liệu của cả hệ thống là ⬜ Trắng** — không có tài liệu nghiệp vụ nào từ phía khách; mọi REQ sinh từ khảo sát UI thực tế. Nghĩa là cơ sở kiểm thử **do chính đội QA dựng**, không có nguồn độc lập để đối chiếu.

## 2. Phạm vi

### 2.1 Trong phạm vi

> Mỗi dòng là **một module × một nền tảng** — đơn vị báo cáo tiến độ theo dõi và báo cáo tổng hợp chấm tiêu chí ra #7.

| Module | Prefix | Nền tảng | Số REQ | Số TC hiện có | Đã từng chạy? | Ghi chú |
|---|---|---|---|---|---|---|
| Đăng nhập & Tài khoản | `LOGIN` | Web | **62** *(🟢 44 · 🟡 8 · ⚪ 10)* | **0** | ❌ Chưa | ✅ Đã có tài liệu. 17/17 AMB đóng bằng **giả định tạm** |
| Khách hàng | `CUST` | Web | **0** — ⬜ **chưa khảo sát** | **0** | ❌ Chưa | ⚠️ Entity **gốc của 11 module**. Ước 50–70 REQ · Risk 🔴. Màn hình: List · Form 2 tab · Detail **22 tab** |
| Dự án | `PRJ` | Web | **0** — ⬜ **chưa khảo sát** | **0** | ❌ Chưa | ⚠️ Phụ thuộc `CUST`. Ước 50–70 REQ · Risk 🔴 · **5 trạng thái**. Màn hình: List · Detail 18 tab · Gantt |
| **Tổng** | | **3 cặp** | **62 / ước 162–202** | **0** | | |

> Nguồn: [`docs/requirements/README.md`](../requirements/README.md) mục 1–2 · [`system_map.md`](../requirements/_discovery/system_map.md) mục 3.
>
> ⛔ **Hai trong ba cặp chưa có một dòng yêu cầu nào.** Đây là đặc điểm chi phối toàn bộ lịch và ước lượng của đợt — xem 7.1, 7.2 và rủi ro `RR-01`.

**REQ cần quyết định lại** — phiếu khai `Đưa lại REQ bị loại vì môi trường: có`:

| REQ | Nội dung | Lý do bị loại trước đây | Quyết định theo phiếu |
|---|---|---|---|
| `REQ-LOGIN-44` | Lưu hồ sơ cá nhân | Phạm vi thao tác: **không Save dữ liệu thật** trên môi trường dùng chung | ✅ **Đưa lại vào phạm vi** — đợt này có môi trường riêng |
| `REQ-LOGIN-51` | Đổi mật khẩu | Như trên · `RISK-LOGIN-03` — thao tác gần như một chiều | 🟡 **Đưa lại có điều kiện**: môi trường riêng đã gỡ được rào cản *dữ liệu dùng chung*, nhưng vẫn **chỉ có 1 tài khoản** (`RISK-LOGIN-01`). Cần ô treo `D2` xong mới chạy được an toàn |
| `REQ-LOGIN-58` | Đổi ngôn ngữ giao diện | Như trên · `RISK-LOGIN-05` — đổi ngôn ngữ là hỏng mọi định vị theo chữ với mọi người dùng chung tài khoản | ✅ **Đưa lại vào phạm vi** — môi trường riêng |

> Bảy REQ ⚪ còn lại (`13` · `28` · `54` · `59` · `60` · `61` · `62`) **không** thuộc nhóm này — chúng ⚪ vì **dựa trên giả định tạm chưa ai xác nhận**, không phải vì môi trường. TC sinh từ nhóm đó phải gắn nhãn `assumption-based` *(ô treo `D5`)*.

### 2.2 NGOÀI phạm vi (out of scope)

> ⚠️ Bảng này **dựng lại theo danh mục `docs/requirements/README.md`** vì danh sách trong phiếu không khớp — điểm lệch `L3`. Chờ QA Lead xác nhận *(ô treo `A4`)*.

| Không kiểm thử | Lý do | Ai chịu trách nhiệm | Nguồn quyết định |
|---|---|---|---|
| **18 module nghiệp vụ truy cập được** còn lại: `TASK` `TIME` `INV` `CN` `EST` `PROP` `ESTREQ` `CTR` `SUB` `EXP` `ITEM` `TIC` `KB` `LEAD` `RPT` `DASH` `UTIL` `TODO` | Không thuộc Release 1.0 | Đợt sau | Phiếu — Anh Tester (QA Lead) · 17-09-2026 |
| **6 module bị chặn quyền**: `STAFF` `ROLE` `SETTING` `GOAL` `SURVEY` `ANN` | ⏸️ Hoãn — tài khoản hiện có bị `/admin/access_denied` ở khu Setup. `ANN` xem được nhưng không tạo được | Chờ tài khoản có quyền khu Setup | [`README.md`](../requirements/README.md) danh mục · [`system_map.md`](../requirements/_discovery/system_map.md) mục 6 |
| **Cổng khách hàng** — khu front-end ngoài `/admin` | Dùng URL khác, loại khỏi phạm vi Release 1.0 | **Chưa xếp đợt** — chờ PO chốt phạm vi cổng khách hàng | Phiếu · [`system_map.md`](../requirements/_discovery/system_map.md) mục 1 |
| Hệ thống **Book API** *(namespace `_book-api/`)* | Không thuộc Release 1.0 | Đợt sau | Phiếu — Anh Tester (QA Lead) · 17-09-2026. ⚠️ Namespace này **chưa tồn tại** trong repo |
| **Ma trận phân quyền** của cả 3 module trong phạm vi | Chỉ có 1 tài khoản; màn hình liệt kê vai trò (`/admin/roles`) bị chặn | PO / Quản trị hệ thống *(ô treo `D1`)* | `AMB-SYS-01` 🔴 · `AMB-LOGIN-01` |
| **Hành vi của `LOGIN` với các vai trò không phải quản trị** | Phạm vi đã thu hẹp còn vai trò quản trị | PO — cấp tài khoản từng vai trò | [`requirements_login.md`](../requirements/login/requirements_login.md) mục *Ngoài phạm vi* · chốt 20-09-2026 |
| **Nhánh khoá tài khoản** sau N lần đăng nhập sai | Không kiểm chứng được — thử sai trên tài khoản thật có thể khoá cả nhóm | PO xác nhận quy tắc bằng văn bản *(ô treo `D5`)* | `RISK-LOGIN-02` · `AMB-LOGIN-02` |
| **Luồng Google Authenticator** và khôi phục khi mất thiết bị (2FA) | Chưa có giả định nào, chưa khảo sát được | Đợt sau | `AMB-LOGIN-15` |
| **Tác vụ nền · thư tự động gửi đi** — phát hành hoá đơn định kỳ, hết hạn dự toán, ký điện tử của khách | Không làm được bằng giao diện quản trị; không có hộp thư để đối chiếu | Đội DEV kiểm ở tầng khác | [`system_map.md`](../requirements/_discovery/system_map.md) mục 7 |
| **App mobile · API REST** | Hệ thống **không có** app mobile, **không có** API REST *(đo 20-09-2026: mọi bảng là `POST /admin/<module>/table`, không có namespace `/api/`)* | — | [`system_map.md`](../requirements/_discovery/system_map.md) mục 1b |
| **Hiệu năng · Bảo mật chuyên sâu** | Phiếu khai `không` | Chưa xếp đợt | Phiếu — mục *Loại kiểm thử* |

> Mục này **chưa được thống nhất** — chờ chữ ký ở mục 11. Thay đổi phạm vi phải cập nhật tài liệu, tăng phiên bản và thông báo lại.
>
> 📌 **Chưa có bảng ISO/IEC 25010 để đối chiếu ô `➖`** — bảng đó nằm trong tài liệu test case, mà toàn hệ thống hiện chưa có test case nào. Khi `/generate-testcases-manual-rbt` chạy xong cho ba module, phải quay lại rà bảng này.

### 2.3 Giả định & Ràng buộc

| Loại | Nội dung | Ảnh hưởng tới kiểm thử | Nguồn |
|---|---|---|---|
| Ràng buộc | **Môi trường đợt này KHÁC môi trường đã khảo sát.** Toàn bộ tài liệu `LOGIN` recon trên **demo dùng chung**; phiếu khai đợt này dùng **môi trường test riêng** do đội DEV dựng | Cho phép đưa lại 3 REQ ⚪ (mục 2.1). Nhưng **mọi AC trong tài liệu được viết từ hành vi của môi trường cũ** — hành vi có thể lệch trên môi trường mới. Xem `RR-04` | Phiếu `Môi trường` ↔ [`README.md`](../requirements/README.md) bảng thuộc tính |
| Ràng buộc | **Chỉ có 1 tài khoản quản trị**, không có tài khoản của vai trò khác | Ma trận phân quyền ngoài phạm vi · không chạy song song nhiều người an toàn · thao tác đổi mật khẩu / 2FA có thể khoá cả nhóm | `RISK-LOGIN-01` · `AMB-SYS-01` |
| Ràng buộc | Hệ thống **không phơi API REST** | Vòng 3 (kiểm thử kỹ thuật) **không có** nhánh gọi API. Không rút được schema entity từ network → recon nặng hơn, mọi validation phải trigger thủ công từng trường | [`system_map.md`](../requirements/_discovery/system_map.md) mục 1b |
| Ràng buộc | QA **không xem được nhật ký hoạt động** (`/admin/utilities/activity_log` → `access_denied`) | Không đối chiếu được hành vi ghi nhận phía sau giao diện | [`README.md`](../requirements/README.md) — Năng lực kiểm thử |
| Ràng buộc | Giao diện hệ thống là **tiếng Anh**, có 26 ngôn ngữ + System Default | TC và automation **không** được định vị phần tử bằng chuỗi hiển thị | `RISK-LOGIN-05` |
| Giả định | **17/17 ambiguity của `LOGIN` đang đóng bằng giả định tạm**, không phải câu trả lời của PO. 4 trong số đó thuộc về **bảo mật** | ⚠️ Giả định sai → **test case sai, không phải hệ thống sai**, và bộ kiểm thử vẫn báo xanh. TC sinh từ 7 REQ ⚪ nhóm giả định phải gắn nhãn `assumption-based` | `DECISION-LOGIN-01` · [`requirements_login.md`](../requirements/login/requirements_login.md) mục 4 |
| Giả định | Đội QA tự dựng cơ sở kiểm thử từ UI — **không có tài liệu nghiệp vụ nào từ khách** | Không có nguồn độc lập để đối chiếu; sai sót trong recon sẽ đi thẳng vào TC | Mức phủ tài liệu ⬜ Trắng |
| Giả định | Khả năng **truy vấn CSDL** và **kiểm tầng tích hợp** của QA: ❔ **chưa xác nhận** | Chưa chốt được nhánh Vòng 3 của bộ TC sẽ đi tới đâu | [`README.md`](../requirements/README.md) — Năng lực kiểm thử |

### 2.4 Các bên liên quan & Giao tiếp

| Bên | Vai trò trong đợt | Liên quan tới kiểm thử | Nhận gì | Tần suất | Kênh |
|---|---|---|---|---|---|
| Anh Tester | QA Lead — duyệt plan | Điều phối đợt, phân công, chốt phạm vi và tiêu chí | Kế hoạch · báo cáo tiến độ · báo cáo tổng hợp | Hằng tuần **thứ Sáu** · cuối đợt | Jira · repo |
| ❓ *(chưa có tên — ô treo `A3`)* | Product Owner / BA — duyệt plan · thực hiện UAT | Nguồn duy nhất trả lời 5 ô treo nhóm D; người thực hiện nghiệm thu | Kế hoạch · báo cáo tiến độ · báo cáo tổng hợp | Hằng tuần **thứ Sáu** · cuối đợt | Email · Jira |
| Đội DEV | Sửa bug · dựng và duy trì môi trường test riêng | Cung cấp môi trường (mốc 05-10-2026), dữ liệu nền, build | Báo cáo lỗi | Khi phát sinh | Jira |
| Hồng · Lan · Huệ | Kiểm thử viên | Viết TC và thực thi từng cặp module × nền tảng | Kế hoạch · báo cáo lỗi | Theo đợt | Jira · repo |

**Mẫu tài liệu dùng trong đợt** — phiếu khai `Dùng mẫu có sẵn của repo: có`:

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
| Component (unit) | ❌ ngoài phạm vi QA | Đội DEV | Theo quy trình Dev |
| Component integration | ❌ ngoài phạm vi QA | Đội DEV | Theo quy trình Dev |
| System | ✅ | QA | Bộ chung mục 4 |
| System integration | ✅ | QA | Bộ chung mục 4 |
| Acceptance (UAT) | ✅ | **PO/BA nội bộ**, QA hỗ trợ | Bộ chung mục 4 *(phiếu không khai tiêu chí riêng)* |

> ISTQB v4 khuyến nghị tiêu chí vào/ra **theo từng cấp độ**. Cấp độ dùng bộ chung thì ghi "Bộ chung mục 4" — không để trống cột.
>
> ⚠️ **System integration trong đợt này có ý nghĩa hẹp hơn thường lệ**: hệ thống không phơi API, không có app mobile, và `CUST` → `PRJ` là quan hệ tích hợp **duy nhất** nằm trong phạm vi *(`CUST` là entity gốc của 11 module, nhưng 10 module còn lại đều ngoài phạm vi)*.

### 3.2 Loại kiểm thử

| Loại test | Nền tảng | Có làm? | Cách làm | Ghi chú |
|---|---|---|---|---|
| Kiểm thử chức năng | Web | ✅ | Manual theo TC — `/execute-test-cases` | Toàn bộ TC của 3 module trong phạm vi |
| Kiểm thử chức năng | Mobile | ⚪ **Ngoài phạm vi** | — | Hệ thống không có app mobile |
| Kiểm thử chức năng | API | ⚪ **Ngoài phạm vi** | — | Hệ thống không phơi API REST |
| Hồi quy | Web | ✅ | Bộ regression 20-11 → 25-11 *(4 ngày làm việc)* | Kết hợp manual + bộ automation nếu kịp |
| Kiểm thử lại lỗi (retest) | Web | ✅ | `/retest-fixed-bugs` | Critical/Major chạy **mode FULL**, Minor/Trivial mode RETEST |
| Tích hợp cross-module | Web | ✅ | `/generate-cross-module-test-plan` | Chỉ cặp `CUST` → `PRJ` — xem ghi chú 3.1 |
| Tự động hoá | Web | ✅ | `/generate-automation-framework` → `/generate-automation-web` | ⛔ Repo **chưa có** framework nào — xem 3.7 |
| Nghiệm thu (UAT) | Web | ✅ | PO/BA nội bộ thực hiện, QA hỗ trợ | 30-11 → 04-12 *(5 ngày làm việc)* |
| **Vòng 3 — kiểm thử kỹ thuật** | Web | 🟡 **Một phần** | Chỉ còn nhánh **DevTools trình duyệt** ✅ | ⚠️ Gọi API ⚪ không áp dụng · Nhật ký hoạt động ❌ bị chặn · CSDL và tầng tích hợp ❔ chưa xác nhận. **Không phải vùng trắng — là vùng bị thu hẹp** |
| Hiệu năng | Web | ❌ | — | Phiếu khai `không` → 2.2 |
| Bảo mật chuyên sâu | Web | ❌ | — | Phiếu khai `không` → 2.2. ⚠️ Xem cảnh báo dưới bảng |
| Tương thích · Khả năng truy cập · Khả dụng · Độ tin cậy và phục hồi | Web | ❓ **Chưa quyết định** | — | Phiếu **để trống** — nghĩa là chưa chốt, không phải "không" *(ô treo `A6`)* |

> ⚠️ **Bảo mật chuyên sâu ghi `không` trong khi 4 giả định bảo mật của `LOGIN` đang treo** (`AMB-LOGIN-02` khoá tài khoản · `05` xác nhận mật khẩu · `10` độ mạnh mật khẩu · `13` chống dò mật khẩu). Hai việc này khác nhau: *không làm kiểm thử bảo mật chuyên sâu* là quyết định hợp lệ, nhưng *bốn câu hỏi bảo mật chưa ai trả lời* vẫn là **nợ phải gỡ** ở ô treo `D5` — nếu không, bộ TC chức năng sẽ khẳng định hành vi sai mà vẫn xanh.

**Tỷ trọng manual/automation** *(nguyên văn phiếu)*: *"Chạy manual toàn bộ TC trên web · automate bộ Smoke và regression của LOGIN, CUST, PRJ bằng Playwright"* — chi tiết ở 3.7.

**Thứ tự ưu tiên thực thi** — theo phụ thuộc entity và rủi ro sản phẩm (8.2):

```
LOGIN  →  CUST  →  PRJ
(cổng vào     (entity gốc,     (phụ thuộc CUST,
 mọi module)   11 module tham chiếu)   5 trạng thái)
```

`LOGIN` chạy trước vì là cổng vào duy nhất; `CUST` trước `PRJ` vì `PRJ` tham chiếu `CUST`. Đây cũng là thứ tự recon đã chốt ở [`system_map.md`](../requirements/_discovery/system_map.md) mục 6.

#### 3.2.1 Kiểm thử phi chức năng

> Đủ **6 dòng**, kể cả loại không làm. Phiếu **không khai dòng nào** ở nhóm `Phi chức năng` → không có ngưỡng nào được đặt. Agent **không tự đặt số**.

| Loại | Có làm? | Mục tiêu đo | Ngưỡng chấp nhận | Cách làm · công cụ | Môi trường | Ai thực hiện | TC đã có |
|---|---|---|---|---|---|---|---|
| Hiệu năng | ❌ Không | — | — | — | — | — | 0 *(chưa có TC nào)* |
| Bảo mật | ❌ Không *(chuyên sâu)* | — | — | — | — | — | 0 |
| Tương thích | ❓ Chưa quyết định | ❓ | ❓ | ❓ | ❓ | ❓ | 0 |
| Khả năng truy cập | ❓ Chưa quyết định | ❓ | ❓ | ❓ | ❓ | ❓ | 0 |
| Khả dụng | ❓ Chưa quyết định | ❓ | ❓ | ❓ | ❓ | ❓ | 0 |
| Độ tin cậy & phục hồi | ❓ Chưa quyết định | ❓ | ❓ | ❓ | ❓ | ❓ | 0 |

> ⚠️ **Dòng *Tương thích* cần chốt sớm nhất.** Phiếu khai môi trường có **2 trình duyệt: Google Chrome (chính) và Firefox**, trong khi toàn bộ recon `LOGIN` làm trên **Chrome `153.0.0.0`** và tài liệu ghi rõ: *"Mọi AC dựa trên thông báo mặc định của trình duyệt chỉ đúng với trình duyệt này"*. Chạy TC trên Firefox mà không khai kiểm thử tương thích thì **không rõ TC fail là lỗi hệ thống hay lệch trình duyệt**. Xem `RR-06`.
>
> Ngưỡng của loại `có` là ứng viên cho **bảng tiêu chí ra bổ sung** (4.2) — gợi ý cho người duyệt, không tự thêm.

### 3.3 Kỹ thuật thiết kế test

> Chỉ liệt kê kỹ thuật **bộ TC trong phạm vi thực sự đã dùng**. Toàn hệ thống hiện **chưa có test case nào**, nên cột này chưa có dữ liệu thật.

| Kỹ thuật | Áp dụng ở đâu |
|---|---|
| — | **Xác định khi sinh TC** cho `LOGIN` × web |
| — | **Xác định khi sinh TC** cho `CUST` × web |
| — | **Xác định khi sinh TC** cho `PRJ` × web |

> Khi `/generate-testcases-manual-rbt` chạy xong, quay lại điền mục này từ bảng kỹ thuật trong tài liệu test case, và rà lại bảng ISO/IEC 25010 cho mục 2.2.

### 3.4 Mức độc lập của kiểm thử

| | |
|---|---|
| Mức độ | **Đội QA riêng trong tổ chức** *(phiếu)* |
| Thể hiện ở đâu | Đội QA gồm 1 QA Lead + 3 kiểm thử viên, phân công theo cặp module × nền tảng (mục 6); không thuộc đội DEV — đội DEV chỉ sửa bug và dựng môi trường |
| Giới hạn | UAT do **PO/BA nội bộ** thực hiện — độc lập về góc nhìn nghiệp vụ nhưng **vẫn trong tổ chức**, không phải khách hàng cuối. Cơ sở kiểm thử cũng do chính đội QA dựng từ UI *(không có tài liệu khách)* → **không có nguồn độc lập để đối chiếu yêu cầu** |

### 3.5 Retest & Regression

- Bug đã fix → `/retest-fixed-bugs`: **Critical/Major** chạy **mode FULL** *(verify + regression quanh vùng fix)*, **Minor/Trivial** chạy mode RETEST
- Mỗi build mới → chạy bộ **Smoke** trước khi thực thi tiếp
- Regression trước phát hành → cửa sổ **20-11 → 25-11-2026** *(4 ngày làm việc)*; dùng bộ regression manual, có thêm bộ automation nếu 3.7 kịp hoàn thành
- ⚠️ **Chưa có bộ Smoke nào** — cần sinh bằng `/generate-checklist-test` hoặc tách từ bộ TC ngay sau mốc 02-10-2026, vì tiêu chí vào #10 phụ thuộc nó

### 3.6 Chỉ số theo dõi

> Nhóm theo ISTQB CTFL v4.0 mục 5.3.1. `/generate-test-progress-report` báo cáo **đúng các chỉ số này** mỗi kỳ — hằng tuần, **thứ Sáu**.

| Nhóm | Chỉ số | Nguồn | Dùng để |
|---|---|---|---|
| Tiến độ kiểm thử | TC đã viết / đã review · TC đã chạy / chưa chạy · PASS · FAIL · BLOCKED | `docs/testcases/` · `execution_report.md` | Báo cáo tiến độ · tiêu chí ra #3, #4, #5, #7 |
| Tiến độ dự án | Công sức thực tế so với ước lượng 7.2 *(19.0 người-ngày)* | Báo cáo tiến độ | Phát hiện trễ sớm |
| **Tiến độ recon** *(chỉ số riêng của đợt này)* | Số REQ đã sinh cho `CUST` · `PRJ` so với ước 50–70 mỗi module | `docs/requirements/README.md` mục 2 | Cảnh báo sớm rủi ro `RR-01` — chỉ số duy nhất theo dõi được việc chặn ở đầu chuỗi |
| Lỗi | Bug mới / đã fix / đang mở theo Severity · regression phát sinh | `docs/bugs/` · Jira | Tiêu chí ra #1, #2 |
| Độ phủ | REQ có TC · REQ Critical có TC PASS | `traceability_matrix.md` | Tiêu chí ra #6 |
| Rủi ro | Trạng thái từng rủi ro ở 8.1 | Báo cáo tiến độ | Kiểm soát rủi ro |

### 3.7 Chiến lược tự động hoá

> Phiếu khai `Loại kiểm thử` → **`Tự động hoá: có`** nhưng nhóm `Chiến lược tự động hoá` **để trống hoàn toàn**. Mục này vì vậy phần lớn là ❓ *(ô treo `A7` `A8` `A9` `C7`)*.

| | |
|---|---|
| Mục tiêu | ❓ *(ô treo `A9`)* |
| **Hiện trạng** *(đọc từ repo 21-09-2026)* | ⛔ **Chưa có gì.** Không có project automation *(không có `package.json` gốc, `pom.xml`, `pyproject.toml`, `playwright.config.*`)* · **0 script** · không có thư mục `reports/` · **không có file CI nào** *(`.github/workflows/`, `.gitlab-ci.yml`, `Jenkinsfile` đều không tồn tại)*. `scripts/testcases-viewer/package.json` là web viewer xem test case, **không phải** framework kiểm thử |
| Tầng kiểm thử (kim tự tháp) | ❓ *(ô treo `A9`)* — ISTQB CTFL v4.0 mục 5.1.6: càng lên tầng UI, test càng ít, chậm và dễ vỡ. ⚠️ Đợt này **chỉ có tầng UI**: tầng unit thuộc đội DEV, tầng API **không tồn tại** vì hệ thống không phơi API → kim tự tháp đảo ngược hoàn toàn, chi phí bảo trì cao |
| Tiêu chí chọn TC để tự động | ❓ *(ô treo `A7`)* |
| Framework · report | **Playwright + TypeScript + Allure report** *(phiếu `Công cụ` → `Tự động hoá`)* · output gom trong `reports/` theo `reporting_rules.md` |
| Hệ thống CI | ❓ Phiếu ghi *"GitLab Actions CI"* — **repo không có file CI nào**. Tên hệ thống cần chốt lại *(ô treo `C7`, điểm lệch `L4`)* |
| Người bảo trì | ❓ *(ô treo `A8`)*. Phiếu phân công **Anh Tester** vai trò `tự động hoá` cho cả 3 cặp — **cùng người đang là QA Lead**. Xem `RR-07` |

**Phạm vi:**

| Tự động | Không tự động | Lý do không tự động |
|---|---|---|
| Bộ **Smoke** và **regression** của `LOGIN` · `CUST` · `PRJ` × web *(suy từ ô `Tỷ trọng thủ công / tự động`)* | ❓ *(ô treo `A9` — phiếu để trống)* | ❓ |
| | 🔒 **Đề xuất của agent, chờ duyệt:** TC sinh từ 7 REQ ⚪ nhóm `assumption-based` | Giả định chưa được xác nhận — tự động hoá sớm là đóng băng một hành vi có thể sai |
| | 🔒 **Đề xuất của agent, chờ duyệt:** TC đổi mật khẩu · bật 2FA · đổi ngôn ngữ | Thao tác gần như một chiều trên tài khoản duy nhất (`RISK-LOGIN-03` · `05`) |

**Kích hoạt chạy:**

| Bộ chạy | Khi nào | Môi trường | Ai xem kết quả | Fail thì |
|---|---|---|---|---|
| Smoke | ❓ *(đề xuất: mỗi build lên môi trường test)* | Môi trường test riêng | ❓ | Chặn thực thi manual — tiêu chí tạm dừng 4.3 |
| Regression | ❓ *(đề xuất: trước phát hành, tức trước 25-11-2026)* | Môi trường test riêng | ❓ | Phân loại bằng `/run-and-fix-tests` — **không** sửa test để né bug |

> ⚠️ Không chốt được *Kích hoạt chạy* thì **không đối chiếu được với lịch 7.1**. Bộ `trước phát hành` phải chạy xong **trước** `Hồi quy đến` (25-11-2026), và mọi bộ gắn với build đều cần môi trường sẵn sàng (05-10-2026). Xem `RR-05`.

**Nguyên tắc:**
- Script chỉ tính là xong khi đạt Definition of Done của `CLAUDE.md` — PASS ổn định ≥ 2 lần liên tiếp, đủ Allure metadata *(tên Tiếng Việt · Description · Severity · Tags · TC ID)* và screenshot cuối **mọi** test
- Kết quả automation **báo riêng**, không cộng vào pass rate manual của tiêu chí ra #3, #4
- Test chập chờn → `/analyze-flaky-tests`, **không** chạy lại tới khi xanh · UI đổi → `/heal-locators` · yêu cầu đổi → `/update-automation-from-impact`
- Locator **không** định vị bằng chuỗi hiển thị — hệ thống có 26 ngôn ngữ, một người đổi là hỏng cả bộ (`RISK-LOGIN-05`)

## 4. Tiêu chí Vào / Ra

### 4.1 Tiêu chí VÀO (Entry) — chưa đủ thì CHƯA bắt đầu test

> Nhóm theo ISTQB CTFL v4.0 mục 5.1.3: nguồn lực · testware · chất lượng ban đầu của đối tượng kiểm thử. **Trạng thái tại ngày lập plan — 21-09-2026.** Mốc *Bắt đầu thực thi* là **06-10-2026**.

| # | Nhóm | Điều kiện | Trạng thái |
|---|---|---|---|
| 1 | Nguồn lực | Nhân lực ở mục 6 đã được phân công, đủ người cho mọi cặp module × nền tảng | ✅ **Đạt** — 3 cặp, 3 kiểm thử viên, mỗi người một cặp |
| 2 | Nguồn lực | Môi trường test sẵn sàng, có dữ liệu nền | ❓ Chưa — đội DEV dựng, dự kiến **05-10-2026**. Dữ liệu nền chưa khai *(ô treo `C1`)* |
| 3 | Nguồn lực | Tài khoản test đủ mọi vai trò trong phạm vi | ❌ **KHÔNG ĐẠT** — chỉ **1 tài khoản quản trị dùng chung**, không có tài khoản vai trò khác *(`AMB-SYS-01` · `RISK-LOGIN-01` · ô treo `D1` `D2`)* |
| 4 | Nguồn lực | Công cụ sẵn sàng: quản lý bug · quản lý kết quả · automation | 🟡 **Một phần** — Jira và repo có; **framework automation và CI chưa tồn tại** *(xem 3.7)* |
| 5 | Nguồn lực | Ngân sách đã duyệt | ⚪ **Không áp dụng** — không có ngân sách riêng *(7.3)* |
| 6 | Testware | Tài liệu requirements của mọi module × nền tảng trong phạm vi đã có | ❌ **KHÔNG ĐẠT** — **1/3**: `LOGIN` ✅ 62 REQ · `CUST` ❌ 0 REQ · `PRJ` ❌ 0 REQ |
| 7 | Testware | AMB 🔴 đã được giải đáp hoặc người duyệt chấp nhận treo | 🟡 **Một phần** — `LOGIN` 17/17 đóng bằng **giả định tạm** *(rủi ro đã được chấp nhận qua `DECISION-LOGIN-01`)*; **`AMB-SYS-01` và `AMB-SYS-02` vẫn treo** *(ô treo `D1` `D3`)* |
| 8 | Testware | Test case đã viết và đã review — đủ từng nền tảng | ❌ **KHÔNG ĐẠT** — **0 TC** toàn hệ thống. Hạn: **02-10-2026** |
| 9 | Chất lượng ban đầu | Build `v1.0.0` đã deploy và truy cập được | ❓ Chưa — chờ môi trường (#2) |
| 10 | Chất lượng ban đầu | Smoke test đã pass | ❓ Chưa — **chưa có bộ Smoke nào** *(xem 3.5)* |
| 11 | Chất lượng ban đầu | *(mobile)* Bản build app cài được trên thiết bị | ⚪ **Không áp dụng** — không có app mobile |
| 12 | Chất lượng ban đầu | *(API)* Snapshot spec khớp build đang test · có quyền gọi thử | ⚪ **Không áp dụng** — hệ thống không phơi API REST |

**Tổng kết tại 21-09-2026: ✅ 1 đạt · 🟡 2 một phần · ❌ 3 không đạt · ❓ 3 chưa tới hạn · ⚪ 3 không áp dụng.**

> ⚠️ Bắt đầu test khi chưa đạt tiêu chí vào là nguyên nhân số một khiến kết quả kiểm thử không dùng được — BLOCKED tràn lan, phải chạy lại từ đầu.
>
> ⛔ **Ba tiêu chí đang KHÔNG ĐẠT (#3, #6, #8) đều nằm trên đường găng.** #6 chặn #8, #8 chặn ngày bắt đầu thực thi 06-10-2026. Xem rủi ro `RR-01`.

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

**Tiêu chí bổ sung của dự án:** *không có* — phiếu khai `Bộ tiêu chí ra: mặc định`, ô `Tiêu chí ra bổ sung` để trống.

☑ **Bộ mặc định**  ☐ Bộ mặc định + bổ sung  ☐ Bộ tiêu chí riêng của dự án

> Gợi ý ISTQB CTFL v4.0 mục 5.1.3 cho bảng bổ sung, nếu dự án muốn thêm: mật độ lỗi · đã thực hiện kiểm thử tĩnh (review requirements/TC) · mọi lỗi tìm thấy đã được báo cáo · toàn bộ regression đã được automate.
>
> 📌 **Tiêu chí #7 chấm trên 3 cặp**: `LOGIN`×web · `CUST`×web · `PRJ`×web. Cả ba hiện đều **0 TC, 0 lần chạy**.
>
> 📌 **Tiêu chí #6 cần mức Critical của REQ** — tài liệu `LOGIN` hiện phân loại REQ theo **trạng thái** (🟢🟡⚪), chưa gán **mức quan trọng**. Việc gán mức phải làm khi sinh TC, nếu không tiêu chí #6 không chấm được.
>
> ⚠️ **Dừng kiểm thử khi hết thời gian hoặc ngân sách** (ISTQB CTFL v4.0 mục 5.1.3): được coi là hợp lệ **chỉ khi** **Product Owner cùng Anh Tester (QA Lead)** đã xem xét và **chấp nhận bằng văn bản** rủi ro phát hành mà chưa đạt đủ tiêu chí. Báo cáo tổng hợp khi đó ghi rõ tiêu chí nào chưa đạt và ai chấp nhận — **không** chấm lại thành "Đạt".

### 4.3 Tiêu chí TẠM DỪNG (Suspension) & tiếp tục

**Tạm dừng kiểm thử khi:** môi trường sập > 4 giờ · build lỗi không đăng nhập được · > 30% TC BLOCKED cùng một nguyên nhân · phát hiện bug Critical chặn luồng chính.

**Tiếp tục khi:** nguyên nhân đã xử lý, có build mới, và đã chạy lại smoke.

> Ngưỡng trên **đã được xác nhận** — phiếu khai `Dùng ngưỡng tạm dừng đề xuất: có`.
>
> 📌 Dòng *(mobile) bản build không cài/không mở được* của mẫu chuẩn **không áp dụng** cho đợt này.
>
> ⚠️ Thêm một ngưỡng nữa đáng cân nhắc, **chưa được duyệt**: mất quyền truy cập **tài khoản quản trị duy nhất** → toàn đội dừng, không có đường vòng (`RISK-LOGIN-01`).

## 5. Môi trường, Dữ liệu & Công cụ

### 5.1 Môi trường kiểm thử

| | |
|---|---|
| Môi trường | **Môi trường test riêng cho Release 1.0** — URL lưu ở `.env`, **không** ghi vào tài liệu này |
| Dùng chung với đội khác? | **Không** *(phiếu)* |
| **Khác môi trường đã khảo sát?** | ✅ **CÓ** — toàn bộ tài liệu `LOGIN` recon trên **demo dùng chung** *(đo được từ dữ liệu rác của automation đội khác: `AUTO_POM_ADD_PROJECT_*`, `New Project 3008`, 172 dự án · 259 công việc)*. Hệ quả: đưa lại được 3 REQ ⚪ **(2.1)**, nhưng mọi AC trong tài liệu viết từ hành vi môi trường cũ → rủi ro `RR-04` |
| Web — trình duyệt | **Google Chrome (chính)** · **Firefox** *(phiếu)*. ⚠️ Recon làm trên Chrome `153.0.0.0` viewport `1600×750`; **Firefox chưa từng được khảo sát** → xem 3.2.1 và `RR-06` |
| Mobile — thiết bị | ⚪ Không áp dụng |
| API — nguồn spec | ⚪ Không áp dụng — hệ thống không phơi API REST |
| Người dựng · ngày sẵn sàng | **Đội DEV** · **05-10-2026** |

### 5.2 Quản lý dữ liệu kiểm thử

> ⚠️ Nhóm `Dữ liệu kiểm thử` của phiếu **để trống hoàn toàn** — 6 ô treo `C1`→`C6`. Đây là vùng thiếu thông tin lớn nhất của plan này.

| | |
|---|---|
| Dữ liệu nền | ❓ *(ô treo `C1`)* |
| Nguồn dữ liệu | ❓ *(ô treo `C2`)* |
| Tài khoản test | ⛔ **Chỉ 1 tài khoản quản trị** — nhân viên id `2`, không có quyền khu Setup. Không có tài khoản vai trò khác. Mật khẩu ở `.env` *(ô treo `D2`)* |
| Dữ liệu thật của khách hàng | ❓ *(ô treo `C3`)* — chưa biết môi trường mới lấy dữ liệu từ đâu |
| Quy tắc sinh dữ liệu | Random + traceable theo `CLAUDE.md` mục 7 — nhìn bản ghi biết test nào tạo. Mẫu: `test_<tên test>_<timestamp>@auto.test` |
| Dọn dữ liệu sau khi chạy | ❓ *(ô treo `C4`)*. 📌 Môi trường riêng **giảm** nhưng **không xoá** nhu cầu dọn — demo cũ đã tích 172 dự án rác chính vì không ai dọn |
| Làm mới dữ liệu nền | ❓ *(ô treo `C5`)* — đề xuất: trước vòng regression 20-11-2026 |
| Người cung cấp | ❓ *(ô treo `C6`)* |

> 🔒 Dữ liệu thật của khách hàng lọt vào evidence (ảnh chụp, bug report) là rủi ro lộ dữ liệu. Ô `C3` chưa trả lời → đã đưa thành rủi ro `RR-08` ở 8.1.
>
> 📸 Evidence chụp **đúng phạm vi đối tượng cần chứng minh** — module `CUST` và `PRJ` là màn hình nghiệp vụ đầy dữ liệu, ảnh full-page sẽ kéo theo tên khách hàng và số tiền không liên quan tới REQ.

### 5.3 Công cụ

| Mục đích | Công cụ | Ghi chú |
|---|---|---|
| Quản lý lỗi | **Jira** · file markdown trong repo (`docs/bugs/`) | **Nguồn chính khi lệch:** Jira cho **trạng thái bug** · repo cho **execution report** |
| Quản lý kết quả kiểm thử | **Jira** · file markdown trong repo (`docs/executions/`) | Như trên |
| Tự động hoá | **Playwright + TypeScript + Allure report** | ⛔ Chưa tồn tại trong repo — chi tiết 3.7 |
| CI | ❓ Phiếu ghi *"GitLab Actions CI"*, repo không có file CI nào *(ô treo `C7`)* | |
| Xem kết quả offline | `scripts/testcases-viewer/` · `scripts/execution-viewer/` · `scripts/bugs-viewer/` | Trang tĩnh sẵn có trong repo, mở bằng double-click |

## 6. Nhân lực & Phân công

| Vai trò | Người | Module × nền tảng phụ trách | Ghi chú |
|---|---|---|---|
| Trưởng nhóm QA | **Anh Tester** | Toàn bộ | Điều phối, chốt phạm vi, lập plan và báo cáo |
| Kiểm thử viên | **Hồng** | `LOGIN` × web | Module đã có 62 REQ — viết TC được ngay |
| Kiểm thử viên | **Lan** | `CUST` × web | ⚠️ **Phải recon trước** — 0 REQ. Entity gốc, Detail **22 tab** |
| Kiểm thử viên | **Huệ** | `PRJ` × web | ⚠️ **Phải recon trước** — 0 REQ. Phụ thuộc `CUST`, 5 trạng thái, có Gantt |
| Tự động hoá | **Anh Tester** | `LOGIN` × web · `CUST` × web · `PRJ` × web | ⚠️ **Cùng người với QA Lead** — kiêm cả ba cặp trong khi chưa có framework nào. Xem `RR-07` |

**Số người thật: 4** *(Anh Tester giữ 2 vai trò)*.

**Nhu cầu đào tạo:** phiếu không khai ô nào → **không có nhu cầu đào tạo được nêu**.
**Nhu cầu tuyển thêm:** **Không** *(phiếu khai rõ `không`)*.

> ⚠️ Phân công phủ đủ 3 cặp module × nền tảng ✅. Nhưng **hai trong ba kiểm thử viên phải làm cả việc recon lẫn việc viết TC** trong cùng 9 ngày làm việc — xem 7.2.

## 7. Lịch trình, Ước lượng & Ngân sách

### 7.1 Lịch trình & Mốc

| Mốc | Ngày | Thứ | Điều kiện hoàn thành |
|---|---|---|---|
| Duyệt kế hoạch | **21-09-2026** | Thứ 2 | Mục 11 có chữ ký — **hôm nay, plan mới ở trạng thái 🟨 Draft** |
| Hoàn tất viết & review TC | **02-10-2026** | Thứ 6 | Đủ cả 3 cặp module × nền tảng ⚠️ |
| Môi trường sẵn sàng | **05-10-2026** | Thứ 2 | Tiêu chí vào #2, #3 |
| Bắt đầu thực thi | **06-10-2026** | Thứ 3 | Đạt **toàn bộ** tiêu chí vào 4.1 |
| Báo cáo tiến độ | **Hằng tuần · thứ Sáu** | | `/generate-test-progress-report` — slug `release_1.0` |
| Đóng băng mã nguồn | **19-11-2026** | Thứ 5 | |
| Hồi quy | **20-11 → 25-11-2026** | Thứ 6 → Thứ 4 | 4 ngày làm việc |
| Nghiệm thu (UAT) | **30-11 → 04-12-2026** | Thứ 2 → Thứ 6 | 5 ngày làm việc · PO/BA nội bộ thực hiện |
| Báo cáo tổng hợp | **10-12-2026** | Thứ 5 | `/generate-test-summary-report` — slug `release_1.0` |
| Phát hành | **15-12-2026** | Thứ 3 | |

> ✅ Thứ tự các mốc hợp lệ, **không mốc nào rơi vào cuối tuần**.
>
> ⛔ **Đoạn găng là 11 ngày đầu, không phải đoạn thực thi.** Từ hôm nay tới 02-10-2026 chỉ có **9 ngày làm việc**, trong đó phải: recon `CUST` → sinh REQ → recon `PRJ` → sinh REQ → viết và review TC cho **cả ba** module. Ước lượng 7.2 **không có hạng mục recon nào**. Xem `RR-01`.
>
> 📌 Giữa *Nghiệm thu đến* (04-12) và *Báo cáo tổng hợp* (10-12) có 3 ngày làm việc đệm; giữa báo cáo và *Phát hành* (15-12) có 2 ngày — đủ để xử lý phát hiện muộn ở mức nhỏ.

### 7.2 Ước lượng công sức

| | |
|---|---|
| Kỹ thuật (ISTQB CTFL v4.0 mục 5.1.4) | **Three-point estimation** — `E = (a + 4m + b) / 6` · `SD = (b − a) / 6` |
| Giả định của ước lượng *(nguyên văn phiếu)* | *"LOGIN đã có 50 TC nên chủ yếu viết mới cho CUST (79 REQ) và PRJ (104 REQ) · không tính công sức PO/BA làm UAT · không tính thời gian Dev sửa bug"* |
| ⛔ **Đối chiếu giả định với repo** | **Cả hai vế đầu đều sai chiều có lợi** — điểm lệch `L1` `L2`: `LOGIN` có **0 TC** *(không phải 50)*; `CUST` và `PRJ` có **0 REQ** *(không phải 79 và 104)*. Nghĩa là ước lượng đang giả định **phần lớn việc đã xong**, trong khi thực tế **chưa bắt đầu** |

| Hạng mục | a (lạc quan) | m (khả năng nhất) | b (bi quan) | E = (a+4m+b)/6 | SD = (b−a)/6 |
|---|---|---|---|---|---|
| Viết & review TC cho `LOGIN` · `CUST` · `PRJ` × web | 7 | 8 | 9 | **8.0** | 0.3 |
| Thực thi manual trên Web | 5 | 6 | 7 | **6.0** | 0.3 |
| Retest bug và regression | 2 | 3 | 4 | **3.0** | 0.3 |
| Hỗ trợ UAT, lập báo cáo và quản lý đợt | 1 | 2 | 3 | **2.0** | 0.3 |
| **Tổng** | 15 | 19 | 23 | **19.0 người-ngày** | **±1.3** |

> `SD` tổng cộng trực tiếp bốn hạng mục (`0.333 × 4 = 1.333`) — **cách cộng thận trọng**, cho khoảng rộng hơn cách cộng theo căn bậc hai phương sai. Công thức ghi ở đây để người duyệt tính lại được.

**Hạng mục THIẾU trong ước lượng** *(ô treo `A10`)*:

| Hạng mục chưa được ước | Vì sao cần | Căn cứ quy mô |
|---|---|---|
| **Recon + sinh REQ cho `CUST`** | 0 REQ hiện có; không có REQ thì không có gì để viết TC | `system_map.md` ước **50–70 REQ**. Màn hình: List · Form 2 tab · Detail **22 tab**. Risk 🔴 |
| **Recon + sinh REQ cho `PRJ`** | Như trên | Ước **50–70 REQ**. List · Detail 18 tab · Gantt · **5 trạng thái**. Risk 🔴 |
| **Dựng framework automation từ đầu** | Repo chưa có gì — xem 3.7 | Playwright + TS + Allure + CI |
| **Sinh bộ Smoke** | Tiêu chí vào #10 phụ thuộc nó | 3 module |

> 📐 **Mốc so sánh có thật:** recon `LOGIN` — module *nhỏ nhất* trong ba, chỉ 6 màn hình — đã sinh ra **62 REQ**, 9 Story, 17 AMB, 5 RISK. `CUST` và `PRJ` mỗi module lớn hơn nhiều lần về số màn hình. Đây là căn cứ duy nhất hiện có để ước; **agent không tự đặt con số**.

**Đối chiếu năng lực:** 4 người × **33 ngày làm việc** *(06-10 → 19-11)* = **132 người-ngày** năng lực danh nghĩa, so với **19.0 người-ngày** ước lượng → **thừa rất lớn (dùng ~14%)**.

> ⚠️ **Con số "thừa" này không có nghĩa là đợt nhàn.** Nó nói một trong hai điều, cần QA Lead xác nhận:
> 1. Đội **không toàn thời gian** cho đợt này *(phiếu không khai tỷ lệ phân bổ)* → năng lực thật thấp hơn 132 nhiều; **hoặc**
> 2. Ước lượng **thấp** vì thiếu hẳn 4 hạng mục ở bảng trên.
>
> Ở cả hai trường hợp, **đoạn thắt cổ chai vẫn là 9 ngày làm việc trước mốc 02-10-2026** — không phải đoạn thực thi 33 ngày. Hạng mục *Viết & review TC* 8.0 người-ngày chia cho 9 ngày làm việc với 3 kiểm thử viên là khả thi **chỉ khi** REQ của `CUST` và `PRJ` đã có sẵn, mà điều đó chưa đúng.

### 7.3 Ngân sách

| Hạng mục | Số tiền | Ghi chú |
|---|---|---|
| — | — | **Không có ngân sách riêng cho kiểm thử** — chi phí nằm trong ngân sách dự án *(phiếu khai rõ `Có ngân sách riêng: không`)* |

> Vì không có ngân sách riêng, mệnh đề *"dừng khi hết ngân sách"* ở 4.2 trên thực tế chỉ còn vế **hết thời gian**.

## 8. Rủi ro

### 8.1 Rủi ro DỰ ÁN & biện pháp

> Rủi ro **của việc kiểm thử** — nhóm theo ISTQB CTFL v4.0 mục 5.2.2: tổ chức · con người · kỹ thuật · nhà cung cấp. Khả năng / Ảnh hưởng là **đề xuất của agent** — người duyệt xác nhận. `/generate-test-progress-report` theo dõi trạng thái từng dòng mỗi thứ Sáu.

| # | Nhóm | Rủi ro | Khả năng | Ảnh hưởng | Biện pháp | Nguồn phát hiện |
|---|---|---|---|---|---|---|
| `RR-01` | Kỹ thuật | **Không kịp mốc 02-10-2026.** `CUST` và `PRJ` chưa có REQ nào; 9 ngày làm việc phải làm cả recon lẫn viết TC cho 3 module | 🔴 Cao | 🔴 Cao — trượt mốc này là trượt ngày bắt đầu thực thi 06-10 | Khởi động `/generate-requirements-from-website CUST` **ngay tuần này**, `PRJ` nối tiếp *(thứ tự phụ thuộc)*. Bổ sung 2 hạng mục recon vào ước lượng 7.2. Cân nhắc giãn mốc hoặc giảm phạm vi xuống 2 module | Đối chiếu danh mục ↔ lịch ↔ ước lượng |
| `RR-02` | Tổ chức | **Ma trận phân quyền của cả 3 module không kiểm chứng được** — chỉ 1 tài khoản, `/admin/roles` bị chặn. Lỗi vượt quyền là loại lỗi nặng nhất của phân hệ tài khoản mà đợt này **không phát hiện được** | 🔴 Cao *(đã xảy ra ở `LOGIN`)* | 🔴 Cao | Xin PO cấp tài khoản từng vai trò *(ô treo `D1`)*. Trước khi có: ghi rõ ở 2.2 là ngoài phạm vi, **không** làm tròn thành đã kiểm | `AMB-SYS-01` 🔴 · `AMB-LOGIN-01` |
| `RR-03` | Tổ chức | **Ước lượng dựa trên hai giả định sai chiều có lợi** — tưởng `LOGIN` đã có 50 TC và `CUST`/`PRJ` đã có REQ | 🔴 Cao *(đã xác định là sai)* | 🟠 Trung bình–Cao | Ước lại 7.2 sau khi chốt ô treo `A10`; dùng recon `LOGIN` (62 REQ) làm mốc so sánh | Điểm lệch `L1` `L2` |
| `RR-04` | Kỹ thuật | **Môi trường đợt này khác môi trường đã khảo sát.** Mọi AC trong tài liệu `LOGIN` viết từ hành vi của demo dùng chung; hành vi trên môi trường riêng có thể lệch | 🟠 Trung bình | 🟠 Trung bình — TC fail nhầm, tốn công điều tra | Chạy bộ Smoke `LOGIN` trên môi trường mới **ngay khi có** (05-10), đối chiếu với evidence cũ trước khi kết luận là bug | Phiếu ↔ `README.md` |
| `RR-05` | Kỹ thuật | **Tự động hoá khai `có` nhưng repo chưa có gì** — không framework, không script, không CI; tên hệ thống CI còn sai | 🔴 Cao | 🟠 Trung bình — bộ regression 20-11 → 25-11 sẽ phải chạy tay hoàn toàn | Chốt ô treo `A7`–`A9` `C7`; chạy `/generate-automation-framework` sớm. **Không** tính bộ automation vào kế hoạch regression cho tới khi có script PASS ổn định 2 lần | Rà repo 21-09-2026 · điểm lệch `L4` |
| `RR-06` | Kỹ thuật | **Firefox nằm trong môi trường nhưng chưa từng được khảo sát.** AC dựa trên thông báo mặc định của trình duyệt chỉ đúng với Chrome `153.0.0.0` | 🟠 Trung bình | 🟡 Thấp–Trung bình — không phân biệt được lỗi hệ thống với lệch trình duyệt | Chốt dòng *Tương thích* ở 3.2.1 *(ô treo `A6`)*. Trước đó: chạy TC trên **Chrome**, chỉ dùng Firefox để đối chiếu, **không** mở bug từ chênh lệch thông báo trình duyệt | `requirements_login_web.md` mục 0 |
| `RR-07` | Con người | **Một người kiêm QA Lead và toàn bộ tự động hoá 3 cặp module** trong khi framework chưa tồn tại | 🟠 Trung bình | 🟠 Trung bình — việc điều phối hoặc việc automation sẽ bị hy sinh | Xác định thứ tự ưu tiên rõ ràng, hoặc chuyển automation sang sau mốc phát hành. Phiếu khai không tuyển thêm người | Mục 6 |
| `RR-08` | Tổ chức | **Chưa biết môi trường mới có dữ liệu thật của khách hàng không.** Nếu có mà không che, ảnh chụp evidence sẽ commit dữ liệu thật lên repo | 🟡 Thấp–Trung bình | 🔴 Cao nếu xảy ra — lộ dữ liệu, không xoá được bằng cách sửa file | Trả lời ô treo `C3` **trước** mốc 05-10. Trước đó: chụp evidence đúng phạm vi đối tượng, **không** full-page màn hình nghiệp vụ | Phiếu `Dữ liệu kiểm thử` để trống |
| `RR-09` | Kỹ thuật | **Một tài khoản quản trị duy nhất là điểm hỏng đơn.** Đổi mật khẩu, bật 2FA hoặc đổi ngôn ngữ nhầm là chặn cả đội | 🟡 Thấp | 🔴 Cao | Ô treo `D2`. Trước khi có tài khoản riêng: `REQ-LOGIN-51` *(đổi mật khẩu)* giữ trạng thái đưa lại **có điều kiện**; cân nhắc thêm ngưỡng tạm dừng ở 4.3 | `RISK-LOGIN-01` · `03` · `05` |
| `RR-10` | Nhà cung cấp *(nội bộ)* | **Môi trường sẵn sàng 05-10, thực thi bắt đầu 06-10** — chỉ 1 ngày đệm, không có chỗ cho sự cố dựng môi trường | 🟠 Trung bình | 🟠 Trung bình | Xin đội DEV dựng sớm hơn, hoặc chấp nhận mốc thực thi lùi theo. Theo dõi ở báo cáo tiến độ thứ Sáu 02-10 | Lịch 7.1 |
| `RR-11` | Tổ chức | **5 ô treo chặn nhất đều chờ Product Owner, mà PO chưa có tên trong phiếu** | 🟠 Trung bình | 🔴 Cao — không ai nhận câu hỏi thì 5 ô treo nhóm D không bao giờ đóng | Điền ô treo `A3` trước khi gửi duyệt plan | Phiếu `Bên liên quan` · `Phê duyệt` |

### 8.2 Rủi ro SẢN PHẨM — tóm tắt

> **Nguồn chính** vẫn là tài liệu requirements (`RISK-<MODULE>-xx`) và tài liệu test case (đánh giá RBT) của từng module — bảng này chỉ **tóm tắt** để người duyệt plan thấy ngay. Sửa rủi ro ở tài liệu nguồn, **không** sửa ở đây.

| Module | Rủi ro | Mức | Kiểm soát bằng | Nguồn |
|---|---|---|---|---|
| `LOGIN` | **Bốn giả định bảo mật chưa ai xác nhận** — khoá tài khoản · xác nhận mật khẩu · độ mạnh mật khẩu · chống dò mật khẩu. Giả định sai thì **test case sai mà vẫn báo xanh** | 🔴 | Ô treo `D5`. TC sinh từ 7 REQ ⚪ nhóm này gắn nhãn `assumption-based` | `AMB-LOGIN-02` · `05` · `10` · `13` |
| `LOGIN` | Một tài khoản quản trị duy nhất trên môi trường dùng chung — mất quyền là chặn tất cả | 🔴 | Xin tài khoản riêng; cấm thao tác có thể khoá tài khoản | [`RISK-LOGIN-01`](../requirements/login/requirements_login.md) |
| `LOGIN` | Không kiểm chứng được nhánh khoá tài khoản — nhánh này **không có bằng chứng nào** | 🔴 | Tài khoản dùng riêng để thử sai, hoặc PO xác nhận quy tắc bằng văn bản | [`RISK-LOGIN-02`](../requirements/login/requirements_login.md) |
| `LOGIN` | Đổi mật khẩu và bật 2FA là thao tác gần như một chiều — có thể khoá quyền truy cập của cả nhóm | 🟠 | Chỉ kiểm chứng trên tài khoản dùng riêng | [`RISK-LOGIN-03`](../requirements/login/requirements_login.md) |
| `LOGIN` | Đổi ngôn ngữ làm hỏng mọi định vị phần tử dựa trên chữ — 26 ngôn ngữ, một người đổi là đổi với mọi người | 🟠 | Automation **không** định vị bằng chuỗi hiển thị; kiểm ngôn ngữ ở đầu mỗi lần chạy | [`RISK-LOGIN-05`](../requirements/login/requirements_login.md) |
| `CUST` | ⚠️ **Chưa recon — chưa có `RISK-CUST-xx` nào.** Đánh giá sơ bộ ở tầng khám phá: **Risk 🔴**, là **entity gốc của 11 module**, Detail 22 tab | 🔴 *(sơ bộ)* | Recon `CUST` sinh ra rủi ro thật — xem `RR-01` | [`system_map.md`](../requirements/_discovery/system_map.md) mục 3 · 4 |
| `PRJ` | ⚠️ **Chưa recon — chưa có `RISK-PRJ-xx` nào.** Đánh giá sơ bộ: **Risk 🔴**, **5 trạng thái**, phụ thuộc `CUST`, có Gantt | 🔴 *(sơ bộ)* | Như trên | [`system_map.md`](../requirements/_discovery/system_map.md) mục 3 |
| Cấp hệ thống | Trang từ chối truy cập báo sai bản chất lỗi — hiển thị *"Something went wrong. Try again"* thay vì thông báo thiếu quyền | 🔴 | Ô treo `D3` — PO xác nhận cố ý hay lỗi | `AMB-SYS-02` |

> ⚠️ **Hai trong ba module trong phạm vi chưa có đánh giá rủi ro sản phẩm thật nào.** Bảng này sẽ đổi đáng kể sau khi recon `CUST` và `PRJ` — phải cập nhật plan (tăng phiên bản) khi điều đó xảy ra.

## 9. Quản lý lỗi

> Nội dung theo ISTQB CTFL v4.0 mục 5.5. Mục này là **phần mở rộng** so với khung 29119-3 — xem 12.1.
>
> ⚠️ Nhóm `Quản lý lỗi` của phiếu **để trống hoàn toàn**. Các bảng dưới đây là **mặc định của repo**, **chưa được dự án xác nhận** — ô treo `B1` `B2`.

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

> ⚠️ Đội dùng **Jira** làm nguồn chính cho trạng thái bug (5.3). Workflow Jira thật có thể có thêm trạng thái *Từ chối* · *Trùng* · *Hoãn* — cần chốt ở ô treo `B1`. Bug *Hoãn* vẫn tính là **đang mở** khi chấm tiêu chí ra #1, #2, trừ khi người có quyền ở 4.2 chấp nhận bằng văn bản.

### 9.2 Thang Severity

> Chép **nguyên văn** `skills-bug-reporter` — *Severity & Priority Guide*. Tiêu chí ra #1, #2 đếm theo thang này *(chờ xác nhận — ô treo `B2`)*.

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

> Severity đánh giá theo **mức ảnh hưởng kỹ thuật**; Priority theo **mức khẩn cấp business**. Hai giá trị độc lập nhau. Tester đề xuất Severity; **Priority do người phân loại lỗi chốt** *(ô treo `B3`)*.

### 9.4 Phân loại lỗi & thời hạn xử lý

| | |
|---|---|
| Người phân loại lỗi (triage) | ❓ *(ô treo `B3`)* |
| Họp phân loại lỗi | ❓ *(ô treo `B4`)* |
| **Bug đang mở tại ngày lập** | **0** — thư mục `docs/bugs/` **chưa tồn tại**, chưa có lần chạy nào nên chưa có bug nào *(đếm 21-09-2026)* |

| Severity | Thời hạn phản hồi | Thời hạn sửa xong |
|---|---|---|
| Critical | ❓ | ❓ |
| Major | ❓ | ❓ |
| Minor | ❓ | ❓ |
| Trivial | ❓ | ❓ |

> Thời hạn **không có mặc định** — phiếu để trống *(ô treo `B5`)*. Bug quá hạn là dữ liệu cho mục *trở ngại* của báo cáo tiến độ.

## 10. Sản phẩm bàn giao

| Sản phẩm | Nơi lưu | Workflow sinh ra |
|---|---|---|
| Master Test Plan + bản lưu phiếu | [`docs/test-plans/test_plan_release_1.0.md`](test_plan_release_1.0.md) · [`test_plan_release_1.0.input.yaml`](test_plan_release_1.0.input.yaml) | `/generate-master-test-plan` |
| Tài liệu requirements | `docs/requirements/<module>/web/` | `/generate-requirements-from-website` |
| Test cases | `docs/testcases/<module>/web/` | `/generate-testcases-manual-rbt` |
| Checklist Smoke | `docs/testcases/<module>/web/` | `/generate-checklist-test` |
| Execution report | `docs/executions/<module>/web/run_*/` | `/execute-test-cases` |
| Retest report | `docs/executions/<module>/web/retest_*/` | `/retest-fixed-bugs` |
| Bug report | `docs/bugs/<module>/web/` | `/create-bug-report` |
| Automation script + report | Project automation · `reports/` | `/generate-automation-framework` → `/generate-automation-web` |
| Ma trận truy vết | `traceability_matrix.md` | `/generate-traceability-matrix` |
| **Báo cáo tiến độ** | `docs/executions/test_progress_release_1.0_<YYYYMMDD>.md` | `/generate-test-progress-report` |
| **Báo cáo tổng hợp** | `docs/executions/test_summary_release_1.0_*.md` | `/generate-test-summary-report` |

> Chỉ nền tảng **web**; không có dòng mobile/API vì hệ thống không có hai mặt đó.

## 11. Phê duyệt

| Vai trò | Tên | Phiên bản duyệt | Ngày | Ý kiến |
|---|---|---|---|---|
| QA Lead | Anh Tester | — | — | ❓ Chưa duyệt |
| Product Owner | ❓ *(ô treo `A3`)* | — | — | ❓ Chưa duyệt |

> Plan ở trạng thái 🟨 **Draft** — còn 29 ô treo, trong đó 5 ô chặn tiêu chí vào. **Không bắt đầu thực thi trước khi mục này có chữ ký.**

## 12. Ánh xạ chuẩn tài liệu

### 12.1 Đối chiếu mục

> Tài liệu này biên soạn **theo cấu trúc** ISO/IEC/IEEE 29119-3 — Test Plan và phủ đủ nội dung điển hình của test plan theo **ISTQB CTFL v4.0 mục 5.1.1**. Cột IEEE 829 theo **khung Test Plan bản 1998**. Bảng dưới để người duyệt đối chiếu; **không** phải tuyên bố đã được đánh giá tuân thủ.

| Mục trong plan | ISO/IEC/IEEE 29119-3 — Test Plan | ISTQB CTFL v4.0 — 5.1.1 | IEEE 829-1998 — Test Plan |
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

**Phần mở rộng ngoài khung chuẩn:** **3.7 Chiến lược tự động hoá** · **9 Quản lý lỗi**. Hai mục này thêm vào vì PM/khách cần thấy ngay trong plan *lỗi được phân loại và xử lý thế nào* và *tự động hoá tới đâu*; không mục nào của 29119-3 bị bỏ.

> ⚠️ **Hai bản IEEE 829 có cấu trúc khác nhau.** Khung 16 mục phẳng ở cột cuối là **829-1998**. Bản 829-2008 tách thành Master Test Plan và Level Test Plan với dàn mục lồng nhau — khách yêu cầu đúng 829-2008 thì xin file template của khách rồi ánh xạ theo file đó *(ô treo `E2`)*.
>
> ⚠️ **Tên mục 29119-3** ghi theo khung Test Plan của chuẩn; giữa các bản phát hành có điều chỉnh tên gọi. Người có bản chuẩn phải **đối chiếu lại cột đó** trước khi đem đi audit *(ô treo `E1`)*. Cột **ISTQB** đã đối chiếu nguyên văn giáo trình **v4.0 (21-04-2023)**; bản sửa lỗi v4.0.1 **chưa** đối chiếu.

### 12.2 Điểm làm khác chính sách & chiến lược kiểm thử chung (Deviations)

**Không áp dụng — tổ chức chưa có Test Policy và Test Strategy** *(phiếu khai `Có chính sách kiểm thử: không` · `Có chiến lược kiểm thử: không`)*.

> Không có chuẩn chung của tổ chức nghĩa là plan này **tự nó là chuẩn** của đợt — thêm một lý do để mục 11 phải có chữ ký trước khi thực thi.
