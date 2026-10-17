# PRD — E-Wallet Platform

> Trạng thái: **DRAFT — chờ review** · Cập nhật: 2026-10-17
> Đổi tên từ `planning.md`. Các phần kỹ thuật trước đây nằm trong file này đã chuyển sang [architecture.md](architecture.md), [erd.md](erd.md), [api/](api/).
> Liên quan: [README](README.md) · [urd.md](urd.md) (`US-xx`) · [business-rules.md](business-rules.md) (`BR-xx`) · [fsd/](fsd/) (`UC-xx`)

---

## 1. Bối cảnh & vấn đề

Ví điện tử (lấy cảm hứng từ MoMo) cho phép người dùng giữ tiền, chuyển tiền và thanh toán ngay trên điện thoại.

Phần khó của sản phẩm này **không nằm ở giao diện**. Nó nằm ở các điểm sau:

- **Tính đúng của tiền**: không mất tiền, không ghi trùng, số dư không âm.
- **Nhất quán khi nhiều hệ thống cùng tham gia một giao dịch** (ví, ngân hàng, merchant), kể cả khi có timeout hoặc mất phản hồi.
- **Tuân thủ quy định** về ví điện tử, định danh, bảo vệ dữ liệu, phòng chống rửa tiền tại Việt Nam.

Đây là project **học tập / portfolio**. Sản phẩm **không xử lý tiền thật**; mọi ngân hàng, merchant và nhà cung cấp eKYC đều là **giả lập**. Mục tiêu là **mô phỏng đúng các vấn đề nghiệp vụ và kiến trúc** của một ví điện tử thật.

## 2. Tầm nhìn sản phẩm

> Một ví điện tử tối giản nhưng **đúng về tiền và đúng về quy định**. Người dùng có thể nạp tiền, chuyển tiền, rút tiền và quét QR thanh toán; mọi giao dịch đều truy vết được, đối soát được và phục hồi được khi có sự cố.

## 3. Mục tiêu & chỉ số thành công

| ID | Mục tiêu | Chỉ số đo (Done khi) |
|---|---|---|
| SM-01 | Toàn vẹn số dư dưới tải song song | 1.000 lệnh chuyển ví–ví chéo, chạy đồng thời: 0 lệch ledger, 0 số dư âm, 0 deadlock không được xử lý |
| SM-02 | Chịu lỗi tích hợp ngân hàng | 100% kịch bản fault injection trong `failure-scenarios.md` đều pass: không ghi nợ 2 lần, hold được giữ/giải phóng đúng |
| SM-03 | Trải nghiệm luồng chính | Demo trọn vẹn nạp → chuyển → QR → hoàn tiền. Giao dịch đồng bộ < 1 s (local); giao dịch qua ngân hàng (không có fault) có kết quả < 5 s |
| SM-04 | Đối soát | Job T+1 phát hiện 100% chênh lệch được cấy sẵn; bất biến TK đảm bảo được kiểm tra hằng ngày |
| SM-05 | Chất lượng mã | Coverage ≥ 80% cho domain `ledger`, `payment`, `limit`; `go test -race` sạch |
| SM-06 | Hợp đồng API | `npx @redocly/cli lint` sạch; contract test corebank và merchant pass |

## 4. Người dùng mục tiêu

Chi tiết persona và user story ở [urd.md](urd.md).

| Nhóm | Kênh | Nhu cầu chính |
|---|---|---|
| Khách hàng cá nhân (≥ 18 tuổi, có CCCD gắn chip, có TK ngân hàng) | `app-mobile` | Nạp, chuyển, rút, quét QR nhanh và an toàn; biết rõ trạng thái giao dịch |
| Nhân viên vận hành (SUPPORT / OPERATOR / CHECKER / ADMIN) | `back-office-web` | Tra cứu, xử lý giao dịch treo, duyệt eKYC, đối soát, điều chỉnh có kiểm soát |
| Merchant | API + `merchant-sim` | Tạo đơn hàng có QR, biết chắc đơn đã được thanh toán, hoàn tiền |

## 5. Các hệ thống tham gia

| Hệ thống | Công nghệ | Người dùng | Vai trò |
|---|---|---|---|
| `wallet-core` | Go — modular monolith (`api` + `worker`) | — | Trung tâm nghiệp vụ; quyết định giao dịch có được thực hiện hay không |
| `app-mobile` | Flutter | Khách hàng | Chỉ gửi yêu cầu và hiển thị; không tự quyết định số dư hay trạng thái |
| `back-office-web` | React | Nhân viên vận hành | Tra cứu, vận hành, thao tác có phân quyền và audit |
| `corebank` | Java / Spring Boot | — | Ngân hàng giả lập: TK khách hàng, **TK đảm bảo** của công ty ví (BR-55), fault injection |
| `merchant-sim` | Go | Merchant | Tạo đơn hàng / QR, nhận callback |

Kiến trúc chi tiết ở [architecture.md](architecture.md).

## 6. Các luồng tiền

| Nghiệp vụ | Nguồn | Đích | Hệ thống ngoài | Đồng bộ? |
|---|---|---|---|---|
| Nạp tiền | TK ngân hàng chính chủ đã liên kết | Ví | corebank | Không (`PENDING`) |
| Chuyển ví–ví | Ví A | Ví B | — | Có |
| Rút tiền | Ví | TK ngân hàng đã liên kết | corebank | Không (hold) |
| Chuyển liên ngân hàng | Ví | TK bất kỳ | corebank (mạng giả lập) | Không (hold) |
| Thanh toán QR | Ví | Merchant | merchant (callback) | Có |
| Hoàn tiền | Merchant | Ví | — | Có |

Bút toán của từng luồng xem [erd.md §3.2](erd.md). Đặc tả chi tiết từng luồng nằm trong [fsd/](fsd/).

## 7. Danh sách tính năng

Ưu tiên theo MoSCoW: **M**ust · **S**hould · **C**ould. Milestone xem §9.

| ID | Tính năng | Ưu tiên | Milestone | User story | FSD |
|---|---|---|---|---|---|
| F-01 | Đăng ký bằng SĐT + OTP, đăng nhập bằng PIN, gắn thiết bị | Must | M3 (M0 dùng seed) | US-01, 03, 05, 06, 07 | fsd/01 |
| F-02 | Đồng ý xử lý dữ liệu (tách riêng sinh trắc học) | Must | M3 | US-02 | fsd/01 |
| F-03 | eKYC mô phỏng (CCCD gắn chip + khuôn mặt), eKYC lại khi giấy tờ hết hạn | Must | M3 | US-04, 08 | fsd/01 |
| F-04 | Ví, số dư khả dụng / đang giữ | Must | M0 | US-40 | fsd/07 |
| F-05 | Chuyển ví–ví | Must | M0 | US-21 | fsd/04 |
| F-06 | Hạn mức giao dịch (tháng, theo lần) | Must | M0 | US-42 | fsd/07 |
| F-07 | Xác thực giao dịch: PIN (M0), OTP + khuôn mặt cho giao dịch lớn (M3) | Must | M0 / M3 | US-24 | fsd/01, fsd/04 |
| F-08 | Liên kết / hủy liên kết ngân hàng | Must | M1 | US-10, 11, 12 | fsd/02 |
| F-09 | Nạp tiền | Must | M1 | US-20 | fsd/03 |
| F-10 | Rút tiền về TK liên kết | Must | M1 | US-22 | fsd/05 |
| F-11 | Chuyển liên ngân hàng | Should | M1 | US-23 | fsd/05 |
| F-12 | Thanh toán QR động | Must | M2 | US-30, 60, 61 | fsd/06 |
| F-13 | Hoàn tiền từ merchant | Should | M2 | US-31, 62 | fsd/06 |
| F-14 | Lịch sử, trạng thái giao dịch, thông báo | Must | M0 (API) / M4 (push) | US-25, 41, 44 | fsd/07 |
| F-15 | Đóng ví (theo yêu cầu, do không hoạt động) | Should | M4 | US-43, 45 | fsd/07 |
| F-16 | Tra soát / khiếu nại | Should | M4 | US-50, 51, 79 | fsd/08 |
| F-17 | Đối soát T+1 & kiểm tra bất biến TK đảm bảo | Must | M1 | US-74, 75 | fsd/09 |
| F-18 | Điều chỉnh tài chính maker–checker | Should | M4 | US-76, 77 | fsd/09 |
| F-19 | Cảnh báo AML cơ bản | Could | M4 | US-78 | fsd/09 |
| F-20 | Back-office: tra cứu, duyệt eKYC thủ công, phong tỏa ví | Should | M4 | US-70…73 | fsd/09 |
| F-21 | Quản trị tham số cấu hình | Could | M4 | US-80 | fsd/09 |
| F-22 | Audit log | Must | M0 | US-81 | fsd/09 |
| F-23 | Merchant API + `merchant-sim` | Must | M2 | US-60…63 | fsd/06 |

### Ngoài phạm vi

- Nghiệp vụ: ví tổ chức / hộ kinh doanh, ví cho người dưới 18 tuổi, merchant cá nhân, QR tĩnh, VietQR/NAPAS thật, thanh toán hóa đơn thiết yếu, ngoại tệ / xuyên biên giới, voucher / cashback / loyalty, tín dụng / trả sau (xem [business-rules.md §3](business-rules.md)).
- Kỹ thuật: Kubernetes, service mesh, Kafka giữa các module, tự xây nhận diện khuôn mặt, fraud detection bằng ML, multi-region.
- Dù ngoài phạm vi những thứ trên, **không được hoãn**: số dư đúng, idempotency, audit, bảo mật, đối soát.

## 8. Yêu cầu phi chức năng

Chi tiết kỹ thuật ở [architecture.md](architecture.md) §1, §8, §10.

| ID | Nhóm | Yêu cầu |
|---|---|---|
| NFR-01 | Toàn vẹn | Ledger double-entry, bất biến được ép ở tầng DB; số dư không âm; job phát hiện drift hằng ngày |
| NFR-02 | Idempotency | Mọi lệnh tạo giao dịch và lệnh tích hợp đều idempotent (`Idempotency-Key`, `bank_operation_id`) |
| NFR-03 | Nhất quán | Ưu tiên nhất quán hơn sẵn sàng; kết quả chưa rõ thì giữ `PENDING`, không đoán |
| NFR-04 | Bảo mật dữ liệu | Mã hóa field-level cho CCCD và sinh trắc học; log tự che dữ liệu nhạy cảm; ảnh lưu trong bucket private |
| NFR-05 | Xác thực | JWT 15 phút, refresh token xoay vòng, gắn thiết bị; PIN băm argon2id; step-up theo ngưỡng |
| NFR-06 | Kiểm toán | Mọi thay đổi tiền và trạng thái đều có lịch sử / audit kèm người thực hiện, lý do, `request_id` |
| NFR-07 | Hiệu năng | Local: chuyển ví–ví P95 < 300 ms; ≥ 100 lệnh ví–ví/giây trên một máy dev |
| NFR-08 | Phục hồi | Worker crash không mất job (outbox + lease); transaction đã commit không bao giờ mất |
| NFR-09 | Quan sát | Trace xuyên suốt mobile → api → worker → corebank; metrics và alert tối thiểu |
| NFR-10 | Lưu trữ | Theo BR-73: KYC 5 năm sau khi đóng ví, ledger 10 năm |
| NFR-11 | Kiểm thử được | corebank có fault injection xác định; có test kiến trúc cho import graph |
| NFR-12 | Ngôn ngữ | Giao diện và thông báo lỗi bằng tiếng Việt; API trả mã lỗi máy đọc được |

## 9. Lộ trình

Xây theo **lát cắt nghiệp vụ**, ưu tiên nền tảng tài chính. Thứ tự đã chỉnh so với `planning.md` cũ:

- eKYC và chuyển liên ngân hàng trở thành bắt buộc vì quy định ([business-rules.md](business-rules.md) BR-04, BR-05).
- Onboarding đầy đủ được gom vào M3. Trong M0–M2, khách hàng "đã eKYC" được tạo bằng seed data, chỉ bật ở profile `dev`.

| Milestone | Nội dung | Tính năng | Done khi |
|---|---|---|---|
| **M0 — Nền tảng tiền** | Khung Go, PostgreSQL, migration, ví, ledger, hạn mức, PIN, chuyển ví–ví, idempotency, outbox, audit | F-04, 05, 06, 07 (PIN), 14 (API), 22 | SM-01 |
| **M1 — Ngân hàng** | corebank (Java) + TK đảm bảo + fault injection; liên kết, nạp, rút, liên ngân hàng; worker retry / tra cứu; đối soát T+1 | F-08…11, 17 | SM-02, SM-04 |
| **M2 — Thanh toán** | Merchant, QR động, callback, hoàn tiền, `merchant-sim` | F-12, 13, 23 | Một đơn không bị thanh toán trùng kể cả khi callback lỗi |
| **M3 — Onboarding & bảo mật** | Đăng ký OTP, consents, eKYC mock + MinIO, thiết bị, step-up OTP + khuôn mặt, giấy tờ hết hạn | F-01, 02, 03, 07 (step-up) | Khách hàng mới đi trọn đăng ký → eKYC → liên kết → ví `ACTIVE` |
| **M4 — Sản phẩm & vận hành** | Flutter hoàn chỉnh, back-office React, push, khiếu nại, điều chỉnh, AML, cấu hình, observability | F-14 (push), 15, 16, 18…21 | SM-03; demo end-to-end trên app + back-office |

## 10. Giả định, ràng buộc, rủi ro

**Giả định**

- Không có tiền thật, không có ngân hàng thật; chỉ một loại tiền VND.
- Chỉ chạy ở môi trường local (Docker Compose); không có production.
- Nhà cung cấp eKYC / SMS / push đều là mock.

**Ràng buộc**

- Pháp lý: TT 40/2024 (sửa bởi TT 41/2025), TT 50/2024 (sửa bởi TT 77/2025), Luật PCRT 2022, Luật BVDLCN 2025. Xem [business-rules.md §2](business-rules.md).
- Nhân lực: 1 người phát triển; công nghệ cố định là Go, Java, Flutter, React.

**Rủi ro**

| Rủi ro | Ảnh hưởng | Giảm thiểu |
|---|---|---|
| Ngưỡng pháp lý chưa chắc chắn (TT 77 là 5 tr hay 10 tr…) | Quy tắc sai | Mọi ngưỡng là cấu hình; danh sách cần đối chiếu ở business-rules §6 |
| Phạm vi quá lớn cho 1 người | Trễ, bỏ dở | Làm theo milestone; M0–M1 là lõi; M4 có thể cắt bớt |
| Viết corebank bằng Java tốn công | Chậm M1 | Giữ corebank ở mức tối thiểu theo `api/corebank.openapi.yaml` |
| Bug về concurrency trong ledger | Sai số dư | Ràng buộc ở DB + test `-race` + test song song (SM-01) |
| Lẫn lộn giữa đề xuất của tôi ([P]) và quy định ([L]) | Hiểu sai về tuân thủ | Nhãn [L]/[P] trên từng BR |

## 11. Lịch sử

- Thảo luận gốc: https://chatgpt.com/share/6ad0fca4-bad0-83ec-a5b8-d8c4a680fca3
- 2026-10-15: bản `planning.md` đầu tiên (gồm cả thiết kế kỹ thuật).
- 2026-10-17: tách thành PRD; phần kỹ thuật chuyển sang architecture / erd / api; quy tắc nghiệp vụ ở `business-rules.md` (đổi tên từ `scope.md`).
