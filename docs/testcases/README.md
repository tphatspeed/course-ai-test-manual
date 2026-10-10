# Danh mục Test Cases — Perfex CRM (bản demo Anh Tester)

> Điểm vào cấp hệ thống cho `docs/testcases/`. **Đọc file này đầu tiên** để biết module nào đã có TC, prefix TC ID nào đã chiếm, mã kế tiếp bắt đầu từ đâu.
> Requirements tương ứng: [`../requirements/README.md`](../requirements/README.md)

**Tiền tố TC ID của dự án:** `CRM_` → `CRM_<MODULE>_TC_<3 số>` (chốt 03-10-2026 ở danh mục requirements). Dải TC ID **chung mọi nền tảng** trong một module.

---

## 1. Bảng danh mục module

| Module | Prefix TC ID | Nền tảng | Index | Số TC | Số part | Dải TC ID | TC kế tiếp | Độ hạt | Mức rủi ro | Cập nhật |
|---|---|---|---|---|---|---|---|---|---|---|
| Đăng nhập | `CRM_LOGIN_TC_` | Web 79 | [login/TEST_CASES_LOGIN_SUMMARY.md](login/TEST_CASES_LOGIN_SUMMARY.md) | 79 | 4 | `001` → `079` | `CRM_LOGIN_TC_080` | GỘP (130 case kiểm) | Cao → Đầy đủ | 04-10-2026 |

**Prefix TC ID đã chiếm:** `CRM_LOGIN_TC_`

---

## 2. Độ phủ so với requirements

| Module | REQ trong requirements | REQ có TC | REQ ngoài phạm vi viết TC | REQ 🔴 thiếu TC | TC `@KnownBug` | TC `@NeedsVerify` | Ghi chú |
|---|---|---|---|---|---|---|---|
| `LOGIN` | 72 | 72 | 0 | 0 | 8 | 12 | 6 REQ ⚪ có TC nhưng chờ tài khoản contact (`AMB-SYS-03`) / hộp thư test (`RISK-LOGIN-03`) |

Module requirements còn lại (26 module ở [`../requirements/README.md`](../requirements/README.md)) **chưa** có requirements chi tiết → chưa sinh TC.

---

## 3. Cấu trúc thư mục chuẩn

```
docs/testcases/
├── README.md                                         ← DANH MỤC (file này)
└── <module>/
    ├── TEST_CASES_<TÊN_MODULE>_SUMMARY.md            ← INDEX — TÊN BẤT BIẾN · không chứa dòng TC
    ├── web/test_cases_<module>_web.md                ← ≤ 50 TC (GỘP) / ≤ 40 TC (TÁCH)
    ├── web/parts/part_NN_web_<slug>.md               ← khi vượt ngưỡng
    ├── mobile/… · api/…                              ← cùng quy tắc, chung dải TC ID
    ├── impact/impact_plan_<TICKET-ID>.md             ← /update-testcases-from-impact PLAN
    ├── impact/delta_tc_<TICKET-ID>.md                ← /update-testcases-from-impact APPLY
    └── review/…                                      ← /review-testcases
```

---

## 4. Kết quả thực thi

| Module | Nền tảng | Lần chạy gần nhất | Báo cáo |
|---|---|---|---|
| `LOGIN` | Web | Chưa chạy | — (`/execute-test-cases` ghi vào `docs/executions/login/web/run_<timestamp>/`) |

---

## 5. Quy trình sử dụng

| Tình huống | Workflow |
|---|---|
| Module có requirements nhưng chưa có TC | `/generate-testcases-from-requirements` (QUICK) · `/generate-testcases-manual-rbt` (FULL RBT) |
| Requirements đổi theo ticket, module **đã có** TC | `/update-testcases-from-impact` (DELTA) — **không** sinh lại |
| Chạy tay bộ TC trên trình duyệt | `/execute-test-cases` |
| Review chất lượng / chấm khả năng automation | `/review-testcases` (REVIEW · FIX · AUTOMATION) |
| Sinh automation | `/generate-automation-from-testcases` |
| Ma trận truy vết | `/generate-traceability-matrix` |
| Checklist tick tay rút từ bộ TC | `/generate-checklist-test` |
| Xem / lọc / xuất Excel | Mở `scripts/testcases-viewer/bundle.html`, nạp các file `part_*.md` |

---

## 6. Nhật ký danh mục

| Ngày | Thay đổi | Người/Workflow |
|---|---|---|
| 04-10-2026 | Khởi tạo danh mục. Thêm `LOGIN`: 79 TC Web (4 part, GỘP, 130 case kiểm) phủ 72 / 72 REQ. Chiếm prefix `CRM_LOGIN_TC_` | `/generate-testcases-from-requirements` |
