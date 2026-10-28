# Quy tắc chung cho AI reviewer

> Trạng thái: **DRAFT** · Cập nhật: 2026-10-28
> Áp dụng cho mọi repo code của E-Wallet. Quy tắc riêng của từng repo nằm trong `REVIEW.md` ở root repo đó. Quy trình: [review-process.md](review-process.md).

## 1. Phạm vi

- Review toàn bộ thay đổi của PR so với base branch, ghi rõ base SHA và head SHA.
- Chỉ báo lỗi do PR gây ra. Lỗi có sẵn chỉ báo khi nó làm phần code mới sai.
- Đọc ticket Jira (`.ai-review/jira.md`), rồi các mục tài liệu mà [review-context.md](review-context.md) chỉ ra cho thư mục bị đổi. Không đọc hết bộ tài liệu.

## 2. Nguồn chuẩn

1. Tài liệu trên nhánh `main` của repo `E-Wallet` là nguồn chuẩn duy nhất về nghiệp vụ và kiến trúc.
2. Ngoại lệ: contract API của một thành phần là file OpenAPI trong repo của chính thành phần đó (Q-07), ví dụ `E-Wallet-Wallet-Core/api/openapi.yaml`.
3. Khi AC trên Jira lệch với `urd.md` hoặc `fsd/`, theo tài liệu và báo chỗ lệch thành một finding loại "thiếu context".

## 3. Cần kiểm tra

**Nghiệp vụ**
- Từng AC của ticket và của `UC-xx` liên quan.
- Chuyển trạng thái hợp lệ và không hợp lệ (erd §6).
- Quyền sở hữu ví và phân quyền.
- Tính nguyên tử: mọi thay đổi liên quan nằm trong cùng transaction.
- Idempotency và retry của mọi lệnh tạo giao dịch.
- Request đồng thời không phá bất biến số dư và ledger.
- Lỗi một phần từ hệ thống ngoài được xử lý an toàn.

**Kiến trúc và quy ước**
- Ranh giới module và chiều phụ thuộc (architecture §4.3).
- Không đi vòng qua service của module để sửa trạng thái quan trọng.
- Quy ước lỗi, log, transaction, thời gian trong `CLAUDE.md` của repo.
- Không đổi contract API mà không cập nhật file OpenAPI.

**Tài liệu**
- Thay đổi code cần cập nhật tài liệu mà chưa cập nhật: báo **P2**. Ví dụ: đổi API mà không sửa OpenAPI; dời code mà không sửa anchor `file:line` trong `docs/go-map/`.

**Ticket**
- PR `feat`/`fix` không có key `SCRUM-NN` ở tiêu đề PR hay commit nào: báo **P2**.
- PR `chore`, `docs`, `Revert`, merge không có key: chỉ ghi chú trong comment tổng hợp.

## 4. Severity

| Mức | Nghĩa | Ví dụ trong dự án |
|---|---|---|
| **P0** | Lỗi bảo mật nghiêm trọng hoặc làm sai lệch tiền | Thay đổi số dư ngoài module `ledger`; bút toán lệch Σnợ ≠ Σcó; dùng float cho tiền; nới lỏng trigger append-only hay `CHECK` số dư; ghi PIN, OTP, token, CCCD ra log |
| **P1** | Sai nghiệp vụ, mất nhất quán dữ liệu hoặc sai phân quyền | Ghi ledger, ví, payment, outbox không cùng transaction; lock không theo `ORDER BY id`; idempotency tạo hai giao dịch; trả 5xx cho lỗi nghiệp vụ; chuyển trạng thái không qua conditional update; vi phạm archtest |
| **P2** | Lỗi chức năng đáng kể hoặc thiếu hành vi bắt buộc | Thiếu một AC; thiếu test cho luồng ngoại lệ chính; dùng `time.Now()` thay `clock.Clock`; thiếu cập nhật tài liệu |
| **P3** | Vấn đề nhỏ, không chặn | Tên gọi, comment sai, đơn giản hóa được |

## 5. Bằng chứng

- Mỗi finding chỉ ra dòng code bị đổi và **kịch bản lỗi cụ thể** (đầu vào, trạng thái → kết quả sai).
- Trích AC, `BR-xx`, `UC-xx` hoặc mục tài liệu liên quan khi có.
- Phân biệt lỗi đã xác nhận với giả định hoặc thiếu thông tin. Không tự đặt ra yêu cầu nghiệp vụ.
- Yêu cầu mơ hồ hay mâu thuẫn: báo là "cần xác nhận", không kết luận code sai.
- Không báo sở thích về style như bug. Không báo trùng cùng một nguyên nhân.

## 6. Định dạng kết quả

- Viết **tiếng Việt**. Tên hàm, biến, đường dẫn giữ nguyên.
- Mỗi finding một inline comment, mở đầu bằng `**AIR-<n> · P<x>** ` rồi một câu nêu lỗi, sau đó kịch bản lỗi và nguồn trích dẫn.
- Một comment tổng hợp có marker `<!-- ai-review -->` ở dòng đầu, gồm: base/head SHA, ticket đã đọc, tài liệu đã đọc, bảng finding (mã, mức, file:dòng, một câu), các điểm "cần xác nhận".
- Không approve, không request changes, không merge, không push commit.
