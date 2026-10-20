# FSD 09 — Vận hành back-office

> Trạng thái: **DRAFT — phần M0 (audit log)** · Cập nhật: 2026-10-18 · Milestone: **M0** (§1, UC-09.01), M1, M4
> Liên quan: [fsd/README](README.md) · [urd.md](../urd.md) US-70…81 · [prd.md](../prd.md) F-17…22 · [business-rules.md](../business-rules.md) BR-56…64, 71 · [architecture.md](../architecture.md) §8.1 · [erd.md](../erd.md) §4.12

| UC | Tên | US | Milestone | Trạng thái |
|---|---|---|---|---|
| [UC-09.01](#uc-0901--tra-cứu-audit-log) | Tra cứu audit log | US-81 | M0 | Draft |
| UC-09.02 | Đăng nhập back-office | — | M4 (M0 dùng tài khoản seed) | Chưa viết |
| UC-09.03 | Tra cứu khách hàng | US-70 | M4 | Chưa viết |
| UC-09.04 | Duyệt eKYC thủ công, xem ảnh giấy tờ | US-71, 72 | M4 | Chưa viết |
| UC-09.05 | Phong tỏa / gỡ phong tỏa ví | US-73 | M4 | Chưa viết |
| UC-09.06 | Chốt giao dịch cần đối soát | US-74 | M1 | Chưa viết |
| UC-09.07 | Xem kết quả đối soát T+1 và TK đảm bảo | US-75 | M1 | Chưa viết |
| UC-09.08 | Tạo / duyệt điều chỉnh tài chính (maker–checker) | US-76, 77 | M4 | Chưa viết |
| UC-09.09 | Xử lý cảnh báo AML | US-78 | M4 | Chưa viết |
| UC-09.10 | Xử lý khiếu nại | US-79 | M4 | Chưa viết |
| UC-09.11 | Quản trị tham số cấu hình | US-80 | M4 | Chưa viết |

Ở M0 chưa có giao diện back-office (React, M4). UC-09.01 được giao dưới dạng API; tài khoản staff được tạo bằng seed `dev` và đăng nhập qua `adminLogin`.

---

## 1. Ghi audit (F-22)

Phần này là **yêu cầu chung** cho mọi module: thao tác nào phải ghi audit, ghi những gì, không được ghi gì. Hạ tầng audit làm ở M0; mỗi milestone sau bổ sung action của mình vào danh mục §1.3.

### 1.1. Nguyên tắc

| # | Yêu cầu | BR |
|---|---|---|
| AU-1 | Ghi audit cho: mọi thao tác của **staff** (kể cả chỉ xem dữ liệu nhạy cảm), mọi thao tác **bảo mật** của khách hàng (khóa PIN, đổi thiết bị, đặt lại PIN, rút đồng ý), mọi quyết định của **hệ thống** làm thay đổi trạng thái ví / khách hàng. | BR-64, 71 |
| AU-2 | **Không** ghi audit cho từng giao dịch tiền thông thường: đã có `ledger.*`, `payment.payments`, `payment.status_history`. | — |
| AU-3 | Ghi cả khi bị từ chối (`outcome = DENIED`, ví dụ sai role) hoặc lỗi (`ERROR`), không chỉ khi thành công. | — |
| AU-4 | Thao tác **thay đổi dữ liệu** của staff bắt buộc có `reason` ≥ 10 ký tự; thiếu → `400 VALIDATION_ERROR`, thao tác không thực hiện. | US-71…80 |
| AU-5 | Audit của thao tác thay đổi dữ liệu được ghi **trong cùng DB transaction** với thao tác. Thao tác chỉ đọc (xem ảnh, tìm kiếm) ghi ngay khi trả kết quả. | NFR-06 |
| AU-6 | `audit.audit_logs` là append-only; không có API sửa hoặc xóa. Lưu theo erd §8. | BR-73 |
| AU-7 | **Cấm** ghi vào bất kỳ trường nào (kể cả `metadata`): số CCCD đầy đủ, ảnh, dữ liệu khuôn mặt, OTP, PIN, token, số TK đầy đủ. Chỉ ghi dạng đã che (CF-02) hoặc ID. | BR-71 |

### 1.2. Trường của một bản ghi

Theo erd §4.12. Bắt buộc: `occurred_at`, `actor_type`, `action`, `resource_type`, `resource_id`, `outcome`, `request_id`. `actor_id` bắt buộc trừ `SYSTEM`. `reason` bắt buộc theo AU-4. `metadata` chứa giá trị trước/sau của trường thay đổi (nếu không nhạy cảm).

Quy ước `action`: `<đối tượng>.<hành động>` viết thường, ví dụ `wallet.freeze`.

### 1.3. Danh mục action

| Action | Actor | Resource | Khi nào | Milestone |
|---|---|---|---|---|
| `auth.pin.locked` | CUSTOMER | customer | PIN bị khóa tạm hoặc khóa cứng (UC-04.02); `metadata.hard` | **M0** |
| `staff.login` | STAFF | staff | Đăng nhập back-office (thành công / thất bại) | **M0** |
| `audit.search` | STAFF | audit_log | Tra cứu audit log (UC-09.01); `metadata` = bộ lọc | **M0** |
| `auth.device.bind` | CUSTOMER | device | Đăng nhập máy mới, máy cũ bị hủy gắn | M3 |
| `auth.pin.reset` | CUSTOMER | customer | Đặt lại PIN qua OTP + khuôn mặt | M3 |
| `consent.withdraw` | CUSTOMER | consent | Rút lại đồng ý | M3 |
| `wallet.suspend` | SYSTEM / STAFF | wallet | CCCD hết hạn, nghi gian lận, lệch đối soát | M1 / M3 |
| `customer.view` | STAFF | customer | Mở hồ sơ khách hàng | M4 |
| `kyc.decide` | STAFF | kyc_session | Duyệt / từ chối eKYC thủ công | M4 |
| `kyc.image.view` | STAFF | kyc_document | Mở link xem ảnh giấy tờ / selfie | M4 |
| `wallet.freeze`, `wallet.unfreeze`, `wallet.reactivate` | STAFF | wallet | Phong tỏa / gỡ / kích hoạt lại | M4 |
| `payment.resolve` | STAFF | payment | Chốt giao dịch `NEEDS_RECONCILIATION` | M1 |
| `recon.item.resolve` | STAFF | recon_item | Xử lý chênh lệch đối soát | M1 |
| `adjustment.create`, `.submit`, `.approve`, `.reject` | STAFF | adjustment | Maker–checker | M4 |
| `aml.alert.update` | STAFF | aml_alert | Đổi trạng thái cảnh báo AML | M4 |
| `dispute.update` | STAFF | dispute | Cập nhật khiếu nại | M4 |
| `setting.update` | STAFF | setting | Đổi tham số cấu hình; `metadata` = giá trị cũ/mới | M4 |

---

## UC-09.01 — Tra cứu audit log

| Thuộc tính | Giá trị |
|---|---|
| Actor | Kiểm soát viên (P5, `CHECKER`), quản trị (P6, `ADMIN`) |
| Mục tiêu | Biết ai đã làm gì, với đối tượng nào, khi nào, vì sao. |
| Trigger | Mở **Audit log** trên back-office; hoặc từ trang chi tiết một đối tượng (ví, khách hàng…) bấm **Lịch sử thao tác** (M4). |
| Tiền điều kiện | Staff đã đăng nhập, có role `CHECKER` hoặc `ADMIN`. |
| Hậu điều kiện | Không thay đổi dữ liệu nghiệp vụ; tạo 1 bản ghi `audit.search`. |
| Milestone · US · F | M0 (API) / M4 (UI) · US-81 · F-22 |

### Luồng chính

| # | Actor | Hệ thống |
|---|---|---|
| 1 | Mở S-09.1. | Mặc định lọc 24 giờ gần nhất; gọi `adminListAuditLogs`. |
| 2 | | Kiểm tra role; trả danh sách mới nhất trước, phân trang keyset (`limit` mặc định 20, tối đa 100). Ghi `audit.search`. |
| 3 | Nhập bộ lọc, bấm **Tìm**. | Gọi lại từ đầu với bộ lọc. |
| 4 | Bấm một dòng. | Mở panel chi tiết: toàn bộ trường của bản ghi, `metadata` dạng JSON đã định dạng. |

### Luồng thay thế

| ID | Rẽ tại | Mô tả |
|---|---|---|
| A1 | 4 | Bấm `request_id` → lọc tất cả bản ghi cùng `request_id`. Bấm `resource_id` → lọc theo đối tượng đó. |
| A2 | 2 | Không có kết quả → "Không có bản ghi phù hợp." |

### Luồng ngoại lệ

| ID | Điều kiện | Mã lỗi | Hành vi |
|---|---|---|---|
| E1 | Role `SUPPORT` hoặc `OPERATOR` | `403 FORBIDDEN` | Menu Audit log không hiển thị với các role này; gọi API trực tiếp bị từ chối và ghi `audit.search` với `outcome = DENIED`. |
| E2 | `from > to` hoặc khoảng > 90 ngày [P] | `400 VALIDATION_ERROR` | "Khoảng thời gian tối đa 90 ngày." |
| E3 | Token staff hết hạn | `401 UNAUTHENTICATED` | Về màn đăng nhập back-office |

### Màn hình & trường

**S-09.1 — Bộ lọc**

| Trường | Kiểu | Map API | Ghi chú |
|---|---|---|---|
| Người thực hiện | text (ID staff / customer) | `actor_id` | |
| Loại đối tượng | chọn | `resource_type` | customer, wallet, payment, kyc_session, adjustment, setting… |
| Mã đối tượng | text | `resource_id` | |
| Action | chọn (theo §1.3) | `action` | |
| Kết quả | chọn | `outcome` | Tất cả · SUCCESS · DENIED · ERROR |
| Từ / đến | datetime | `from`, `to` | Giờ VN; mặc định 24 giờ gần nhất |

**S-09.1 — Cột kết quả**: thời gian, actor (`actor_type` + `actor_id`), action, đối tượng (`resource_type`/`resource_id`), kết quả, lý do (cắt 80 ký tự), `request_id`.

### Dữ liệu

| Hạng mục | Chi tiết |
|---|---|
| API | `adminListAuditLogs` (role `CHECKER`, `ADMIN`) |
| Bảng | `audit.audit_logs`; index `(resource_type, resource_id, occurred_at)`, `(actor_id, occurred_at)` (erd §4.12) |
| Audit | `audit.search` (§1.3) |

### Tiêu chí chấp nhận

Từ urd.md **US-81**: lọc được theo người thực hiện, đối tượng, thời gian; log không chứa CCCD đầy đủ, OTP, PIN.

| # | Given | When | Then |
|---|---|---|---|
| AC-1 | Khách hàng A bị khóa PIN (UC-04.02) | CHECKER lọc `resource_id` = A | Thấy 1 dòng `auth.pin.locked`, `actor_type = CUSTOMER` |
| AC-2 | Staff role `SUPPORT` | Gọi `adminListAuditLogs` | `403`; có 1 dòng `audit.search` với `outcome = DENIED` |
| AC-3 | CHECKER tra cứu | Tra cứu lại ngay sau đó | Thấy dòng `audit.search` của lần tra cứu trước |
| AC-4 | Dữ liệu test có CCCD, PIN, OTP đã biết | Tìm các giá trị đó trong toàn bộ `audit.audit_logs` (mọi cột, kể cả `metadata`) | Không tìm thấy |
