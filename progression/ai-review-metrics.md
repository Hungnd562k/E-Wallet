# Đo chất lượng AI reviewer

> Quy trình: [docs/review-process.md](../docs/review-process.md) §9. Cập nhật mỗi khi có một lần review AI.

## Định nghĩa

| Chỉ số | Cách tính |
|---|---|
| Finding đúng | Số finding chủ dự án xác nhận là lỗi thật ÷ tổng finding |
| Bỏ sót | Lỗi quan trọng phát hiện sau đó (review tay, test, production) mà reviewer không báo |
| Nhiễu | Finding trùng hoặc không có bằng chứng ÷ tổng finding |
| Thời gian | Từ lúc workflow bắt đầu đến lúc đăng review |

## Giai đoạn 2: chạy thử trên PR cũ (wallet-core)

| PR | Vì sao chọn | Lỗi đã biết |
|---|---|---|
| #1 | Định chọn vì gắn với SCRUM-25, nhưng commit SCRUM-25 (`9a05355`) chính là **base** của PR nên không nằm trong diff. Diff chỉ là tài liệu và cấu hình lint | Không có lỗi đã biết trong diff |
| SCRUM-25 | Cần PR riêng cho commit `9a05355` (base là cha của nó) để có đáp án ISS-41 | ISS-41; bị revert ở `52ce052` vì phá dependency, unit test fail |
| #5 | Outbox + worker lease (SCRUM-19) | — |
| #6 | Che dữ liệu nhạy cảm cho audit/log (SCRUM-75, 76) | — |
| #7 | `audit.Record` trong transaction (SCRUM-77) | — |

## Nhật ký

| Ngày | Repo | PR | Head SHA | Số finding (P0/P1/P2/P3) | Đúng | Nhiễu | Bỏ sót đã biết | Thời gian | Ghi chú |
|---|---|---|---|---|---|---|---|---|---|
| 2026-10-28 | wallet-core | #1 | `8fbe1ab` | 0 (0/0/0/0) | — | 0 | Không áp dụng | 75 giây | Kết luận đúng: diff chỉ đổi docs/cấu hình. 3 ghi chú "cần xác nhận" hợp lý. Không đọc được `REVIEW.md` vì head cũ hơn file này |
| 2026-10-28 | wallet-core | #7 | `061f0be` | 2 (0/0/2/0) | 2/2 | 0 | Chưa biết | 90 giây | AIR-1: cắt `User-Agent` theo byte, không lọc UTF-8 không hợp lệ, nên header làm `audit.Record` lỗi và rollback giao dịch (đã kiểm tra, đúng, vẫn còn trên `development`). AIR-2: anchor `go-map` của `Recover`/`statusWriter` lệch L30→L87, L59→L116 (đã kiểm tra, đúng). Không có Jira (chưa có secret); không có `REVIEW.md` ở head cũ |

## Nhận xét sau 2 lần đầu

- Tỷ lệ finding đúng 2/2, không có nhiễu. Mẫu còn quá nhỏ để kết luận.
- PR cũ hơn `REVIEW.md` không có file này ở head. Workflow đã được sửa để đọc `REVIEW.md` từ nhánh mặc định khi head không có.
- Chưa đo được lỗi bỏ sót vì chưa chạy trên PR có lỗi đã biết (SCRUM-25).
