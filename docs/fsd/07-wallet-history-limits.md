# FSD 07 — Ví, hạn mức, lịch sử giao dịch

> Trạng thái: **DRAFT — phần M0** · Cập nhật: 2026-10-18 · Milestone: **M0** (UC-07.01…04), M4 (UC-07.05…07)
> Liên quan: [fsd/README](README.md) (CF-xx, thông báo) · [urd.md](../urd.md) US-25, 40…45 · [prd.md](../prd.md) F-04, F-06, F-14, F-15 · [business-rules.md](../business-rules.md) BR-05, 24…28, 36, 38, 45, 46, 50, 62, 63 · [erd.md](../erd.md) §4.3, §4.5, §4.7, §6

| UC | Tên | Milestone | Trạng thái |
|---|---|---|---|
| [UC-07.01](#uc-0701--xem-ví-và-số-dư) | Xem ví và số dư | M0 | Draft |
| [UC-07.02](#uc-0702--xem-hạn-mức) | Xem hạn mức | M0 | Draft |
| [UC-07.03](#uc-0703--xem-lịch-sử-giao-dịch) | Xem lịch sử giao dịch | M0 | Draft |
| [UC-07.04](#uc-0704--xem-chi-tiết-và-theo-dõi-trạng-thái-giao-dịch) | Xem chi tiết và theo dõi trạng thái giao dịch | M0 | Draft |
| UC-07.05 | Đóng ví theo yêu cầu (US-43, BR-62, `closeWallet`) | M4 | Chưa viết |
| UC-07.06 | Đóng ví do không hoạt động (US-45, BR-63) | M4 | Chưa viết |
| UC-07.07 | Nhận thông báo giao dịch (US-44) | M4 | Chưa viết |

Các UC trong file này đều **chỉ đọc**: không thay đổi tiền, không cần PIN, không cần `Idempotency-Key`. Tiền điều kiện chung: khách hàng đã đăng nhập trên thiết bị đang hoạt động (CF-07). Ví ở **mọi trạng thái**, kể cả `INACTIVE`, `SUSPENDED`, `FROZEN`, đều xem được (BR-07).

---

## UC-07.01 — Xem ví và số dư

| Thuộc tính | Giá trị |
|---|---|
| Actor | Khách hàng |
| Mục tiêu | Biết mình đang có bao nhiêu tiền dùng được và bao nhiêu đang bị giữ. |
| Trigger | Mở app (màn hình chính); kéo để làm mới; quay lại màn hình chính sau một giao dịch. |
| Hậu điều kiện | Không thay đổi dữ liệu. |
| Milestone · US · F | M0 · US-40 · F-04 |

### Luồng chính

| # | Actor | Hệ thống |
|---|---|---|
| 1 | Mở màn hình chính (S-07.1). | Gọi `getWallet`. |
| 2 | | Trả `status`, `status_reason`, `available_balance`, `held_balance`, `total_balance`. |
| 3 | | Hiện số dư khả dụng (to, nổi bật). Nếu `held_balance > 0`, hiện thêm dòng "Đang xử lý: {held_balance}" kèm biểu tượng ⓘ. |
| 4 | Bấm ⓘ. | Giải thích: "Tiền đang được giữ cho giao dịch rút / chuyển ngân hàng chưa hoàn tất. Nếu giao dịch không thành công, tiền sẽ được trả lại ví." (BR-50) |

### Luồng thay thế

| ID | Rẽ tại | Mô tả |
|---|---|---|
| A1 | 3 | Ví không `ACTIVE`: hiện banner trên đầu màn hình theo CF-08; các nút giao dịch (Chuyển tiền, Nạp, Rút, Quét QR) bị làm mờ, bấm vào thì hiện lại thông điệp của banner. Với `INACTIVE` banner có nút **Tiếp tục** dẫn tới bước onboarding còn thiếu (fsd/01). |
| A2 | 3 | Người dùng bấm biểu tượng 👁 để ẩn/hiện số dư. Mặc định **hiện**; lựa chọn lưu trên máy [P]. |

### Luồng ngoại lệ

| ID | Điều kiện | Mã lỗi | Hành vi |
|---|---|---|---|
| E1 | Mất mạng / lỗi hệ thống | — / `5xx` | Hiện số dư lần gần nhất kèm dòng "Cập nhật lúc {HH:mm}" và nút **Thử lại**. Không hiện số 0. |

### Màn hình & trường

**S-07.1 — Màn hình chính (phần ví)**

| Trường | Nguồn | Hiển thị |
|---|---|---|
| Số dư khả dụng | `available_balance` | CF-01 |
| Đang xử lý | `held_balance` | Chỉ hiện khi > 0 |
| Tổng số dư | `total_balance` | Chỉ hiện trong màn chi tiết ví, = khả dụng + đang xử lý (US-40) |
| Banner trạng thái | `status`, `status_reason` | CF-08 |

### Xử lý phía hệ thống

- `available_balance` = số dư tài khoản ledger `WALLET:<id>`; `held_balance` = tổng `ledger.holds` `ACTIVE` của ví. Đọc trực tiếp từ ledger, không cache, để luôn khớp ngay sau giao dịch.
- Ở M0 chưa có rút/chuyển ngân hàng nên `held_balance` luôn = 0.

### Tiêu chí chấp nhận

Từ urd.md **US-40**: tổng = khả dụng + đang giữ.

| # | Given | When | Then |
|---|---|---|---|
| AC-1 | A vừa nhận 200.000 đ (UC-04.01 thành công) | A mở màn hình chính | Số dư đã gồm 200.000 đ (không có độ trễ) |
| AC-2 | Ví `SUSPENDED` / `ID_EXPIRED` | Mở màn hình chính | Thấy số dư và banner "Ví của bạn đang tạm dừng…"; nút giao dịch bị làm mờ |
| AC-3 | Bất kỳ | Gọi `getWallet` | `total_balance = available_balance + held_balance` |

---

## UC-07.02 — Xem hạn mức

| Thuộc tính | Giá trị |
|---|---|
| Actor | Khách hàng |
| Mục tiêu | Biết trong tháng còn được giao dịch bao nhiêu, mỗi lần tối đa bao nhiêu, khi nào phải xác thực thêm. |
| Trigger | Mở **Ví → Hạn mức giao dịch**; hoặc bấm link "Xem hạn mức" trong thông báo `LIMIT_EXCEEDED`. |
| Hậu điều kiện | Không thay đổi dữ liệu. |
| Milestone · US · F | M0 · US-42 · F-06 |

### Luồng chính

| # | Actor | Hệ thống |
|---|---|---|
| 1 | Mở S-07.2. | Gọi `getWalletLimits`. |
| 2 | | Trả hạn mức tháng hiện tại (giờ VN), mức theo lần, ngưỡng xác thực mạnh và tổng hôm nay, hạn chế thiết bị mới (nếu có). |
| 3 | | Hiện các khối theo bảng S-07.2. |

### Màn hình & trường

**S-07.2 — Hạn mức giao dịch**

| Khối | Trường | Nguồn | Hiển thị |
|---|---|---|---|
| Hạn mức tháng {MM/yyyy} | Đã dùng / tổng, còn lại | `month_general.used`, `.limit`, `.remaining` | Thanh tiến độ; ≥ 80% màu cam, 100% màu đỏ |
| | Ghi chú | cố định | "Áp dụng cho chuyển tiền ví–ví, chuyển tới tài khoản ngân hàng khác và thanh toán. Không tính nạp tiền, rút về tài khoản liên kết, nhận tiền và tiền hoàn." (BR-25) |
| | Hạn mức dịch vụ thiết yếu | `month_essential` | Chỉ hiện khi khác `null` (BR-26; MVP chưa có merchant thiết yếu) |
| Mỗi giao dịch | Tối thiểu chuyển / nạp, tối đa | `per_transaction.min_transfer`, `.min_topup`, `.max` | CF-01 |
| Xác thực bằng khuôn mặt | Ngưỡng một lần, ngưỡng ngày, đã giao dịch hôm nay | `strong_auth.single_threshold`, `.daily_threshold`, `.today_other_total` | "Giao dịch trên {single_threshold} hoặc tổng trong ngày trên {daily_threshold} cần xác thực OTP và khuôn mặt." (BR-36) |
| Thiết bị mới | Hạn mức ngày, đến khi | `new_device_restriction.daily_limit`, `.until` | Chỉ hiện khi khác `null` (BR-38) |
| Kỳ hạn mức | | `period` | "Hạn mức tháng được làm mới lúc 00:00 ngày 1 hằng tháng (giờ Việt Nam)." (BR-27) |

`today_own_account_total` (nhóm nạp/rút với TK chính chủ, BR-37) không hiển thị cho khách hàng ở MVP [P]; chỉ dùng nội bộ.

### Xử lý phía hệ thống

- `period` = tháng hiện tại theo `limit.timezone`. Nếu chưa có dòng `limits.usage` cho kỳ này thì `used = 0` (không tạo dòng khi chỉ đọc).
- `limit` lấy từ tham số cấu hình hiện hành (`limit.monthly_general`…); đổi tham số (F-21, M4) có hiệu lực ngay với giao dịch tiếp theo, không hồi tố.
- Giao dịch thất bại hoặc bị đảo được **hoàn lại** hạn mức (BR-29) qua `limits.consumptions.reverted_at`; giao dịch bị đảo ở tháng sau thì hoàn vào **kỳ gốc** của giao dịch, không vào kỳ hiện tại [P].
- `remaining = max(0, limit − used)`.

### Tiêu chí chấp nhận

Từ urd.md **US-42**: thấy đã dùng / còn lại trong tháng hiện tại; thấy ngưỡng cần xác thực mạnh.

| # | Given | When | Then |
|---|---|---|---|
| AC-1 | A chuyển 3 lần tổng 1.500.000 đ trong tháng 10 | Xem hạn mức | `used = 1.500.000`, `remaining = 98.500.000` |
| AC-2 | Giao dịch cuối lúc 23:59 ngày 31/10 giờ VN | Xem lúc 00:01 ngày 01/11 giờ VN | `period = 2026-11`, `used = 0` |
| AC-3 | A nhận 5.000.000 đ từ B | Xem hạn mức | `used` của A không đổi |

---

## UC-07.03 — Xem lịch sử giao dịch

| Thuộc tính | Giá trị |
|---|---|
| Actor | Khách hàng |
| Mục tiêu | Xem lại các giao dịch tiền vào / ra, lọc theo loại và thời gian. |
| Trigger | Mở tab **Lịch sử**; hoặc bấm **Xem tất cả** dưới danh sách giao dịch gần đây ở màn hình chính. |
| Hậu điều kiện | Không thay đổi dữ liệu. |
| Milestone · US · F | M0 · US-41 · F-14 |

### Luồng chính

| # | Actor | Hệ thống |
|---|---|---|
| 1 | Mở S-07.3. | Gọi `listWalletActivities` không có bộ lọc, `limit = 20`. |
| 2 | | Trả danh sách mới nhất trước và `next_cursor`. Hiện danh sách, nhóm theo ngày ("Hôm nay", "Hôm qua", `dd/MM/yyyy`). |
| 3 | Cuộn tới cuối. | Nếu `next_cursor` khác `null`: gọi tiếp với `cursor`; nối vào danh sách. |
| 4 | Bấm một dòng. | Mở UC-07.04 với `payment_id`. |

### Luồng thay thế

| ID | Rẽ tại | Mô tả |
|---|---|---|
| A1 | 1 | Chọn bộ lọc (S-07.3a): loại, chiều (tiền vào / tiền ra), khoảng thời gian. Áp dụng → gọi lại từ đầu, bỏ `cursor` cũ. |
| A2 | 2 | Không có giao dịch: empty state "Chưa có giao dịch nào." (không lọc) hoặc "Không có giao dịch phù hợp." + nút **Xóa bộ lọc** (có lọc). |
| A3 | 2 | Kéo xuống để làm mới: gọi lại từ đầu. |

### Luồng ngoại lệ

| ID | Điều kiện | Mã lỗi | Hành vi |
|---|---|---|---|
| E1 | `from > to`, hoặc khoảng > 12 tháng [P] | `400 VALIDATION_ERROR` | Client chặn trước: "Khoảng thời gian tối đa 12 tháng." |
| E2 | `cursor` không hợp lệ / hết hạn | `400 VALIDATION_ERROR` | Gọi lại từ đầu |
| E3 | Mất mạng khi tải trang tiếp | — | Dòng cuối "Không tải được. Thử lại", giữ danh sách đã có |

### Màn hình & trường

**S-07.3 — Lịch sử (mỗi dòng)**

| Trường | Nguồn | Hiển thị |
|---|---|---|
| Biểu tượng + tên loại | `type`, `direction` | Bảng nhãn bên dưới |
| Đối tác | `counterparty_display` | Đã che (CF-02) |
| Số tiền | `amount`, `direction` | `+200.000 đ` (xanh) với IN, `−200.000 đ` (mặc định) với OUT |
| Trạng thái | `status` | CF-03; chỉ hiện nhãn khi khác `SUCCEEDED` để danh sách gọn [P] |
| Giờ | `occurred_at` | `HH:mm` (CF-01) |

Nhãn loại giao dịch:

| `type` | OUT | IN |
|---|---|---|
| `TRANSFER` | Chuyển tiền tới {đối tác} | Nhận tiền từ {đối tác} |
| `TOPUP` | — | Nạp tiền từ {ngân hàng} |
| `WITHDRAW` | Rút về {ngân hàng} | — |
| `INTERBANK` | Chuyển tới {ngân hàng} | — |
| `QR_PAYMENT` | Thanh toán {merchant} | — |
| `REFUND` | — | Hoàn tiền từ {merchant} |
| `ADJUSTMENT` | Điều chỉnh | Điều chỉnh |

**S-07.3a — Bộ lọc**

| Trường | Kiểu | Giá trị | Map API |
|---|---|---|---|
| Loại | chọn một | Tất cả · Chuyển tiền · Nạp · Rút · Chuyển ngân hàng · Thanh toán · Hoàn tiền | `type` |
| Chiều | chọn một | Tất cả · Tiền vào · Tiền ra | `direction` |
| Thời gian | chọn một | 7 ngày · 30 ngày · Tháng này · Tháng trước · Tùy chọn (chọn ngày đầu–cuối) | `from` = 00:00 ngày đầu, `to` = 00:00 ngày sau ngày cuối (giờ VN) |

### Xử lý phía hệ thống

- Đọc từ read-model `payment.wallet_activities` của ví khách hàng, phân trang **keyset** theo `(occurred_at DESC, id DESC)` (erd §4.7); không dùng `OFFSET`.
- Mỗi giao dịch có 1 dòng **cho mỗi ví liên quan**: chuyển ví–ví tạo 2 dòng (OUT ở ví gửi, IN ở ví nhận); mỗi người chỉ thấy dòng của mình.
- `status` của dòng được cập nhật cùng transaction với payment, nên lịch sử luôn khớp màn chi tiết.
- `limit` tối đa 100 (schema `Limit`).

### Tiêu chí chấp nhận

Từ urd.md **US-41**: phân trang, mới nhất trước; mỗi dòng có loại, đối tác (đã che), số tiền, trạng thái.

| # | Given | When | Then |
|---|---|---|---|
| AC-1 | A chuyển cho B 1 lần | A và B xem lịch sử | A thấy 1 dòng OUT, B thấy 1 dòng IN, cùng `payment_id` |
| AC-2 | A có 45 giao dịch | Cuộn hết danh sách | Gọi 3 trang (20, 20, 5); trang cuối `next_cursor = null`; không trùng, không sót |
| AC-3 | Có giao dịch mới trong lúc đang cuộn | Tải trang tiếp | Không bị lặp dòng ở trang sau (tính chất keyset) |
| AC-4 | Lọc "Tiền vào", "Tháng này" | Áp dụng | Chỉ có dòng IN có `occurred_at` trong tháng hiện tại giờ VN |

---

## UC-07.04 — Xem chi tiết và theo dõi trạng thái giao dịch

| Thuộc tính | Giá trị |
|---|---|
| Actor | Khách hàng |
| Mục tiêu | Biết rõ một giao dịch đang xử lý, thành công, thất bại hay đã hoàn, và tiền đang ở đâu. |
| Trigger | Bấm một dòng ở lịch sử (UC-07.03); màn kết quả sau khi tạo giao dịch (S-04.4 và tương tự); mở từ thông báo (M4). |
| Hậu điều kiện | Không thay đổi dữ liệu. |
| Milestone · US · F | M0 · US-25 · F-14 |

### Luồng chính

| # | Actor | Hệ thống |
|---|---|---|
| 1 | Mở S-07.4 với `payment_id`. | Gọi `getPayment`. |
| 2 | | Trả giao dịch theo **góc nhìn người đang xem** (`direction`, `counterparty`). |
| 3 | | Hiện trạng thái (CF-03) và các trường theo bảng S-07.4. |

### Luồng thay thế

| ID | Rẽ tại | Mô tả |
|---|---|---|
| A1 | 3 | `status = PENDING`: hiện dòng giải thích theo loại (bảng dưới) và **tự cập nhật**: gọi lại `getPayment` sau 2s, 4s, 8s, 16s, rồi mỗi 30s; dừng khi trạng thái khác `PENDING` hoặc rời màn hình. Ở M0 không có giao dịch `PENDING` (chỉ có chuyển ví–ví), luồng này dùng từ M1. |
| A2 | 3 | `PENDING` quá `payment.pending_max_hours` (tính từ `created_at`): hiện thêm gợi ý tra soát và nút **Gửi tra soát** (CF-03; fsd/08, M4). |
| A3 | 3 | `FAILED`: hiện lý do theo `failure_code` (bảng dưới) và câu "Tiền chưa bị trừ khỏi ví của bạn." hoặc "Tiền đã được trả lại ví của bạn." tùy loại. |
| A4 | 3 | `REVERSED`: hiện "Giao dịch đã được hoàn lại vào ví." và liên kết tới giao dịch điều chỉnh / hoàn tiền nếu có. |
| A5 | 3 | Bấm **Sao chép mã giao dịch** → copy `payment_id`, toast "Đã sao chép". |

Giải thích khi `PENDING`:

| `type` | Thông điệp |
|---|---|
| `TOPUP` | Ngân hàng đang xử lý. Tiền sẽ vào ví ngay khi ngân hàng xác nhận. |
| `WITHDRAW`, `INTERBANK` | Ngân hàng đang xử lý. Số tiền đang được giữ và sẽ được trả lại ví nếu giao dịch không thành công. |

Lý do thất bại (`failure_code` → thông điệp). Bảng bổ sung dần theo từng milestone:

| `failure_code` | Thông điệp | Milestone |
|---|---|---|
| `BANK_REJECTED_INSUFFICIENT_FUNDS` | Tài khoản ngân hàng không đủ số dư. | M1 |
| `BANK_REJECTED_LINK_REVOKED` | Liên kết ngân hàng không còn hiệu lực. Vui lòng liên kết lại. | M1 |
| (khác / không có) | Giao dịch không thành công. Vui lòng thử lại hoặc liên hệ CSKH. | — |

### Luồng ngoại lệ

| ID | Điều kiện | Mã lỗi | Hành vi |
|---|---|---|---|
| E1 | `payment_id` không tồn tại **hoặc** không liên quan tới ví của người xem | `404 NOT_FOUND` | "Không tìm thấy giao dịch." Trả 404 thay vì 403 để không lộ sự tồn tại của giao dịch người khác. |

### Màn hình & trường

**S-07.4 — Chi tiết giao dịch**

| Trường | Nguồn | Ghi chú |
|---|---|---|
| Trạng thái | `status` | CF-03, biểu tượng lớn trên đầu |
| Số tiền | `amount`, `direction` | Dấu `+`/`−` như UC-07.03 |
| Loại | `type` | Nhãn như UC-07.03 |
| Đối tác | `counterparty.display_name`, `.reference` | Đã che |
| Nội dung | `description` | Ẩn nếu trống |
| Phí | `fee` | Luôn `0 đ` trong MVP (BR-30) |
| Thời gian tạo / hoàn tất | `created_at`, `completed_at` | CF-01 |
| Mã giao dịch | `payment_id` | Có nút sao chép |
| Lý do thất bại | `failure_code`, `failure_message` | Chỉ khi `FAILED` |
| Giao dịch gốc | `refund_of` | Chỉ với `REFUND` (M2) |

### Xử lý phía hệ thống

- Khách hàng được xem payment nếu ví của họ là `source_wallet_id` hoặc `dest_wallet_id`.
- `NEEDS_RECONCILIATION` trả ra là `PENDING` (CF-03); `CREATED` không bao giờ trả ra.
- Với người nhận, `description` vẫn hiện (nội dung chuyển khoản là thông tin dành cho người nhận); `counterparty` là người gửi đã che.

### Tiêu chí chấp nhận

Từ urd.md **US-25**: chỉ 4 trạng thái hiển thị; thất bại có lý do dễ hiểu; đang xử lý quá 24 giờ thì hướng dẫn tra soát.

| # | Given | When | Then |
|---|---|---|---|
| AC-1 | Payment `TRANSFER` A→B | A gọi `getPayment` | `direction = OUT`, `counterparty` = B đã che |
| AC-2 | Như trên | B gọi `getPayment` | `direction = IN`, `counterparty` = A đã che |
| AC-3 | Như trên | C (không liên quan) gọi | `404 NOT_FOUND` |
| AC-4 | Payment nội bộ `NEEDS_RECONCILIATION` (M1) | Khách hàng xem | `status = PENDING`, có gợi ý tra soát |
