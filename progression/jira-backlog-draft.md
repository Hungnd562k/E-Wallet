# Jira backlog draft — project SCRUM

> Nguồn: [urd.md](../docs/urd.md) (US-xx, AC), [prd.md](../docs/prd.md) §7 (F → milestone), §9 (milestone), [architecture.md](../docs/architecture.md), [erd.md](../docs/erd.md).
> Trạng thái: **ĐÃ ĐẨY LÊN JIRA** (project SCRUM, board 1). Cập nhật: 2026-10-17.

## Quy ước

- **Epic** = E0–E8. **Story** = US-xx (description = user story + AC trong urd.md + BR). **Task** = việc kỹ thuật không có US (T-xx).
- **Labels**: milestone (`M0`…`M4`) + feature (`F-xx`).
- Summary dạng `US-21 Chuyển ví–ví bằng SĐT` để JQL tìm lại được (tránh tạo trùng).
- Story points: để trống, ước lượng khi sprint planning.

## Sprint 1 — M0 Nền tảng tiền

- Thời gian: **2026-10-19 → 2026-11-01** (2 tuần).
- Goal: **SM-01** — 1.000 lệnh chuyển ví–ví song song: 0 lệch ledger, 0 số dư âm, 0 deadlock không xử lý.
- Phạm vi: 10 task E0 + 6 story (US-21, 25, 40, 41, 42, 81).

| Key | Loại | Epic | Summary | Labels |
|---|---|---|---|---|
| T-01 | Task | E0 | Khung `wallet-core` (Go modular monolith: `api` + `worker`), cấu trúc package & test kiến trúc import graph (arch §4) | M0 |
| T-02 | Task | E0 | Docker Compose local (PostgreSQL…), CI: lint, `go test -race`, redocly lint | M0 |
| T-03 | Task | E0 | Migration + schema `identity`, `wallet`, `ledger`, `limits`, `payment`, `audit`, `platform` (erd §4) | M0 |
| T-04 | Task | E0 | Ledger double-entry, ràng buộc DB: số dư không âm, bút toán cân (erd §3, NFR-01) | M0, F-04 |
| T-05 | Task | E0 | Middleware `Idempotency-Key` cho mọi lệnh tạo giao dịch (NFR-02) | M0 |
| T-06 | Task | E0 | Outbox + worker lease, không mất job khi crash (arch §6, NFR-08) | M0 |
| T-07 | Task | E0 | Hạ tầng audit log: người thực hiện, lý do, `request_id`, che dữ liệu nhạy cảm (NFR-06) | M0, F-22 |
| T-08 | Task | E0 | Xác thực giao dịch bằng PIN (argon2id, khóa sau 5 lần sai) — phần M0 của F-07 | M0, F-07 |
| T-09 | Task | E0 | Seed data khách hàng "đã eKYC" + ví `ACTIVE`, chỉ bật ở profile `dev` | M0 |
| T-10 | Task | E0 | Test tải/song song SM-01 + job phát hiện drift ledger | M0 |
| US-40 | Story | E5 | Xem số dư khả dụng và số tiền đang giữ | M0, F-04 |
| US-21 | Story | E3 | Chuyển tiền cho người dùng ví khác bằng SĐT | M0, F-05 |
| US-42 | Story | E5 | Xem hạn mức tháng còn lại | M0, F-06 |
| US-25 | Story | E3 | Biết rõ trạng thái giao dịch (API) | M0, F-14 |
| US-41 | Story | E5 | Lịch sử giao dịch, lọc theo loại/thời gian (API) | M0, F-14 |
| US-81 | Story | E8 | Tra cứu audit log thao tác nhạy cảm (API) | M0, F-22 |

## Backlog (chưa vào sprint)

### M1 — Ngân hàng

| Key | Loại | Epic | Summary | Labels |
|---|---|---|---|---|
| T-11 | Task | E0 | `corebank` (Java/Spring Boot) tối thiểu theo `corebank.openapi.yaml` + TK đảm bảo + fault injection | M1 |
| T-12 | Task | E0 | Worker retry / tra cứu trạng thái ngân hàng, `bank_operation_id` | M1 |
| US-10 | Story | E2 | Liên kết TK/thẻ ghi nợ chính chủ | M1, F-08 |
| US-11 | Story | E2 | Chọn liên kết mặc định | M1, F-08 |
| US-12 | Story | E2 | Hủy liên kết | M1, F-08 |
| US-20 | Story | E3 | Nạp tiền từ ngân hàng liên kết | M1, F-09 |
| US-22 | Story | E3 | Rút tiền về TK liên kết | M1, F-10 |
| US-23 | Story | E3 | Chuyển tới TK ngân hàng bất kỳ | M1, F-11 |
| US-74 | Story | E8 | Chốt kết quả giao dịch treo cần đối soát | M1, F-17 |
| US-75 | Story | E8 | Xem kết quả đối soát hằng ngày & TK đảm bảo | M1, F-17 |

### M2 — Thanh toán

| Key | Loại | Epic | Summary | Labels |
|---|---|---|---|---|
| T-13 | Task | E0 | `merchant-sim` (Go) + contract test merchant | M2, F-23 |
| US-60 | Story | E7 | Merchant tạo đơn hàng và nhận QR | M2, F-12, F-23 |
| US-61 | Story | E7 | Merchant nhận callback HMAC / tra cứu trạng thái đơn | M2, F-12 |
| US-62 | Story | E7 | Merchant hoàn tiền toàn bộ / một phần | M2, F-13 |
| US-63 | Story | E7 | Merchant hủy đơn chưa trả | M2, F-23 |
| US-30 | Story | E4 | Quét QR thanh toán tại quầy | M2, F-12 |
| US-31 | Story | E4 | Nhận tiền hoàn từ merchant | M2, F-13 |

### M3 — Onboarding & bảo mật

| Key | Loại | Epic | Summary | Labels |
|---|---|---|---|---|
| US-01 | Story | E1 | Đăng ký bằng SĐT + OTP | M3, F-01 |
| US-02 | Story | E1 | Đồng ý từng loại điều khoản (tách sinh trắc học) | M3, F-02 |
| US-03 | Story | E1 | Đặt / đổi PIN 6 số | M3, F-01 |
| US-04 | Story | E1 | eKYC CCCD gắn chip + khuôn mặt | M3, F-03 |
| US-05 | Story | E1 | Đăng nhập SĐT + PIN | M3, F-01 |
| US-06 | Story | E1 | Đăng nhập an toàn trên máy mới | M3, F-01 |
| US-07 | Story | E1 | Đặt lại PIN khi quên / bị khóa | M3, F-01 |
| US-08 | Story | E1 | Nhắc CCCD hết hạn & eKYC lại | M3, F-03 |
| US-24 | Story | E3 | Step-up OTP + khuôn mặt cho giao dịch lớn | M3, F-07 |

### M4 — Sản phẩm & vận hành

| Key | Loại | Epic | Summary | Labels |
|---|---|---|---|---|
| US-44 | Story | E5 | Thông báo tiền vào/ra, đổi trạng thái (push) | M4, F-14 |
| US-43 | Story | E5 | Đóng ví theo yêu cầu | M4, F-15 |
| US-45 | Story | E5 | Báo trước khi đóng ví do không hoạt động | M4, F-15 |
| US-50 | Story | E6 | Gửi khiếu nại giao dịch | M4, F-16 |
| US-51 | Story | E6 | Theo dõi tiến độ khiếu nại | M4, F-16 |
| US-70 | Story | E8 | CSKH tra cứu khách hàng | M4, F-20 |
| US-71 | Story | E8 | Duyệt / từ chối eKYC thủ công | M4, F-20 |
| US-72 | Story | E8 | Xem ảnh CCCD/selfie có audit | M4, F-20 |
| US-73 | Story | E8 | Phong tỏa / gỡ phong tỏa ví | M4, F-20 |
| US-76 | Story | E8 | Tạo đề nghị điều chỉnh tài chính | M4, F-18 |
| US-77 | Story | E8 | Duyệt điều chỉnh (maker–checker) | M4, F-18 |
| US-78 | Story | E8 | Xử lý cảnh báo AML | M4, F-19 |
| US-79 | Story | E8 | Xử lý khiếu nại đúng hạn | M4, F-16 |
| US-80 | Story | E8 | Quản trị tham số hạn mức / ngưỡng | M4, F-21 |

## Epic

| Key | Summary |
|---|---|
| E0 | Nền tảng kỹ thuật (không có trong urd.md — bổ sung cho task kỹ thuật) |
| E1 | Onboarding & tài khoản |
| E2 | Liên kết ngân hàng |
| E3 | Chuyển & nhận tiền |
| E4 | Thanh toán QR |
| E5 | Ví, lịch sử, hạn mức |
| E6 | Tra soát & khiếu nại |
| E7 | Merchant |
| E8 | Vận hành back-office |

**Tổng**: 9 epic · 43 story · 13 task. Sprint 1: 16 issue.

## Mapping Jira key

Sprint 1 = sprint id 1 trên board 1 (chưa Start). SCRUM-1…4 là issue mẫu, đã chuyển Done.

| Draft | Jira key |
|---|---|
| E0 | [SCRUM-5](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-5) |
| E1 | [SCRUM-6](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-6) |
| E2 | [SCRUM-7](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-7) |
| E3 | [SCRUM-8](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-8) |
| E4 | [SCRUM-9](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-9) |
| E5 | [SCRUM-10](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-10) |
| E6 | [SCRUM-11](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-11) |
| E7 | [SCRUM-12](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-12) |
| E8 | [SCRUM-13](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-13) |
| T-01 | [SCRUM-14](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-14) |
| T-02 | [SCRUM-15](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-15) |
| T-03 | [SCRUM-16](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-16) |
| T-04 | [SCRUM-17](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-17) |
| T-05 | [SCRUM-18](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-18) |
| T-06 | [SCRUM-19](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-19) |
| T-07 | [SCRUM-20](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-20) |
| T-08 | [SCRUM-21](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-21) |
| T-09 | [SCRUM-22](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-22) |
| T-10 | [SCRUM-23](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-23) |
| US-40 | [SCRUM-24](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-24) |
| US-21 | [SCRUM-25](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-25) |
| US-42 | [SCRUM-26](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-26) |
| US-25 | [SCRUM-27](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-27) |
| US-41 | [SCRUM-28](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-28) |
| US-81 | [SCRUM-29](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-29) |
| T-11 | [SCRUM-30](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-30) |
| T-12 | [SCRUM-31](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-31) |
| T-13 | [SCRUM-32](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-32) |
| US-10 | [SCRUM-33](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-33) |
| US-11 | [SCRUM-34](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-34) |
| US-12 | [SCRUM-35](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-35) |
| US-20 | [SCRUM-36](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-36) |
| US-22 | [SCRUM-37](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-37) |
| US-23 | [SCRUM-38](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-38) |
| US-74 | [SCRUM-39](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-39) |
| US-75 | [SCRUM-40](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-40) |
| US-60 | [SCRUM-41](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-41) |
| US-61 | [SCRUM-42](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-42) |
| US-62 | [SCRUM-43](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-43) |
| US-63 | [SCRUM-44](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-44) |
| US-30 | [SCRUM-45](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-45) |
| US-31 | [SCRUM-46](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-46) |
| US-01 | [SCRUM-47](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-47) |
| US-02 | [SCRUM-48](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-48) |
| US-03 | [SCRUM-49](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-49) |
| US-04 | [SCRUM-50](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-50) |
| US-05 | [SCRUM-51](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-51) |
| US-06 | [SCRUM-52](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-52) |
| US-07 | [SCRUM-53](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-53) |
| US-08 | [SCRUM-54](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-54) |
| US-24 | [SCRUM-55](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-55) |
| US-44 | [SCRUM-56](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-56) |
| US-43 | [SCRUM-57](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-57) |
| US-45 | [SCRUM-58](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-58) |
| US-50 | [SCRUM-59](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-59) |
| US-51 | [SCRUM-60](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-60) |
| US-70 | [SCRUM-61](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-61) |
| US-71 | [SCRUM-62](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-62) |
| US-72 | [SCRUM-63](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-63) |
| US-73 | [SCRUM-64](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-64) |
| US-76 | [SCRUM-65](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-65) |
| US-77 | [SCRUM-66](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-66) |
| US-78 | [SCRUM-67](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-67) |
| US-79 | [SCRUM-68](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-68) |
| US-80 | [SCRUM-69](https://hungvtpt258-1792254313388.atlassian.net/browse/SCRUM-69) |
