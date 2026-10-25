# Chuẩn hóa tài liệu dự án E-Wallet

> Trạng thái: **DRAFT — chờ chủ dự án xác nhận** · Lập: 2026-10-21
> Nguồn: `E-Wallet@087c3ff` (nhánh `master`, nay đã gộp vào `main`) · `E-Wallet-Wallet-Core@7332c9e` (nhánh `development`). Đối chiếu mã cập nhật theo `development@9a05355` (2026-10-25)

Thư mục này là kết quả của một lần rà soát và chuẩn hóa toàn bộ tài liệu dự án: kiểm kê nguồn, đối chiếu chéo, đánh giá chất lượng, lập danh mục yêu cầu, ma trận truy vết, danh sách vấn đề và câu hỏi cần xác nhận.

**Tài liệu nguồn (prd, urd, business-rules, fsd, architecture, erd, api) không bị sửa nội dung.** Mọi sửa đổi được đề xuất ở G và chờ câu trả lời ở F.

## Đọc theo thứ tự

| | Tệp | Nội dung | Dành cho |
|---|---|---|---|
| A | [A-bao-cao-tong-quan.md](A-bao-cao-tong-quan.md) | Kết luận, kiểm kê nguồn, đánh giá chất lượng, số liệu | Đọc đầu tiên |
| F | [F-cau-hoi-xac-nhan.md](F-cau-hoi-xac-nhan.md) | 31 câu hỏi cần chủ dự án trả lời, có ưu tiên | Chủ dự án |
| D | [D-bao-cao-van-de.md](D-bao-cao-van-de.md) | 41 vấn đề: mâu thuẫn, mơ hồ, thiếu, trùng lặp, lỗi thời, lệch mã | Chủ dự án, BA, dev |
| I | [I-checklist-chat-luong.md](I-checklist-chat-luong.md) | Checklist chất lượng, sẵn sàng theo milestone và theo tài liệu | Chủ dự án |
| B | [B-bo-tai-lieu-chuan-hoa.md](B-bo-tai-lieu-chuan-hoa.md) | Cấu trúc đích, nhãn, khối kiểm soát, mẫu, thuật ngữ | BA, người viết tài liệu |
| C | [C-danh-muc-yeu-cau.md](C-danh-muc-yeu-cau.md) | Danh mục SM, F, US, BR, NFR, UC, CF, AU có đánh giá | BA, tester |
| H | [H-ma-tran-truy-vet.md](H-ma-tran-truy-vet.md) | Ma trận US → F → UC → BR → API → ERD → TC → triển khai | BA, dev, tester |
| G | [G-gia-dinh-de-xuat.md](G-gia-dinh-de-xuat.md) | Giả định của lần chuẩn hóa, đề xuất biên tập và nghiệp vụ | Chủ dự án |
| E | [E-nhat-ky-thay-doi.md](E-nhat-ky-thay-doi.md) | Thay đổi đã làm và thay đổi chờ xác nhận | Mọi người |

## Nhãn

[ĐÃ XÁC NHẬN] · [ĐÃ BIÊN TẬP] · [ĐỀ XUẤT] · [GIẢ ĐỊNH] · [CẦN XÁC NHẬN] · [TBD] · [MÂU THUẪN] — định nghĩa ở [B §3](B-bo-tai-lieu-chuan-hoa.md#3-nhãn-trạng-thái-nội-dung).

## Cập nhật bộ báo cáo

- C và H được sinh từ bảng của tài liệu nguồn (nội dung yêu cầu chép nguyên văn). Khi nguồn đổi, cập nhật lại hai tệp này trước.
- Khi một câu hỏi ở F được trả lời: ghi câu trả lời vào F, chuyển vấn đề ở D sang "Đã xử lý", sửa tài liệu nguồn, ghi `CHG-xx` vào E.
- Bộ báo cáo tham khảo ISO/IEC/IEEE 29148 về đặc tính của yêu cầu tốt, nhưng **không tuyên bố** bộ tài liệu tuân thủ chuẩn này.
