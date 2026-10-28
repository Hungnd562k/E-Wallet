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
| #1 | Có commit SCRUM-25 (US-21) đã bị revert | ISS-41 trong `docs/chuan-hoa/D-bao-cao-van-de.md` |
| #5 | Outbox + worker lease (SCRUM-19) | — |
| #6 | Che dữ liệu nhạy cảm cho audit/log (SCRUM-75, 76) | — |
| #7 | `audit.Record` trong transaction (SCRUM-77) | — |

## Nhật ký

| Ngày | Repo | PR | Head SHA | Số finding (P0/P1/P2/P3) | Đúng | Nhiễu | Bỏ sót đã biết | Thời gian | Ghi chú |
|---|---|---|---|---|---|---|---|---|---|
