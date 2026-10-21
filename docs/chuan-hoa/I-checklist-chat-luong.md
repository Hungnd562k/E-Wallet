# I. Checklist chất lượng và trạng thái sẵn sàng

> Trạng thái: **DRAFT** · Lập: 2026-10-21 · Phạm vi: `E-Wallet` nhánh `master` commit `087c3ff`; đối chiếu `E-Wallet-Wallet-Core` commit `7332c9e`
> Liên quan: [README](README.md) · [D. Vấn đề](D-bao-cao-van-de.md) · [F. Câu hỏi](F-cau-hoi-xac-nhan.md)

Kết quả: **Đạt** · **Một phần** · **Chưa đạt** · **Chưa kiểm** (không kiểm trong lần này).

## 1. Checklist chất lượng bộ tài liệu

| # | Tiêu chí | Kết quả | Bằng chứng |
|---|---|---|---|
| 1 | Mọi tài liệu nguồn được kiểm kê và đọc được | Đạt | 16 tệp ở `E-Wallet`, 4 nhóm ở wallet-core ([A §2](A-bao-cao-tong-quan.md#2-kiểm-kê-nguồn)); tất cả UTF-8, đọc được |
| 2 | Mỗi tài liệu có dòng trạng thái · ngày · liên quan | Một phần | Có ở mọi tài liệu chính; 3 tài liệu có ngày / trạng thái lệch lịch sử (ISS-04) |
| 3 | ID duy nhất trong từng loại | Đạt | Script đếm: 6 SM, 23 F, 12 NFR, 43 US, 54 BR — không trùng |
| 4 | Mọi ID được trích dẫn đều tồn tại | Đạt | Không có BR / US / F "ma" trong 11 tệp (D §9) |
| 5 | Liên kết F ↔ US nhất quán hai chiều | Một phần | 2 cặp lệch (ISS-10) |
| 6 | Mỗi US có tiêu chí chấp nhận | Một phần | 41/43 (thiếu US-11, US-44 — ISS-26) |
| 7 | AC viết dạng Given/When/Then, có số đo | Đạt | urd §2; UC M0 có thêm bảng AC hệ thống |
| 8 | Mỗi BR có nhãn [L]/[P] và căn cứ | Đạt | 54/54 có nhãn; [L] có trích điều khoản |
| 9 | Điểm chưa chắc về pháp lý được ghi rõ | Đạt | business-rules §2 (cột độ tin cậy), §6 |
| 10 | Mọi con số là tham số cấu hình ở một chỗ | Một phần | business-rules §5 có 23 nhóm key; 1 key FSD thiếu (ISS-27); số được chép lại ở nhiều tài liệu (ISS-32) |
| 11 | Không có mâu thuẫn giữa các tài liệu | **Chưa đạt** | 13 mâu thuẫn (ISS-01…13), 2 ở mức Cao |
| 12 | Không có yêu cầu mơ hồ ở mức gây triển khai sai | **Chưa đạt** | 10 điểm mơ hồ (ISS-14…23), 1 ở mức Cao |
| 13 | FSD phủ các US của milestone hiện tại (M0) | Đạt | 6 story của Sprint 1 đều có UC (H.2) |
| 14 | FSD phủ toàn bộ US | **Chưa đạt** | 7/43 US có UC; 6/9 file FSD chưa viết (ISS-25) |
| 15 | Có kịch bản kiểm thử (TC) truy vết tới yêu cầu | **Chưa đạt** | `failure-scenarios.md` chưa viết (ISS-24) |
| 16 | Truy vết US → UC → BR → API → ERD | Một phần | Đủ cho M0 (fsd/README §7); BR → API có từ `x-br`; BR → ERD có từ erd §9 |
| 17 | Yêu cầu phi chức năng đo được | Một phần | 7/12 NFR và 5/7 UR-NF đã có cách kiểm rõ; còn lại ISS-22 |
| 18 | Có phiên bản, người review, phê duyệt | **Chưa đạt** | ISS-28 |
| 19 | Có bảng thuật ngữ | **Chưa đạt** | Bản nháp ở [B §6](B-bo-tai-lieu-chuan-hoa.md#6-bảng-thuật-ngữ) (ISS-29) |
| 20 | Tài liệu nằm ở nhánh mặc định của repo | **Chưa đạt** | ISS-34 |
| 21 | Tài liệu và mã nguồn không lệch nhau | Một phần | Mã lỗi, `getPayment`, bản sao OpenAPI lệch (ISS-36…40) |
| 22 | Hai contract OpenAPI lint sạch | Chưa kiểm | docs/README ghi "lint sạch"; không chạy lại (GD-07) |
| 23 | Sơ đồ Mermaid hiển thị được | Chưa kiểm | Không render trong lần này |
| 24 | Tách bạch quy định pháp luật và quyết định sản phẩm | Đạt | Nhãn [L]/[P] trên từng BR; prd §10 rủi ro "lẫn lộn [P] và [L]" |

Tổng: Đạt 8 · Một phần 7 · Chưa đạt 7 · Chưa kiểm 2 (trên 24 tiêu chí; tiêu chí 13 và 14 tách riêng phạm vi M0 và toàn bộ).

## 2. Trạng thái sẵn sàng theo milestone

| Milestone | Sẵn sàng của tài liệu | Còn thiếu / cần chốt | Câu hỏi |
|---|---|---|---|
| **M0** — Nền tảng tiền (Sprint 1, đến 2026-11-01) | **Dùng được, có điều kiện** | Có đủ US, AC, UC, API, ERD cho 6 story của Sprint 1. Cần chốt 2 mâu thuẫn ảnh hưởng hành vi chuyển tiền (ISS-01, ISS-02), phạm vi BR-28 (ISS-15), quyền xem giao dịch của người nhận (ISS-38), phạm vi Sprint 1 của `createTransfer` (ISS-39). Chưa có TC cho SM-01. | Q-01, Q-02, Q-12, Q-30, Q-31 |
| **M1** — Ngân hàng | **Chưa sẵn sàng** | Thiếu fsd/02, 03, 05; UC-09.06, 09.07; `failure-scenarios.md` (SM-02 phụ thuộc trực tiếp). BR-25 phạm vi liên ngân hàng còn mở. Contract corebank đã có. | Q-14, Q-15, Q-24 |
| **M2** — Thanh toán | **Chưa sẵn sàng** | Thiếu fsd/06. Hiển thị hoàn tiền (ISS-09), F-23 (ISS-10). | Q-09, Q-21 |
| **M3** — Onboarding & bảo mật | **Chưa sẵn sàng** | Thiếu fsd/01. Khóa PIN (ISS-11), step-up (ISS-16), thiết bị mới (ISS-21), BR-06 (ISS-07). | Q-08, Q-10, Q-13, Q-18 |
| **M4** — Sản phẩm & vận hành | **Chưa sẵn sàng** | Thiếu fsd/08, UC-07.05…07, UC-09.02…11. Ngày làm việc (ISS-19), ví không hoạt động (ISS-20), AC US-11, US-44 (ISS-26), milestone back-office (ISS-03). | Q-03, Q-16, Q-17, Q-28 |

## 3. Trạng thái sẵn sàng theo tài liệu

| Tài liệu | Mức | Ghi chú |
|---|---|---|
| prd.md | Dùng được | 2 điểm nhỏ (ISS-10, ISS-12) |
| urd.md | Dùng được | Thiếu AC 2 story; 2 điểm cần làm rõ (ISS-09, ISS-11) |
| business-rules.md | Dùng được, có điều kiện | 15 BR có nhãn [CẦN XÁC NHẬN] / [MÂU THUẪN] ở C.4; các điểm pháp lý ở §6 còn mở |
| fsd/README + 04, 07, 09 | Dùng được cho M0, có điều kiện | DRAFT; chờ Q-01, Q-02 |
| architecture.md | Cần cập nhật | ADR-13 và §11 không còn đúng (ISS-05); §5.2 mâu thuẫn FSD (ISS-01, 02) |
| erd.md | Dùng được | Có thể làm chuẩn tham chiếu cho tên gọi (ISS-13) |
| wallet-core.openapi.yaml | Dùng được | Cần chốt nguồn chuẩn (ISS-36); `x-br` BR-28 (ISS-15) |
| corebank.openapi.yaml | Dùng được | Không phát hiện vấn đề riêng |
| progression/jira-backlog-draft.md | Tham khảo | Không phải nguồn yêu cầu |
| `Q&A.md` | Chưa dùng được | Chỉ có câu hỏi (ISS-29) |
| wallet-core `CLAUDE.md`, `api/openapi.yaml` | Cần cập nhật | ISS-35, ISS-36 |

## 4. Tự kiểm tra bộ báo cáo A–I

| # | Kiểm tra | Kết quả |
|---|---|---|
| 1 | Mỗi `ISS-xx` trong bảng tổng hợp D có mục chi tiết | Đạt (40/40) |
| 2 | Mỗi `Q-xx` được D trích dẫn đều có trong F, và ngược lại | Đạt (31/31) |
| 3 | Mọi `ISS-xx` được C, H, I trích dẫn đều tồn tại trong D | Đạt |
| 4 | Nội dung yêu cầu ở C chép nguyên văn từ nguồn bằng script, không gõ lại | Đạt |
| 5 | Không tạo người duyệt, phê duyệt, phiên bản, nguồn hay liên kết truy vết không có trong nguồn | Đạt — mẫu ở B §4 để trống ô phê duyệt; ma trận H chỉ dùng liên kết có sẵn |
| 6 | Không tự chọn bên đúng khi có mâu thuẫn | Đạt — mọi mâu thuẫn có nhãn [MÂU THUẪN], gợi ý ghi [ĐỀ XUẤT] và chuyển thành câu hỏi |
| 7 | Không sửa nội dung nghiệp vụ của tài liệu nguồn | Đạt — chỉ thêm một dòng mục lục ở docs/README (E, CHG-02) |
| 8 | Không tuyên bố tuân thủ ISO/IEC/IEEE 29148 | Đạt |
