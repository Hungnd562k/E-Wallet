# B. Bộ tài liệu chuẩn hóa — cấu trúc đích và quy ước

> Trạng thái: **DRAFT — chờ chủ dự án xác nhận** · Lập: 2026-10-21
> Liên quan: [README](README.md) · [docs/README.md](../README.md) · [C. Danh mục yêu cầu](C-danh-muc-yeu-cau.md)

## 1. Cách hiểu "bộ tài liệu chuẩn hóa" trong dự án này

Bộ tài liệu hiện có đã theo một cấu trúc BA rõ ràng (PRD → URD → BR → FSD → kiến trúc / dữ liệu / API) với quy ước ID và chuỗi truy vết (docs/README §1–3). Quy tắc của chính dự án là "**không lặp lại nội dung**: tài liệu sau trỏ tới tài liệu trước bằng ID".

Vì vậy lần chuẩn hóa này **không viết lại** các tài liệu nguồn thành một bản mới. Làm vậy sẽ tạo ra bản sao thứ hai của mọi yêu cầu, trái quy tắc trên và dễ đổi nghĩa nguồn. Thay vào đó, "bộ tài liệu chuẩn hóa" gồm:

1. **Phần thân**: các tài liệu nguồn hiện có, giữ nguyên nội dung (§2).
2. **Cấu trúc đích**: danh sách đầy đủ tài liệu cần có, trạng thái từng tài liệu và việc cần làm (§2).
3. **Quy ước chung** bổ sung: nhãn trạng thái nội dung (§3), khối kiểm soát tài liệu (§4), mẫu (§5), thuật ngữ (§6), ID mới (§7).
4. **Các bản tổng hợp** đi kèm: [C](C-danh-muc-yeu-cau.md) (danh mục yêu cầu gom một chỗ), [H](H-ma-tran-truy-vet.md) (ma trận truy vết).

Các sửa đổi trên tài liệu nguồn chỉ được làm sau khi có câu trả lời ở [F](F-cau-hoi-xac-nhan.md); danh sách sửa đổi đã đề xuất ở [G §2](G-gia-dinh-de-xuat.md#2-đề-xuất-đề-xuất).

## 2. Cấu trúc đích

Thứ tự đọc giữ như docs/README: **vì sao → cho ai → làm gì → chạy chi tiết ra sao → xây thế nào → kiểm thế nào**. Cột "Tham khảo 29148" chỉ để định hướng loại thông tin; bộ tài liệu **không tuyên bố tuân thủ** ISO/IEC/IEEE 29148.

| # | Tài liệu | Loại | Tham khảo 29148 | Có chưa | Trạng thái nguồn | Việc cần làm | Vấn đề |
|---|---|---|---|---|---|---|---|
| 1 | `docs/README.md` | Mục lục, quy ước | — | Có | — | Thêm dòng cho `chuan-hoa/` (đã làm, xem E) và `glossary.md` khi có | — |
| 2 | `docs/prd.md` | PRD (gộp BRD) | Business requirements | Có | REVIEWED | Sửa F-23 (Q-21), phạm vi (Q-22) | ISS-10, ISS-12 |
| 3 | `docs/urd.md` | URD | Stakeholder requirements | Có | REVIEWED | AC cho US-11, US-44; làm rõ US-05, US-25 | ISS-11, ISS-26, ISS-09 |
| 4 | `docs/business-rules.md` | Danh mục quy tắc nghiệp vụ | Một phần của SRS | Có | REVIEWED (ngày đầu file lệch) | Chốt các mục §6; BR-06, 26, 28, 38, 45, 57, 60, 63 | ISS-07, 14–21 |
| 5 | `docs/fsd/README.md` + `fsd/04`, `07`, `09` | FSD / SRS (M0) | System / software requirements | Có | DRAFT | Xử lý ISS-01, 02; ghi giới hạn M0 | ISS-01, 02, 27 |
| 6 | `docs/fsd/01, 02, 03, 05, 06, 08` | FSD M1–M4 | System / software requirements | **Chưa** | — | Viết theo template fsd/README §3; ưu tiên 02, 03, 05 | ISS-25 |
| 7 | `docs/architecture.md` | SAD / TDD | Architecture description | Có | DRAFT hay REVIEWED? | ADR polyrepo, Go version, back-office milestone, §5.2 | ISS-02–06, 13 |
| 8 | `docs/erd.md` | TDD — dữ liệu | — | Có | REVIEWED (ngày đầu file lệch) | Không cần sửa nội dung; là chuẩn tham chiếu cho tên gọi | ISS-04 |
| 9 | `docs/api/wallet-core.openapi.yaml`, `corebank.openapi.yaml` | Interface spec | Interface requirements | Có | Draft, "lint sạch" | Chốt nguồn chuẩn; `x-br` của BR-28 | ISS-15, 36 |
| 10 | `docs/failure-scenarios.md` | Test spec (TC-xx) | Verification | **Chưa** | — | Viết theo mẫu §5.3 | ISS-24 |
| 11 | `docs/glossary.md` | Thuật ngữ | Definitions | **Chưa** (bản nháp ở §6) | — | Tạo từ §6, thay `Q&A.md` | ISS-29 |
| 12 | `docs/runbooks/` | Runbook vận hành | — | **Chưa** | — | Ít nhất runbook BR-72 | ISS-30 |
| 13 | `progression/` | Kế hoạch, backlog | — | Có | Đã đẩy lên Jira | Không phải nguồn yêu cầu (GD-09) | ISS-33 |
| 14 | `docs/chuan-hoa/` | Báo cáo chuẩn hóa A–I | — | Có (lần này) | DRAFT | Cập nhật khi trả lời F | — |

Tài liệu của repo `E-Wallet-Wallet-Core` (README, CLAUDE.md, `docs/go-map/`) là tài liệu dành cho người phát triển; không thuộc bộ tài liệu yêu cầu nhưng phải trỏ đúng sang đây (ISS-35, ISS-36).

## 3. Nhãn trạng thái nội dung

Dùng trong các báo cáo chuẩn hóa và khi viết mới. Đặt nhãn ngay sau câu hoặc trong cột "Nhãn".

| Nhãn | Dùng khi | Ai được gỡ nhãn |
|---|---|---|
| **[ĐÃ XÁC NHẬN]** | Nội dung lấy nguyên văn từ tài liệu nguồn trạng thái REVIEWED, chưa thấy vấn đề mở. Không đồng nghĩa phê duyệt chính thức (GD-04). | — |
| **[ĐÃ BIÊN TẬP]** | Đã sửa câu chữ, định dạng, tên gọi **mà không đổi nghĩa**. Phải ghi vào [E](E-nhat-ky-thay-doi.md). | Chủ dự án khi review |
| **[ĐỀ XUẤT]** | Gợi ý của người chuẩn hóa, chưa được chấp nhận. | Chủ dự án |
| **[GIẢ ĐỊNH]** | Điều được coi là đúng để tiếp tục làm việc khi thiếu dữ liệu; có ghi "nếu sai thì". | Chủ dự án |
| **[CẦN XÁC NHẬN]** | Nguồn mơ hồ, thiếu, hoặc đang DRAFT; có câu hỏi ở F. | Chủ dự án, sau khi trả lời |
| **[TBD]** | Đã được nêu tên (ví dụ UC, file) nhưng chưa có nội dung. | Người viết, khi viết xong |
| **[MÂU THUẪN]** | Hai nguồn trở lên nói khác nhau; chưa chọn bên nào. | Chủ dự án, sau khi chọn |

Quy tắc: không gộp hai nhãn mâu thuẫn trên cùng một mục; nếu một mục vừa mơ hồ vừa mâu thuẫn, dùng [MÂU THUẪN].

## 4. Khối kiểm soát tài liệu

Mẫu [ĐỀ XUẤT] thay cho dòng trạng thái hiện tại ở đầu mỗi tài liệu (DX-01). Các ô người review / phê duyệt **để trống cho tới khi chủ dự án cung cấp**; không tự điền.

```markdown
> | Phiên bản | Trạng thái | Cập nhật | Người soạn | Người review | Phê duyệt |
> |---|---|---|---|---|---|
> | 1.1 | REVIEWED | 2026-10-18 | <tên> | <tên hoặc để trống> | <tên + ngày, hoặc để trống> |
>
> Liên quan: [README](README.md) · …
```

Bộ trạng thái (DX-02):

| Trạng thái | Nghĩa |
|---|---|
| `DRAFT` | Đang viết; nội dung có thể đổi bất kỳ lúc nào |
| `IN REVIEW` | Đã gửi review; chỉ sửa theo góp ý |
| `REVIEWED` | Đã review xong; còn chờ phê duyệt hoặc không cần phê duyệt |
| `APPROVED` | Đã có người có thẩm quyền đồng ý; sửa đổi sau đó phải tăng phiên bản và ghi lịch sử |
| `SUPERSEDED` | Đã được thay bằng tài liệu khác (ghi rõ tài liệu thay thế) |

Quy tắc phiên bản: tăng số phụ (1.0 → 1.1) khi sửa biên tập hoặc thêm nội dung không đổi nghĩa; tăng số chính (1.x → 2.0) khi đổi quy tắc nghiệp vụ hoặc phạm vi. Dòng lịch sử cuối file ghi: ngày · phiên bản · thay đổi · người làm.

## 5. Mẫu

### 5.1. Mẫu quy tắc nghiệp vụ (BR)

Giữ dạng bảng của business-rules §4, bổ sung hai cột tùy chọn khi viết mới:

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú | Tham số §5 | Kiểm bằng |
|---|---|---|---|---|---|
| BR-xx | Một câu, một ý, có số liệu dạng tên key | [L] / [P] | Điều khoản + độ tin cậy | `key.name` | UC-xx.yy AC-n / TC-xx |

### 5.2. Mẫu user story và tiêu chí chấp nhận

```markdown
| ID | User story | F | BR |
|---|---|---|---|
| US-xx | Là <persona>, tôi muốn <hành động> để <giá trị> | F-xx | BR-xx |

- **US-xx**
  - Given <bối cảnh>, When <hành động>, Then <kết quả đo được>.
```

Mỗi US có ít nhất một AC. Số liệu trong AC ghi kèm tên key: "15 phút (`qr.ttl_minutes`)".

### 5.3. Mẫu kịch bản kiểm thử (TC)

Đề xuất cho `failure-scenarios.md` (ISS-24). Quy ước ID theo docs/README §2: `TC-xx`.

```markdown
### TC-xx — <Tên kịch bản>

| Thuộc tính | Giá trị |
|---|---|
| Kiểm cho | UC-xx.yy AC-n · BR-xx · NFR-xx · SM-xx |
| Milestone | M0 / M1 … |
| Loại | Unit · Integration · Concurrency · Contract · Fault-injection E2E (architecture §12) |
| Tiền điều kiện / dữ liệu | Ví, số dư, tham số cấu hình dùng |
| Fault (nếu có) | `X-Fault: commit-then-drop` … (architecture §7.1) |
| Bước | 1. … 2. … |
| Kết quả mong đợi | Số dư, trạng thái, bút toán (erd §3.2), event, audit |
| Bất biến phải giữ | Σdebit = Σcredit; số dư ≥ 0; không ghi nợ 2 lần … |
| Tự động hóa | Tên test, ví dụ `TestConcurrentCrossTransfers` |
```

### 5.4. Mẫu ghi vấn đề (ISS)

| ID | Loại | Mức | Vị trí | Mô tả + bằng chứng | Ảnh hưởng | Đề xuất | Câu hỏi |
|---|---|---|---|---|---|---|---|
| ISS-xx | Mâu thuẫn / Mơ hồ / Thiếu / Trùng lặp / Lỗi thời / Lệch mã | Cao / TB / Thấp | file §mục | Trích dẫn từng nguồn | | [ĐỀ XUẤT] … | Q-xx |

## 6. Bảng thuật ngữ

Bản nháp cho `docs/glossary.md` (DX-04). Định nghĩa rút ra từ tài liệu nguồn; cột "Nguồn" cho biết chỗ định nghĩa hoặc dùng chính. Thuật ngữ tiếng Anh giữ nguyên khi tài liệu nguồn giữ nguyên (docs/README §3).

| Thuật ngữ | Định nghĩa trong dự án | Nguồn |
|---|---|---|
| Ví điện tử (ví) | Tài khoản giá trị của một khách hàng cá nhân trong `wallet-core`; mỗi khách hàng có 1 ví VND | BR-02, erd §4.3 |
| eKYC | Định danh khách hàng điện tử: OCR CCCD → liveness → so khớp khuôn mặt với ảnh trong chip CCCD (hoặc VNeID). Trong dự án đều là provider mock | BR-04 |
| CCCD gắn chip | Căn cước công dân có chip; giấy tờ duy nhất được chấp nhận ở MVP | BR-01, BR-04 |
| Liveness | Kiểm tra người thật đang đứng trước camera (không phải ảnh / video giả) | BR-04, erd `kyc.checks` |
| Face match | So khớp khuôn mặt chụp được với ảnh tham chiếu (chip CCCD hoặc mẫu đã lưu) | BR-04, BR-36 |
| Liên kết ngân hàng | Gắn TK thanh toán VND hoặc thẻ ghi nợ chính chủ với ví để nạp / rút | BR-10…14 |
| TK đảm bảo (safeguard) | Tài khoản đảm bảo thanh toán của công ty ví tại ngân hàng; mọi dòng tiền thật đi qua đây. Trong ledger là `SAFEGUARD_BANK` | BR-55, erd §3.1 |
| Ledger | Sổ cái kế toán kép của `wallet-core`; module duy nhất được thay đổi số dư | architecture §4.1, erd §3 |
| Bút toán kép (double-entry) | Mỗi giao dịch kế toán có ít nhất hai dòng, tổng nợ = tổng có | erd §3, NFR-01 |
| Bút toán đảo | Bút toán ngược chiều để hủy tác dụng của một bút toán đã ghi; không sửa / xóa bút toán cũ | BR-48, erd §4.4 `reverses_id` |
| Ghi có bù | Ghi có cho ví khi ngân hàng đã ghi nợ nhưng ví chưa ghi có (đối soát) | BR-58, erd §3.2 dòng 7a |
| Số dư khả dụng | Số tiền ví được phép chi; = số dư tài khoản `WALLET:<id>` | erd §3.1 |
| Số dư đang giữ (hold) | Số tiền đã tách khỏi số dư khả dụng cho lệnh rút / chuyển ngân hàng chưa có kết quả. Giao diện gọi là "Đang xử lý" | BR-50, fsd/07 UC-07.01 |
| Tổng số dư | Khả dụng + đang giữ | US-40 |
| Hạn mức tháng | Tổng giá trị giao dịch tính hạn mức của một khách hàng trong tháng dương lịch giờ VN | BR-25, BR-27 |
| Step-up | Xác thực bổ sung (OTP + khuôn mặt) cho giao dịch vượt ngưỡng hoặc thiết bị mới | BR-36, CF-05 |
| Challenge | Phiên xác thực step-up gắn với đúng một giao dịch, dùng một lần, có hạn | BR-40, erd §4.6 |
| `Idempotency-Key` | Khóa do client sinh cho mỗi lệnh tạo giao dịch; gửi lại cùng khóa thì nhận lại đúng kết quả cũ, không thực hiện lần hai | NFR-02, CF-04 |
| `bank_operation_id` | ID ổn định của một lệnh gửi corebank, dùng lại qua mọi lần retry / tra cứu | BR-49, erd §4.8 |
| Outbox | Bảng sự kiện ghi cùng transaction nghiệp vụ; worker đọc và xử lý bất đồng bộ (at-least-once) | architecture §5.1, §6 |
| Payment (giao dịch) | Bản ghi một lệnh tiền của khách hàng (`TOPUP`, `TRANSFER`, `WITHDRAW`, `INTERBANK`, `QR_PAYMENT`, `REFUND`, `ADJUSTMENT`); ID public có prefix `txn_` | erd §4.7 |
| Đang xử lý (`PENDING`) | Trạng thái hiển thị khi hệ thống chưa chắc kết quả; gồm cả `NEEDS_RECONCILIATION` nội bộ | BR-46, CF-03 |
| `NEEDS_RECONCILIATION` | Trạng thái nội bộ của giao dịch quá 24 giờ chưa rõ kết quả; chuyển cho vận hành | BR-46, erd §6 |
| Đối soát T+1 | So khớp danh sách lệnh trong ngày giữa ví và corebank vào ngày hôm sau, xử lý chênh lệch | BR-58, architecture §6 |
| Bất biến TK đảm bảo | Số dư TK đảm bảo ≥ tổng số dư (kể cả hold) của mọi ví; kiểm hằng ngày | BR-56 |
| Merchant | Tổ chức đã ký hợp đồng (seed sẵn trong DB), trạng thái `ACTIVE`, nhận thanh toán QR; trong dự án tương tác qua `merchant-sim` | BR-53, prd §4 |
| QR động | QR có số tiền cố định, gắn một đơn hàng, hết hạn sau 15 phút; chỉ chứa token, thông tin lấy từ backend | BR-52, ADR-14 |
| Callback | Lời gọi từ ví tới merchant báo đơn đã thanh toán / hoàn tiền; ký HMAC, retry tối đa 24 giờ | BR-51, architecture §7.2 |
| Maker–checker | Một người tạo, một người khác duyệt; áp dụng cho điều chỉnh tài chính | BR-64 |
| Tra soát / khiếu nại | Yêu cầu của khách hàng xem xét lại một giao dịch; có mã và hạn trả lời | BR-60 |
| AML | Phòng chống rửa tiền; ở MVP là các rule tạo cảnh báo cho vận hành xem | BR-74 |
| Fault injection | Cơ chế của corebank giả lập để cố ý gây chậm, timeout, mất phản hồi… phục vụ kiểm thử | architecture §7.1, ADR-12 |
| [L] / [P] | Nhãn của BR: [L] xuất phát từ quy định pháp luật; [P] là quyết định sản phẩm | business-rules §1 |
| Milestone M0…M4 | Các mốc phát hành theo lát cắt nghiệp vụ | prd §9 |
| MoSCoW | Thang ưu tiên Must / Should / Could | prd §7 |

## 7. Quy ước ID bổ sung

Các ID có sẵn giữ nguyên theo docs/README §2. Lần chuẩn hóa này thêm các ID **chỉ dùng trong `chuan-hoa/`**:

| Tiền tố | Ý nghĩa | Định nghĩa tại |
|---|---|---|
| `ISS-xx` | Vấn đề tài liệu | [D](D-bao-cao-van-de.md) |
| `Q-xx` | Câu hỏi cần xác nhận | [F](F-cau-hoi-xac-nhan.md) |
| `GD-xx` | Giả định của lần chuẩn hóa | [G §1](G-gia-dinh-de-xuat.md) |
| `DX-xx` | Đề xuất | [G §2](G-gia-dinh-de-xuat.md) |
| `CHG-xx` | Thay đổi đã thực hiện | [E](E-nhat-ky-thay-doi.md) |

Quy tắc duy nhất: một ID không bao giờ được dùng lại cho nội dung khác, kể cả khi mục bị xóa (ghi "đã bỏ" thay vì xóa dòng).
