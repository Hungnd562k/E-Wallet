# D. Báo cáo lỗi và vấn đề

> Trạng thái: **DRAFT — chờ chủ dự án xác nhận** · Lập: 2026-10-21
> Nguồn đối chiếu: `E-Wallet` nhánh `master` commit `087c3ff` · `E-Wallet-Wallet-Core` nhánh `development` commit `7332c9e`. §8 đối chiếu lại với `development` commit `9a05355` ngày 2026-10-25
> Liên quan: [README](README.md) · [F. Câu hỏi](F-cau-hoi-xac-nhan.md) · [G. Giả định & đề xuất](G-gia-dinh-de-xuat.md)

Báo cáo này liệt kê các điểm **mâu thuẫn, mơ hồ, thiếu, trùng lặp, lỗi thời** trong bộ tài liệu, và các điểm **lệch giữa tài liệu và mã nguồn**. Mỗi vấn đề có bằng chứng trỏ về vị trí trong nguồn.

Nguyên tắc: khi hai nguồn nói khác nhau, báo cáo **không tự chọn bên đúng**. Phần "Đề xuất" chỉ là gợi ý [ĐỀ XUẤT]; mọi thay đổi ảnh hưởng nghiệp vụ được đưa vào [F. Câu hỏi cần xác nhận](F-cau-hoi-xac-nhan.md).

## 1. Thang mức độ

| Mức | Ý nghĩa |
|---|---|
| **Cao** | Ảnh hưởng tới tính đúng của tiền, tuân thủ pháp lý, hoặc chặn milestone kế tiếp |
| **Trung bình** | Dễ dẫn tới triển khai hoặc kiểm thử sai nếu không làm rõ |
| **Thấp** | Trình bày, ký hiệu, nhất quán nội bộ; không đổi hành vi |

## 2. Tổng hợp

| ID | Loại | Mức | Tóm tắt | Câu hỏi |
|---|---|---|---|---|
| ISS-01 | Mâu thuẫn | **Cao** | Lỗi nghiệp vụ có được lưu theo `Idempotency-Key` hay không · **Đã xử lý** (CHG-04) | Q-01 |
| ISS-02 | Mâu thuẫn | Trung bình | Thứ tự kiểm tra của `createTransfer` (PIN/step-up trước hay sau idempotency) · **Đã xử lý** (CHG-05) | Q-02 |
| ISS-03 | Mâu thuẫn | Trung bình | `back-office-web` làm ở M3 hay M4 | Q-03 |
| ISS-04 | Mâu thuẫn | Thấp | Trạng thái và ngày cập nhật ở đầu tài liệu không khớp lịch sử | Q-04 |
| ISS-05 | Mâu thuẫn | **Cao** | ADR-13 "Monorepo" khác cấu trúc repo thực tế | Q-05 |
| ISS-06 | Mâu thuẫn | Thấp | Phiên bản Go: 1.23+ hay ≥ 1.26 | Q-05 |
| ISS-07 | Mâu thuẫn | Trung bình | Vòng đời ở BR-06 khác hai máy trạng thái trong erd | Q-08 |
| ISS-08 | Mâu thuẫn | Thấp | BR-45 khác erd §6 với giao dịch đồng bộ | Q-08 |
| ISS-09 | Mâu thuẫn | Trung bình | Nhãn "Đã hoàn" (`REVERSED`) không áp dụng cho hoàn tiền merchant | Q-09 |
| ISS-10 | Mâu thuẫn | Thấp | F-23 ↔ US-61, US-62 lệch giữa PRD và URD | Q-21 |
| ISS-11 | Mâu thuẫn | Trung bình | Phạm vi khóa PIN: đăng nhập hay giao dịch | Q-10 |
| ISS-12 | Mâu thuẫn | Thấp | Danh sách "ngoài phạm vi" ở PRD §7 và business-rules §3 khác nhau | Q-22 |
| ISS-13 | Mâu thuẫn | Thấp | Lệch tên gọi / ký hiệu giữa các tài liệu (7 điểm) | Q-25 |
| ISS-14 | Mơ hồ | Trung bình | BR-26: "tổng tối đa 300 tr" tính thế nào | Q-11 |
| ISS-15 | Mơ hồ | Trung bình | BR-28 áp dụng cho loại giao dịch nào | Q-12 |
| ISS-16 | Mơ hồ | **Cao** | BR-36: ngưỡng pháp lý chưa chắc và cách cộng dồn theo nhóm | Q-13 |
| ISS-17 | Mơ hồ | Trung bình | BR-25: chuyển ra TK ngân hàng khác có tính hạn mức không | Q-14 |
| ISS-18 | Mơ hồ | Thấp | BR-11 (TT45), BR-74 (ngưỡng PCRT) mới có nguồn thứ cấp | Q-15 |
| ISS-19 | Mơ hồ | Trung bình | "15 ngày làm việc" (BR-60) chưa có định nghĩa lịch làm việc | Q-16 |
| ISS-20 | Mơ hồ | Thấp | BR-63: thế nào là "phát sinh giao dịch" | Q-17 |
| ISS-21 | Mơ hồ | Trung bình | BR-38: "24 giờ đầu" và "10 tr/ngày" tính thế nào | Q-18 |
| ISS-22 | Mơ hồ | Thấp | Một số yêu cầu phi chức năng chưa đo được | Q-26 |
| ISS-23 | Mơ hồ | Thấp | Mốc tính "PENDING quá 24 giờ": `created_at` hay `pending_since` | Q-27 |
| ISS-24 | Thiếu | **Cao** | `failure-scenarios.md` (TC-xx) chưa viết | Q-24 |
| ISS-25 | Thiếu | **Cao** (cho M1) | 6/9 file FSD và 13/20 UC chưa viết | Q-24 |
| ISS-26 | Thiếu | Trung bình | US-11, US-44 không có tiêu chí chấp nhận | Q-28 |
| ISS-27 | Thiếu | Thấp | Tham số [P] mới trong FSD chưa có ở business-rules §5 | Q-20 |
| ISS-28 | Thiếu | Trung bình | Không có hồ sơ phiên bản / người duyệt / phê duyệt | Q-04 |
| ISS-29 | Thiếu | Thấp | Không có bảng thuật ngữ; `Q&A.md` chỉ có câu hỏi | Q-23 |
| ISS-30 | Thiếu | Thấp | BR-72 (runbook 72 giờ) chưa có artefact, milestone | Q-29 |
| ISS-31 | Thiếu | Thấp | Nguồn gốc thảo luận (link ChatGPT) không được lưu trong repo | — |
| ISS-32 | Trùng lặp | Trung bình | Giá trị tham số được chép lại ở nhiều tài liệu | Q-20 |
| ISS-33 | Trùng lặp | Thấp | Jira draft xếp US-24 vào M3, PRD/FSD tách phần PIN vào M0 | — |
| ISS-34 | Lỗi thời | **Cao** | Tài liệu nằm ở nhánh `master`; nhánh mặc định `main` gần như trống · **Đã xử lý** (CHG-03) | Q-06 |
| ISS-35 | Lỗi thời | Thấp | `wallet-core/CLAUDE.md` còn trỏ `scope.md` · **Đã xử lý** (CHG-07) | — |
| ISS-36 | Lỗi thời | Trung bình | Bản sao `wallet-core/api/openapi.yaml` lệch bản gốc · **Đã xử lý** (CHG-06) | Q-07 |
| ISS-37 | Lệch mã | Trung bình | Mã lỗi trong code khác catalogue `x-error-codes` | Q-19 |
| ISS-38 | Lệch mã | Trung bình | `getPayment` chỉ cho người khởi tạo xem; response khác schema | Q-30 |
| ISS-39 | Lệch mã | Trung bình | `createTransfer` chưa có PIN, hạn mức, khoảng số tiền; tra SĐT mới làm một phần | Q-31 |
| ISS-40 | Lệch mã | Thấp | `GET /v1/wallet` đọc header trực tiếp, `status_reason` trả `""` | — |
| ISS-41 | Lệch mã | **Cao** | `createTransfer` trên `development`: nhánh `recipient.wallet_id` bị mất, dùng mã khách hàng làm mã ví, thiếu `return` | — |

Theo mức: **Cao 7** · Trung bình 17 · Thấp 17. Theo loại: mâu thuẫn 13 · mơ hồ 10 · thiếu 8 · trùng lặp 2 · lỗi thời 3 · lệch mã 5. ISS-41 thêm ngày 2026-10-25.

---

## 3. Mâu thuẫn

### ISS-01 — Lỗi nghiệp vụ có được lưu theo `Idempotency-Key` hay không · Cao · [MÂU THUẪN]

| Nguồn | Nói gì |
|---|---|
| architecture.md §5.2 (sau sequence) | "Bất kỳ bước nào lỗi thì toàn bộ rollback: hạn mức không bị trừ, không có bút toán, **key idempotency không bị chiếm**." |
| fsd/README.md CF-04 bước 5 | Server lưu kết quả cuối gồm `2xx` **và lỗi nghiệp vụ `422`**. AC-7 của UC-04.01: gửi lại cùng key sau `INSUFFICIENT_FUNDS` vẫn nhận `INSUFFICIENT_FUNDS`. |
| erd.md §4.7 (ghi chú `payment.payments`) | "Response lỗi được lưu trong `platform.idempotency_keys` để request lặp lại nhận cùng kết quả." |
| wallet-core.openapi.yaml `info.description` | "trả đúng response đã lưu (kể cả response lỗi nghiệp vụ)" |
| Mã `internal/payment/service.go` (`Transfer`) | Gọi `SaveFailure` để lưu lỗi nghiệp vụ sau khi rollback |

**Ảnh hưởng**: hành vi client khi gửi lại (CF-04) phụ thuộc trực tiếp vào điểm này; đọc architecture.md sẽ hiểu sai.
**Đề xuất** [ĐỀ XUẤT]: 4/5 nguồn (gồm cả mã) thống nhất "lưu lỗi nghiệp vụ". Nếu chủ dự án xác nhận, sửa câu trong architecture §5.2 thành "…key idempotency chỉ lưu lại kết quả lỗi nghiệp vụ, không lưu lỗi hệ thống".

**Trạng thái**: Đã xử lý, 2026-10-21. Chủ dự án chọn Q-01 (a); architecture §5.2 đã sửa (CHG-04).

### ISS-02 — Thứ tự kiểm tra của `createTransfer` · Trung bình · [MÂU THUẪN]

- architecture §5.2 (sequence): `payment/http → authz.Verify` (PIN/step-up) **trước** `BEGIN` và trước bước idempotency.
- fsd/04 UC-04.01 "Xử lý phía hệ thống": (3) idempotency → (4) PIN → (5) khoảng số tiền → (6) step-up → (7) khóa ví → (9) hạn mức. UC-04.02 thêm: cập nhật bộ đếm PIN chạy trong **transaction riêng**.

**Ảnh hưởng**: thứ tự này quyết định lỗi nào trả về khi nhiều điều kiện cùng sai, và việc lần gửi lại có kiểm PIN hay không. FSD ghi rõ "Thứ tự kiểm tra cố định để lỗi trả về nhất quán".
**Đề xuất** [ĐỀ XUẤT]: coi fsd/04 là đặc tả chi tiết; cập nhật sequence ở architecture §5.2 cho khớp, sau khi xác nhận.

**Trạng thái**: Đã xử lý, 2026-10-21. Chủ dự án chọn Q-02 (a); sequence ở architecture §5.2 đã sửa (CHG-05).

### ISS-03 — `back-office-web` làm ở M3 hay M4 · Trung bình · [MÂU THUẪN]

- architecture §3 bảng container: "Kênh vận hành, **làm ở M3**". §11: `back-office-web/ # React + Vite + TS (M3)`.
- prd §9: M4 gồm "back-office React"; F-20 milestone M4; fsd/09: "Ở M0 chưa có giao diện back-office (React, M4)".

### ISS-04 — Trạng thái và ngày cập nhật ở đầu tài liệu không khớp · Thấp · [MÂU THUẪN]

| Tài liệu | Dòng đầu | Bảng docs/README §1 | Lịch sử cuối file |
|---|---|---|---|
| architecture.md | DRAFT — chờ review · 2026-10-17 | Reviewed | 2026-10-18: Review done |
| business-rules.md | REVIEWED · 2026-10-16 | Reviewed | 2026-10-18: Review done |
| erd.md | REVIEWED · 2026-10-17 | Reviewed | 2026-10-18: Review done |

Không thể biết bản nào đúng nếu không hỏi. Liên quan ISS-28.

### ISS-05 — ADR-13 "Monorepo" khác cấu trúc repo thực tế · Cao · [MÂU THUẪN]

- architecture §11 và ADR-13: một monorepo `E-Wallet/` chứa `docs/`, `wallet-core/`, `corebank/`…
- Thực tế: `wallet-core` là repo riêng `Hungnd562k/E-Wallet-Wallet-Core`; `wallet-core/CLAUDE.md` gọi tài liệu ở "sibling repo `../E-Wallet/docs/`". §11 cũng nói `api/openapi.yaml` "chuyển từ docs/api khi scaffold", nhưng hiện có **hai bản** (ISS-36).

**Ảnh hưởng**: lý do của ADR-13 ("đổi contract đồng bộ trong 1 commit") không còn đúng; cần cơ chế đồng bộ contract giữa repo.
**Đề xuất** [ĐỀ XUẤT]: ghi một ADR mới (ví dụ ADR-16 "Polyrepo") thay thế ADR-13 nếu chủ dự án xác nhận đây là quyết định chủ ý.

### ISS-06 — Phiên bản Go · Thấp · [MÂU THUẪN]

architecture §3: `wallet-api` "Go 1.23+". wallet-core/CLAUDE.md: "Go ≥ 1.26 is required (golang.org/x/sync and sqlc need it)".

### ISS-07 — Vòng đời ở BR-06 khác hai máy trạng thái trong erd · Trung bình · [MÂU THUẪN]

- BR-06 mô tả một vòng đời: `REGISTERED → KYC_PENDING → KYC_VERIFIED → ACTIVE`, thêm `SUSPENDED`, `FROZEN`, `CLOSED`, `KYC_REJECTED`, `MANUAL_REVIEW`.
- erd tách thành: `identity.customers.onboarding_status` (`REGISTERED`, `KYC_PENDING`, `KYC_VERIFIED`, `KYC_REJECTED`, `MANUAL_REVIEW`) và `wallet.wallets.status` (`INACTIVE`, `ACTIVE`, `SUSPENDED`, `FROZEN`, `CLOSED`), cộng `kyc.sessions.status`. BR-06 không có `INACTIVE`; erd không có `ACTIVE` cho khách hàng.

**Đề xuất** [ĐỀ XUẤT]: viết lại BR-06 thành hai vòng đời (khách hàng / ví) theo erd. Đây là thay đổi câu chữ của một BR nên cần xác nhận.

### ISS-08 — BR-45 khác erd §6 với giao dịch đồng bộ · Thấp · [MÂU THUẪN]

BR-45: `CREATED → PENDING → SUCCEEDED / FAILED`. erd §6: TRANSFER, QR_PAYMENT, REFUND đi thẳng `— → SUCCEEDED`; `CREATED` chỉ dùng nội bộ. Mã `payment/domain` cho phép `CREATED → SUCCEEDED`. BR-45 đúng với giao dịch qua ngân hàng nhưng thiếu nhánh đồng bộ.

### ISS-09 — Nhãn "Đã hoàn" không áp dụng cho hoàn tiền merchant · Trung bình · [MÂU THUẪN]

- US-25 và CF-03: trạng thái hiển thị "Đã hoàn" = `REVERSED`.
- erd §6: `SUCCEEDED → REVERSED` chỉ khi có **bút toán đảo được duyệt (maker–checker)**. Hoàn tiền của merchant (BR-48, US-31, US-62) tạo **payment `REFUND` mới**, giao dịch QR gốc vẫn `SUCCEEDED`.

**Ảnh hưởng**: khách hàng bị merchant hoàn tiền vẫn thấy giao dịch gốc là "Thành công", trái với kỳ vọng "Đã hoàn" của US-25.

### ISS-10 — F-23 ↔ US-61, US-62 · Thấp · [MÂU THUẪN]

prd §7: F-23 liệt kê `US-60…63`. urd §2 E7: US-61 → F-12, US-62 → F-13 (không có F-23). Kiểm bằng script đối chiếu hai chiều; đây là hai cặp lệch duy nhất trong 23 F × 43 US.

### ISS-11 — Phạm vi khóa PIN · Trung bình · [MÂU THUẪN]

- US-05 (đăng nhập bằng SĐT + PIN): "Sai PIN 5 lần liên tiếp thì bị khóa 30 phút".
- BR-39: "→ **khóa giao dịch** 30 phút".
- UC-04.02: bộ đếm "theo khách hàng, dùng chung cho mọi loại giao dịch và mọi thiết bị"; không nói về đăng nhập.

Chưa rõ: PIN đăng nhập và PIN giao dịch dùng chung bộ đếm không; khóa có chặn đăng nhập không.

### ISS-12 — Danh sách "ngoài phạm vi" khác nhau · Thấp · [MÂU THUẪN]

business-rules §3 có "người nước ngoài" và "chữ ký số, FIDO (loại giao dịch D)"; prd §7 không có. prd §7 có các mục kỹ thuật (Kubernetes, Kafka…) mà business-rules không có (hợp lý). business-rules §1 tự nhận là "nguồn duy nhất" cho phạm vi MVP.

### ISS-13 — Lệch tên gọi / ký hiệu · Thấp · [MÂU THUẪN]

| # | Nguồn A | Nguồn B |
|---|---|---|
| a | BR-57, BR-30: tài khoản `bank_settlement`, `wallet_hold`, `merchant_payable`, `fee_revenue`; architecture §14: `ledger.bank_settlement` | erd §3.1: `SAFEGUARD_BANK`, `WALLET_HOLD`, `MERCHANT_PAYABLE:<id>`, `FEE_REVENUE` |
| b | architecture §5.3: topup trả `202 PENDING + transaction_id`; BR-40 "challenge chứa `transaction_id`" | OpenAPI `Payment.payment_id` (giá trị có prefix `txn_`); erd §4.7 "`transaction_id` public" |
| c | architecture §7.2: `/merchant/v1/orders/{order_id}` | OpenAPI: `/merchant/v1/orders/{merchant_order_id}` |
| d | fsd/README CF-01: "API trả ISO 8601 có offset" | OpenAPI: "Thời gian RFC 3339 (UTC)" |
| e | architecture §9: `CHECK (available_balance >= 0)` | erd §4.4 và migration 0002: `CHECK (allow_negative OR balance >= 0)` |
| f | architecture §9: ví dụ migration `0003_ledger_init.up.sql` | Thực tế: `0002_ledger_init.up.sql` |
| g | architecture §4.1: module `wallet` có vòng đời `ACTIVE, SUSPENDED, FROZEN, CLOSED` | erd/OpenAPI: thêm `INACTIVE` |

Không mục nào đổi hành vi nếu chỉ sửa câu chữ, trừ (a) nếu có code dùng tên cũ.

---

## 4. Mơ hồ

### ISS-14 — BR-26 "tổng tối đa 300.000.000đ/tháng" · Trung bình · [CẦN XÁC NHẬN]

Có thể hiểu (1) GENERAL + ESSENTIAL ≤ 300 tr, hoặc (2) riêng phần ESSENTIAL ≤ 300 tr. erd §4.5 chọn (1) (`GENERAL + ESSENTIAL ≤ limit.monthly_with_essential`). Key `limit.monthly_with_essential` gợi ý (1). Cần xác nhận vì là quy tắc [L].

### ISS-15 — BR-28 áp dụng cho loại giao dịch nào · Trung bình · [CẦN XÁC NHẬN]

BR-28 nói "chuyển/thanh toán". OpenAPI gắn `x-br: BR-28` cho `createTransfer`, `createWithdrawal`, `createInterbankTransfer` (có rút tiền) nhưng **không** cho `payQr` (là thanh toán) và `createTopup`. `WalletLimits.per_transaction.max` không nói áp dụng cho loại nào. Chưa rõ tối đa 50 tr/lần có áp dụng cho nạp và rút không.

### ISS-16 — BR-36: ngưỡng pháp lý và cộng dồn theo nhóm · Cao · [CẦN XÁC NHẬN]

- business-rules §6.1 đã tự ghi: ngưỡng 10 tr / 20 tr theo TT50 gốc, nhưng một số phân tích TT77/2025 nêu 5 tr và 100 tr. Chưa đối chiếu Phụ lục 01–02 bản gốc.
- US-24 AC: "tổng trong ngày > 20 triệu" (không nói "cùng nhóm"); BR-36 và BR-37: cộng dồn **theo nhóm** (a) nạp/rút TK chính chủ, (b) còn lại. FSD dùng `DAY_OTHER` cho chuyển ví–ví.

### ISS-17 — BR-25: chuyển ra TK ngân hàng khác có tính hạn mức không · Trung bình · [CẦN XÁC NHẬN]

Đã được ghi ở business-rules §6.2: "Hiện tại tôi tính vào hạn mức, là cách an toàn hơn". US-23 và OpenAPI `createInterbankTransfer` theo cách này. Vẫn là điểm mở về pháp lý.

### ISS-18 — BR-11 và BR-74 mới có nguồn thứ cấp · Thấp · [CẦN XÁC NHẬN]

business-rules §6.3 (ngưỡng 500 tr theo TT27/2025) và §6.4 (TT45/2025) đã tự ghi là "mới xác nhận qua nguồn thứ cấp".

### ISS-19 — "15 ngày làm việc" · Trung bình · [CẦN XÁC NHẬN]

BR-60, US-50, erd `dispute.disputes.due_at` (+ 15 ngày làm việc), key `dispute.resolve_business_days`. Chưa có định nghĩa ngày làm việc (thứ 7? ngày lễ Việt Nam?) và chưa có bảng lịch nghỉ, nên `due_at` chưa tính được.

### ISS-20 — BR-63: "không phát sinh giao dịch" · Thấp · [CẦN XÁC NHẬN]

Chưa rõ nhận tiền, đăng nhập, xem số dư có tính là "hoạt động" không. Mã hiện tại (`wallet.TouchActivity(src, dst)`) cập nhật `last_activity_at` cho **cả người nhận**.

### ISS-21 — BR-38: "24 giờ đầu" và "10.000.000đ/ngày" · Trung bình · [CẦN XÁC NHẬN]

"Trong 24 giờ đầu… áp hạn mức 10 tr/ngày": 24 giờ trượt từ `bound_at` hay ngày lịch? Cộng dồn loại giao dịch nào (cả nạp/rút TK chính chủ, nhóm (a) BR-37?). Key `auth.new_device_window_hours` gợi ý cửa sổ 24 giờ trượt; cách cộng chưa có.

### ISS-22 — Yêu cầu phi chức năng chưa đo được · Thấp · [CẦN XÁC NHẬN]

- UR-NF-02 "~1 giây" so với NFR-07 "P95 < 300 ms" và SM-03 "< 1 s": ba con số cho cùng loại giao dịch.
- UR-NF-07 "≤ 10 phút, không cần hỗ trợ": chưa có quy trình kiểm thử người dùng.
- NFR-09 "trace xuyên suốt… metrics và alert tối thiểu": chưa có tiêu chí đạt.
- SM-03 "demo trọn vẹn": chưa có kịch bản demo.
- NFR-07: chưa có cấu hình đo (số ví, phân bố số tiền, phần cứng "máy dev").

### ISS-23 — Mốc tính "PENDING quá 24 giờ" · Thấp · [CẦN XÁC NHẬN]

fsd/07 UC-07.04 A2: tính từ `created_at`. erd §4.7: cột `pending_since` "phục vụ BR-46 (24h)". Với giao dịch tạo lệnh ngay ở `PENDING` thì hai mốc trùng nhau; khác nhau nếu sau này có bước `CREATED` kéo dài.

---

## 5. Thiếu

### ISS-24 — `failure-scenarios.md` (TC-xx) chưa viết · Cao · [TBD]

docs/README §1 ghi "Chưa viết". Phụ thuộc: SM-02 (tiêu chí Done của M1), mắt xích cuối của chuỗi truy vết (TC), NFR-03, NFR-08, NFR-11, architecture §12. Không có TC nào trong ma trận [H](H-ma-tran-truy-vet.md).

### ISS-25 — FSD và UC chưa viết · Cao cho M1 · [TBD]

Chưa viết: `fsd/01, 02, 03, 05, 06, 08`; UC-07.05…07; UC-09.03…11. UC-09.02 (đăng nhập back-office, phần API thuộc M0) đã viết ngày 2026-10-25 (CHG-09), nên M0 không còn UC nào chưa viết. **M1 cần** fsd/02 (liên kết), fsd/03 (nạp), fsd/05 (rút, liên ngân hàng), UC-09.06, UC-09.07 (đối soát). Sprint 1 kết thúc 2026-11-01 (jira-backlog-draft).

### ISS-26 — US-11, US-44 không có tiêu chí chấp nhận · Trung bình · [CẦN XÁC NHẬN]

urd §2 không có mục AC cho US-11 (chọn liên kết mặc định) và US-44 (thông báo). 41/43 US còn lại có AC.

### ISS-27 — Tham số [P] mới trong FSD chưa có ở business-rules §5 · Thấp · [CẦN XÁC NHẬN]

`lookup.max_per_hour = 30` (fsd/04 E4) có dạng key cấu hình nhưng không có trong business-rules §5. Các số [P] khác trong FSD không đặt thành key: khoảng lọc lịch sử 12 tháng (fsd/07 E1), khoảng lọc audit 90 ngày (fsd/09 E2), client timeout 15 s và 3 lần thử (CF-06), chờ 1 s tối đa 5 lần (CF-04), 5 người nhận gần đây (fsd/04 A1).

### ISS-28 — Không có hồ sơ phiên bản / người duyệt / phê duyệt · Trung bình · [CẦN XÁC NHẬN]

Tài liệu có trạng thái "REVIEWED" nhưng không ghi người review, phiên bản, hay quyết định phê duyệt. Lần chuẩn hóa này **không tạo** thông tin phê duyệt (xem G, GD-04); chỉ đề xuất khối kiểm soát tài liệu ở [B §4](B-bo-tai-lieu-chuan-hoa.md#4-khối-kiểm-soát-tài-liệu).

### ISS-29 — Không có bảng thuật ngữ · Thấp

`Q&A.md` có 3 câu hỏi (e-KYC, bút toán double-entry, merchant) nhưng chưa có câu trả lời. Persona P1 được mô tả "không hiểu thuật ngữ như đối soát hay hold", nhưng chưa có nơi định nghĩa thuật ngữ thống nhất. Bản nháp bảng thuật ngữ ở [B §6](B-bo-tai-lieu-chuan-hoa.md#6-bảng-thuật-ngữ).

### ISS-30 — BR-72 chưa có artefact · Thấp · [CẦN XÁC NHẬN]

BR-72 yêu cầu "có runbook xử lý sự cố lộ dữ liệu, thông báo trong 72 giờ" nhưng không có F, US, milestone, file runbook hay vị trí dự kiến.

### ISS-31 — Nguồn thảo luận gốc nằm ngoài repo · Thấp

prd §11 trỏ tới một link chia sẻ ChatGPT. Link ngoài có thể mất; nội dung không được lưu trong repo. Lần chuẩn hóa này không truy cập link đó (G, GD-06).

---

## 6. Trùng lặp

### ISS-32 — Giá trị tham số được chép lại ở nhiều nơi · Trung bình

docs/README §3 quy định "Mọi con số… là tham số cấu hình — xem business-rules.md §5" và "Không lặp lại nội dung". Thực tế nhiều giá trị được chép nguyên số: urd AC (OTP 5 phút, 10 tr, 20 tr, 100 tr, 15 phút, 90 ngày…), architecture §6 (24h, 15 phút, 7 ngày), §8.1 (5 phút), erd (5 phút, 30 ngày, +15 phút, +24h), OpenAPI `info.description` (10 tr, 20 tr, 5 phút, 7 ngày). Khi đổi tham số (F-21) dễ sót.

### ISS-33 — US-24 trong Jira draft · Thấp

progression/jira-backlog-draft xếp US-24 vào M3. prd F-07 là "M0 / M3" và UC-04.02 thực hiện "US-24 (phần PIN)" ở M0, còn Sprint 1 dùng T-08 cho PIN. Không sai, nhưng truy vết US-24 ↔ Sprint 1 bị đứt.

---

## 7. Lỗi thời / lệch giữa hai repo

### ISS-34 — Tài liệu nằm ở nhánh `master`, nhánh mặc định là `main` · Cao · [CẦN XÁC NHẬN]

Repo `Hungnd562k/E-Wallet`: nhánh mặc định `main` chỉ có `README.md` (2 dòng) và `.gitignore`. Toàn bộ `docs/` và `progression/` nằm ở nhánh `master` (commit mới nhất `087c3ff`, 2026-10-20). Người clone repo theo mặc định không thấy tài liệu; `wallet-core/CLAUDE.md` mặc định `../E-Wallet/docs/` tồn tại.

**Trạng thái**: Đã xử lý, 2026-10-21. Chủ dự án chọn Q-06: merge vào `main` rồi bỏ `master` (CHG-03). Việc xóa nhánh `master` còn chờ chủ dự án.

### ISS-35 — `wallet-core/CLAUDE.md` còn trỏ `scope.md` · Thấp

CLAUDE.md liệt kê `scope.md: the BR-xx business rules`. File này đã đổi tên thành `business-rules.md` ngày 2026-10-17 (docs/README §4).

**Trạng thái**: Đã xử lý, 2026-10-21 (CHG-07).

### ISS-36 — Bản sao OpenAPI trong wallet-core lệch bản gốc · Trung bình · [CẦN XÁC NHẬN]

So sánh `E-Wallet-Wallet-Core/api/openapi.yaml` với `E-Wallet/docs/api/wallet-core.openapi.yaml` (`diff`), có 4 khác biệt: bản sao còn trỏ `docs/scope.md` (2 chỗ), thiếu tham số lọc `action`, `outcome` và response `403` của `adminListAuditLogs`. CLAUDE.md nói bản trong wallet-core "is a copy"; architecture §11 nói contract "chuyển từ docs/api khi scaffold". Chưa rõ bản nào là nguồn chuẩn.

**Trạng thái**: Đã xử lý, 2026-10-21. Chủ dự án chọn Q-07 (b): `E-Wallet-Wallet-Core/api/openapi.yaml` là nguồn chuẩn (CHG-06).

---

## 8. Lệch giữa tài liệu và mã nguồn (wallet-core `development`)

> Bản đầu (2026-10-21) đối chiếu commit `7332c9e`; bản đầu ghi nhầm commit này thuộc nhánh `main`, thực ra nó thuộc `development` (nhánh mặc định của repo). Cập nhật 2026-10-25 theo `development` commit `9a05355`.

Phần này là **quan sát** để tài liệu và mã không trôi xa nhau, không phải review mã. CLAUDE.md của wallet-core đã tự liệt kê một số "khoảng trống có chủ đích"; các điểm đó vẫn được ghi ở đây để ma trận truy vết phản ánh đúng.

### ISS-37 — Mã lỗi khác catalogue `x-error-codes` · Trung bình · [CẦN XÁC NHẬN]

| Trong mã | Catalogue OpenAPI / fsd §5 |
|---|---|
| `INVALID_REQUEST` (`apperr.Invalid`) | `VALIDATION_ERROR` |
| `UNAUTHORIZED` | `UNAUTHENTICATED` |
| `PAYMENT_NOT_FOUND`, `WALLET_NOT_FOUND`, `LEDGER_ACCOUNT_NOT_FOUND` | `NOT_FOUND` |
| `SAME_WALLET` | `RECIPIENT_INVALID` với `details.reason = SELF` |
| `WALLET_INVALID_TRANSITION`, `CONCURRENT_UPDATE` | `STATE_CONFLICT` |
| `UNSUPPORTED_CURRENCY` | Không có |
| `CUSTOMER_NOT_FOUND` (thêm ở `9a05355`) | `NOT_FOUND` (fsd/04 E2: không phân biệt SĐT chưa có ví và ví không nhận được) |
| `UNVERIFIED_CUSTOMER` (thêm ở `9a05355`, trả `400`) | `404 NOT_FOUND` theo fsd/04 E2, hoặc `422 RECIPIENT_INVALID` (`reason = NOT_ACTIVE`) theo E7 |
| `INVALID_PHONE_NUMBER` (thêm ở `9a05355`) | `VALIDATION_ERROR` |
| `LEDGER_ACCOUNT_CLOSED`, `CURRENCY_MISMATCH`, `LEDGER_UNBALANCED` (lỗi nội bộ của ledger) | Không có |

2026-10-25: `NOT_SUPPORTED_YET` không còn trong mã. Số mã không có trong catalogue tăng từ 11 lên 15.

fsd/README §5 lập thông báo tiếng Việt theo mã của catalogue, nên client sẽ không hiển thị đúng thông báo với các mã trong mã nguồn.

### ISS-38 — `getPayment` chỉ cho người khởi tạo xem · Trung bình

`payment.Service.Get` trả `NOT_FOUND` khi `row.CustomerID != customerID`, nên **người nhận** không xem được giao dịch. fsd/07 UC-07.04 AC-2 yêu cầu người nhận xem được với `direction = IN`. Response hiện có `source_wallet_id`, `dest_wallet_id` (không có trong schema `Payment`) và thiếu `direction`, `counterparty`.

### ISS-39 — `createTransfer` M0 chưa đủ so với fsd/04 · Trung bình

So với 12 bước của UC-04.01: chưa kiểm PIN (bước 4; trường `pin` bị bỏ qua), chưa kiểm khoảng số tiền BR-28 (`AMOUNT_OUT_OF_RANGE`, bước 5; mã chỉ kiểm `amount > 0`), chưa step-up (bước 6), chưa hạn mức (bước 9), chưa có `lookupRecipient`, và `recipient.phone` trả `422 NOT_SUPPORTED_YET`. CLAUDE.md ghi đây là khoảng trống có chủ đích, có TODO tại đúng vị trí. Sprint 1 có T-08 (PIN) và US-42 (hạn mức), nên đây là trạng thái tiến độ hơn là lỗi tài liệu.

**Cập nhật 2026-10-25** (`9a05355`, SCRUM-25): đã có module `identity` với `GetCustomerByPhone`, và `createTransfer` đã nhận `recipient.phone`. Vẫn chưa có PIN, khoảng số tiền, step-up, hạn mức và endpoint `lookupRecipient`. CLAUDE.md mục "Deliberate gaps" vẫn ghi `recipient.phone` trả `422 NOT_SUPPORTED_YET`, nên đã lỗi thời. Lỗi trong phần mới làm được tách ra ISS-41.

### ISS-40 — `GET /v1/wallet` · Thấp

`wallethttp.InquiryBalanceByWalletID` đọc header `X-Dev-Customer-Id` trực tiếp (biến tên `walletId` nhưng giá trị là customer id) thay vì lấy khách hàng từ `auth.CustomerFrom`, nên sẽ không chạy khi đổi sang JWT. `status_reason` trả chuỗi rỗng `""` trong khi schema `Wallet.status_reason` là `string | null`. `held_balance` cố định 0, khớp ghi chú M0 của fsd/07.

### ISS-41 — `createTransfer` trên `development` sai khi xử lý người nhận · Cao

Quan sát ở `internal/payment/http/handler.go` (`createTransfer`, commit `9a05355`). Đây là phần đang làm dở của SCRUM-25, ghi lại vì nó làm lệch hành vi đã đặc tả ở fsd/04 và hỏng luồng chuyển tiền duy nhất đã chạy được:

1. Nhánh `recipient.wallet_id` bị xóa. Khi request chỉ có `wallet_id`, mã ví người nhận là UUID rỗng, nên chuyển ví–ví theo `wallet_id` không còn chạy. OpenAPI và fsd/04 vẫn cho phép cả `wallet_id` và `phone`.
2. Khi chuyển theo SĐT, mã gán cho người nhận là `customer.Id` chứ không phải mã ví. Theo `wallet.wallets`, `id` khác `customer_id` (migration 0003), nên giao dịch sẽ không tìm thấy ví.
3. Sau khi trả `UNVERIFIED_CUSTOMER` thiếu `return`, nên handler vẫn gọi `Transfer` và ghi response lần hai.
4. Mã lỗi khác catalogue và fsd/04 E2 (xem ISS-37).

**Cập nhật 2026-10-25** (`development@85493db`): commit `9a05355` đã được revert (`52ce052`, lý do: phá cấu trúc dependency, unit test lỗi). `createTransfer` quay về trạng thái trước SCRUM-25 (chỉ nhận `recipient.wallet_id`), nên 3 lỗi trên không còn trong mã; US-21 theo SĐT làm lại sau. Khi làm lại, vẫn áp dụng đề xuất dưới đây.

**Đề xuất** [ĐỀ XUẤT]: tra SĐT → khách hàng → ví (qua `wallet`), giữ nhánh `wallet_id`, thêm `return`, dùng `NOT_FOUND` / `RECIPIENT_INVALID` theo fsd/04, và thêm integration test cho cả hai kiểu người nhận. Việc sửa mã thuộc repo wallet-core, không nằm trong lần chuẩn hóa tài liệu.

---

## 9. Kiểm tra đã chạy

| Kiểm tra | Cách làm | Kết quả |
|---|---|---|
| F ↔ US hai chiều | Script đọc bảng prd §7 và urd §2, mở rộng dải `…` | 2 lệch (ISS-10) |
| BR được trích dẫn có tồn tại | Quét mọi `BR-xx` trong 11 file tài liệu và OpenAPI | Không có BR "ma" (chỉ có dải `BR-56…64` vắt qua số không dùng như BR-59, không phải lỗi) |
| Mã lỗi FSD ↔ OpenAPI | So bảng fsd/README §5 với `x-error-codes` (38 mã) | Khớp hoàn toàn |
| Mã lỗi mã nguồn ↔ OpenAPI | Quét `apperr.New(...)` trong `internal/` | 11 mã không có trong catalogue (ISS-37); 15 mã ở `9a05355` (2026-10-25) |
| Key cấu hình FSD ↔ business-rules §5 | Quét key dạng `a.b` | 1 key thiếu (ISS-27) |
| US có AC | Quét mục **US-xx** trong urd §2 | 41/43 (ISS-26) |
| OpenAPI bản gốc ↔ bản sao | `diff` | 4 khác biệt (ISS-36) |
| Lint OpenAPI | **Không chạy lại** trong lần này | — (README ghi "lint sạch") |
