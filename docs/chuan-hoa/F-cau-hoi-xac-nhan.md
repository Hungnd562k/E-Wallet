# F. Danh sách câu hỏi cần xác nhận

> Trạng thái: **MỞ** · Lập: 2026-10-21 · Người trả lời: chủ dự án
> Liên quan: [README](README.md) · [D. Vấn đề](D-bao-cao-van-de.md) · [G. Giả định & đề xuất](G-gia-dinh-de-xuat.md)

Mỗi câu hỏi gắn với vấn đề ở [D](D-bao-cao-van-de.md), nêu các phương án đã thấy trong nguồn và phương án gợi ý [ĐỀ XUẤT]. Gợi ý **không** phải quyết định: tài liệu nguồn chỉ được sửa sau khi có câu trả lời.

**Ưu tiên**

- **P1**: cần trả lời trước khi kết thúc Sprint 1 (2026-11-01) hoặc trước khi viết FSD của M1.
- **P2**: cần trước milestone liên quan.
- **P3**: dọn dẹp, trả lời lúc nào cũng được.

Cột **Ảnh hưởng nghiệp vụ** = **Có** nghĩa là câu trả lời có thể đổi hành vi với khách hàng hoặc quy tắc tiền; cần chủ dự án quyết định, không chỉ sửa câu chữ.

## 1. Tổng hợp

| ID | Câu hỏi | Ưu tiên | Ảnh hưởng nghiệp vụ | Vấn đề |
|---|---|---|---|---|
| Q-01 | Lỗi nghiệp vụ có được lưu theo `Idempotency-Key`? | P1 | Có | ISS-01 |
| Q-02 | Thứ tự kiểm tra của lệnh tạo giao dịch theo nguồn nào? | P1 | Có | ISS-02 |
| Q-03 | `back-office-web` làm ở M3 hay M4? | P2 | Không | ISS-03 |
| Q-04 | Trạng thái thật của architecture, business-rules, erd? Có cần ghi người duyệt? | P1 | Không | ISS-04, ISS-28 |
| Q-05 | Polyrepo là quyết định chủ ý? Phiên bản Go chuẩn? | P1 | Không | ISS-05, ISS-06 |
| Q-06 | Đưa tài liệu về nhánh mặc định `main`? | P1 | Không | ISS-34 |
| Q-07 | OpenAPI: bản nào là nguồn chuẩn? | P1 | Không | ISS-36 |
| Q-08 | Viết lại BR-06 / BR-45 theo máy trạng thái của erd? | P2 | Có | ISS-07, ISS-08 |
| Q-09 | Giao dịch QR đã được merchant hoàn tiền hiển thị thế nào? | P2 | Có | ISS-09 |
| Q-10 | PIN đăng nhập và PIN giao dịch dùng chung bộ đếm khóa? | P2 | Có | ISS-11 |
| Q-11 | BR-26: "tổng tối đa 300 tr" hiểu theo cách nào? | P2 | Có | ISS-14 |
| Q-12 | BR-28 (1.000đ – 50 tr/lần) áp dụng cho loại giao dịch nào? | P1 | Có | ISS-15 |
| Q-13 | Ngưỡng step-up BR-36 và cách cộng dồn theo nhóm | P2 | Có | ISS-16 |
| Q-14 | BR-25: chuyển ra TK ngân hàng khác có tính hạn mức? | P1 | Có | ISS-17 |
| Q-15 | Có cần đối chiếu văn bản gốc TT45/2025, TT27/2025 trước M1? | P3 | Có | ISS-18 |
| Q-16 | "Ngày làm việc" ở BR-60 tính thế nào? | P2 | Có | ISS-19 |
| Q-17 | BR-63: hoạt động nào làm mới "ngày giao dịch cuối"? | P3 | Có | ISS-20 |
| Q-18 | BR-38: cửa sổ 24 giờ và loại giao dịch tính vào 10 tr/ngày | P2 | Có | ISS-21 |
| Q-19 | Mã lỗi: sửa mã nguồn theo catalogue hay bổ sung catalogue? | P1 | Không | ISS-37 |
| Q-20 | Đưa `lookup.max_per_hour` và các số [P] khác của FSD vào business-rules §5? | P2 | Không | ISS-27, ISS-32 |
| Q-21 | F-23 có bao gồm US-61, US-62 không? | P3 | Không | ISS-10 |
| Q-22 | Danh sách "ngoài phạm vi" chuẩn nằm ở đâu? | P3 | Có | ISS-12 |
| Q-23 | Giữ `Q&A.md` hay thay bằng bảng thuật ngữ? | P3 | Không | ISS-29 |
| Q-24 | Thứ tự viết tài liệu còn thiếu trước M1 | P1 | Không | ISS-24, ISS-25 |
| Q-25 | Thống nhất tên gọi ở ISS-13 theo erd/OpenAPI? | P3 | Không | ISS-13 |
| Q-26 | Ngưỡng đo cho UR-NF-02, UR-NF-07, NFR-07, NFR-09, SM-03 | P2 | Không | ISS-22 |
| Q-27 | Mốc tính "PENDING quá 24 giờ" | P3 | Có | ISS-23 |
| Q-28 | Tiêu chí chấp nhận cho US-11, US-44 | P2 | Có | ISS-26 |
| Q-29 | BR-72 (runbook 72 giờ) làm ở milestone nào? | P3 | Không | ISS-30 |
| Q-30 | Người nhận có được xem giao dịch ở M0 không? | P1 | Có | ISS-38 |
| Q-31 | Phạm vi Sprint 1 của `createTransfer` (PIN, hạn mức, tra SĐT) | P1 | Có | ISS-39 |

Theo ưu tiên: P1 = 12 · P2 = 11 · P3 = 8. Có ảnh hưởng nghiệp vụ: 18/31.

## 2. Chi tiết

### Q-01 — Lỗi nghiệp vụ có được lưu theo `Idempotency-Key`? (ISS-01)

architecture §5.2 nói key "không bị chiếm" khi lỗi; CF-04, erd §4.7, OpenAPI và mã nguồn đều lưu lỗi nghiệp vụ `422`.

- (a) Lưu lỗi nghiệp vụ `422`; không lưu `401/423/409/429/5xx` (như CF-04). [ĐỀ XUẤT] → sửa câu trong architecture §5.2.
- (b) Không lưu lỗi nào (như architecture §5.2) → phải sửa CF-04, AC-7 UC-04.01, erd §4.7, OpenAPI và mã.
- Dùng (a) mặc định nếu như luồng phù hợp, tôi chưa lm tới đoạn đó nên cũng chưa hiểu hết luồng đó chi tiết như nào, nếu trong tương lai phát sinh mâu thuẫn sẽ sửa đổi sau.

### Q-02 — Thứ tự kiểm tra của lệnh tạo giao dịch (ISS-02)

- (a) Theo fsd/04: idempotency → PIN → khoảng số tiền → step-up → khóa ví → trạng thái → hạn mức → bút toán. [ĐỀ XUẤT] → cập nhật sequence architecture §5.2.
- (b) Theo architecture §5.2: xác thực (PIN/step-up) trước rồi mới mở transaction và idempotency → sửa bảng ở fsd/04 và UC-04.02.
- Dùng (a)

### Q-03 — `back-office-web` M3 hay M4? (ISS-03)

prd §9, F-20 và fsd/09 nói M4; architecture §3, §11 nói M3. [ĐỀ XUẤT] M4 theo PRD (PRD là nguồn cho lộ trình theo docs/README §1).

### Q-04 — Trạng thái tài liệu và thông tin duyệt (ISS-04, ISS-28)

1. architecture.md đang là DRAFT hay REVIEWED? Ngày cập nhật đúng của business-rules.md và erd.md là ngày nào?
2. Có muốn ghi người review / người phê duyệt và số phiên bản ở đầu mỗi tài liệu (mẫu ở [B §4](B-bo-tai-lieu-chuan-hoa.md#4-khối-kiểm-soát-tài-liệu))? Nếu có, ai là người phê duyệt?

### Q-05 — Cấu trúc repo và phiên bản Go (ISS-05, ISS-06)

1. Tách `wallet-core` thành repo riêng có phải quyết định chủ ý không? Nếu có, [ĐỀ XUẤT] thêm ADR-16 "Polyrepo" thay ADR-13 và cập nhật architecture §11.
2. Phiên bản Go tối thiểu là 1.26 (theo `go.mod` / CLAUDE.md)? Nếu đúng, sửa architecture §3.

### Q-06 — Nhánh chứa tài liệu (ISS-34)

`docs/` chỉ có ở nhánh `master`; nhánh mặc định là `main`.

- (a) Merge `master` vào `main` và dùng `main` làm nhánh chính. [ĐỀ XUẤT]
- (b) Đổi nhánh mặc định của repo sang `master`.
- (c) Giữ nguyên.
- Tạo và merge Pull request vào nhánh main, sau đó discard master đi

PR của lần chuẩn hóa này được mở **vào `master`** để không trộn với quyết định này.

### Q-07 — Nguồn chuẩn của OpenAPI (ISS-36)

- (a) `E-Wallet/docs/api/` là nguồn chuẩn; wallet-core giữ bản sao và có bước kiểm tra `diff` trong CI. [ĐỀ XUẤT]
- (b) `E-Wallet-Wallet-Core/api/openapi.yaml` là nguồn chuẩn (như architecture §11 dự kiến); docs chỉ trỏ link.
- Dùng (b)

Dù chọn bên nào, bản sao hiện tại đang thiếu thay đổi của commit `SCRUM-70`.

### Q-08 — Viết lại BR-06 và BR-45 (ISS-07, ISS-08)

[ĐỀ XUẤT] tách BR-06 thành hai câu: vòng đời **khách hàng** (`onboarding_status`) và vòng đời **ví** (`INACTIVE → ACTIVE → SUSPENDED / FROZEN → CLOSED`), đúng như erd §4.1, §4.3, §6. Bổ sung vào BR-45 nhánh "giao dịch đồng bộ: `SUCCEEDED` ngay". Nội dung nghiệp vụ không đổi so với erd, nhưng vì sửa câu chữ BR nên cần đồng ý.

### Q-09 — Hiển thị giao dịch QR đã được hoàn tiền (ISS-09)

- (a) Giữ nguyên: giao dịch gốc "Thành công", có thêm giao dịch "Hoàn tiền từ {merchant}". Sửa câu US-25 / CF-03 cho rõ "Đã hoàn" chỉ dành cho bút toán đảo.
- (b) Thêm trạng thái hiển thị "Đã hoàn một phần / Đã hoàn" cho giao dịch gốc dựa trên `merchant.orders.refunded_amount`.

Chưa có gợi ý: đây là quyết định trải nghiệm người dùng.

### Q-10 — Bộ đếm khóa PIN (ISS-11)

1. Đăng nhập bằng PIN (US-05) và xác nhận giao dịch (UC-04.02) dùng **chung** bộ đếm `pin_failed_count`?
2. Khi PIN bị khóa, khách hàng còn đăng nhập được (để xem số dư) không?

### Q-11 — BR-26 (ISS-14)

- (a) GENERAL + ESSENTIAL ≤ 300 tr/tháng (như erd §4.5).
- (b) Riêng ESSENTIAL ≤ 300 tr/tháng, cộng với 100 tr GENERAL.

### Q-12 — Phạm vi BR-28 (ISS-15)

Mức 1.000đ – 50.000.000đ/lần áp dụng cho: chuyển ví–ví? thanh toán QR? rút về TK liên kết? chuyển liên ngân hàng? nạp tiền (đã có mức tối thiểu riêng 10.000đ ở BR-20)? Sau khi chốt, `x-br` của `payQr`, `createTopup` cần cập nhật.

### Q-13 — Step-up BR-36 (ISS-16)

1. Đã đối chiếu Phụ lục 01–02 TT77/2025 bản gốc chưa (business-rules §6.1)? Nếu chưa, có giữ 10 tr / 20 tr làm mặc định cấu hình không?
2. AC của US-24 có cần thêm "cùng nhóm (BR-37)" cho khớp BR-36 không?

### Q-14 — BR-25 và chuyển liên ngân hàng (ISS-17)

Giữ cách hiện tại "tính vào hạn mức" (business-rules §6.2) làm quyết định chính thức cho M1?

### Q-15 — Nguồn pháp lý thứ cấp (ISS-18)

Có cần đối chiếu văn bản gốc TT45/2025 (BR-11) và TT27/2025 (BR-74) trước khi đóng M1 không, hay chấp nhận như hiện tại vì là project học tập?

### Q-16 — Ngày làm việc (ISS-19)

Ngày làm việc = thứ 2 – thứ 6, trừ ngày lễ Việt Nam? Danh sách ngày lễ lấy từ đâu (tham số cấu hình theo năm)?

### Q-17 — "Hoạt động" của ví (ISS-20)

Hoạt động nào làm mới `last_activity_at`: (a) chỉ giao dịch do khách hàng khởi tạo; (b) mọi giao dịch kể cả nhận tiền (như mã hiện tại); (c) cả đăng nhập.

### Q-18 — Hạn chế thiết bị mới (ISS-21)

1. "24 giờ đầu" là 24 giờ trượt tính từ `bound_at`?
2. "10 tr/ngày" cộng mọi giao dịch ra, hay chỉ nhóm (b) của BR-37? Nạp tiền có bị chặn không?

### Q-19 — Mã lỗi (ISS-37)

- (a) Sửa mã nguồn để dùng đúng mã trong `x-error-codes` (`VALIDATION_ERROR`, `UNAUTHENTICATED`, `NOT_FOUND`, `RECIPIENT_INVALID`, `STATE_CONFLICT`). [ĐỀ XUẤT]
- (b) Bổ sung các mã mới vào catalogue và fsd/README §5.

### Q-20 — Tham số trong FSD (ISS-27, ISS-32)

1. Thêm `lookup.max_per_hour = 30` vào business-rules §5?
2. Các giá trị [P] khác trong FSD (12 tháng, 90 ngày, 15 s, 5 người nhận gần đây…) có cần là tham số cấu hình không, hay là hằng số của client?
3. Có áp dụng quy tắc "tài liệu khác chỉ ghi tên key, không ghi số" (docs/README §3) cho urd, architecture, erd, OpenAPI không?

### Q-21 — F-23 (ISS-10)

- (a) Sửa prd §7: F-23 chỉ gồm US-60, US-63. [ĐỀ XUẤT]
- (b) Sửa urd: US-61 → F-12, F-23; US-62 → F-13, F-23.

### Q-22 — Danh sách "ngoài phạm vi" (ISS-12)

[ĐỀ XUẤT] business-rules §3 là nguồn cho phạm vi nghiệp vụ; prd §7 chỉ giữ phần kỹ thuật và trỏ sang. Cần xác nhận "người nước ngoài" và "chữ ký số, FIDO" có nằm ngoài phạm vi theo PRD không.

### Q-23 — `Q&A.md` (ISS-29)

Giữ `Q&A.md` là ghi chú cá nhân, hay thay bằng `docs/glossary.md` theo bản nháp ở [B §6](B-bo-tai-lieu-chuan-hoa.md#6-bảng-thuật-ngữ)?

### Q-24 — Thứ tự viết tài liệu còn thiếu (ISS-24, ISS-25)

[ĐỀ XUẤT] theo thứ tự: (1) `failure-scenarios.md` phần M0 (TC cho SM-01, idempotency, PIN), (2) fsd/02, fsd/03, fsd/05, (3) UC-09.06, 09.07, (4) `failure-scenarios.md` phần M1 (fault injection cho SM-02). Thứ tự này có phù hợp với kế hoạch Sprint 2 không?

### Q-25 — Tên gọi (ISS-13)

Thống nhất theo erd/OpenAPI (`SAFEGUARD_BANK`, `payment_id`, `{merchant_order_id}`, RFC 3339 UTC, `CHECK (allow_negative OR balance >= 0)`, ví có trạng thái `INACTIVE`)? Mục (a) có đổi tên tài khoản trong BR-57 nên cần đồng ý.

### Q-26 — Ngưỡng đo phi chức năng (ISS-22)

1. Chuẩn hiệu năng cho giao dịch đồng bộ là P95 < 300 ms (NFR-07)? Nếu đúng, UR-NF-02 và SM-03 trỏ về NFR-07.
2. UR-NF-07 được kiểm bằng cách nào (bao nhiêu người, kịch bản)?
3. NFR-09 đạt khi nào (ví dụ: một `trace_id` đi qua api → worker → corebank trong Jaeger)?

### Q-27 — Mốc tính 24 giờ (ISS-23)

Tính từ `pending_since` (erd) hay `created_at` (fsd/07)?

### Q-28 — AC cho US-11, US-44 (ISS-26)

Cần AC Given/When/Then cho: đặt liên kết mặc định (khi có 1 liên kết thì tự là mặc định? hủy liên kết mặc định thì sao?) và thông báo (loại sự kiện nào, kênh nào, nội dung có che dữ liệu không).

### Q-29 — BR-72 (ISS-30)

Runbook sự cố lộ dữ liệu làm ở milestone nào, lưu ở đâu (ví dụ `docs/runbooks/`)?

### Q-30 — Người nhận xem giao dịch ở M0 (ISS-38)

UC-07.04 AC-2 yêu cầu người nhận xem được giao dịch. Đây có thuộc phạm vi Sprint 1 (US-25) không, hay để sau?

### Q-31 — Phạm vi Sprint 1 của `createTransfer` (ISS-39)

Sprint 1 có bao gồm PIN (T-08), kiểm hạn mức (US-42), `AMOUNT_OUT_OF_RANGE` (BR-28), `lookupRecipient` và `recipient.phone` (US-21) không? Nếu có phần nào để sau, [ĐỀ XUẤT] ghi vào fsd/04 như CF-05 đã làm với step-up ("Giới hạn ở M0").
