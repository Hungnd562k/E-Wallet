# FSD — Đặc tả chức năng

> Trạng thái: **DRAFT — phạm vi M0** · Cập nhật: 2026-10-18
> Liên quan: [README](../README.md) · [urd.md](../urd.md) (`US-xx`) · [prd.md](../prd.md) (`F-xx`) · [business-rules.md](../business-rules.md) (`BR-xx`) · [erd.md](../erd.md) · [api/wallet-core.openapi.yaml](../api/wallet-core.openapi.yaml)

FSD mô tả **từng chức năng chạy chi tiết ra sao**: các bước, màn hình, trường dữ liệu, kiểm tra, thông báo, trạng thái và dữ liệu thay đổi. FSD **không** lặp lại:

- *vì sao* có chức năng → PRD, URD;
- *nội dung* quy tắc nghiệp vụ → business-rules.md (FSD chỉ trỏ `BR-xx` và nói quy tắc được kiểm tra **ở bước nào**);
- *chi tiết kỹ thuật* (schema request/response, cột DB) → OpenAPI, erd.md.

Ứng với milestone M0, hệ thống chỉ giao **API**. Màn hình app vẫn được đặc tả ở đây vì đó là yêu cầu chức năng; app Flutter làm ở M4.

---

## 1. Danh sách file

| File | Phạm vi | Milestone | Trạng thái |
|---|---|---|---|
| `01-onboarding.md` | Đăng ký, PIN, consent, eKYC, thiết bị mới, step-up | M3 | Chưa viết |
| `02-bank-link.md` | Liên kết / hủy liên kết ngân hàng | M1 | Chưa viết |
| `03-topup.md` | Nạp tiền | M1 | Chưa viết |
| [04-p2p-transfer.md](04-p2p-transfer.md) | Chuyển ví–ví, xác thực PIN | **M0** | Draft |
| `05-withdraw-interbank.md` | Rút tiền, chuyển liên ngân hàng | M1 | Chưa viết |
| `06-qr-payment-refund.md` | Thanh toán QR, hoàn tiền, merchant API | M2 | Chưa viết |
| [07-wallet-history-limits.md](07-wallet-history-limits.md) | Ví, hạn mức, lịch sử, trạng thái giao dịch (đóng ví, thông báo: M4) | **M0** / M4 | Draft (phần M0) |
| `08-disputes.md` | Tra soát, khiếu nại | M4 | Chưa viết |
| [09-backoffice-ops.md](09-backoffice-ops.md) | Audit log (M0); đối soát, duyệt KYC, phong tỏa, điều chỉnh, AML, cấu hình | **M0** / M1 / M4 | Draft (phần M0) |

## 2. Quy ước

| Loại | Định dạng | Ví dụ |
|---|---|---|
| Use case | `UC-<số file>.<nn>` | `UC-04.01` — UC đầu tiên trong `04-p2p-transfer.md` |
| Màn hình | `S-<số file>.<n>` | `S-04.3` — màn xác nhận chuyển tiền |
| Luồng thay thế / ngoại lệ | `A<n>` / `E<n>` trong UC | `UC-04.01/E3` |
| Hành vi dùng chung | `CF-xx` | §4 file này |
| Thông báo | Theo mã lỗi API | `INSUFFICIENT_FUNDS` — §5 file này |

- Đánh số UC không đổi khi thêm UC mới (thêm vào cuối file).
- Luồng ngoại lệ ghi: **điều kiện → mã lỗi API → thông báo** (thông báo lấy từ §5, chỉ ghi lại nếu khác).
- Giá trị số (hạn mức, ngưỡng, TTL) ghi kèm tên tham số cấu hình ở business-rules.md §5, ví dụ `txn.max_amount_per_txn`.
- Nhãn **[P]** đánh dấu quyết định sản phẩm mới đưa ra trong FSD (chưa có ở BR), cần review.

## 3. Template use case

```markdown
### UC-xx.yy — <Tên>

| Thuộc tính | Giá trị |
|---|---|
| Actor | |
| Mục tiêu | |
| Trigger | |
| Tiền điều kiện | |
| Hậu điều kiện — thành công | |
| Hậu điều kiện — thất bại | |
| Milestone · US · F | |

**Luồng chính** — các bước đánh số, actor ↔ hệ thống.
**Luồng thay thế** — A1, A2… (rẽ nhánh ở bước nào, quay lại bước nào).
**Luồng ngoại lệ** — E1, E2… bảng: điều kiện · bước · mã lỗi API · thông báo / hành vi.
**Màn hình & trường** — S-xx.n: bảng trường · kiểu · bắt buộc · kiểm tra · nguồn dữ liệu.
**Xử lý phía hệ thống** — thứ tự kiểm tra, BR áp dụng ở bước nào.
**Dữ liệu** — API (`operationId`) · bảng ERD · chuyển trạng thái (erd §6) · bút toán (erd §3.2) · event outbox · audit.
**Sequence** — chỉ với luồng tiền.
**Tiêu chí chấp nhận** — trỏ AC của US trong urd.md + AC bổ sung ở mức hệ thống.
```

## 4. Hành vi dùng chung (CF)

Áp dụng cho mọi UC; các UC chỉ ghi phần khác biệt.

### CF-01 — Tiền và thời gian

- Tiền là số nguyên VND, không có phần lẻ (BR-02). Hiển thị: dấu chấm phân cách hàng nghìn + ` đ`, ví dụ `1.250.000 đ`. Ô nhập tiền tự chèn dấu chấm khi gõ, chỉ nhận chữ số.
- Thời gian hiển thị theo giờ `Asia/Ho_Chi_Minh` (BR-27), dạng `HH:mm dd/MM/yyyy`. API trả ISO 8601 có offset.
- "Hôm nay", "tháng này" luôn tính theo giờ VN, không theo giờ máy người dùng.

### CF-02 — Che dữ liệu

Do backend che; client không nhận bản đầy đủ (BR-71, UR-NF-05).

| Dữ liệu | Quy tắc | Ví dụ |
|---|---|---|
| Họ tên người khác | Giữ nguyên họ; tên đệm giữ chữ đầu + `***`; tên giữ chữ đầu | `NGUYEN VAN BINH` → `NGUYEN V*** B` |
| SĐT người khác | Giữ `+84` + 2 số đầu + 3 số cuối | `+84901234567` → `+8490****567` |
| Số TK / thẻ | Chỉ 4 số cuối | `****1234` |
| Số CCCD | 3 số đầu + 3 số cuối | `079*******123` |

Thông tin của **chính** khách hàng (tên, SĐT) hiển thị đầy đủ.

### CF-03 — Trạng thái giao dịch hiển thị

| Trạng thái nội bộ (erd §6) | API `PaymentStatus` | Nhãn | Màu |
|---|---|---|---|
| `CREATED` | — (không lộ ra) | — | — |
| `PENDING`, `NEEDS_RECONCILIATION` | `PENDING` | Đang xử lý | Vàng |
| `SUCCEEDED` | `SUCCEEDED` | Thành công | Xanh lá |
| `FAILED` | `FAILED` | Thất bại | Đỏ |
| `REVERSED` | `REVERSED` | Đã hoàn | Xám |

- Không bao giờ hiện "Thất bại" khi hệ thống chưa chắc chắn (BR-46, UR-NF-03).
- `PENDING` quá `payment.pending_max_hours` (24h) → dưới trạng thái hiện thêm: "Giao dịch đang được kiểm tra. Bạn có thể gửi yêu cầu tra soát." + nút **Gửi tra soát** (fsd/08, M4).

### CF-04 — Chống gửi trùng (idempotency)

Áp dụng cho mọi lệnh tạo giao dịch (NFR-02, UR-NF-04).

1. Client sinh `Idempotency-Key` (UUID v4) **khi mở màn xác nhận** và giữ nguyên key đó cho tới khi rời màn.
2. Bấm xác nhận lần 2, mất mạng rồi gửi lại, hoặc gửi lại sau step-up: **dùng lại key cũ**.
3. Sửa số tiền / người nhận (quay lại màn trước) → sinh key mới.
4. Phản hồi:

| Tình huống | Server trả | Client làm |
|---|---|---|
| Key mới | Kết quả xử lý | Hiện kết quả |
| Key đã xử lý xong, cùng nội dung | Đúng response đã lưu | Hiện kết quả như lần đầu |
| Key đang được xử lý | `409 IDEMPOTENCY_IN_PROGRESS` | Chờ 1s rồi gửi lại, tối đa 5 lần, giữ màn "Đang xử lý" |
| Key đã dùng với nội dung khác | `422 IDEMPOTENCY_KEY_REUSED` | Lỗi lập trình phía client; hiện thông báo chung, sinh key mới |

5. Server chỉ **lưu lại kết quả cuối** gắn với key: `2xx` và lỗi nghiệp vụ `422` (thiếu số dư, vượt hạn mức…, erd §4.7 ghi chú). Các phản hồi **không** chiếm key, gửi lại cùng key sẽ được xử lý lại: `401` (`PIN_INVALID`, `STEP_UP_REQUIRED`), `423 PIN_LOCKED`, `409`, `429`, `5xx`. Nhờ vậy người dùng nhập lại PIN đúng hoặc hoàn tất step-up mà không cần key mới.
6. PIN **không** thuộc nội dung so sánh (nhập lại PIN khác không bị coi là nội dung khác).

### CF-05 — Xác thực bổ sung (step-up)

Khi giao dịch vượt ngưỡng BR-36 (hoặc thiết bị mới, BR-38), server trả `401 STEP_UP_REQUIRED` kèm `challenge_id`, `methods`, `expires_at` (architecture §7.3).

1. Client chuyển sang màn xác thực: nhập OTP rồi chụp khuôn mặt (`verifyChallengeOtp`, `verifyChallengeFace`).
2. Xong thì gửi lại **đúng lệnh cũ, cùng `Idempotency-Key`**, thêm header `X-Challenge-Id`.
3. Người dùng hủy hoặc challenge hết hạn (`auth.challenge_ttl_seconds` = 300) → giao dịch **không** được thực hiện; quay về màn xác nhận (US-24).

Đặc tả màn hình OTP / khuôn mặt nằm ở fsd/01 (M3).

> **Giới hạn ở M0** [P]: server đã kiểm ngưỡng và trả `STEP_UP_REQUIRED`, nhưng các endpoint hoàn tất challenge làm ở M3. Vì vậy ở M0, giao dịch vượt ngưỡng **không thể hoàn tất**. Test tải SM-01 dùng số tiền dưới `auth.strong_single_threshold` và tổng ngày dưới `auth.strong_daily_threshold`.

### CF-06 — Lỗi mạng, timeout, lỗi hệ thống

- Client timeout 15s cho lệnh tạo giao dịch. Hết thời gian hoặc mất mạng → **không** báo thất bại; hiện "Đang xử lý", gửi lại cùng key (CF-04). Sau 3 lần vẫn không có phản hồi → mở màn chi tiết giao dịch nếu đã biết `payment_id`, nếu chưa thì hướng dẫn kiểm tra lịch sử.
- `5xx`: "Hệ thống đang bận, vui lòng thử lại sau ít phút." Không tự gửi lại lệnh tạo giao dịch khi đã nhận `5xx` có thân lỗi (server đã chắc chắn rollback); người dùng bấm thử lại → cùng key.
- `429 RATE_LIMITED`: tôn trọng header `Retry-After`.

### CF-07 — Phiên đăng nhập

- `401 UNAUTHENTICATED` → client dùng refresh token lấy access token mới rồi gửi lại 1 lần; vẫn lỗi → về màn đăng nhập (fsd/01).
- `401 DEVICE_NOT_BOUND` → phiên trên máy này đã bị thu hồi (đăng nhập máy khác, BR-38) → về màn đăng nhập, thông báo §5.

## 5. Danh mục thông báo

Thông báo tiếng Việt cho các mã lỗi trong `x-error-codes` của [wallet-core.openapi.yaml](../api/wallet-core.openapi.yaml). Mỗi thông báo nói người dùng **làm gì tiếp** (UR-NF-06). `{…}` lấy từ `details` của response.

Bảng dưới gồm các mã dùng ở M0; mã khác bổ sung khi viết file tương ứng.

| Mã lỗi | HTTP | Thông báo | Hành động trên UI |
|---|---|---|---|
| `VALIDATION_ERROR` | 400 | Thông tin chưa hợp lệ, vui lòng kiểm tra lại. | Đánh dấu trường lỗi theo `details.fields` |
| `UNAUTHENTICATED` | 401 | Phiên đăng nhập đã hết hạn, vui lòng đăng nhập lại. | CF-07 |
| `DEVICE_NOT_BOUND` | 401 | Tài khoản đã đăng nhập trên thiết bị khác. Vui lòng đăng nhập lại. | Về màn đăng nhập |
| `PIN_INVALID` | 401 | Mã PIN không đúng. Bạn còn {remaining_attempts} lần thử. | Xóa ô PIN, cho nhập lại |
| `PIN_LOCKED` | 423 | Bạn đã nhập sai PIN quá số lần cho phép. Giao dịch bị tạm khóa đến {locked_until}. | Đóng bàn phím PIN; nút **Quên PIN** |
| `PIN_LOCKED` (`details.hard_locked = true`) | 423 | Mã PIN đã bị khóa. Vui lòng đặt lại PIN để tiếp tục giao dịch. | Nút **Đặt lại PIN** (fsd/01) |
| `STEP_UP_REQUIRED` | 401 | Giao dịch cần xác thực thêm bằng OTP và khuôn mặt. | CF-05 |
| `FORBIDDEN` | 403 | Bạn không có quyền thực hiện thao tác này. | — |
| `ONBOARDING_INCOMPLETE` | 403 | Bạn cần hoàn tất định danh và liên kết ngân hàng để giao dịch. | Nút **Tiếp tục** → bước onboarding còn thiếu |
| `WALLET_NOT_ACTIVE` | 403 | Ví của bạn đang {wallet_status_label}, chưa thể giao dịch. {reason_hint} | Xem CF-08 cho nhãn và gợi ý |
| `NOT_FOUND` | 404 | Không tìm thấy thông tin. | — |
| `IDEMPOTENCY_IN_PROGRESS` | 409 | (không hiện; giữ màn "Đang xử lý") | CF-04 |
| `IDEMPOTENCY_KEY_REUSED` | 422 | Đã có lỗi xảy ra, vui lòng thực hiện lại giao dịch. | CF-04 |
| `INSUFFICIENT_FUNDS` | 422 | Số dư không đủ. Số dư khả dụng: {available}. | Nút **Nạp tiền** (M1) |
| `LIMIT_EXCEEDED` (`limit_type = MONTH_GENERAL`) | 422 | Giao dịch vượt hạn mức tháng. Bạn còn có thể giao dịch {remaining} trong tháng này. | Gợi ý nhập số tiền ≤ còn lại |
| `LIMIT_EXCEEDED` (`limit_type = NEW_DEVICE_DAILY`) | 422 | Trong 24 giờ đầu trên thiết bị mới, bạn chỉ được giao dịch tối đa {limit}/ngày. Còn lại hôm nay: {remaining}. | — |
| `AMOUNT_OUT_OF_RANGE` | 422 | Số tiền mỗi lần phải từ {min} đến {max}. | Đánh dấu ô số tiền |
| `RECIPIENT_INVALID` (`reason = SELF`) | 422 | Bạn không thể chuyển tiền cho chính mình. | Quay lại ô người nhận |
| `RECIPIENT_INVALID` (`reason = NOT_ACTIVE`) | 422 | Ví người nhận hiện không thể nhận tiền. | Quay lại ô người nhận |
| `RATE_LIMITED` | 429 | Bạn thao tác quá nhanh, vui lòng thử lại sau {retry_after} giây. | Khóa nút trong `Retry-After` |
| `INTERNAL_ERROR` | 500 | Hệ thống đang bận, vui lòng thử lại sau ít phút. | CF-06 |
| `SERVICE_UNAVAILABLE` | 503 | Hệ thống đang bảo trì, vui lòng thử lại sau. | CF-06 |

### CF-08 — Nhãn trạng thái ví

| `status` / `status_reason` | Nhãn | Gợi ý |
|---|---|---|
| `INACTIVE` | chưa kích hoạt | Hoàn tất định danh và liên kết ngân hàng để bắt đầu. |
| `SUSPENDED` / `ID_EXPIRED` | tạm dừng | CCCD đã hết hạn. Vui lòng định danh lại. |
| `SUSPENDED` / `FRAUD_SUSPECTED`, `RECON_MISMATCH` | tạm dừng | Vui lòng liên hệ CSKH để được hỗ trợ. |
| `FROZEN` / `CUSTOMER_REQUEST` | đang bị khóa theo yêu cầu của bạn | Liên hệ CSKH để mở khóa. |
| `FROZEN` / `AUTHORITY_REQUEST` | đang bị phong tỏa | Vui lòng liên hệ CSKH. |
| `CLOSED` | đã đóng | — |

Không hiển thị cho khách hàng lý do chi tiết của `FRAUD_SUSPECTED` (tránh lộ tiêu chí phát hiện gian lận).

## 6. Danh sách use case

| UC | Tên | File | Milestone | Trạng thái |
|---|---|---|---|---|
| UC-04.01 | Chuyển tiền ví–ví bằng SĐT | 04 | M0 | Draft |
| UC-04.02 | Xác thực giao dịch bằng PIN | 04 | M0 | Draft |
| UC-07.01 | Xem ví và số dư | 07 | M0 | Draft |
| UC-07.02 | Xem hạn mức | 07 | M0 | Draft |
| UC-07.03 | Xem lịch sử giao dịch | 07 | M0 | Draft |
| UC-07.04 | Xem chi tiết và theo dõi trạng thái giao dịch | 07 | M0 | Draft |
| UC-07.05 | Đóng ví theo yêu cầu | 07 | M4 | Chưa viết |
| UC-07.06 | Đóng ví do không hoạt động | 07 | M4 | Chưa viết |
| UC-07.07 | Nhận thông báo giao dịch | 07 | M4 | Chưa viết |
| UC-09.01 | Tra cứu audit log | 09 | M0 | Draft |

## 7. Ma trận truy vết US ↔ UC ↔ BR ↔ API

Chỉ gồm các UC đã viết. Cập nhật khi thêm file.

| US | UC | BR chính | API (`operationId`) | Dữ liệu chính (erd) |
|---|---|---|---|---|
| US-21 | UC-04.01 | BR-23…29, 35, 36, 47 | `lookupRecipient`, `createTransfer` | `payment.payments`, `ledger.*`, `limits.*`, `platform.idempotency_keys`, `platform.outbox_events` |
| US-24 (phần PIN) | UC-04.02 | BR-35, 39 | (trong mọi lệnh tạo giao dịch) | `identity.credentials` |
| US-40 | UC-07.01 | BR-05, 24, 50 | `getWallet` | `wallet.wallets`, `ledger.accounts` |
| US-42 | UC-07.02 | BR-25…28, 36, 38 | `getWalletLimits` | `limits.usage` |
| US-41 | UC-07.03 | — | `listWalletActivities` | `payment.wallet_activities` |
| US-25 | UC-07.04 | BR-45, 46 | `getPayment` | `payment.payments` |
| US-81 | UC-09.01 | BR-64, 71 | `adminListAuditLogs` | `audit.audit_logs` |

## 8. Lịch sử

- 2026-10-18: bản đầu, phạm vi M0 (04, 07 phần M0, 09 phần audit).
