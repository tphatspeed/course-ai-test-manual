# Test Cases — Đăng nhập (`LOGIN`) (tổng 88 TC · 1 nền tảng)

> **INDEX module — tên file bất biến, KHÔNG chứa dòng TC.** TC chi tiết nằm ở các part theo [Bản đồ tài liệu](#bản-đồ-tài-liệu).
> ← Danh mục: [`../README.md`](../README.md) · Requirements: [`REQUIREMENTS_LOGIN_SUMMARY.md`](../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md)

| Thông tin | Nội dung |
|---|---|
| Nguồn requirement | [REQUIREMENTS_LOGIN_SUMMARY.md](../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) → [web/requirements_login_web.md](../../requirements/login/web/requirements_login_web.md) (đợt 4, 03-10-2026) |
| Dải TC ID đã dùng | `CRM_LOGIN_TC_001` → `CRM_LOGIN_TC_088` — chung mọi nền tảng. TC kế tiếp: `CRM_LOGIN_TC_089` |
| Mode · độ hạt | QUICK (`/generate-testcases-from-requirements`) · **GỘP** — 88 TC · **145 case kiểm** (biến thể Kiểu A + mục kiểm Kiểu B; TC không gộp tính 1) · tối đa 5 biến thể / TC |
| **Mức rủi ro · độ sâu** | `Cao` → **Đầy đủ** — V1 đủ nhánh, V2 mọi nhánh có điều kiện kích hoạt, V3 · V4 đủ nhánh áp dụng được.<br>Căn cứ chấm Cao: đụng **xác thực / phân quyền** · là **cổng vào** mọi module (hỏng là chặn cả đợt kiểm thử) · có thao tác không hồi lại được (gửi email đặt lại mật khẩu ra ngoài).<br>**Đã ở mức cao nhất — rà lại độ sâu khi:** thêm phương thức xác thực mới (2FA, SSO), có tài khoản Admin / contact, hoặc PO cam kết đa trình duyệt / ngưỡng hiệu năng |
| Phạm vi REQ | **72 / 72 REQ** có TC. Không REQ nào bị loại khỏi phạm vi viết TC. Ngoài phạm vi: cột **Admin** của Ma trận Phân quyền (`AMB-LOGIN-01` ✅ — PO chỉ cho test bằng `EMAIL_ADMIN`) · đổi mật khẩu khi đã đăng nhập, 2FA (thuộc `PROFILE`) · cổng khách hàng sau đăng nhập (thuộc `PORTAL`) · cấu hình bảo mật đăng nhập (thuộc `SETTING`) |
| Evidence | Đã mở **17 / 17** ảnh trong [`requirements/login/web/evidence/`](../../requirements/login/web/evidence/) — không phát hiện mâu thuẫn tài liệu ↔ ảnh |
| Ngày sinh | 04-10-2026 |

**Tổng hợp nhanh:** 4 TC `Critical` · 45 `High` · 32 `Medium` · 7 `Low` — 11 TC `@Smoke` · 8 TC `@KnownBug` (FAIL có chủ đích tới khi Dev sửa — `RISK-LOGIN-07`) · 25 TC `@TechCheck` · 19 TC `@NeedsVerify` · 1 TC `@PersonalOnly`.

---

## Bản đồ tài liệu

| Nền tảng | File | Nhóm chức năng | Số TC | Case kiểm | TC ID | REQ bao phủ |
|---|---|---|---|---|---|---|
| Web | [web/parts/part_01_web_smoke.md](web/parts/part_01_web_smoke.md) | **V1 Smoke** — giao diện 4 màn hình, lối vào, đăng nhập / đăng xuất thành công | 12 | 24 | 001–011 · 080 | REQ-LOGIN-01 → 04 · 15 · 16 · 17 · 19 · 21 · 26 · 27 · 32 · 40 · 49 · 50 · 51 |
| Web | [web/parts/part_02_web_quan_tri.md](web/parts/part_02_web_quan_tri.md) | **V2** — form Login quản trị · phiên · Remember me · đăng xuất · đoán lỗi | 28 | 42 | 012–038 · 085 | REQ-LOGIN-01 · 04 → 13 · 17 → 19 · 21 → 25 · 27 → 31 · 66 · 67 · 71 · 72 |
| Web | [web/parts/part_03_web_quen_mat_khau_cong.md](web/parts/part_03_web_quen_mat_khau_cong.md) | **V2** — Quên mật khẩu quản trị · Login cổng · Quên mật khẩu cổng | 23 | 39 | 039–060 · 084 | REQ-LOGIN-33 → 39 · 40 · 41 → 49 · 52 → 58 · 64 · 69 |
| Web | [web/parts/part_04_web_ky_thuat_phi_chuc_nang.md](web/parts/part_04_web_ky_thuat_phi_chuc_nang.md) | **V3** Permission · Security — **V4** Accessibility · Responsive · Compatibility | 25 | 40 | 061–079 · 081–083 · 086–088 | REQ-LOGIN-02 · 03 · 10 · 14 · 17 · 20 · 27 · 32 · 37 · 40 · 41 · 45 · 46 · 51 · 56 · 59 → 63 · 65 · 68 · 70 |
| Mobile · API | — | Module không có nền tảng này (requirements: Web ✅ · Android/iOS/API không có) | — | — | — | — |

> Tách 4 part vì file nền tảng Web có 88 TC > ngưỡng 50 TC của độ hạt GỘP. Cắt tại ranh giới vòng / nhóm chức năng. TC bổ sung ngày 10-10-2026 (`_080` → `_088`) nằm cuối part của đúng nhóm chức năng — số nối tiếp dải, không chèn giữa.

---

## Assumptions (QUICK mode — giả định đã áp dụng)

| Mã | Điểm chưa rõ | Giả định đã áp dụng | TC bị ảnh hưởng |
|---|---|---|---|
| ASM-01 | Khảo sát chỉ chạy thử email **thiếu `@`** ở các ô Email kiểu email (Login quản trị, Quên mật khẩu 2 khu) | Các dạng sai cấu trúc khác (thiếu tên miền, thiếu phần trước `@`, nhiều `@`, có dấu cách) cũng bị **trình duyệt** chặn như thiếu `@` — theo quy tắc chuẩn của ô email trong trình duyệt | `_015-b→e` · `_039-b,c` · `_055-b,c` |
| ASM-02 | Email / Password chỉ gồm dấu cách | Hệ thống coi như bỏ trống → hiện đúng thông báo bắt buộc nhập (trình duyệt cắt khoảng trắng ô Email; máy chủ cắt khoảng trắng khi kiểm bắt buộc) | `_012-b` · `_013-b` |
| ASM-03 | Chỉ chạy thử 1 dạng email sai cho thông báo định dạng của máy chủ (`@autotest` ở quản trị, thiếu `@` ở cổng) | Các dạng sai khác lọt qua trình duyệt (hai dấu chấm liền nhau ở phần trước `@`, thiếu tên miền ở ô chữ tự do, chuỗi script…) nhận **cùng** thông báo `The Email Address field must contain a valid email address.` | `_016-b` · `_047-b→e` · `_067` |
| ASM-04 | Bấm vào **chữ** `Remember me` có tích ô không | Có — nhãn gắn với ô tích (cả form quản trị lẫn cổng) | `_023` bước 4 · `_084` bước 4 |
| ASM-05 | Thứ tự `Tab` thật của form chưa đo | Theo thứ tự hiển thị từ trên xuống (form một cột); ô chọn `Language` mở bằng `Enter`, chọn bằng mũi tên | `_074` · `_075` · `_086` · `_087` · `_088` |
| ASM-06 | Ô Email / Password không khai giới hạn độ dài (Field Spec: không `maxlength`) | Không có ràng buộc độ dài ở form Login: mật khẩu 1 và 255 ký tự, email có phần trước `@` dài 64 ký tự đều tới máy chủ và nhận thông báo sai thông tin chung | `_019-b` · `_022-a,b` |
| ASM-07 | Mất mạng lúc bấm Login | Trình duyệt hiện trang báo không có kết nối; có mạng lại thì đăng nhập bình thường | `_038` |
| ASM-08 | Đăng xuất ở tab 1 thì tab 2 ra sao | Tab 2 thao tác tiếp bị đưa về Login — suy từ REQ-LOGIN-27 (phiên kết thúc) + REQ-LOGIN-01 | `_037` |
| ASM-09 | Bấm Login hai lần liên tiếp | Hệ thống xử lý bình thường, vào Dashboard, không hiện trang lỗi hết hạn / thao tác không hợp lệ | `_035` |
| ASM-10 | Trang đích sau khi contact đăng nhập cổng chưa quan sát | TC chỉ chấm "rời trang Login, không còn form, không có thông báo lỗi" | `_011` · `_060` bước 6 — lần chạy đầu ghi lại địa chỉ trang đích để đóng ASM này |
| ASM-11 | Câu chữ của trang 404 chưa quan sát | TC chỉ chấm "trang không tìm thấy, không phải trang lỗi máy chủ" | `_045` |
| ASM-12 | Nguyên văn thông báo khi contact đăng nhập khu quản trị chưa chốt (REQ-LOGIN-59) | TC không chấm câu chữ — chỉ chấm không vào được | `_062` |
| ASM-13 | Giao diện ở kích thước điện thoại / máy tính bảng chưa khảo sát (khảo sát chỉ `1600×750`) | Menu mobile có Logout và 4 trang xác thực hiển thị đủ như desktop, kể cả khi phóng to 125% (PO xác nhận cho Logout ở `AMB-LOGIN-17`; mở rộng cho trang Login / Quên mật khẩu). Danh sách kích thước **do agent chọn — chưa có người chốt** (review 10-10-2026) | `_077` · `_078` · `_081` · `_082` · `_083` |
| ASM-14 | Chưa có cam kết danh sách trình duyệt hỗ trợ | Chọn 3 trình duyệt phổ biến ngoài Chrome: Firefox · Safari · Edge bản mới nhất — **do agent chọn, chưa có người chốt** (review 10-10-2026) | `_079` |
| ASM-15 | Mã chuyển hướng thật là `307` (`AMB-LOGIN-25`) | Mọi TC chỉ chấm **địa chỉ cuối** trên thanh địa chỉ, không chấm mã chuyển hướng | `_005` · `_009` · `_010` · `_024` · `_033` · `_054` … |
| ASM-16 | Email chứa dấu nháy đơn (`auto'--@auto.test`) chưa gửi thử | Hợp lệ cấu trúc ở cả trình duyệt lẫn máy chủ → đi tới bước xác thực → thông báo sai thông tin chung (Login) / thông báo chung (Quên mật khẩu); ô giữ nguyên chuỗi | `_050-b` · `_064` · `_066` · `_068` |
| ASM-17 | Đích cuối khi bấm logo trang Login quản trị (`https://crm.anhtester.com/`) chưa quan sát | TC chỉ chấm rời khu quản trị, địa chỉ không chứa `/admin` | `_080` (trước 10-10-2026 nằm ở `_006` bước 4) |
| ASM-18 | Popup cảnh báo bộ đếm giờ khi Logout (REQ-LOGIN-28) lấy từ mã trang, chưa kích hoạt thật (`AMB-LOGIN-23`) | Popup hiện đúng nguyên văn trong mã, có nút `Logout`; đóng popup (×, `Esc`, bấm ra ngoài) thì không đăng xuất | `_031` · `_085` |
| ASM-19 | Ảnh evidence cổng chỉ có ca bỏ trống **cả hai** ô | Bỏ trống riêng từng ô hiện đúng thông báo dưới ô đó như ca bỏ trống cả hai | `_048-a` · `_049-a` |

---

## Kỹ thuật thiết kế đã áp dụng

### Decision Table — kết quả bấm `Login` ở form quản trị

| Rule | Email rỗng | Email qua kiểm tra trình duyệt | Email đúng định dạng máy chủ | Password rỗng | Email tồn tại | Mật khẩu đúng | → Kết quả | TC |
|---|---|---|---|---|---|---|---|---|
| R1 | Có | — | — | Không | — | — | `The Email Address field is required.` | `_012` |
| R2 | Không | Có | Có | Có | — | — | `The Password field is required.` | `_013` |
| R3 | Có | — | — | Có | — | — | Hai thông báo bắt buộc | `_014` |
| R4 | Không | **Không** | — | Không | — | — | Trình duyệt chặn, không gửi | `_015` |
| R5 | Không | Có | **Không** | Không | — | — | `The Email Address field must contain a valid email address.` | `_016` |
| R6 | Không | Có | Có | Không | **Không** | — | `Invalid email or password` | `_019` · `_064` |
| R7 | Không | Có | Có | Không | Có | **Không** | `Invalid email or password` (giống R6) | `_020` · `_073` |
| R8 | Không | Có | Có | Không | Có | Có | Vào Dashboard | `_008` · `_017` · `_018` |

### State Transition — trạng thái phiên khu quản trị

| Từ \ Sự kiện | Đăng nhập (không ghi nhớ) | Đăng nhập (có ghi nhớ) | Phiên mất / hết hạn | Logout | Mở trang quản trị | Back / dùng lại phiên cũ |
|---|---|---|---|---|---|---|
| **Chưa đăng nhập** | ✅ → Đã đăng nhập `_008` | ✅ → Đã đăng nhập + ghi nhớ `_026` | — | — | ❌ chặn, về Login `_005` · `_024` | — |
| **Đã đăng nhập** | ❌ không hiện lại form `_009` · `_036` | — | ✅ → Chưa đăng nhập `_025` · `_030` | ✅ → Đã đăng xuất `_010` | ✅ vào được `_009` | — |
| **Đã đăng nhập + ghi nhớ** | — | — | ✅ tự đăng nhập lại `_027` · `_028` | ✅ → Đã đăng xuất `_034` | ✅ | — |
| **Đã đăng xuất** | ✅ → Đã đăng nhập `_020` bước 6 · `_079` | — | — | — | ❌ chặn `_010` bước 4 · `_037` | ❌ Back `_033` · phiên cũ `_032` · ghi nhớ cũ `_034` |

---

## Bảng Đối Soát Coverage (toàn module)

> Một nền tảng (Web) → cột nền tảng gộp vào cột `TC IDs`. `Loại case`: **P** = Positive · **N** = Negative · **B** = Boundary. REQ phát biểu một chiều (VD "bỏ trống thì báo lỗi") chấm đủ khi chiều đó có TC; chiều đối ứng nằm ở REQ khác. Cột Ghi chú nêu TC gánh nhiều REQ mà có đúng 1 REQ chỉ dựa vào nó (phép thử 6b — được giữ).

| REQ ID | Trạng thái REQ | Mô tả ngắn | Số TC | TC IDs (Web) | Loại case | Đủ? | Ghi chú |
|---|---|---|---|---|---|---|---|
| REQ-LOGIN-01 | 🟢 | Chưa đăng nhập thì mọi trang quản trị chuyển về trang Login | 3 | `_005`, `_024`, `_037` | N | ✅ | — |
| REQ-LOGIN-02 | 🟢 | Trang Login quản trị hiển thị đủ thành phần | 3 | `_001`, `_074`, `_078` | P · B | ✅ | `@NeedsVerify` ở `_074`, `_078` |
| REQ-LOGIN-03 | 🟢 | Ô Email được đặt con trỏ sẵn khi mở trang | 2 | `_001`, `_074` | P | ✅ | `@NeedsVerify` ở `_074` |
| REQ-LOGIN-04 | 🟢 | Ô Password che ký tự đã nhập | 2 | `_001`, `_022` | P · B | ✅ | — |
| REQ-LOGIN-05 | 🟢 | Bỏ trống Email thì báo bắt buộc | 2 | `_012`, `_014` | N | ✅ | — |
| REQ-LOGIN-06 | 🟢 | Bỏ trống Password thì báo bắt buộc | 2 | `_013`, `_014` | N | ✅ | — |
| REQ-LOGIN-07 | 🟢 | Bỏ trống cả hai trường thì hiện đủ hai thông báo | 1 | `_014` | N | ✅ | 6b: REQ duy nhất chỉ dựa vào `_014` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-08 | 🟢 | Email thiếu "@" bị trình duyệt chặn, không gửi lên server | 1 | `_015` | N | ✅ | — |
| REQ-LOGIN-09 | 🟢 | Email có "@" nhưng tên miền không hợp lệ bị server từ chối | 1 | `_016` | N | ✅ | 6b: REQ duy nhất chỉ dựa vào `_016` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-10 | 🟡 | Thông tin đăng nhập sai thì báo lỗi chung | 6 | `_019`, `_020`, `_021`, `_022`, `_064`, `_065` | N · B | ✅ | B: `_019-b` (64 ký tự) · `_022-a,b` (1 / 255 ký tự) |
| REQ-LOGIN-11 | 🟢 | Thông báo sai thông tin chỉ hiện một lần | 1 | `_021` | N | ✅ | 6b: REQ duy nhất chỉ dựa vào `_021` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-12 | 🟢 | Máy chủ không trả lại giá trị Email sau khi đăng nhập lỗi | 3 | `_013`, `_016`, `_019` | N · B | ✅ | — |
| REQ-LOGIN-13 | 🟢 | Máy chủ không trả lại giá trị Password sau khi đăng nhập lỗi | 2 | `_012`, `_019` | N · B | ✅ | — |
| REQ-LOGIN-14 | 🟢 | Form đăng nhập gửi kèm CSRF token | 1 | `_063` | P | ✅ | — |
| REQ-LOGIN-15 | 🟢 | Link "Forgot Password?" mở trang quên mật khẩu quản trị | 1 | `_006` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_006` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-16 | 🟢 | Logo trang Login dẫn về trang chủ cổng khách hàng | 2 | `_001`, `_080` | P | ✅ | `@NeedsVerify` ở `_080` |
| REQ-LOGIN-17 | 🟢 | Đăng nhập đúng thì vào Dashboard | 5 | `_008`, `_009`, `_035`, `_038`, `_079` | P | ✅ | — |
| REQ-LOGIN-18 | 🟢 | Sau đăng nhập luôn vào Dashboard, không quay về trang đã định mở | 1 | `_024` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_024` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-19 | 🟢 | Đã đăng nhập mà mở trang Login thì chuyển về Dashboard | 2 | `_009`, `_036` | P | ✅ | — |
| REQ-LOGIN-20 | 🟢 | Cookie phiên được bảo vệ khỏi JavaScript và chỉ gửi qua HTTPS | 1 | `_069` | P | ✅ | — |
| REQ-LOGIN-21 | 🟢 | Checkbox Remember me mặc định không tick | 3 | `_001`, `_023`, `_026` | P | ✅ | — |
| REQ-LOGIN-22 | 🟢 | Không tick Remember me thì không tạo cookie ghi nhớ | 1 | `_025` | P | ✅ | — |
| REQ-LOGIN-23 | 🟡 | Tick Remember me thì tạo cookie ghi nhớ đúng hình thái | 3 | `_026`, `_027`, `_034` | P | ✅ | — |
| REQ-LOGIN-24 | 🟢 | Cookie ghi nhớ đăng nhập lại được khi phiên đã mất | 2 | `_027`, `_028` | P | ✅ | — |
| REQ-LOGIN-25 | 🟢 | Dùng cookie ghi nhớ thì hạn cookie được gia hạn | 1 | `_028` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_028` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-26 | 🟢 | Menu người dùng ở header có mục Logout dùng được | 1 | `_010` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_010` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-27 | 🟢 | Bấm Logout thì kết thúc phiên và về trang Login | 5 | `_010`, `_032`, `_033`, `_037`, `_079` | P | ✅ | — |
| REQ-LOGIN-28 | 🟢 | Đang có timer công việc chạy thì Logout hỏi xác nhận | 2 | `_031`, `_085` | P · N | ✅ | `@NeedsVerify` ở `_031`, `_085` |
| REQ-LOGIN-29 | 🟢 | Đăng xuất cấp session id mới | 1 | `_032` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_032` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-30 | 🟢 | Bấm Back sau khi đăng xuất không hiển thị lại trang quản trị | 1 | `_033` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_033` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-31 | 🟢 | Đăng xuất vô hiệu hoá cookie ghi nhớ ở phía máy chủ | 1 | `_034` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_034` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-32 | 🟢 | Trang Quên mật khẩu quản trị hiển thị đủ thành phần | 3 | `_002`, `_083`, `_086` | P | ✅ | `@NeedsVerify` ở `_083`, `_086` |
| REQ-LOGIN-33 | 🟢 | Email thiếu "@" bị trình duyệt chặn | 1 | `_039` | N | ✅ | — |
| REQ-LOGIN-34 | 🟡 | Bỏ trống Email ở Quên mật khẩu quản trị thì báo bắt buộc nhập | 1 | `_040` | N | ✅ | `@KnownBug` — hiện vi phạm, TC FAIL tới khi Dev sửa |
| REQ-LOGIN-35 | 🟢 | Email sai tên miền bị server từ chối | 1 | `_041` | N | ✅ | 6b: REQ duy nhất chỉ dựa vào `_041` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-36 | 🟡 | Quên mật khẩu quản trị hiện cùng một thông báo dù email có tồn tại hay không | 2 | `_042`, `_043` | N | ✅ | `@KnownBug` — hiện vi phạm, TC FAIL tới khi Dev sửa · `@NeedsVerify` ở `_043` |
| REQ-LOGIN-37 | 🟢 | Sau lỗi, ô Email giữ lại giá trị đã nhập | 3 | `_041`, `_066`, `_086` | N | ✅ | — |
| REQ-LOGIN-38 | ⚪ | Email tồn tại thì hệ thống gửi email đặt lại mật khẩu | 1 | `_043` | P | ✅ | REQ ⚪ chưa kiểm chứng · `@NeedsVerify` ở `_043` · 6b: REQ duy nhất chỉ dựa vào `_043` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-39 | ⚪ | Liên kết trong email đặt lại mật khẩu dùng được để đặt mật khẩu mới | 1 | `_044` | P | ✅ | REQ ⚪ chưa kiểm chứng · `@NeedsVerify` ở `_044` |
| REQ-LOGIN-40 | 🟢 | Trang Login cổng khách hàng hiển thị đủ thành phần | 5 | `_003`, `_007`, `_075`, `_081`, `_084` | P | ✅ | `@NeedsVerify` ở `_075`, `_081` |
| REQ-LOGIN-41 | 🟢 | Email sai định dạng bị server từ chối, hiển thị dưới ô Email | 2 | `_047`, `_067` | N | ✅ | — |
| REQ-LOGIN-42 | 🟢 | Bỏ trống Email thì báo bắt buộc ngay dưới ô | 1 | `_048` | N | ✅ | — |
| REQ-LOGIN-43 | 🟢 | Bỏ trống Password thì báo bắt buộc ngay dưới ô | 1 | `_049` | N | ✅ | — |
| REQ-LOGIN-44 | 🟢 | Thông tin sai thì hiện toast lỗi tự ẩn | 1 | `_050` | N | ✅ | 6b: REQ duy nhất chỉ dựa vào `_050` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-45 | 🟢 | Máy chủ không trả lại giá trị Email sau khi đăng nhập cổng lỗi | 3 | `_047`, `_050`, `_067` | N | ✅ | — |
| REQ-LOGIN-46 | 🟢 | Đổi Language thì trang Login hiển thị theo ngôn ngữ đã chọn | 4 | `_051`, `_052`, `_053`, `_088` | P | ✅ | `@NeedsVerify` ở `_088` |
| REQ-LOGIN-47 | 🟢 | Ngôn ngữ đã chọn được ghi nhớ khi tải lại | 1 | `_052` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_052` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-48 | 🟢 | Ngôn ngữ cổng khách hàng không đổi ngôn ngữ trang Login quản trị | 1 | `_053` | P | ✅ | 6b: REQ duy nhất chỉ dựa vào `_053` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-49 | 🟢 | Đăng ký tài khoản khách hàng đang tắt | 2 | `_003`, `_054` | P | ✅ | — |
| REQ-LOGIN-50 | ⚪ | Liên hệ đăng nhập cổng khách hàng thành công | 1 | `_011` | P | ✅ | REQ ⚪ chưa kiểm chứng · `@NeedsVerify` ở `_011` |
| REQ-LOGIN-51 | 🟢 | Trang Quên mật khẩu cổng hiển thị đủ thành phần | 4 | `_004`, `_007`, `_082`, `_087` | P | ✅ | `@NeedsVerify` ở `_082`, `_087` |
| REQ-LOGIN-52 | 🟢 | Email thiếu "@" bị trình duyệt chặn | 1 | `_055` | N | ✅ | — |
| REQ-LOGIN-53 | 🟡 | Bỏ trống Email ở Quên mật khẩu cổng thì báo bắt buộc nhập | 1 | `_056` | N | ✅ | `@KnownBug` — hiện vi phạm, TC FAIL tới khi Dev sửa |
| REQ-LOGIN-54 | 🟢 | Email sai tên miền bị server từ chối | 1 | `_057` | N | ✅ | 6b: REQ duy nhất chỉ dựa vào `_057` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-55 | 🟡 | Quên mật khẩu cổng hiện cùng một thông báo dù email có tồn tại hay không | 2 | `_058`, `_059` | N | ✅ | `@KnownBug` — hiện vi phạm, TC FAIL tới khi Dev sửa · `@NeedsVerify` ở `_059` |
| REQ-LOGIN-56 | 🟢 | Sau lỗi, ô Email giữ lại giá trị đã nhập | 3 | `_057`, `_068`, `_087` | N | ✅ | — |
| REQ-LOGIN-57 | ⚪ | Email contact tồn tại thì hệ thống gửi email đặt lại mật khẩu | 1 | `_059` | P | ✅ | REQ ⚪ chưa kiểm chứng · `@NeedsVerify` ở `_059` · 6b: REQ duy nhất chỉ dựa vào `_059` — REQ khác của TC đó có TC chống lưng → giữ |
| REQ-LOGIN-58 | ⚪ | Liên kết đặt lại mật khẩu cổng dùng được | 1 | `_060` | P | ✅ | REQ ⚪ chưa kiểm chứng · `@NeedsVerify` ở `_060` |
| REQ-LOGIN-59 | ⚪ | Tài khoản contact không đăng nhập được khu quản trị | 1 | `_062` | N | ✅ | REQ ⚪ chưa kiểm chứng · `@NeedsVerify` ở `_062` |
| REQ-LOGIN-60 | 🟢 | Tài khoản Staff không đăng nhập được cổng khách hàng | 1 | `_061` | N | ✅ | — |
| REQ-LOGIN-61 | 🟢 | Đăng nhập sai nhiều lần liên tiếp không làm hiện CAPTCHA, form vẫn xử lý bình thường | 1 | `_072` | N | ✅ | — |
| REQ-LOGIN-62 | 🟢 | Tài khoản không bị khoá sau nhiều lần nhập sai mật khẩu | 1 | `_073` | N | ✅ | — |
| REQ-LOGIN-63 | 🟢 | Cookie ghi nhớ đăng nhập không đọc được bằng JavaScript | 1 | `_071` | P | ✅ | `@KnownBug` — hiện vi phạm, TC FAIL tới khi Dev sửa |
| REQ-LOGIN-64 | 🟡 | Liên kết đặt lại mật khẩu quản trị không hợp lệ hiển thị trang 404 | 1 | `_045` | N | ✅ | `@KnownBug` — hiện vi phạm, TC FAIL tới khi Dev sửa |
| REQ-LOGIN-65 | 🟢 | Đăng nhập cấp session id mới (chống session fixation) | 1 | `_070` | P | ✅ | — |
| REQ-LOGIN-66 | 🟢 | Hạn phiên đăng nhập được gia hạn sau mỗi lần hoạt động | 1 | `_029` | P | ✅ | — |
| REQ-LOGIN-67 | 🟢 | Phiên đăng nhập hết hạn sau 8 giờ không hoạt động | 1 | `_030` | P | ✅ | — |
| REQ-LOGIN-68 | 🟢 | Ở viewport mobile, menu mobile có mục Logout dùng được | 1 | `_077` | P | ✅ | `@NeedsVerify` ở `_077` |
| REQ-LOGIN-69 | 🟡 | Tiêu đề tab trang Quên mật khẩu quản trị là "Forgot Password?" | 1 | `_046` | P | ✅ | `@KnownBug` — hiện vi phạm, TC FAIL tới khi Dev sửa |
| REQ-LOGIN-70 | 🟢 | Ô Email và Password của form Login quản trị khai báo thuộc tính `autocomplete` | 1 | `_076` | P | ✅ | `@KnownBug` — hiện vi phạm, TC FAIL tới khi Dev sửa |
| REQ-LOGIN-71 | 🟢 | Email đăng nhập quản trị không phân biệt chữ hoa chữ thường | 1 | `_017` | P | ✅ | — |
| REQ-LOGIN-72 | 🟢 | Khoảng trắng đầu/cuối của Email không cản trở đăng nhập quản trị | 1 | `_018` | P | ✅ | — |

**Tổng:** 72 / 72 REQ có ≥ 1 TC · 0 dòng 🔴 · 0 REQ ngoài phạm vi viết TC · 6 REQ ⚪ (38 · 39 · 50 · 57 · 58 · 59) có TC nhưng chờ điều kiện (`AMB-SYS-03`, `RISK-LOGIN-03`).

**Phép thử chiều ngược 6b:** chạy trên cả 4 part (chạy lại 10-10-2026 sau khi tách `_006` / thêm `_080`–`_088`) — **không vi phạm** (không TC nào gánh ≥ 2 REQ mà các REQ đó không có TC khác chống lưng).

---

## Bảng Đối Soát Evidence

| Ảnh evidence | Màn hình / trạng thái | TC dựa vào | Đầy đủ? |
|---|---|---|---|
| `admin_login_default_fullpage.png` | Login quản trị — mặc định, ô Email đang focus | `_001` · `_023` · `_078` | ✅ full-page |
| `admin_login_submit_empty_fullpage.png` | Login quản trị — bỏ trống cả hai | `_014` | ✅ full-page |
| `admin_login_invalid_email_server_fullpage.png` | Login quản trị — email sai tên miền | `_016` | ✅ full-page |
| `admin_login_invalid_credentials_fullpage.png` | Login quản trị — `Invalid email or password`, ô Email trống | `_019` · `_020` · `_021` | ✅ full-page |
| `header_user_menu_open_element.png` | Menu ảnh đại diện đang mở | `_010` | ✅ element (chỉ dropdown) |
| `admin_forgot_password_default_fullpage.png` | Quên mật khẩu quản trị — mặc định, không có lối về Login | `_002` | ✅ full-page |
| `admin_forgot_password_submit_empty_fullpage.png` | Quên mật khẩu quản trị — bỏ trống → `Email not found` | `_040` | ⚠️ bằng chứng **lỗi hiện tại**, không phải hành vi đúng |
| `admin_forgot_password_invalid_email_fullpage.png` | Quên mật khẩu quản trị — sai tên miền, ô giữ giá trị | `_041` | ✅ full-page |
| `admin_forgot_password_email_not_found_fullpage.png` | Quên mật khẩu quản trị — email không tồn tại → `Email not found`, ô giữ giá trị | `_042` · `_066` | ⚠️ bằng chứng **lỗi hiện tại** cho `_042`; ✅ cho việc ô giữ giá trị |
| `portal_login_default_fullpage.png` | Login cổng — mặc định (English) | `_003` · `_007` · `_075` | ✅ full-page |
| `portal_login_submit_empty_fullpage.png` | Login cổng — bỏ trống cả hai, lỗi dưới từng ô | `_048-b` · `_049-b` | ✅ full-page |
| `portal_login_invalid_email_fullpage.png` | Login cổng — thiếu `@`, lỗi dưới ô Email | `_047-a` · `_067` | ✅ full-page |
| `portal_login_invalid_credentials_toast_viewport.png` | Login cổng — thông báo nổi `Invalid username or password` | `_050` · `_061` | ✅ viewport (thông báo ở đầu trang) |
| `portal_login_language_vietnamese_fullpage.png` | Login cổng — Language = Vietnamese | `_051` · `_052` · `_053` | ✅ full-page |
| `portal_forgot_password_default_fullpage.png` | Quên mật khẩu cổng — mặc định, nút `Submit` | `_004` · `_007` | ✅ full-page |
| `portal_forgot_password_submit_empty_fullpage.png` | Quên mật khẩu cổng — bỏ trống → `Email not found` | `_056` | ⚠️ bằng chứng **lỗi hiện tại** |
| `portal_forgot_password_email_not_found_fullpage.png` | Quên mật khẩu cổng — email không tồn tại → `Email not found`, ô giữ giá trị | `_057` · `_058` · `_068` | ⚠️ lỗi hiện tại cho `_058`; ✅ cho việc ô giữ giá trị |
| (không có) | Popup cảnh báo bộ đếm giờ khi Logout · cách đóng popup | `_031` · `_085` | 🔴 THIẾU — `@NeedsVerify` |
| (không có) | Menu mobile + 4 trang xác thực ở kích thước điện thoại / máy tính bảng / phóng to 125% | `_077` · `_078` · `_081` · `_082` · `_083` | 🔴 THIẾU — `@NeedsVerify` |
| (không có) | Email đặt lại mật khẩu + trang đặt mật khẩu mới (2 khu) | `_043` · `_044` · `_059` · `_060` | 🔴 THIẾU — `@NeedsVerify` (chờ hộp thư test) |
| (không có) | Contact đăng nhập (cổng thành công · khu quản trị bị từ chối) | `_011` · `_062` | 🔴 THIẾU — `@NeedsVerify` (chờ `AMB-SYS-03`) |
| (không có) | Thứ tự `Tab` của 4 form · chọn Language bằng bàn phím | `_074` · `_075` · `_086` · `_087` · `_088` | 🔴 THIẾU — `@NeedsVerify` |
| (không có) | Trang đích khi bấm logo Login quản trị | `_080` | 🔴 THIẾU — `@NeedsVerify` |
| (không có ảnh — có số liệu DOM / network trong AC) | Chuyển hướng, cookie, tiêu đề tab, CSRF, đăng ký tắt, trang 404 (hiện `500`) | `_005` · `_008` → `_009` · `_024` → `_030` · `_032` → `_034` · `_045` · `_046` · `_054` · `_063` · `_069` → `_071` | ✅ chống lưng bằng số liệu DOM / network ghi trong AC (không chụp Dashboard / cookie vì chứa dữ liệu nghiệp vụ / bí mật) |

### Vùng chưa có evidence — đề xuất recon bổ sung

| Vùng | TC `@NeedsVerify` | Đề xuất (chuẩn 7.2.1 của `skills-requirements-analyzer`) |
|---|---|---|
| Popup cảnh báo bộ đếm giờ khi Logout | `_031` · `_085` | Tạo task riêng `auto_login_timer_<ts>`, bật bộ đếm giờ, bấm Logout, chụp **element** popup; ghi lại popup có nút đóng (×) không, `Esc` / bấm ra ngoài có đóng không; dọn task sau khi chụp |
| Menu mobile · 4 trang xác thực ở `375×812` / `768×1024` / phóng to 125% | `_077` · `_078` · `_081` · `_082` · `_083` | Recon ở chế độ thiết bị, đo breakpoint làm menu mobile hiện ra, ghi cách hiển thị đầu trang cổng ở kích thước nhỏ, chụp viewport |
| Thứ tự `Tab` thật · ô chọn Language bằng bàn phím | `_074` · `_075` · `_086` · `_087` · `_088` | Đọc thứ tự focus bằng Playwright MCP (nhấn Tab, ghi phần tử đang focus) ở 4 form, thử `Enter` + mũi tên trên ô Language; ghi vào Field Spec |
| Trang đích của logo `https://crm.anhtester.com/` khi chưa đăng nhập | `_080` | Mở thử, ghi địa chỉ cuối |
| Contact đăng nhập 2 khu | `_011` · `_062` | Chờ tài khoản contact (`AMB-SYS-03`) |
| Email đặt lại + trang đặt mật khẩu mới | `_043` · `_044` · `_059` · `_060` | Chờ hộp thư test đọc được qua API + tài khoản Staff riêng (`RISK-LOGIN-03`). Khi recon: ghi nhãn các ô, nút và nguyên văn thông báo của trang đặt lại → viết lại bước 3–4 của `_044`, `_060` |

---

## Đối soát Field-Level Validation (bảng 15 loại)

| Field (form) | Loại | Mục trong bảng 15 loại → TC |
|---|---|---|
| Email Address (Login quản trị · ô email) | Email | Format hợp lệ ✅ `_008` · Thiếu `@` ✅ `_015-a` · Thiếu domain ✅ `_015-b` · Domain không hợp lệ ✅ `_016` · Nhiều `@` ✅ `_015-d` · Ký tự đặc biệt trước `@` ✅ `_064` · Max length ✅ `_019-b` (thăm dò — không khai giới hạn) · Case sensitivity ✅ `_017` · Email đã tồn tại ➖ không phải form tạo mới. + Required ✅ `_012` · khoảng trắng ✅ `_012-b` · `_018` — **đủ 9/9** |
| Password (Login quản trị) | Password | Min/Max length ✅ `_022-a,b` (không khai giới hạn) · Ký tự đặc biệt ✅ `_022-c` · Yêu cầu hoa/thường ➖ form đăng nhập không áp chính sách độ mạnh (thuộc đổi mật khẩu — `PROFILE`) · Yêu cầu số ➖ cùng lý do · Copy-paste bị chặn? ✅ `_022-e` (dán được) · Hiện/ẩn ✅ `_001` (không có nút hiện) · Confirm password ➖ form không có ô xác nhận. + Required ✅ `_013` · che ký tự ✅ `_022` · chuỗi tiêm ✅ `_065` — **đủ 7/7** |
| Remember me (Login quản trị) | Checkbox | Mặc định ✅ `_001` · Check/Uncheck ✅ `_023` · Required ➖ không bắt buộc · Nhóm radio ➖ không phải radio — **đủ 4/4** |
| Email Address (Quên mật khẩu quản trị · ô email) | Email | Format hợp lệ ✅ `_042` · `_043` · Thiếu `@` ✅ `_039-a` · Thiếu domain ✅ `_039-b` · Domain không hợp lệ ✅ `_041` · Nhiều `@` ⏭️ (đã phủ ở `_015-d`, cùng cơ chế trình duyệt — **agent đề xuất, chờ QA lead duyệt 04-10-2026**; rà lại nếu `_039` FAIL) · Ký tự đặc biệt trước `@` ✅ `_066` · Max length ⏭️ (cùng đề xuất; rà lại khi Field Spec khai giới hạn) · Case sensitivity ⏸️ chặn bởi `RISK-LOGIN-03` (cần email tồn tại) · Email tồn tại ✅ `_043` — + Required ✅ `_040` |
| Language (Login cổng) | Dropdown | Mặc định ✅ `_003` · Tất cả option hợp lệ ⏭️ chỉ kiểm `English` + `Vietnamese` (có evidence); 24 ngôn ngữ còn lại chỉ đếm đủ 26 ở `_003` — **agent đề xuất, chờ QA lead duyệt 04-10-2026**; rà lại khi có yêu cầu bản dịch cụ thể · Option disabled ➖ không có · Thay đổi selection ✅ `_051` · `_052` · Required ➖ không có option rỗng |
| Email Address (Login cổng · ô chữ tự do) | Email | Format hợp lệ ⏸️ `_011` (chờ contact) · Thiếu `@` ✅ `_047-a` · Thiếu domain ✅ `_047-b` · Domain không hợp lệ ✅ `_047-d` · Nhiều `@` ✅ `_047-e` · Ký tự đặc biệt / XSS / SQLi ✅ `_050-b` · `_067` · Max length ⏭️ (như trên) · Case sensitivity ⏸️ chờ contact · Email tồn tại ✅ `_061` (Staff bị từ chối). + Required ✅ `_048` |
| Password (Login cổng) | Password | Required ✅ `_049` · Min/Max · ký tự đặc biệt · dán ⏭️ — ô không có ràng buộc, đã phủ đủ 7 mục ở form quản trị `_022` — **agent đề xuất, chờ QA lead duyệt 04-10-2026**; rà lại khi có tài khoản contact (`_011`) · Hiện/ẩn ✅ `_003` (che ký tự theo REQ-LOGIN-40) · hoa/thường · số · confirm ➖ như form quản trị |
| Remember me (Login cổng) | Checkbox | Mặc định ✅ `_003` · Check/Uncheck ✅ `_084` · Tác dụng ⏸️ chờ contact (REQ-LOGIN-40 chỉ khai hiển thị) |
| Email Address (Quên mật khẩu cổng · ô email) | Email | Như form quản trị: Thiếu `@` ✅ `_055-a` · Thiếu domain ✅ `_055-b` · Domain không hợp lệ ✅ `_057` · Ký tự đặc biệt ✅ `_068` · Email tồn tại ✅ `_059` · Nhiều `@` · Max length ⏭️ (như trên) · + Required ✅ `_056` |

---

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | TC ID / Lý do |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | `_001`–`_004` (4 TC · 15 mục kiểm) — nhãn nguyên văn, thứ tự, mặc định, con trỏ |
| 1 | Open form | ✅ | `_005`–`_007` · `_080` (4 TC · 5 case) — URL trực tiếp, deep link, liên kết, logo, nút đầu trang |
| 1 | Display | ➖ | Module không hiển thị dữ liệu nghiệp vụ (tiền, ngày, danh sách). Định dạng thông báo lỗi / thông báo nổi kiểm ở V2 (`_014` · `_050`) |
| 1 | Input valid data · Save | ✅ | `_008` · `_011` (2 TC · 2 case) — `_011` `@NeedsVerify` chờ contact |
| 1 | Verify data | ✅ | `_009` · `_010` (2 TC · 2 case) — phiên được giữ · đăng xuất kết thúc phiên |
| 2 | UI Behavior | ✅ | `_023` · `_051` · `_084` · `_085` (4 TC · 10 case) — Remember me 2 form, đổi ngôn ngữ, đóng popup cảnh báo bộ đếm giờ |
| 2 | Required | ✅ | `_012`–`_014` · `_040` · `_048` · `_049` · `_056` (7 TC · 11 case) |
| 2 | Validation | ✅ | `_015`–`_019` · `_022` · `_039` · `_041` · `_047` · `_055` · `_057` (11 TC · 29 case) — đối soát bảng 15 loại ở mục trên: Email 9/9 · Password 7/7 · Checkbox 4/4 · Dropdown 5/5 |
| 2 | Equivalence Partitioning | ✅ | `_008` · `_015` · `_016` · `_019` · `_020` (5 TC · 11 case) — 5 lớp email: chặn ở trình duyệt / chặn ở máy chủ / không tồn tại / tồn tại sai MK / đúng |
| 2 | Boundary Value Analysis | ➖ | Không field nào khai `min` / `max` (Field Spec mục 3). Vẫn thăm dò độ dài ở `_019-b` · `_022-a,b` |
| 2 | Business Rule | ✅ | `_017` · `_018` · `_020` · `_021` · `_024` · `_025` · `_029` · `_030` · `_031` · `_042` · `_043` · `_054` · `_058` · `_059` (14 TC · 15 case) |
| 2 | Decision Table | ✅ | 8 rule → `_008` · `_012`–`_016` · `_019` · `_020` (8 TC · 16 case) — bảng ở mục *Kỹ thuật thiết kế* |
| 2 | State Transition | ✅ | 4 trạng thái phiên, transition hợp lệ + bị chặn → `_005` · `_008` · `_009` · `_010` · `_025` · `_027` · `_030` · `_032`–`_034` (10 TC · 11 case) |
| 2 | Dependency | ✅ | `_051`–`_053` (3 TC · 7 case) — Language → nhãn trang, ghi nhớ, không lan sang khu quản trị |
| 2 | Use Case / Scenario | ✅ | `_024` (deep link → Login → Dashboard) · `_043` → `_044` · `_059` → `_060` (quên → nhận mail → đặt lại → đăng nhập) (5 TC · 5 case) |
| 2 | Save / Edit / Delete | ➖ | Module không có CRUD — không tạo / sửa / xoá bản ghi nghiệp vụ |
| 2 | Error Guessing | ✅ | `_035`–`_038` (4 TC · 4 case) — bấm 2 lần, Back sau đăng nhập, 2 tab, mất mạng |
| 3 | Permission | ✅ | `_061` · `_062` (2 TC · 2 case) + `_005` — ranh giới 2 khu xác thực × loại tài khoản. Cột Admin ngoài phạm vi (`AMB-LOGIN-01`) |
| 3 | Security | ✅ | `_063`–`_073` (11 TC · 18 case) + `_020` · `_032`–`_034` · `_054` — CSRF, chuỗi tiêm, cookie, session, chống dò mật khẩu / email |
| 3 | API | ➖ | QA không có quyền gọi API — đội Dev xác minh (README: *Năng lực kiểm thử của QA*, chốt 03-10-2026) |
| 3 | Database | ➖ | QA không có quyền truy vấn CSDL — đội Dev xác minh |
| 3 | Integration | ➖ | Gửi email đặt lại mật khẩu (SMTP) — QA không có quyền kiểm tầng tích hợp / hộp thư — đội Dev xác minh. Phần thấy được từ người dùng nằm ở `_043` · `_059` |
| 3 | Logging / Audit | ➖ | Requirements không có yêu cầu nhật ký đăng nhập; tài khoản QA bị chặn trang nhật ký hoạt động (Access denied) — chờ tài khoản Admin (`AMB-SYS-01`) |
| 4 | Compatibility | ✅ | `_079` (1 TC · 3 trình duyệt) — **không** kiểm trình duyệt trên điện thoại thật |
| 4 | Responsive / UI Stability | ✅ | `_077` · `_078` · `_081`–`_083` (5 TC · 11 case) — 4 trang xác thực ở `375×812` · `768×1024` · `1366×768` · phóng to 125%. Danh sách kích thước chưa có người chốt (`ASM-13`) |
| 4 | Accessibility | ✅ | `_074`–`_076` · `_086`–`_088` (6 TC · 6 case) + `_001` 🔧 (văn bản thay thế của logo) — bàn phím ở 4 form + ô Language, viền focus, gợi ý tự điền. WCAG đầy đủ / trình đọc màn hình ➖ cần công cụ riêng |
| 4 | Performance | ➖ | Không có ngưỡng thời gian cam kết, không có công cụ tải — đội Hạ tầng |
| 4 | Regression | ➖ | `docs/bugs/login/` chưa có bug nào được đóng |
| 4 | E2E | ➖ | Luồng đăng nhập → Dashboard dừng ở cửa vào `DASH` (chưa có tài liệu). Luồng xuyên module chuyển sang `/generate-cross-module-test-plan` khi `NAV` / `DASH` có requirements |

---

## Rà soát đặc tính chất lượng (ISO/IEC 25010:2023)

| Đặc tính | Trạng thái | TC ID / Lý do |
|---|---|---|
| Functional Suitability | ✅ Có TC | `_001`–`_062` — đủ 72 REQ, Positive / Negative / Boundary |
| Performance Efficiency | ➖ Ngoài phạm vi | Không có ngưỡng cam kết, không có công cụ tải — **đội Hạ tầng**, đợt sau |
| Compatibility | ✅ Có TC | `_079` (Firefox · Safari · Edge). Không kiểm trình duyệt điện thoại thật — **QA lead** quyết khi có cam kết hỗ trợ |
| Interaction Capability | ✅ Có TC | `_012`–`_016` · `_040` · `_048` · `_049` (thông báo lỗi rõ nghĩa) · `_074`–`_076` · `_086`–`_088` (bàn phím, tự điền) · `_050` (thông báo tự ẩn) |
| Reliability | ✅ Có TC | `_029` · `_030` (hạn phiên) · `_035` (bấm 2 lần) · `_038` (mất mạng) |
| Security | ✅ Có TC | `_005` · `_020` · `_032`–`_034` · `_042` · `_054` · `_058` · `_061`–`_073`. Pentest / quét lỗ hổng ➖ — **đội Security / Dev** |
| Maintainability | ➖ Không áp dụng | Đặc tính của mã nguồn — code review / static analysis của **đội Dev** |
| Flexibility | ✅ Có TC | `_051`–`_053` (đổi ngôn ngữ) · `_077` · `_078` · `_081`–`_083` (kích thước màn hình, phóng to) |
| Safety | ➖ Không áp dụng | App nghiệp vụ, lỗi đăng nhập không gây thiệt hại vật lý — **PO** xác nhận khi lập Master Test Plan |

---

## Đối soát cột Automation

| Nền tảng | Yes | Partial | No | ⏸️ Hoãn |
|---|---|---|---|---|
| Web | 78 | 10 | 0 | 19 |

> `⏸️ Hoãn` đếm chồng lên `Yes` / `Partial` — là trạng thái tạm, cột `Automation` giữ giá trị thật. 8 TC `@KnownBug` **vẫn automate** (test đỏ chính là thứ phơi bug), execution report tách nhóm "FAIL — lỗi đã biết".

### Điều kiện cần chuẩn bị

| # | Điều kiện | Ai cấp | Trạng thái | TC phụ thuộc |
|---|---|---|---|---|
| 1 | `EMAIL_ADMIN` / `PASSWORD_ADMIN` trong `.env` trên máy chạy script | QA | ✅ Đã có | Mọi TC đăng nhập thành công (`_008`, `_009`, `_017`, `_018`, `_020`, `_024`–`_038`, `_061`, `_069`–`_071`, `_073`, `_077`, `_079`, `_085`) |
| 2 | Tài khoản contact (`EMAIL_CONTACT` / `PASSWORD_CONTACT`) | User / PO (`AMB-SYS-03`) | ⏳ Chưa có | `_011` · `_059` · `_060` · `_062` |
| 3 | Hộp thư test đọc được qua API (Mailpit / MailHog…) + tài khoản Staff riêng gắn hộp thư | DevOps / PO (`RISK-LOGIN-03`) | ⏳ Chưa có | `_043` · `_044` · `_059` · `_060` |
| 4 | Hạn phiên ngắn trên môi trường test (hoặc lịch chạy đêm ≥ 8 giờ) | Dev / quản trị môi trường | ⏳ Chưa có | `_030` |
| 5 | Khung giờ chạy riêng, không song song, cho TC nhập sai mật khẩu trên tài khoản dùng chung | QA lead | ⏳ Chưa chốt | `_073` |
| 6 | Ảnh gốc visual regression đã duyệt cho bố cục ở các kích thước / mức phóng to | QA lead | ⏳ Chưa có | `_078` · `_081` · `_082` · `_083` |

### TC Partial · No · Hoãn

| TC ID | Nền tảng | Automation | Trục chặn | Điều kiện · phần kiểm tay · lý do |
|---|---|---|---|---|
| CRM_LOGIN_TC_011 | Web | Yes · ⏸️ Hoãn | — | Chờ điều kiện #2 |
| CRM_LOGIN_TC_030 | Web | Partial | 1 · Phụ thuộc thời gian thật phía máy chủ | Điều kiện #4 — `page.clock` không tác động hạn phiên máy chủ; script phải khôi phục cookie `sp_session` đã lưu trước khi nạp lại (bước 4). Hiện gắn `@PersonalOnly`, QA tự chạy |
| CRM_LOGIN_TC_031 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ recon popup (`ASM-18`) |
| CRM_LOGIN_TC_043 | Web | Partial · ⏸️ Hoãn | 1 · Email | Điều kiện #3 |
| CRM_LOGIN_TC_044 | Web | Partial · ⏸️ Hoãn | 1 · Email | Điều kiện #3 |
| CRM_LOGIN_TC_059 | Web | Partial · ⏸️ Hoãn | 1 · Email | Điều kiện #2 + #3 |
| CRM_LOGIN_TC_060 | Web | Partial · ⏸️ Hoãn | 1 · Email | Điều kiện #2 + #3 |
| CRM_LOGIN_TC_062 | Web | Yes · ⏸️ Hoãn | — | Chờ điều kiện #2 |
| CRM_LOGIN_TC_073 | Web | Partial | 2 · Môi trường dùng chung | Điều kiện #5 — có thể khoá tài khoản cả lớp nếu PO xác nhận sai (`RISK-LOGIN-01`) |
| CRM_LOGIN_TC_074 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ đo thứ tự Tab (`ASM-05`) |
| CRM_LOGIN_TC_075 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ đo thứ tự Tab (`ASM-05`) |
| CRM_LOGIN_TC_077 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ recon menu mobile (`ASM-13`) |
| CRM_LOGIN_TC_078 | Web | Partial · ⏸️ Hoãn | 2 · "chữ không đè, không cắt" cần mắt người | Script chấm đủ thành phần + không cuộn ngang; bố cục kiểm tay hoặc điều kiện #6 |
| CRM_LOGIN_TC_080 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ recon trang đích của logo (`ASM-17`) |
| CRM_LOGIN_TC_081 | Web | Partial · ⏸️ Hoãn | 2 · "chữ không đè, không cắt" cần mắt người | Như `_078` — điều kiện #6 |
| CRM_LOGIN_TC_082 | Web | Partial · ⏸️ Hoãn | 2 · "chữ không đè, không cắt" cần mắt người | Như `_078` — điều kiện #6 |
| CRM_LOGIN_TC_083 | Web | Partial · ⏸️ Hoãn | 2 · "chữ không đè, không cắt" cần mắt người | Như `_078` — điều kiện #6 |
| CRM_LOGIN_TC_085 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ recon cách đóng popup (`ASM-18`) |
| CRM_LOGIN_TC_086 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ đo thứ tự Tab (`ASM-05`) |
| CRM_LOGIN_TC_087 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ đo thứ tự Tab (`ASM-05`) |
| CRM_LOGIN_TC_088 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ thử ô chọn Language bằng bàn phím (`ASM-05`) |

**Thứ tự ưu tiên automate:** (1) `@Smoke` `_001`–`_010` → (2) TC nhiều biến thể ở Validation: `_015` · `_019` · `_022` · `_047` · `_064`–`_068` → (3) Security / phiên: `_005` · `_024`–`_034` · `_069`–`_072` → (4) `@KnownBug` (8 TC) để theo dõi bug → (5) phần còn lại.

---

## Nhật ký thay đổi

| Ngày | Nguồn | TC ảnh hưởng | Vòng · nhánh | Thay đổi | Mốc git |
|---|---|---|---|---|---|
| 04-10-2026 | `/generate-testcases-from-requirements` (QUICK · GỘP) | `CRM_LOGIN_TC_001` → `079` | V1 → V4 — mọi nhánh | Sinh mới bộ TC từ requirements đợt 4 (72 REQ). 4 part · 79 TC · 130 case kiểm. 19 ASM · 8 `@KnownBug` · 12 `@NeedsVerify` | Chưa có — `docs/` chưa được commit (HEAD `ae09f57`) |
| 10-10-2026 | `/review-testcases` Mode FIX — báo cáo [`review/testcase_review_report_web_20261010.md`](review/testcase_review_report_web_20261010.md) | Sửa 22 TC: `_001` · `_006` · `_009` · `_010` · `_011` · `_016` · `_020` · `_025`–`_032` · `_034` · `_038` · `_043` · `_044` · `_059` · `_060` · `_078` — Thêm 9 TC: `_080`–`_088` | V1 Open form · V2 UI Behavior · V4 Responsive · Accessibility | **Sửa:** Pre-Condition tự mô tả trạng thái, bỏ phụ thuộc TC khác (`_009`, `_010`, `_027`, `_028`, `_044`, `_060`) · bước so sánh tự tạo mốc trong chính TC (`_020`, `_043`, `_059`) · thao tác cookie / DevTools chuyển xuống `🔧` (`_025`–`_029`, `_032`, `_034`) · `_030` thêm bước khôi phục cookie phiên để chứng minh máy chủ huỷ phiên · `_031` tách bước · `_006` bỏ bước logo (chuyển sang `_080`, bỏ `@NeedsVerify` khỏi TC smoke) · `_011` Expected loại trừ trang lỗi · `_016-b` đổi sang lớp hai dấu chấm liền nhau · `_044`, `_060` thêm kiểm mật khẩu cũ bị từ chối + bước dọn `.env` · `_001` thêm `🔧` văn bản thay thế của logo · `_038` thêm cách ngắt mạng không cần DevTools · `_078` thêm biến thể `d` phóng to 125%. **Thêm:** `_080` logo · `_081`–`_083` responsive 3 trang còn lại · `_084` Remember me cổng · `_085` đóng popup bộ đếm giờ · `_086`–`_088` bàn phím. 88 TC · 145 case. **Chưa làm** (chờ quyết định): gap #3, #4, #8 của báo cáo (cần chốt Expected qua `/update-requirements-from-ticket` / chạy thử) · 4 ô `⏭️` thiếu người quyết ở *Đối soát Field-Level* (chờ QA lead) · `_069`–`_071`, `_076` giữ nguyên (chấp nhận trần 11/12) | `d0d0a6d` |
