# Module 13 — Mục tiêu · Khảo sát · Thông báo

← Quay lại [system_map.md](../system_map.md)

| Module | Prefix | Nền tảng | Bí danh |
|---|---|---|---|
| Mục tiêu | `GOAL` | Web | Goals |
| Khảo sát | `SURVEY` | Web | Surveys |
| Thông báo | `ANN` | Web | Announcements |

> Ba module nhỏ nằm ngoài chuỗi nghiệp vụ chính (tiền · dự án · khách hàng). Chung file cho gọn, **prefix vẫn tách riêng**.

---

## `GOAL` — Mục tiêu

| Route | Phản hồi |
|---|---|
| `/admin/goals` | 🚫 → `/admin/access_denied` |

Theo dõi mục tiêu doanh số / hiệu suất của nhân viên. **Chưa nhìn thấy màn hình nào.**

**Risk 🟡 Trung bình** — gắn với số liệu doanh thu và đánh giá nhân viên, nhưng không tạo ra chứng từ tiền.

---

## `SURVEY` — Khảo sát

| Route | Phản hồi |
|---|---|
| `/admin/surveys` | 🚫 → `/admin/access_denied` |

Tạo khảo sát và gửi cho khách hàng / nhân viên, thu phản hồi. **Chưa nhìn thấy màn hình nào.**

**Risk 🟡 Trung bình** — có gửi thư ra ngoài và thu dữ liệu từ người trả lời.

---

## `ANN` — Thông báo

| Route | Phản hồi |
|---|---|
| `/admin/announcements` | ✅ **mở được** |

### Route ẩn — không có trong menu điều hướng

Không xuất hiện ở thanh bên, cũng không ở thanh đầu trang. Chỉ vào được bằng URL trực tiếp. Tôi phát hiện khi dò ranh giới khu Setup.

| Thuộc tính | Ghi nhận |
|---|---|
| Cột | Name · Date — chỉ **2 cột** |
| Thanh công cụ | **Chỉ `Export`** — không có nút tạo mới |
| Số bản ghi | `No entries found` — 0 |

> Màn hình **chỉ đọc**: xem và xuất được, nhưng không tạo được. Nhiều khả năng nút tạo nằm ở khu Setup (🚫). Cần xác minh khi có account đủ quyền — ứng viên `AMB-ANN-01`.

**Risk 🟢 Thấp** — nội dung thông báo nội bộ, 2 cột, 0 bản ghi.

---

## Vùng chưa xác minh

| Việc | Lý do |
|---|---|
| Toàn bộ `GOAL` · `SURVEY` | 🚫 `access_denied` |
| Cách tạo thông báo | Không có nút tạo ở màn hình truy cập được |
| Thông báo hiển thị ở đâu cho người nhận | 0 bản ghi, không quan sát được |
| `ANN` có đúng là một module riêng không | Nếu khu Setup mở ra cho thấy nó chỉ là một trang con của `SETTING`, cân nhắc gộp. **An toàn** — tầng khám phá không mang mã REQ nên gộp lại sau không phá traceability |

## Evidence

Ba module này **không có ảnh riêng**, lý do ghi rõ:

| Module | Vì sao không có ảnh |
|---|---|
| `GOAL` · `SURVEY` | 🚫 `access_denied` — ảnh sẽ giống hệt [`access_denied_setup_area_viewport.png`](../evidence/access_denied_setup_area_viewport.png), chụp thêm là nhân bản vô ích |
| `ANN` | Màn hình rỗng 2 cột, không có nút nào ngoài `Export` — nội dung đã chép đủ ở bảng trên. Sẽ chụp ở tầng recon module nếu module vào phạm vi |
