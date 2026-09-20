# Requirements — Đăng nhập & Tài khoản cá nhân (`LOGIN`)

> **Đây là file INDEX của module.** Mọi workflow phía sau đọc file này trước, rồi theo [Bản đồ tài liệu](#bản-đồ-tài-liệu) sang file nền tảng.

---

## Bảng metadata

| Mục | Giá trị |
|---|---|
| **Module** | Đăng nhập & Tài khoản cá nhân |
| **Prefix** | `LOGIN` *(cấp bởi `/discover-system` 20-09-2026 — không tự đặt lại)* |
| **Hệ thống** | Perfex CRM — Anh Tester Demo |
| **Nền tảng** | **Web ✅** *(hệ thống hiện chỉ có mặt web — xem [`README.md`](../README.md))* |
| **Dải mã đã dùng** | `REQ-LOGIN-01` → `REQ-LOGIN-62` *(đợt 1: UI recon 01→58 · đợt 2: DECISION-LOGIN-01 59→62)* · `AMB-LOGIN-01` → `AMB-LOGIN-17` · `RISK-LOGIN-01` → `RISK-LOGIN-05` |
| **Mã kế tiếp** | Đợt phân tích sau bắt đầu từ `REQ-LOGIN-63` · `AMB-LOGIN-18` · `RISK-LOGIN-06` — **KHÔNG đánh lại từ 01** |
| **Số REQ** | 62 *(🟢 44 · 🟡 8 · ⚪ 10 — chi tiết ở mục 3)* |
| **Số Story** | 9 |
| **Mức phủ tài liệu** | ⬜ Trắng — không có tài liệu, toàn bộ REQ sinh từ khảo sát UI thực tế |
| **Ngày khảo sát** | 20-09-2026 *(cập nhật `DECISION-LOGIN-01` cùng ngày)* |
| **Vai trò trong phạm vi** | ⚠️ **Chỉ vai trò quản trị (admin)** — chốt 20-09-2026. Các vai trò khác **ngoài phạm vi đợt này**, xem `AMB-LOGIN-01` |
| **Trạng thái ambiguity** | 17/17 đã đóng theo **giả định tạm** *(không phải câu trả lời của PO)* — xem cảnh báo ở mục 4 |

### Tổng quan

Module `LOGIN` là **cổng vào duy nhất** của khu quản trị Perfex CRM, kèm toàn bộ những gì thuộc về tài khoản của chính người đang đăng nhập. Module quyết định ai vào được hệ thống, phiên sống bao lâu, và người dùng tự quản lý thông tin cá nhân — mật khẩu — xác thực hai lớp — ngôn ngữ ra sao.

**Trong phạm vi:**

- Đăng nhập, đăng xuất, bảo vệ route khu quản trị, vòng đời phiên
- Màn hình Quên mật khẩu (phần giao diện)
- Trang Hồ sơ cá nhân `/admin/profile`
- Trang Sửa hồ sơ `/admin/staff/edit_profile` — 3 biểu mẫu: thông tin cá nhân, đổi mật khẩu, xác thực hai lớp
- Menu tài khoản ở thanh đầu trang và chức năng đổi ngôn ngữ

**Ngoài phạm vi:**

| Hạng mục | Thuộc về |
|---|---|
| Tạo / sửa / xoá tài khoản nhân viên | Module `STAFF` *(đang ⏸️ chặn quyền)* |
| Định nghĩa vai trò và quyền hạn | Module `ROLE` *(đang ⏸️ chặn quyền)* |
| Bảng chấm công chi tiết (`My Timesheets`) | Module `TIME` — module này chỉ ghi nhận **lối vào** từ menu tài khoản |
| Danh sách dự án hiển thị trên trang hồ sơ | Module `PRJ` — module này chỉ ghi nhận **sự tồn tại** của bảng và các cột |
| Cổng đăng nhập dành cho khách hàng | Ngoài phạm vi đợt này — dùng URL khác, user chốt để đợt sau |
| **Hành vi của module với các vai trò không phải admin** | ⚠️ **Ngoài phạm vi đợt này** — chốt 20-09-2026 (`AMB-LOGIN-01`). Hệ thống **có** vai trò khác nhưng chưa có tài khoản để kiểm chứng. Mọi REQ trong tài liệu này đặc tả hành vi **quan sát được ở vai trò quản trị**; chưa khẳng định điều gì về vai trò khác |

---

## Bản đồ tài liệu

| Nền tảng | File | Story | REQ bao phủ |
|---|---|---|---|
| Chung ≥ 2 nền tảng | — | — | *(chưa có — module mới chỉ khảo sát trên web)* |
| Web | [`web/requirements_login_web.md`](web/requirements_login_web.md) | STORY-LOGIN-01 → 09 | `REQ-LOGIN-01` → `REQ-LOGIN-62` |

> Module hiện chỉ có mặt web nên **toàn bộ REQ nằm ở file nền tảng**. Khi khảo sát thêm app mobile hoặc API, rule nào trùng thì **chuyển dòng REQ cũ lên index** và mở rộng cột `Nền tảng` — **giữ nguyên mã**, không cấp mã mới (skill mục 2.2).

**Evidence:** [`web/evidence/`](web/evidence/) — 10 ảnh, danh mục đầy đủ ở mục 7 của file nền tảng.

---

## 1. Ma trận Phân quyền

Đợt khảo sát này chỉ có **một tài khoản quản trị**. User xác nhận hệ thống **có vai trò khác** nhưng chốt lượt này chỉ kiểm trên vai trò admin.

> ⚠️ **Quyết định phạm vi 20-09-2026 (`DECISION-LOGIN-01`):** cột `Vai trò khác` **ngoài phạm vi đợt này**. Ma trận vẫn giữ nguyên ký hiệu `❔` vì đó là **sự thật kỹ thuật** — ta chưa kiểm chứng. Quyết định của user thu hẹp *khối lượng công việc*, không biến "chưa biết" thành "đã biết". Khi có tài khoản vai trò khác, chỉ cần điền cột này mà không phải sửa REQ nào.

**Ký hiệu mức bằng chứng:** `✅`/`❌` = đã kiểm chứng thật · `⚠️✅`/`⚠️❌` = suy từ màn hình cấu hình quyền của hệ thống · `❔` = chưa có căn cứ · `—` = không áp dụng.

| Hành động | Chưa đăng nhập | Admin | Role khác |
|---|---|---|---|
| Mở trang đăng nhập `/admin/authentication` | ✅ | ❌ *(bị đưa về Dashboard — REQ-LOGIN-19)* | ❔ |
| Mở màn hình Quên mật khẩu | ✅ | ❔ | ❔ |
| Truy cập route khu quản trị (`/admin/`, `/admin/clients`) | ❌ *(REQ-LOGIN-16)* | ✅ *(REQ-LOGIN-21)* | ❔ |
| Xem hồ sơ cá nhân `/admin/profile` | ❔ | ✅ | ❔ |
| Mở form Sửa hồ sơ | ❔ | ✅ | ❔ |
| Lưu thay đổi thông tin cá nhân | ❔ | ❔ *(REQ-LOGIN-44 ⚪)* | ❔ |
| Đổi email đăng nhập | — *(không vào được form)* | ❌ *(REQ-LOGIN-38 · 39)* | ❔ |
| Lưu đổi mật khẩu | ❔ | ❔ *(REQ-LOGIN-51 ⚪)* | ❔ |
| Lưu thiết lập xác thực hai lớp | ❔ | ❔ *(REQ-LOGIN-54 ⚪)* | ❔ |
| Đổi ngôn ngữ giao diện | ❔ | ❔ *(REQ-LOGIN-58 ⚪)* | ❔ |
| Đăng xuất | — *(không có phiên để kết thúc)* | ✅ *(REQ-LOGIN-22 · 23)* | ❔ |

```
Tổng 33 ô (11 hàng × 3 cột) = Đã kiểm chứng 9 · Suy diễn 0 · Chưa rõ 22 · Không áp dụng 2
Hai ô "không áp dụng" là [Đổi email đăng nhập × Chưa đăng nhập] và [Đăng xuất × Chưa đăng nhập]
  — trạng thái chưa đăng nhập không tiếp cận được form hồ sơ và không có phiên để kết thúc.
Cột "Role khác" toàn bộ ❔ vì chưa có tài khoản (AMB-LOGIN-01).
11 ô ❔ ở cột "Chưa đăng nhập"/"Admin" là các hành động ghi dữ liệu — phạm vi khảo sát
  đã loại trừ trên môi trường dùng chung (RISK-LOGIN-03 · RISK-LOGIN-04).
```

> ⚠️ Ô `❔` **không đồng nghĩa với `❌`**. "Chưa kiểm chứng" khác hẳn "không có quyền".

---

## 2. Ma trận Trạng thái

Module không có entity nghiệp vụ mang vòng đời trạng thái (không tạo/duyệt/huỷ bản ghi nào). Thứ gần nhất với một máy trạng thái là **phiên đăng nhập** — ghi lại dưới đây vì nó quyết định mọi hành vi còn lại của module.

| Trạng thái hiện tại | Hành động cho phép | Trạng thái kế tiếp | Ai được thực hiện |
|---|---|---|---|
| Chưa đăng nhập | Gửi thông tin đăng nhập hợp lệ | Đã đăng nhập | Bất kỳ ai có tài khoản hợp lệ |
| Chưa đăng nhập | Gửi thông tin đăng nhập sai | Chưa đăng nhập *(kèm thông báo `Invalid email or password`)* | Bất kỳ ai |
| Chưa đăng nhập | Mở route khu quản trị | Chưa đăng nhập *(bị đưa về trang đăng nhập, không báo lý do)* | Bất kỳ ai |
| Đã đăng nhập | Dùng các chức năng khu quản trị | Đã đăng nhập | Theo quyền của tài khoản |
| Đã đăng nhập | Mở trang đăng nhập | Đã đăng nhập *(bị đưa về Dashboard)* | Người đang có phiên |
| Đã đăng nhập | Bấm `Logout` ở menu tài khoản | Chưa đăng nhập | Người đang có phiên |
| Đã đăng nhập | Phiên hết hạn do thời gian | ❔ **Chưa xác minh** — không rõ thời hạn phiên | — |

> Trạng thái xác thực hai lớp (`off` / `email` / `google`) là **thiết lập**, không phải vòng đời — đặc tả ở REQ-LOGIN-52 → 54.

---

## 3. Trạng thái REQ của module

| Ký hiệu | Số lượng | REQ |
|---|---|---|
| 🟢 Active | 44 | Toàn bộ trừ các mã liệt kê bên dưới |
| 🟡 Changed / kiểm chứng một phần | 8 | `REQ-LOGIN-06` *(chỉ kiểm chứng được một phía)* · `REQ-LOGIN-37` *(suy từ nhãn)* · **sửa bởi `DECISION-LOGIN-01`:** `REQ-LOGIN-14` · `17` · `18` · `26` · `27` · `42` |
| 🔴 Deprecated | 0 | — |
| ⚪ Chưa kiểm chứng | 10 | **Bị chặn bởi phạm vi thao tác:** `REQ-LOGIN-44` *(lưu hồ sơ)* · `51` *(đổi mật khẩu)* · `58` *(đổi ngôn ngữ)*<br>**Dựa trên giả định tạm:** `REQ-LOGIN-13` *(Remember me)* · `28` *(Quên mật khẩu)* · `54` *(2FA)* · `59` *(không khoá tài khoản)* · `60` *(bỏ trống ô xác nhận)* · `61` *(độ dài tối thiểu)* · `62` *(máy chủ kiểm trường bắt buộc)* |

**Tổng: 62 REQ** (44 + 8 + 0 + 10 = 62 ✔)

> **10 REQ ⚪ chia hai loại khác nhau, đừng lẫn:**
> - **3 REQ** bị chặn vì phạm vi thao tác (không lưu dữ liệu thật trên môi trường dùng chung) — hành vi *có thật*, chỉ là chưa chạy tới.
> - **7 REQ** mô tả hành vi **theo giả định tạm**, chưa ai xác nhận. Kịch bản kiểm thử sinh từ nhóm này phải gắn nhãn `assumption-based`; nếu giả định sai thì **chính test case là thứ sai**, không phải hệ thống.

---

## 4. Điểm Mơ Hồ & Rủi Ro

> ## 🚨 Cảnh báo bắt buộc đọc trước khi dùng tài liệu này
>
> **17/17 ambiguity đã đóng bằng `⏭️ Bỏ qua` — nghĩa là "test theo giả định tạm và chấp nhận rủi ro", KHÔNG phải "PO đã trả lời".**
>
> | Điều này có nghĩa | Điều này KHÔNG có nghĩa |
> |---|---|
> | Đội kiểm thử được phép tiến hành, không chờ nữa | ❌ Các câu hỏi đã được giải đáp |
> | Kịch bản kiểm thử bám theo giả định đã ghi | ❌ Giả định đã được xác minh là đúng |
> | Giả định sai → **test case sai**, không phải hệ thống sai | ❌ Test xanh = hệ thống không có lỗi ở vùng đó |
>
> **Bốn giả định thuộc về bảo mật, rủi ro cao nhất nếu sai:**
>
> | AMB | Giả định đang áp | Hậu quả nếu giả định sai |
> |---|---|---|
> | `AMB-LOGIN-02` | Không có cơ chế khoá tài khoản | Bỏ sót toàn bộ nhánh khoá tài khoản — hoặc ngược lại, hệ thống **thật sự** không chống được dò mật khẩu |
> | `AMB-LOGIN-05` | Đổi mật khẩu được dù bỏ trống ô xác nhận | Người dùng gõ nhầm mật khẩu mới sẽ tự khoá mình khỏi hệ thống mà không có test nào bắt được |
> | `AMB-LOGIN-10` | Máy chủ có ràng buộc độ dài tối thiểu | Đặt được mật khẩu một ký tự mà bộ kiểm thử vẫn báo xanh |
> | `AMB-LOGIN-13` | Có giới hạn tần suất ở tầng hạ tầng | Cổng đăng nhập bị dò mật khẩu không giới hạn |
>
> **Kịch bản kiểm thử sinh từ 7 REQ ⚪ nhóm "dựa trên giả định" (`13` · `28` · `54` · `59` · `60` · `61` · `62`) phải gắn nhãn `assumption-based`** để khi PO trả lời thật thì biết ngay cái nào phải viết lại.

### 4.1. Ambiguities

| Mã | Câu hỏi | Nguy cơ nếu không giải quyết | Mức độ | Assumption tạm | Trạng thái | Kết luận |
|---|---|---|---|---|---|---|
| `AMB-LOGIN-01` | Hệ thống có những vai trò nào, và mỗi vai trò làm được gì trong module này? Xin tài khoản của từng vai trò để kiểm chứng **22 ô đang `❔`** ở ma trận phân quyền | Ma trận phân quyền chỉ tin được 9/33 ô. Không phát hiện được lỗi vượt quyền — loại lỗi nặng nhất của phân hệ tài khoản | 🔴 | Mọi vai trò đều tự quản lý được hồ sơ và mật khẩu của chính mình | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | **Phạm vi thu hẹp** *(không phải câu trả lời)*: đợt này chỉ đặc tả vai trò quản trị. 22 ô `❔` ở ma trận **giữ nguyên** — mở lại AMB này khi có tài khoản vai trò khác |
| `AMB-LOGIN-02` | Hệ thống có khoá tài khoản sau N lần đăng nhập sai không? Nếu có thì N bằng bao nhiêu, khoá trong bao lâu, và thông báo hiển thị là gì? | Không viết được test case cho toàn bộ nhánh khoá tài khoản. Nếu **không có** cơ chế khoá thì đây là lỗ hổng chống dò mật khẩu | 🔴 | Không có cơ chế khoá — mọi lần sai đều trả cùng `Invalid email or password` | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định → sinh **REQ-LOGIN-59** ⚪. Chấp nhận rủi ro bỏ sót toàn bộ nhánh khoá tài khoản. Nếu giả định đúng thì đây là **khiếm khuyết bảo mật cần báo cáo**, không phải hành vi mong muốn |
| `AMB-LOGIN-03` | Checkbox `Remember me` có `value="estimate"` — cố ý hay lỗi sao chép mã? | Giá trị gửi lên máy chủ không mang ngữ nghĩa của chức năng. Nếu máy chủ đối chiếu giá trị này thì chức năng có thể **không hoạt động** mà không ai biết | 🔴 | Máy chủ chỉ xét sự có mặt của tham số, không xét giá trị → chức năng vẫn chạy | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định → cập nhật **REQ-LOGIN-13**. Giá trị `estimate` vẫn được ghi nguyên trạng ở REQ-LOGIN-12 để automation dùng đúng |
| `AMB-LOGIN-04` | Vì sao chuyển hướng xác thực trả mã `307` thay vì `302`/`303`? | `307` giữ nguyên phương thức và thân yêu cầu — với `GET` thì vô hại, nhưng lệch quy ước và có thể gây hành vi lạ nếu về sau áp cho `POST`. Cùng hệ thống lại dùng `303` đúng chuẩn cho luồng sau đăng nhập | 🟡 | Là lựa chọn của framework, không ảnh hưởng người dùng | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định. ⚠️ **Vẫn CẤM assert mã `307`** ở REQ-LOGIN-16 · 19 · 22 — giả định "framework chọn vậy" **không** loại trừ được khả năng công cụ đo ghi nhận sai (skill 4.3.9) |
| `AMB-LOGIN-05` | Ô `Repeat new password` không bắt buộc — người dùng đổi mật khẩu mà bỏ trống ô xác nhận thì có được không? | Mất hoàn toàn cơ chế chống gõ nhầm. Người dùng gõ sai mật khẩu mới sẽ **tự khoá mình khỏi hệ thống** | 🔴 | Máy chủ chấp nhận, đổi mật khẩu theo ô `newpassword` | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định → sinh **REQ-LOGIN-60** ⚪. 🚨 REQ đó mô tả hành vi giả định, **không** xác nhận nó đúng đắn — thiếu cơ chế chống gõ nhầm vẫn là khiếm khuyết cần báo cáo |
| `AMB-LOGIN-06` | Khi người dùng bị đưa về trang đăng nhập do chưa xác thực hoặc hết phiên, hệ thống có nên báo lý do không? | Người dùng đang làm việc bị đá ra giữa chừng mà không hiểu vì sao — dễ báo nhầm thành lỗi hệ thống | 🟡 | Hành vi cố ý, không báo gì | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(hành vi cố ý)* → **REQ-LOGIN-17** chuyển 🟡. Kịch bản kiểm thử khẳng định "không có thông báo", không mở bug |
| `AMB-LOGIN-07` | Sau khi buộc đăng nhập, hệ thống có nên đưa người dùng về đúng trang họ đang muốn mở không? | Người dùng phải tự tìm lại đường. Với thao tác sâu nhiều bước thì đây là mất mát thật sự | 🟡 | Hành vi cố ý, luôn về Dashboard | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(hành vi cố ý)* → **REQ-LOGIN-18** chuyển 🟡. Không mở bug về việc mất trang đích |
| `AMB-LOGIN-08` | Màn hình Quên mật khẩu không có lối quay lại trang đăng nhập — cố ý hay thiếu sót? | Người dùng vào nhầm phải dùng nút Back của trình duyệt hoặc gõ lại URL | 🟡 | Thiếu sót giao diện | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(thiếu sót giao diện)* → **REQ-LOGIN-26** chuyển 🟡. Chấp nhận không sửa ở giai đoạn này, không chặn phát hành |
| `AMB-LOGIN-09` | Tiêu đề trang Quên mật khẩu vẫn là tiêu đề trang đăng nhập — cố ý? | Tab trình duyệt không phân biệt được hai màn hình; ảnh hưởng người dùng mở nhiều tab và công cụ đo hành vi | 🟡 | Thiếu sót, không cố ý | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(thiếu sót, không cố ý)* → **REQ-LOGIN-27** chuyển 🟡. Chấp nhận không sửa ở giai đoạn này |
| `AMB-LOGIN-10` | Mật khẩu mới có ràng buộc độ mạnh nào ở phía máy chủ không (độ dài, ký tự bắt buộc)? | Không biết biên để viết test case. Nếu không có ràng buộc nào thì đặt được mật khẩu một ký tự | 🔴 | Máy chủ có kiểm độ dài tối thiểu nhưng giao diện không nêu | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định → sinh **REQ-LOGIN-61** ⚪. ⛔ **Ngưỡng cụ thể vẫn chưa có** — giả định chỉ nói *có* ràng buộc, không nói *bao nhiêu* → **chưa phủ được giá trị biên**. Cần PO chốt con số |
| `AMB-LOGIN-11` | Vì sao lỗi thiếu trường trả về trong cùng phản hồi (`200`) còn lỗi sai thông tin lại đi qua chuyển hướng (`303`)? | Hai kịch bản kiểm thử phải chờ khác nhau. Không biết trước thì test tự động sẽ chập chờn | 🟡 | Là cách framework xử lý hai loại lỗi khác nhau, không phải lỗi | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(cách framework xử lý, không phải lỗi)* → **REQ-LOGIN-14** chuyển 🟡. Hai đường phản hồi được coi là hành vi đúng |
| `AMB-LOGIN-12` | Cookie phiên có những cờ nào (`Secure`, `SameSite`), thời hạn bao lâu, và có được cấp lại sau khi đăng nhập không? | Không đánh giá được rủi ro cố định phiên (session fixation) và không kiểm chứng được `Remember me` | 🟡 | Cookie có `HttpOnly` *(đã kiểm chứng)*; các cờ khác chưa rõ | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định. **Không sinh REQ** — giả định không kết luận thêm được gì ngoài `HttpOnly` đã kiểm chứng. ⚠️ Chấp nhận **không biết** cờ `Secure` / `SameSite` và thời hạn phiên |
| `AMB-LOGIN-13` | Hệ thống chống đăng nhập tự động hàng loạt bằng cách nào? Không có CAPTCHA, chưa quan sát được giới hạn số lần thử | Cổng đăng nhập có thể bị dò mật khẩu không giới hạn | 🔴 | Có giới hạn ở tầng máy chủ hoặc hạ tầng nhưng không lộ ra giao diện | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(có giới hạn ở tầng hạ tầng)*. **Không sinh REQ** — hành vi này không quan sát được từ giao diện nên không viết được AC kiểm được. ⚠️ Phân biệt với `AMB-LOGIN-02`: giả định là **không khoá tài khoản** nhưng **có giới hạn tần suất** ở hạ tầng — hai cơ chế khác nhau, không mâu thuẫn |
| `AMB-LOGIN-14` | Gửi yêu cầu khôi phục mật khẩu cho email **không tồn tại** thì hệ thống báo gì? Có tiết lộ email đó tồn tại hay không? | Nếu thông báo khác nhau giữa email có thật và không có thật thì đây là kênh dò tài khoản — phá vỡ nguyên tắc đã làm đúng ở màn hình đăng nhập (REQ-LOGIN-06) | 🟡 | Thông báo chung cho mọi trường hợp | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(thông báo chung)* → cập nhật **REQ-LOGIN-28**. Nội dung nguyên văn chưa biết → kịch bản chỉ so sánh hai thông báo có giống nhau không |
| `AMB-LOGIN-15` | Bật xác thực hai lớp rồi thì luồng đăng nhập thay đổi ra sao? Có màn hình nhập mã riêng không? Mất thiết bị thì khôi phục thế nào? | Toàn bộ nhánh đăng nhập có 2FA không có test case. Đây là nhánh dễ hỏng và hậu quả nặng | 🟡 | Bật `email` thì sau bước nhập mật khẩu có thêm màn hình nhập mã gửi qua email | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(có màn hình nhập mã qua email)* → cập nhật **REQ-LOGIN-54**. ⛔ Luồng `Google Authenticator` và cách khôi phục khi mất thiết bị **vẫn chưa có giả định nào** |
| `AMB-LOGIN-16` | Danh sách `Direction` có lựa chọn đầu tiên rỗng cả giá trị lẫn nhãn — cố ý? | Người dùng chọn nhầm mục rỗng thì lưu giá trị gì? Tự động hoá đếm 3 lựa chọn trong khi người dùng chỉ thấy 2 | 🟡 | Là mục giữ chỗ vô hại, máy chủ bỏ qua giá trị rỗng | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(mục giữ chỗ vô hại)* → **REQ-LOGIN-42** chuyển 🟡. Kịch bản vẫn phải đếm đủ 3 lựa chọn và không chọn mục rỗng |
| `AMB-LOGIN-17` | Các trường có nhãn `*` nhưng DOM không có thuộc tính `required` — hệ thống có kiểm tra ở máy chủ không, hay chỉ dựa vào tầng trình duyệt? | Nếu chỉ chặn ở trình duyệt thì vô hiệu hoá JavaScript là gửi được dữ liệu rỗng. Ảnh hưởng cả form hồ sơ lẫn form đổi mật khẩu | 🟡 | Máy chủ có kiểm lại — form đăng nhập đã chứng minh máy chủ biết kiểm tra trường bắt buộc | ⏭️ **Bỏ qua** — áp giả định tạm *(20-09-2026 · DECISION-LOGIN-01)* | Áp giả định *(máy chủ có kiểm lại)* → sinh **REQ-LOGIN-62** ⚪. Căn cứ: form đăng nhập đã chứng minh máy chủ biết kiểm trường bắt buộc |

### 4.2. Risks

| Mã | Rủi ro | Mô tả | Mitigation |
|---|---|---|---|
| `RISK-LOGIN-01` | Một tài khoản quản trị duy nhất trên môi trường dùng chung | Toàn bộ đợt kiểm thử phụ thuộc một tài khoản mà nhiều người cùng dùng. Mất quyền truy cập là chặn tất cả | Xin thêm tài khoản riêng cho từng người kiểm thử. Trước đó: cấm mọi thao tác có thể khoá tài khoản |
| `RISK-LOGIN-02` | Không kiểm chứng được nhánh khoá tài khoản | Thử sai mật khẩu trên tài khoản thật có thể kích hoạt khoá và chặn cả nhóm. Nhánh này vì vậy **không có** bằng chứng nào | Xin một tài khoản dùng riêng để thử sai, hoặc xin PO xác nhận quy tắc bằng văn bản (`AMB-LOGIN-02`) |
| `RISK-LOGIN-03` | Đổi mật khẩu và bật xác thực hai lớp là thao tác gần như một chiều | Đổi mật khẩu làm sai lệch thông tin đăng nhập đang cấu hình; bật 2FA có thể khoá quyền truy cập của cả nhóm nếu không nhận được mã | Chỉ kiểm chứng trên tài khoản dùng riêng. Trên môi trường hiện tại: dừng ở mức kiểm tra giao diện |
| `RISK-LOGIN-04` | Dữ liệu hồ sơ có thể bị người khác sửa giữa lúc kiểm thử | Môi trường dùng chung, nhiều người cùng thao tác trên một tài khoản. Kết quả kiểm thử hồ sơ có thể sai lệch do người khác | Ghi lại giá trị trước khi thao tác; chạy lại kịch bản nếu nghi ngờ; ưu tiên khung giờ ít người dùng |
| `RISK-LOGIN-05` | Đổi ngôn ngữ làm hỏng mọi định vị phần tử dựa trên chữ | 26 ngôn ngữ khả dụng; một người đổi là toàn bộ chuỗi giao diện đổi theo với mọi người dùng chung tài khoản | Kịch bản tự động **không** định vị phần tử bằng chuỗi hiển thị. Kiểm tra ngôn ngữ giao diện ở đầu mỗi lần chạy |

---

## 5. Phân rã Epic / Story

| Story ID | Tên Story | REQ bao phủ | Số REQ | AMB / RISK liên quan | Ghi chú phạm vi |
|---|---|---|---|---|---|
| STORY-LOGIN-01 | Đăng nhập vào khu quản trị | `REQ-LOGIN-01` → `15` · **`59`** | 16 | AMB-LOGIN-02 · 03 · 11 · 13 · RISK-LOGIN-02 | Story lớn nhất và rủi ro cao nhất. `REQ-LOGIN-59` *(không khoá tài khoản)* là **giả định**, cần tài khoản riêng mới kiểm được |
| STORY-LOGIN-02 | Bảo vệ route và vòng đời phiên | `REQ-LOGIN-16` → `21` | 6 | AMB-LOGIN-06 · 07 | Nền tảng bảo mật của **mọi** module khác, không riêng `LOGIN` |
| STORY-LOGIN-03 | Đăng xuất | `REQ-LOGIN-22` → `24` | 3 | — | Gồm cả việc phân biệt lối đăng xuất dùng được với phần tử chỉ tồn tại trong DOM |
| STORY-LOGIN-04 | Khôi phục mật khẩu | `REQ-LOGIN-25` → `28` | 4 | AMB-LOGIN-08 · 09 · 14 | Mới khảo sát được phần giao diện — luồng gửi email chưa chạy |
| STORY-LOGIN-05 | Xem hồ sơ cá nhân | `REQ-LOGIN-29` → `34` | 6 | — | Trang chỉ đọc, phủ đầy đủ |
| STORY-LOGIN-06 | Sửa thông tin cá nhân | `REQ-LOGIN-35` → `44` · **`62`** | 11 | AMB-LOGIN-16 · 17 · RISK-LOGIN-04 | Validation tầng trình duyệt phủ đầy đủ; `REQ-LOGIN-62` *(máy chủ kiểm lại)* là **giả định** |
| STORY-LOGIN-07 | Đổi mật khẩu | `REQ-LOGIN-45` → `51` · **`60`** · **`61`** | 9 | AMB-LOGIN-05 · 10 | Story chịu ảnh hưởng nặng nhất của `DECISION-LOGIN-01`: 2 trong 9 REQ là giả định về bảo mật |
| STORY-LOGIN-08 | Xác thực hai lớp | `REQ-LOGIN-52` → `54` | 3 | AMB-LOGIN-15 | Chỉ mô tả được thiết lập; hiệu lực là **giả định**, luồng Google Authenticator vẫn trắng |
| STORY-LOGIN-09 | Menu tài khoản và ngôn ngữ | `REQ-LOGIN-55` → `58` | 4 | RISK-LOGIN-05 | 26 ngôn ngữ — ảnh hưởng trực tiếp tới chiến lược định vị phần tử |

**Dòng tổng kiểm chứng:**

```
Tổng: 9 Story / 62 REQ — mọi REQ thuộc đúng một Story, không mồ côi, không trùng
16 + 6 + 3 + 4 + 6 + 11 + 9 + 3 + 4 = 62 ✔
```

### Bảng đối chiếu AMB / RISK

| Nhóm | Mã | Nằm ở đâu |
|---|---|---|
| AMB thuộc Story | `AMB-LOGIN-02` · `03` · `05` · `06` · `07` · `08` · `09` · `10` · `11` · `13` · `14` · `15` · `16` · `17` | Phân bổ ở bảng Story |
| AMB cấp Epic | `AMB-LOGIN-01` | Ma trận Phân quyền (mục 1) — cắt ngang mọi Story |
| AMB cấp Epic | `AMB-LOGIN-04` | Mã chuyển hướng xác thực — xuất hiện ở STORY-02, 03 và cả luồng của STORY-01, không quy về một Story |
| AMB cấp Epic | `AMB-LOGIN-12` | Giới hạn của công cụ khảo sát — ảnh hưởng REQ-LOGIN-13 (STORY-01) và REQ-LOGIN-20 (STORY-02) |
| RISK thuộc Story | `RISK-LOGIN-02` · `04` · `05` | Phân bổ ở bảng Story |
| RISK cấp Epic | `RISK-LOGIN-01` | Rủi ro mức môi trường — chi phối toàn bộ 9 Story |
| RISK cấp Epic | `RISK-LOGIN-03` | Cắt ngang STORY-07 và STORY-08 |

Tổng: **17 AMB** (14 thuộc Story + 3 cấp Epic ✔) · **5 RISK** (3 thuộc Story + 2 cấp Epic ✔)

### Hạng mục cấp Epic — cố ý không gán vào Story nào

| Hạng mục | Lý do |
|---|---|
| Ma trận Phân quyền (mục 1) | Cắt ngang mọi Story — mỗi hành động trong ma trận thuộc một Story khác nhau |
| Ma trận Trạng thái phiên (mục 2) | Máy trạng thái phiên chi phối STORY-01, 02, 03 cùng lúc |
| Yêu cầu phi chức năng (mục 6 của file nền tảng) | Áp cho toàn module, không thuộc một luồng cụ thể |
| `AMB-LOGIN-04` · `AMB-LOGIN-12` · `RISK-LOGIN-01` · `RISK-LOGIN-03` | Xem bảng đối chiếu ngay trên |

### Thứ tự triển khai đề xuất

> ✅ **Cập nhật `DECISION-LOGIN-01` (20-09-2026):** không Story nào còn bị **ambiguity** chặn — cả 17 AMB đã đóng theo giả định. Cái chặn còn lại là **rủi ro thao tác** (`RISK`), tức là *không làm được trên môi trường này*, không phải *không biết phải kỳ vọng gì*.

| # | Story | Lý do xếp trước | Tình trạng |
|---|---|---|---|
| 1 | **STORY-LOGIN-01** Đăng nhập | Mọi Story khác đều cần đăng nhập được trước. Rủi ro cao nhất | ✅ Sẵn sàng — **trừ** `REQ-LOGIN-59` *(giả định, cần tài khoản riêng — `RISK-LOGIN-02`)* |
| 2 | **STORY-LOGIN-02** Bảo vệ route | Nền tảng bảo mật cho **mọi** module của hệ thống, không chỉ module này | ✅ Sẵn sàng |
| 3 | **STORY-LOGIN-03** Đăng xuất | Khép vòng đời phiên; cần cho việc dọn trạng thái giữa các kịch bản kiểm thử | ✅ Sẵn sàng |
| 4 | **STORY-LOGIN-05** Xem hồ sơ | Chỉ đọc, phủ đầy đủ, không phụ thuộc gì | ✅ Sẵn sàng |
| 5 | **STORY-LOGIN-06** Sửa thông tin cá nhân | Phần validation đã đủ bằng chứng để viết test case | ⚠️ `REQ-LOGIN-44` chặn bởi `RISK-LOGIN-04` · `REQ-LOGIN-62` là giả định |
| 6 | **STORY-LOGIN-09** Menu và ngôn ngữ | Ảnh hưởng chiến lược định vị phần tử → nên chốt sớm trước khi viết nhiều kịch bản | ⚠️ `REQ-LOGIN-58` chặn bởi `RISK-LOGIN-05` |
| 7 | **STORY-LOGIN-04** Khôi phục mật khẩu | Giao diện đã đủ bằng chứng; luồng gửi email chạy theo giả định | ⚠️ `REQ-LOGIN-28` là giả định — cần quyền truy cập hộp thư mới kiểm chứng đầu-cuối |
| 8 | **STORY-LOGIN-07** Đổi mật khẩu | Validation tầng trình duyệt đã chắc; 2 REQ giả định về bảo mật cần tài khoản riêng | ⚠️ `REQ-LOGIN-51` · `60` · `61` chặn bởi `RISK-LOGIN-03`. ⛔ `REQ-LOGIN-61` **không phủ được giá trị biên** cho tới khi PO chốt ngưỡng độ dài |
| 9 | **STORY-LOGIN-08** Xác thực hai lớp | Rủi ro cao nhất khi thao tác, giá trị kiểm thử chỉ đến sau khi có tài khoản riêng | ⚠️ `REQ-LOGIN-54` là giả định, chặn bởi `RISK-LOGIN-03`. Luồng Google Authenticator **chưa có giả định nào** → chưa viết được TC |

---

## 6. Nhật ký Thay đổi

| Ngày | Nguồn | REQ ảnh hưởng | Loại | Tóm tắt thay đổi | TC cần xử lý |
|---|---|---|---|---|---|
| 20-09-2026 | DECISION-LOGIN-01 | `REQ-LOGIN-59` → `62` | 🟢 Thêm | Sinh 4 REQ mới từ giả định được chốt: `59` không khoá tài khoản *(AMB-02)* · `60` đổi mật khẩu khi bỏ trống ô xác nhận *(AMB-05)* · `61` máy chủ ràng buộc độ dài tối thiểu *(AMB-10)* · `62` máy chủ kiểm lại trường bắt buộc *(AMB-17)*. **Cả 4 đều ⚪, gắn nhãn `assumption-based`** | ➕ Viết mới, **đánh dấu `assumption-based`** |
| 20-09-2026 | DECISION-LOGIN-01 | `REQ-LOGIN-14` · `17` · `18` · `26` · `27` · `42` | 🟡 Sửa | Chốt 6 hành vi quan sát được là **cố ý / khiếm khuyết chấp nhận được** theo giả định `AMB-11` · `06` · `07` · `08` · `09` · `16`. Hệ quả: kịch bản kiểm thử khẳng định hiện trạng, **không mở bug** cho 6 điểm này | ⚠️ Review & sửa kỳ vọng |
| 20-09-2026 | DECISION-LOGIN-01 | `REQ-LOGIN-13` · `28` · `54` | 🟡 Sửa | Ba REQ ⚪ được bổ sung nội dung giả định *(AMB-03 · 14 · 15)* — trước chỉ ghi "chưa kiểm chứng", nay có kỳ vọng cụ thể để viết TC. Trạng thái vẫn ⚪ | ⚠️ Review & sửa kỳ vọng |
| 20-09-2026 | DECISION-LOGIN-01 | — *(không REQ nào)* | ✏️ Biên tập | **17/17 AMB chuyển `❓ Chờ trả lời` → `⏭️ Bỏ qua`** (áp giả định tạm, chấp nhận rủi ro). `AMB-LOGIN-01` đóng theo hướng **thu hẹp phạm vi**: đợt này chỉ vai trò quản trị — ma trận phân quyền **giữ nguyên** 22 ô `❔`. Thêm cảnh báo đầu mục 4 và mục Vai trò ngoài phạm vi | — |
| 20-09-2026 | UI recon | `REQ-LOGIN-01` → `58` | 🟢 Thêm | Khởi tạo tài liệu từ khảo sát UI thực tế trên nền tảng web. 58 REQ / 9 Story / 17 AMB / 5 RISK. Phạm vi thao tác do user chốt: thử sai chỉ bằng email không tồn tại · không lưu dữ liệu thật · không gửi email đặt lại mật khẩu · không đổi ngôn ngữ | — *(viết TC mới)* |

---

## 7. Bước tiếp theo

Tài liệu này (có REQ ID) là đầu vào chuẩn cho:

| Workflow | Dùng để |
|---|---|
| `/generate-testcases-manual-rbt` | Sinh manual test case theo quy trình AI-RBT 6 bước — nên chạy trước cho STORY-LOGIN-01 → 03 |
| `/generate-testcases-from-requirements` | Sinh nhanh test case (mode QUICK) |
| `/generate-traceability-matrix` | Dựng ma trận truy vết Requirements ↔ Test Cases ↔ Automation |

✅ **Cập nhật 20-09-2026:** mọi Story đã viết được test case — 17 AMB đã đóng theo giả định, không còn chờ PO.

⚠️ **Hai ràng buộc khi sinh test case:**

1. **7 REQ nhóm "dựa trên giả định"** (`13` · `28` · `54` · `59` · `60` · `61` · `62`) → gắn nhãn `assumption-based`. Khi PO trả lời thật, đây là danh sách phải rà lại đầu tiên.
2. **`REQ-LOGIN-61` chưa phủ được giá trị biên** — giả định nói *có* ràng buộc độ dài tối thiểu nhưng không nói *bao nhiêu*. Chỉ viết được case "mật khẩu 1 ký tự bị từ chối", chưa viết được case biên.

📄 **Impact Report của đợt cập nhật này:** [`impact/impact_DECISION-LOGIN-01.md`](impact/impact_DECISION-LOGIN-01.md)
