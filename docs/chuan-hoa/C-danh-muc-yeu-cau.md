# C. Danh mục yêu cầu

> Trạng thái: **DRAFT — chờ chủ dự án xác nhận** · Lập: 2026-10-21 · Nguồn: `prd.md`, `urd.md`, `business-rules.md`, `fsd/*` tại nhánh `master` commit `087c3ff`
> Liên quan: [README](README.md) · [D. Vấn đề](D-bao-cao-van-de.md) · [H. Truy vết](H-ma-tran-truy-vet.md)

Danh mục này **gom** các yêu cầu đã có trong tài liệu nguồn vào một chỗ để tra cứu và đánh giá; **không thay thế** tài liệu nguồn. Nội dung yêu cầu được chép **nguyên văn** từ nguồn (cột "Nội dung"). Cột đánh giá ("Kiểm chứng được", "Nhãn", "Vấn đề") là phần bổ sung của lần chuẩn hóa này.

Nhãn dùng trong danh mục (định nghĩa đầy đủ ở [B §3](B-bo-tai-lieu-chuan-hoa.md#3-nhãn-trạng-thái-nội-dung)):

- **[ĐÃ XÁC NHẬN]**: lấy nguyên văn từ tài liệu nguồn có trạng thái REVIEWED và chưa phát hiện vấn đề mở. Đây là xác nhận theo trạng thái tài liệu, **không phải** phê duyệt chính thức (xem G, GD-04).
- **[CẦN XÁC NHẬN]**: nguồn còn mơ hồ, thiếu, hoặc tài liệu nguồn đang DRAFT.
- **[MÂU THUẪN]**: có ít nhất một nguồn khác nói khác; chưa chọn bên nào.
- **[TBD]**: đã được liệt kê nhưng chưa được đặc tả.

## C.1. Mục tiêu và chỉ số thành công (SM)

| ID | Mục tiêu | Chỉ số đo (nguyên văn prd §3) | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|
| SM-01 | Toàn vẹn số dư dưới tải song song | 1.000 lệnh chuyển ví–ví chéo, chạy đồng thời: 0 lệch ledger, 0 số dư âm, 0 deadlock không được xử lý | Có — test song song tự động | [ĐÃ XÁC NHẬN] |  |
| SM-02 | Chịu lỗi tích hợp ngân hàng | 100% kịch bản fault injection trong `failure-scenarios.md` đều pass: không ghi nợ 2 lần, hold được giữ/giải phóng đúng | Chưa — phụ thuộc `failure-scenarios.md` chưa viết | [CẦN XÁC NHẬN] | ISS-24 |
| SM-03 | Trải nghiệm luồng chính | Demo trọn vẹn nạp → chuyển → QR → hoàn tiền. Giao dịch đồng bộ < 1 s (local); giao dịch qua ngân hàng (không có fault) có kết quả < 5 s | Một phần — "demo trọn vẹn" chưa có kịch bản demo | [CẦN XÁC NHẬN] | ISS-22 |
| SM-04 | Đối soát | Job T+1 phát hiện 100% chênh lệch được cấy sẵn; bất biến TK đảm bảo được kiểm tra hằng ngày | Có — cần bộ dữ liệu cấy lệch | [ĐÃ XÁC NHẬN] |  |
| SM-05 | Chất lượng mã | Coverage ≥ 80% cho domain `ledger`, `payment`, `limit`; `go test -race` sạch | Có | [ĐÃ XÁC NHẬN] |  |
| SM-06 | Hợp đồng API | `npx @redocly/cli lint` sạch; contract test corebank và merchant pass | Có — contract test merchant phụ thuộc M2 | [ĐÃ XÁC NHẬN] |  |

## C.2. Tính năng (F)

| ID | Tính năng | Ưu tiên | Milestone | User story (theo PRD) | FSD | Đặc tả FSD | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|---|---|
| F-01 | Đăng ký bằng SĐT + OTP, đăng nhập bằng PIN, gắn thiết bị | Must | M3 (M0 dùng seed) | US-01, 03, 05, 06, 07 | fsd/01 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-02 | Đồng ý xử lý dữ liệu (tách riêng sinh trắc học) | Must | M3 | US-02 | fsd/01 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-03 | eKYC mô phỏng (CCCD gắn chip + khuôn mặt), eKYC lại khi giấy tờ hết hạn | Must | M3 | US-04, 08 | fsd/01 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-04 | Ví, số dư khả dụng / đang giữ | Must | M0 | US-40 | fsd/07 | Có UC (Draft) | [ĐÃ XÁC NHẬN] |  |
| F-05 | Chuyển ví–ví | Must | M0 | US-21 | fsd/04 | Có UC (Draft) | [ĐÃ XÁC NHẬN] |  |
| F-06 | Hạn mức giao dịch (tháng, theo lần) | Must | M0 | US-42 | fsd/07 | Có UC (Draft) | [CẦN XÁC NHẬN] | ISS-14, ISS-15, ISS-17 |
| F-07 | Xác thực giao dịch: PIN (M0), OTP + khuôn mặt cho giao dịch lớn (M3) | Must | M0 / M3 | US-24 | fsd/01, fsd/04 | Có UC (Draft) | [CẦN XÁC NHẬN] | ISS-16, ISS-33 |
| F-08 | Liên kết / hủy liên kết ngân hàng | Must | M1 | US-10, 11, 12 | fsd/02 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-09 | Nạp tiền | Must | M1 | US-20 | fsd/03 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-10 | Rút tiền về TK liên kết | Must | M1 | US-22 | fsd/05 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-11 | Chuyển liên ngân hàng | Should | M1 | US-23 | fsd/05 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-12 | Thanh toán QR động | Must | M2 | US-30, 60, 61 | fsd/06 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-13 | Hoàn tiền từ merchant | Should | M2 | US-31, 62 | fsd/06 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-14 | Lịch sử, trạng thái giao dịch, thông báo | Must | M0 (API) / M4 (push) | US-25, 41, 44 | fsd/07 | Có UC (Draft) | [ĐÃ XÁC NHẬN] |  |
| F-15 | Đóng ví (theo yêu cầu, do không hoạt động) | Should | M4 | US-43, 45 | fsd/07 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-16 | Tra soát / khiếu nại | Should | M4 | US-50, 51, 79 | fsd/08 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-17 | Đối soát T+1 & kiểm tra bất biến TK đảm bảo | Must | M1 | US-74, 75 | fsd/09 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-18 | Điều chỉnh tài chính maker–checker | Should | M4 | US-76, 77 | fsd/09 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-19 | Cảnh báo AML cơ bản | Could | M4 | US-78 | fsd/09 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-20 | Back-office: tra cứu, duyệt eKYC thủ công, phong tỏa ví | Should | M4 | US-70…73 | fsd/09 | Chưa có UC | [MÂU THUẪN] | ISS-03 |
| F-21 | Quản trị tham số cấu hình | Could | M4 | US-80 | fsd/09 | Chưa có UC | [ĐÃ XÁC NHẬN] |  |
| F-22 | Audit log | Must | M0 | US-81 | fsd/09 | Có UC (Draft) | [ĐÃ XÁC NHẬN] |  |
| F-23 | Merchant API + `merchant-sim` | Must | M2 | US-60…63 | fsd/06 | Chưa có UC | [MÂU THUẪN] | ISS-10 |

## C.3. User story (US)

Cột **AC** cho biết urd.md có tiêu chí chấp nhận Given/When/Then cho story đó hay không. Cột **UC** lấy từ ma trận fsd/README §7 (chỉ có UC đã viết).

| ID | Epic | User story (nguyên văn) | Persona / Role | F | BR | Milestone (theo F) | AC | UC | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|---|---|---|---|
| US-01 | E1 | Là khách hàng mới, tôi muốn đăng ký bằng số điện thoại và mã OTP để có tài khoản ví | P1, P2 | F-01 | BR-03 | M3 (M0 dùng seed) | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-02 | E1 | Là khách hàng, tôi muốn đọc và đồng ý từng loại điều khoản (riêng phần dữ liệu sinh trắc học) để biết dữ liệu của mình được dùng thế nào | P1, P2 | F-02 | BR-70 | M3 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-03 | E1 | Là khách hàng, tôi muốn đặt mã PIN 6 số để xác nhận giao dịch | P1, P2 | F-01 | BR-35, 41 | M3 (M0 dùng seed) | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-04 | E1 | Là khách hàng, tôi muốn định danh bằng CCCD gắn chip và khuôn mặt ngay trên app để kích hoạt ví mà không cần ra quầy | P1, P2 | F-03 | BR-01, 04…06 | M3 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-05 | E1 | Là khách hàng, tôi muốn đăng nhập bằng SĐT + PIN trên điện thoại của mình | P1, P2 | F-01 | BR-38, 39 | M3 (M0 dùng seed) | Có | — | [MÂU THUẪN] | ISS-11 |
| US-06 | E1 | Là khách hàng đổi điện thoại, tôi muốn đăng nhập trên máy mới một cách an toàn | P1, P2 | F-01 | BR-38 | M3 (M0 dùng seed) | Có | — | [CẦN XÁC NHẬN] | ISS-21 |
| US-07 | E1 | Là khách hàng quên PIN hoặc bị khóa PIN, tôi muốn đặt lại PIN mà không phải liên hệ tổng đài | P1, P2 | F-01 | BR-39 | M3 (M0 dùng seed) | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-08 | E1 | Là khách hàng, tôi muốn được nhắc trước khi CCCD hết hạn và định danh lại để ví không bị gián đoạn | P1, P2 | F-03 | BR-07 | M3 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-10 | E2 | Là khách hàng, tôi muốn liên kết tài khoản hoặc thẻ ghi nợ đứng tên mình để nạp và rút tiền | P1, P2 | F-08 | BR-10…12, 14 | M1 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-11 | E2 | Là khách hàng có nhiều liên kết, tôi muốn chọn một liên kết mặc định | P1, P2 | F-08 | — | M1 | **Không** | — | [CẦN XÁC NHẬN] | ISS-26 |
| US-12 | E2 | Là khách hàng, tôi muốn hủy một liên kết không còn dùng | P1, P2 | F-08 | BR-13 | M1 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-20 | E3 | Là khách hàng, tôi muốn nạp tiền từ ngân hàng đã liên kết vào ví | P1, P2 | F-09 | BR-20, 49 | M1 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-21 | E3 | Là khách hàng, tôi muốn chuyển tiền cho người dùng ví khác bằng số điện thoại | P1, P2 | F-05 | BR-23…29, 47 | M0 | Có | UC-04.01 | [ĐÃ XÁC NHẬN] |  |
| US-22 | E3 | Là khách hàng, tôi muốn rút tiền từ ví về tài khoản ngân hàng của mình | P1, P2 | F-10 | BR-21, 50 | M1 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-23 | E3 | Là khách hàng, tôi muốn chuyển tiền từ ví tới tài khoản ngân hàng bất kỳ | P1, P2 | F-11 | BR-22, 25, 50 | M1 | Có | — | [CẦN XÁC NHẬN] | ISS-17 |
| US-24 | E3 | Là khách hàng, tôi muốn giao dịch lớn được bảo vệ thêm bằng OTP + khuôn mặt | P1, P2 | F-07 | BR-35…40 | M0 / M3 | Có | UC-04.02 | [CẦN XÁC NHẬN] | ISS-16 |
| US-25 | E3 | Là khách hàng, tôi muốn biết rõ giao dịch đang xử lý, thành công hay thất bại, và tiền của tôi đang ở đâu | P1, P2 | F-14 | BR-45, 46 | M0 (API) / M4 (push) | Có | UC-07.04 | [CẦN XÁC NHẬN] | ISS-09, ISS-23 |
| US-30 | E4 | Là khách hàng, tôi muốn quét QR tại quầy để trả tiền đơn hàng | P1, P7 | F-12 | BR-51, 52 | M2 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-31 | E4 | Là khách hàng, tôi muốn nhận lại tiền khi merchant hoàn tiền | P1, P7 | F-13 | BR-48 | M2 | Có | — | [CẦN XÁC NHẬN] | ISS-09 |
| US-40 | E5 | Là khách hàng, tôi muốn xem số dư khả dụng và số tiền đang giữ | P1, P2 | F-04 | BR-24, 50 | M0 | Có | UC-07.01 | [ĐÃ XÁC NHẬN] |  |
| US-41 | E5 | Là khách hàng, tôi muốn xem lịch sử giao dịch, lọc theo loại và thời gian | P1, P2 | F-14 | — | M0 (API) / M4 (push) | Có | UC-07.03 | [ĐÃ XÁC NHẬN] |  |
| US-42 | E5 | Là khách hàng, tôi muốn biết hạn mức tháng còn lại | P1, P2 | F-06 | BR-25…28 | M0 | Có | UC-07.02 | [ĐÃ XÁC NHẬN] |  |
| US-43 | E5 | Là khách hàng, tôi muốn đóng ví khi không còn dùng | P1, P2 | F-15 | BR-62 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-44 | E5 | Là khách hàng, tôi muốn nhận thông báo khi tiền vào/ra hoặc giao dịch đổi trạng thái | P1, P2 | F-14 | — | M0 (API) / M4 (push) | **Không** | — | [CẦN XÁC NHẬN] | ISS-26 |
| US-45 | E5 | Là khách hàng lâu không dùng, tôi muốn được báo trước khi ví bị đóng vì không hoạt động | P1, P2 | F-15 | BR-63 | M4 | Có | — | [CẦN XÁC NHẬN] | ISS-20 |
| US-50 | E6 | Là khách hàng, tôi muốn gửi khiếu nại cho một giao dịch (chuyển nhầm, chưa nhận được tiền…) | P1, P3 | F-16 | BR-47, 60 | M4 | Có | — | [CẦN XÁC NHẬN] | ISS-19 |
| US-51 | E6 | Là khách hàng, tôi muốn theo dõi tiến độ và kết quả khiếu nại | P1, P3 | F-16 | BR-60 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-60 | E7 | Là merchant, tôi muốn tạo đơn hàng và nhận QR để khách quét | P7 | F-12, F-23 | BR-51…53 | M2 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-61 | E7 | Là merchant, tôi muốn được báo ngay khi đơn đã thanh toán, và tự tra cứu được nếu không nhận được báo | P7 | F-12 | BR-51 | M2 | Có | — | [MÂU THUẪN] | ISS-10 |
| US-62 | E7 | Là merchant, tôi muốn hoàn tiền toàn bộ hoặc một phần cho đơn đã thanh toán | P7 | F-13 | BR-48 | M2 | Có | — | [MÂU THUẪN] | ISS-10 |
| US-63 | E7 | Là merchant, tôi muốn hủy đơn khách chưa trả | P7 | F-23 | — | M2 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-70 | E8 | Là nhân viên CSKH, tôi muốn tìm khách hàng theo SĐT / mã ví và xem giao dịch, liên kết, trạng thái eKYC để trả lời khách | SUPPORT+ | F-20 | BR-71 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-71 | E8 | Là nhân viên vận hành, tôi muốn duyệt hoặc từ chối hồ sơ eKYC cần kiểm tra thủ công | OPERATOR | F-20 | BR-06 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-72 | E8 | Là nhân viên vận hành, tôi muốn xem ảnh CCCD / selfie khi cần, và mọi lần xem đều được ghi lại | OPERATOR | F-20 | BR-71 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-73 | E8 | Là nhân viên vận hành, tôi muốn phong tỏa / gỡ phong tỏa ví theo yêu cầu hợp lệ | OPERATOR | F-20 | BR-06, 61 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-74 | E8 | Là nhân viên vận hành, tôi muốn thấy và chốt kết quả các giao dịch treo cần đối soát | OPERATOR | F-17 | BR-46, 58 | M1 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-75 | E8 | Là nhân viên vận hành / kiểm soát, tôi muốn xem kết quả đối soát hằng ngày và tình trạng TK đảm bảo | OPERATOR, CHECKER | F-17 | BR-56, 58 | M1 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-76 | E8 | Là nhân viên vận hành, tôi muốn tạo đề nghị điều chỉnh tài chính có lý do để xử lý chênh lệch / khiếu nại | OPERATOR | F-18 | BR-58, 64 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-77 | E8 | Là kiểm soát viên, tôi muốn duyệt hoặc từ chối điều chỉnh do người khác tạo | CHECKER | F-18 | BR-64 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-78 | E8 | Là nhân viên vận hành, tôi muốn xử lý cảnh báo AML, và tạm dừng ví khi cần | OPERATOR | F-19 | BR-61, 74 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-79 | E8 | Là nhân viên CSKH / vận hành, tôi muốn xử lý khiếu nại đúng hạn | SUPPORT, OPERATOR | F-16 | BR-60 | M4 | Có | — | [CẦN XÁC NHẬN] | ISS-19 |
| US-80 | E8 | Là quản trị, tôi muốn chỉnh tham số hạn mức / ngưỡng mà không cần deploy | ADMIN | F-21 | BR-12, 25…28, 36 | M4 | Có | — | [ĐÃ XÁC NHẬN] |  |
| US-81 | E8 | Là kiểm soát viên, tôi muốn tra cứu audit log của mọi thao tác nhạy cảm | CHECKER, ADMIN | F-22 | BR-64, 71 | M0 | Có | UC-09.01 | [ĐÃ XÁC NHẬN] |  |

## C.4. Quy tắc nghiệp vụ (BR)

Nhãn nguồn **[L]** (luật) / **[P]** (quyết định sản phẩm) giữ nguyên từ business-rules.md. Cột "Tham số" là key cấu hình ở business-rules §5.


### A. Khách hàng & mở ví

| ID | Nội dung (nguyên văn) | Nguồn | Tham số §5 | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|
| BR-01 | Chỉ mở ví cho **cá nhân, công dân Việt Nam, từ đủ 18 tuổi**, có CCCD gắn chip còn hiệu lực. | [P] | `customer.min_age` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-02 | Mỗi khách hàng có **1 ví cá nhân**, chỉ dùng VND, số tiền là số nguyên (đồng). | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-03 | Số điện thoại phải được xác thực bằng OTP khi đăng ký và khi đổi số. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-04 | eKYC bắt buộc: OCR CCCD → kiểm tra liveness → **so khớp khuôn mặt với ảnh trong chip CCCD** (hoặc VNeID). Tất cả là provider mock. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-05 | **Không có ví "chưa xác minh" được giao dịch.** Ví chỉ `ACTIVE` khi đã có eKYC đạt **và** ít nhất 1 liên kết ngân hàng. Trước đó chỉ được đăng nhập và xem. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-06 | Vòng đời: `REGISTERED → KYC_PENDING → KYC_VERIFIED → ACTIVE`; ngoài ra có `SUSPENDED` (giấy tờ hết hạn, nghi gian lận), `FROZEN` (bị khóa theo yêu cầu của KH hoặc cơ quan có thẩm quyền), `CLOSED`. eKYC lỗi kỹ thuật → giữ `KYC_PENDING` (retry); eKYC không đạt → `KYC_REJECTED` (cho làm lại tối đa 3 lần/ngày) hoặc chuyển `MANUAL_REVIEW`. | [P] | `kyc.max_attempts_per_day` | Có | [MÂU THUẪN] | ISS-07 |
| BR-07 | Giấy tờ sắp hết hạn: thông báo trước **30 ngày**; tới ngày hết hạn ví tự chuyển `SUSPENDED` cho tới khi eKYC lại. Khi `SUSPENDED` vẫn được xem nhưng **không** nạp/chuyển/thanh toán; được nhận hoàn tiền. | [L] | `kyc.id_expiry_notice_days` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-08 | Lưu đầy đủ dữ liệu nhận biết KH: ảnh CCCD, dữ liệu sinh trắc, thông tin thiết bị, nhật ký eKYC. | [L] | — | Một phần | [ĐÃ XÁC NHẬN] |  |

### B. Liên kết ngân hàng

| ID | Nội dung (nguyên văn) | Nguồn | Tham số §5 | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|
| BR-10 | Chỉ liên kết **tài khoản thanh toán VND hoặc thẻ ghi nợ đứng tên chính chủ ví**. corebank xác minh họ tên + số CCCD khớp với hồ sơ ví. Không chấp nhận TK chung. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-11 | **Không** liên kết thẻ tín dụng làm nguồn nạp. | [L] | — | Có | [CẦN XÁC NHẬN] | ISS-18 |
| BR-12 | Mỗi khách hàng được liên kết tối đa **3** nguồn. | [P] | `banklink.max_per_customer` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-13 | Phải duy trì ≥ 1 liên kết: **không cho hủy liên kết cuối cùng** khi ví còn hoạt động; muốn bỏ hết thì phải đóng ví (BR-62). | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-14 | Liên kết qua OTP do corebank (giả lập) gửi; wallet **không** lưu mật khẩu Internet Banking. Corebank timeout → liên kết giữ `PENDING`, tra cứu lại. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |

### C. Luồng tiền & hạn mức

| ID | Nội dung (nguyên văn) | Nguồn | Tham số §5 | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|
| BR-20 | **Nạp tiền** chỉ từ nguồn đã liên kết của chính chủ. Không nhận tiền mặt. Tối thiểu **10.000đ**/lần. | [L]/[P] | `topup.min_amount` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-21 | **Rút tiền** chỉ về TK/thẻ đã liên kết. Không cho rút tiền mặt. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-22 | **Chuyển ra ngân hàng** (TK bất kỳ, kể cả ngân hàng khác qua mạng giả lập): bắt buộc gửi kèm thông tin tối thiểu của người gửi (họ tên, mã ví) và nội dung chuyển. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-23 | **Chuyển ví–ví** chỉ tới ví `ACTIVE` khác trong hệ thống; hiển thị tên người nhận (đã che một phần) trước khi xác nhận. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-24 | Số dư khả dụng **không bao giờ âm**: không thấu chi, không cấp tín dụng, không trả lãi trên số dư. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-25 | **Hạn mức tháng**: tổng (chuyển ví–ví + chuyển ra ngân hàng **khác TK liên kết** + thanh toán) ≤ **100.000.000đ/tháng/khách hàng**. **Không** tính: nạp tiền, rút về TK liên kết chính chủ, nhận tiền, hoàn tiền. | [L] | `limit.monthly_general` | Một phần (chờ chốt giá trị/phạm vi) | [CẦN XÁC NHẬN] | ISS-17 |
| BR-26 | Thanh toán cho merchant thuộc nhóm `ESSENTIAL` (dịch vụ công, điện, nước, viễn thông, học phí, viện phí, bảo hiểm, trả nợ tại TCTD) có hạn mức cộng thêm; **tổng tối đa 300.000.000đ/tháng**. MVP chưa có merchant thiết yếu nhưng vẫn có cột `merchant.category` và bộ đếm riêng. | [L] | `limit.monthly_with_essential` | Một phần (chờ chốt giá trị/phạm vi) | [CẦN XÁC NHẬN] | ISS-14 |
| BR-27 | "Tháng" = tháng dương lịch theo múi giờ `Asia/Ho_Chi_Minh`; "ngày" tương tự. | [P] | `limit.timezone` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-28 | Theo lần: chuyển/thanh toán tối thiểu **1.000đ**, tối đa **50.000.000đ**/lần. | [P] | `transfer.min_amount` / `payment.min_amount`, `txn.max_amount_per_txn` | Một phần (chờ chốt giá trị/phạm vi) | [CẦN XÁC NHẬN] | ISS-15 |
| BR-29 | Hạn mức được kiểm tra **ở backend, trong cùng DB transaction với bút toán** (khóa bộ đếm hạn mức của KH), không tin client. Giao dịch bị từ chối/đảo thì hoàn lại hạn mức đã dùng. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-30 | Phí: **miễn phí toàn bộ trong MVP**. API và ledger vẫn có trường `fee` và tài khoản `fee_revenue`. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |

### D. Xác thực giao dịch

| ID | Nội dung (nguyên văn) | Nguồn | Tham số §5 | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|
| BR-35 | Mọi giao dịch làm thay đổi tiền (nạp, chuyển, rút, thanh toán) phải xác nhận bằng **PIN 6 số**. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-36 | Nếu **G > 10.000.000đ** hoặc **G + tổng đã giao dịch trong ngày cùng nhóm > 20.000.000đ** → bắt buộc **so khớp khuôn mặt (mock) + OTP** (giao dịch loại C). | [L] | `auth.strong_single_threshold`, `auth.strong_daily_threshold` | Một phần (chờ chốt giá trị/phạm vi) | [CẦN XÁC NHẬN] | ISS-16 |
| BR-37 | Nhóm giao dịch để cộng dồn theo ngày: (a) nạp/rút với TK liên kết chính chủ; (b) các giao dịch còn lại. | [L] | — | Có | [CẦN XÁC NHẬN] | ISS-16 |
| BR-38 | Mỗi KH chỉ có **1 thiết bị đang hoạt động**. Đăng nhập trên thiết bị mới phải qua OTP + so khớp khuôn mặt; thiết bị cũ bị hủy gắn. Trong 24 giờ đầu sau khi đổi thiết bị, áp hạn mức 10.000.000đ/ngày. | [L]/[P] | `auth.new_device_daily_limit` / `auth.new_device_window_hours` | Một phần (chờ chốt giá trị/phạm vi) | [CẦN XÁC NHẬN] | ISS-21 |
| BR-39 | Sai PIN **5 lần liên tiếp** → khóa giao dịch **30 phút**; sai liên tiếp sau đó → khóa tới khi đặt lại PIN (qua OTP + khuôn mặt). | [P] | `auth.pin_max_failures` / `auth.pin_lock_minutes` | Có | [MÂU THUẪN] | ISS-11 |
| BR-40 | Xác thực phải gắn với đúng giao dịch: challenge (OTP/face) chứa `transaction_id`, số tiền, người nhận; dùng một lần, hết hạn sau 5 phút. | [P] | `auth.challenge_ttl_seconds` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-41 | PIN là 6 chữ số, **không** chấp nhận chuỗi lặp (`111111`) hoặc tăng/giảm liên tiếp (`123456`, `654321`) và không trùng ngày sinh dạng `DDMMYY`. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |

### E. Trạng thái giao dịch & xử lý lỗi

| ID | Nội dung (nguyên văn) | Nguồn | Tham số §5 | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|
| BR-45 | Trạng thái payment: `CREATED → PENDING → SUCCEEDED / FAILED`, cùng `NEEDS_RECONCILIATION` và `REVERSED`. Chỉ backend được chuyển trạng thái. | [P] | — | Có | [MÂU THUẪN] | ISS-08 |
| BR-46 | Ngân hàng timeout / không rõ kết quả → hiển thị **"Đang xử lý" (`PENDING`)**, **không bao giờ** tự báo thất bại khi chưa có xác nhận. Worker tra cứu lại với backoff; sau **24 giờ** chưa rõ → `NEEDS_RECONCILIATION`, chuyển cho vận hành. | [P] | `payment.pending_max_hours` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-47 | Chuyển ví–ví là **tức thời, đồng bộ và không hủy được**. Chuyển nhầm → xử lý qua tra soát (BR-60), người nhận phải đồng ý hoàn. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-48 | KH **không được hủy** giao dịch sau khi đã ghi nợ. Sai sót chỉ được sửa bằng **bút toán đảo** (hệ thống/vận hành, maker–checker) hoặc **hoàn tiền** (merchant khởi tạo, ≤ số đã thanh toán, trong 90 ngày). | [P] | `refund.window_days` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-49 | Nạp tiền: chỉ ghi có ví khi corebank **xác nhận** đã ghi nợ TK nguồn. Retry/tra cứu luôn dùng cùng `bank_operation_id`. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-50 | Rút/chuyển ra NH: giữ chỗ (hold) số tiền khi tạo lệnh. Thất bại **chắc chắn** → giải phóng hold ngay. Kết quả không rõ → **giữ nguyên hold** cho tới khi tra cứu hoặc đối soát có kết luận. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-51 | Thanh toán QR: `SUCCEEDED` ngay khi ledger commit (ví là nguồn sự thật). Callback cho merchant được retry với backoff tối đa **24 giờ**; merchant có API tra cứu theo `order_id`. `(merchant_id, order_id)` là **unique**, nên một đơn hàng không thể bị thanh toán 2 lần. | [P] | `merchant.callback_retry_max_hours` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-52 | QR MVP chỉ là **QR động**: có số tiền cố định, `order_id`, hết hạn sau **15 phút**. App lấy thông tin merchant/đơn hàng **từ backend** theo mã trong QR, không tin nội dung QR. | [P] | `qr.ttl_minutes` | Có | [ĐÃ XÁC NHẬN] |  |
| BR-53 | Merchant phải là tổ chức đã ký hợp đồng (seed sẵn trong DB), trạng thái `ACTIVE`. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |

### F. Tài khoản đảm bảo & đối soát

| ID | Nội dung (nguyên văn) | Nguồn | Tham số §5 | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|
| BR-55 | Công ty ví có **tài khoản đảm bảo thanh toán** tại corebank, tách riêng, chỉ dùng cho dịch vụ ví. Mọi dòng tiền thật đi qua tài khoản này: nạp = TK KH → TK đảm bảo; rút/chuyển ra NH = TK đảm bảo → TK đích. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-56 | Bất biến: **số dư TK đảm bảo ≥ tổng số dư (kể cả phần hold) của mọi ví**. Kiểm tra tự động **hằng ngày**; vi phạm → cảnh báo mức nghiêm trọng, tạm dừng rút/chuyển ra NH cho tới khi xử lý. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-57 | Tài khoản ledger `bank_settlement` phản ánh TK đảm bảo; `wallet_hold` cho tiền giữ chỗ; `merchant_payable` cho tiền phải trả merchant (quyết toán merchant: ngoài MVP, chỉ ghi nhận công nợ). | [P] | — | Có | [MÂU THUẪN] | ISS-13 |
| BR-58 | **Đối soát T+1** với corebank theo danh sách bank operations trong ngày. Xử lý chênh lệch: NH đã ghi nợ mà ví chưa ghi có → **ghi có bù**; ví đã ghi có mà NH không ghi nợ → **bút toán đảo**; payout không rõ → chốt theo kết quả của NH. Mọi điều chỉnh có audit (người làm, người duyệt, lý do). | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |

### G. Vận hành, khiếu nại, đóng ví

| ID | Nội dung (nguyên văn) | Nguồn | Tham số §5 | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|
| BR-60 | Tra soát/khiếu nại: KH gửi trong vòng **60 ngày** kể từ ngày giao dịch; trả lời trong tối đa **15 ngày làm việc**; có mã yêu cầu và trạng thái theo dõi. | [L]/[P] | `dispute.submit_window_days` / `dispute.resolve_business_days` | Một phần (chờ chốt giá trị/phạm vi) | [CẦN XÁC NHẬN] | ISS-19 |
| BR-61 | Từ chối/tạm dừng giao dịch khi nghi gian lận (theo AML rule hoặc bộ tiêu chí rủi ro) → ví `SUSPENDED` và tạo case cho vận hành. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-62 | **Đóng ví** theo yêu cầu: không còn giao dịch `PENDING`/hold; số dư phải rút hết về TK liên kết trước khi đóng. | [L]/[P] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-63 | Ví **không phát sinh giao dịch trong 12 tháng** → thông báo trước 30 ngày rồi đóng; số dư tự hoàn về TK liên kết. | [P] | `wallet.dormant_close_months` / `wallet.dormant_notice_days` | Một phần (chờ chốt giá trị/phạm vi) | [CẦN XÁC NHẬN] | ISS-20 |
| BR-64 | Back-office **không** sửa số dư hoặc trạng thái giao dịch trực tiếp. Mọi điều chỉnh tài chính qua nghiệp vụ điều chỉnh, **maker–checker** (2 người khác nhau), bắt buộc ghi lý do. | [P] | — | Có | [ĐÃ XÁC NHẬN] |  |

### H. Dữ liệu cá nhân, PCRT, lưu trữ

| ID | Nội dung (nguyên văn) | Nguồn | Tham số §5 | Kiểm chứng được | Nhãn | Vấn đề |
|---|---|---|---|---|---|---|
| BR-70 | Màn hình đồng ý xử lý dữ liệu nêu rõ mục đích, loại dữ liệu, thời hạn lưu, bên nhận chia sẻ (corebank); **đồng ý riêng** cho dữ liệu sinh trắc học; lưu phiên bản điều khoản đã đồng ý. | [L] | — | Một phần (review nội dung màn hình) | [ĐÃ XÁC NHẬN] |  |
| BR-71 | Mã hóa khi lưu: ảnh CCCD, dữ liệu khuôn mặt, số CCCD (lưu thêm hash để tra cứu). Ảnh lưu trong private object storage. Log/audit không chứa số CCCD đầy đủ, ảnh, OTP, PIN, token. | [L] | — | Có | [ĐÃ XÁC NHẬN] |  |
| BR-72 | Có runbook xử lý sự cố lộ dữ liệu, thông báo trong **72 giờ**. | [L] | — | Chưa (chưa có runbook) | [CẦN XÁC NHẬN] | ISS-30 |
| BR-73 | Thời hạn lưu: hồ sơ KYC **5 năm** kể từ khi đóng ví; dữ liệu giao dịch tối thiểu 5 năm, mặc định **10 năm** cho ledger vì là chứng từ kế toán. | [L]/[P] | `retention.kyc_years_after_close` / `retention.ledger_years` | Một phần (chỉ kiểm được cấu hình job) | [ĐÃ XÁC NHẬN] |  |
| BR-74 | AML cơ bản: bảng `aml_alerts` với các rule: (a) chuyển tiền điện tử ≥ ngưỡng báo cáo (mặc định 500.000.000đ, cộng dồn theo ngày); (b) nạp xong rút/chuyển ra gần như toàn bộ trong 24 giờ; (c) ≥ 20 giao dịch nhận từ ≥ 10 ví khác nhau trong 24 giờ. Alert chỉ tạo case để vận hành xem, không tự khóa ví (trừ khi vận hành quyết định). | [L]/[P] | `aml.ewire_report_threshold` | Có | [CẦN XÁC NHẬN] | ISS-18 |

## C.5. Yêu cầu phi chức năng

### Hệ thống (NFR, prd §8)

| ID | Nhóm | Yêu cầu (nguyên văn) | Cách kiểm chứng hiện có | Nhãn | Vấn đề |
|---|---|---|---|---|---|
| NFR-01 | Toàn vẹn | Ledger double-entry, bất biến được ép ở tầng DB; số dư không âm; job phát hiện drift hằng ngày | Trigger DB (erd §4.4), job drift, test SM-01 | [ĐÃ XÁC NHẬN] |  |
| NFR-02 | Idempotency | Mọi lệnh tạo giao dịch và lệnh tích hợp đều idempotent (`Idempotency-Key`, `bank_operation_id`) | AC-2, AC-7 của UC-04.01; test idempotency | [ĐÃ XÁC NHẬN] | ISS-01 |
| NFR-03 | Nhất quán | Ưu tiên nhất quán hơn sẵn sàng; kết quả chưa rõ thì giữ `PENDING`, không đoán | Chưa có — cần kịch bản trong `failure-scenarios.md` | [CẦN XÁC NHẬN] | ISS-24 |
| NFR-04 | Bảo mật dữ liệu | Mã hóa field-level cho CCCD và sinh trắc học; log tự che dữ liệu nhạy cảm; ảnh lưu trong bucket private | Review cấu hình mã hóa + test redaction log | [ĐÃ XÁC NHẬN] |  |
| NFR-05 | Xác thực | JWT 15 phút, refresh token xoay vòng, gắn thiết bị; PIN băm argon2id; step-up theo ngưỡng | Test xác thực (M3) | [ĐÃ XÁC NHẬN] |  |
| NFR-06 | Kiểm toán | Mọi thay đổi tiền và trạng thái đều có lịch sử / audit kèm người thực hiện, lý do, `request_id` | AC-4 UC-09.01; status_history | [ĐÃ XÁC NHẬN] |  |
| NFR-07 | Hiệu năng | Local: chuyển ví–ví P95 < 300 ms; ≥ 100 lệnh ví–ví/giây trên một máy dev | Test tải; chưa có kịch bản đo cụ thể (số ví, dữ liệu) | [CẦN XÁC NHẬN] | ISS-22 |
| NFR-08 | Phục hồi | Worker crash không mất job (outbox + lease); transaction đã commit không bao giờ mất | Test kill worker; chưa có kịch bản viết sẵn | [CẦN XÁC NHẬN] | ISS-24 |
| NFR-09 | Quan sát | Trace xuyên suốt mobile → api → worker → corebank; metrics và alert tối thiểu | Chưa có tiêu chí đo (trace "xuyên suốt" kiểm thế nào) | [CẦN XÁC NHẬN] | ISS-22 |
| NFR-10 | Lưu trữ | Theo BR-73: KYC 5 năm sau khi đóng ví, ledger 10 năm | Kiểm cấu hình job retention | [ĐÃ XÁC NHẬN] |  |
| NFR-11 | Kiểm thử được | corebank có fault injection xác định; có test kiến trúc cho import graph | archtest đã có; fault injection phụ thuộc M1 | [CẦN XÁC NHẬN] | ISS-24 |
| NFR-12 | Ngôn ngữ | Giao diện và thông báo lỗi bằng tiếng Việt; API trả mã lỗi máy đọc được | Review UI + catalogue mã lỗi | [ĐÃ XÁC NHẬN] | ISS-37 |

### Người dùng (UR-NF, urd §3)

| ID | Yêu cầu (nguyên văn) | Đo bằng (nguồn) | Đánh giá | Nhãn | Vấn đề |
|---|---|---|---|---|---|
| UR-NF-01 | Toàn bộ giao diện và thông báo bằng tiếng Việt, tránh thuật ngữ kỹ thuật ("Đang xử lý" thay vì `PENDING`) | Review UI | Kiểm chứng được | [ĐÃ XÁC NHẬN] |  |
| UR-NF-02 | Giao dịch đồng bộ (chuyển ví–ví, QR) có phản hồi trong ~1 giây | NFR-07 | "~1 giây" không có ngưỡng chính xác; NFR-07 dùng P95 < 300 ms, SM-03 dùng < 1 s | [CẦN XÁC NHẬN] | ISS-22 |
| UR-NF-03 | Không bao giờ báo "thất bại" khi hệ thống chưa chắc chắn | BR-46 | Kiểm chứng được | [ĐÃ XÁC NHẬN] |  |
| UR-NF-04 | Thao tác lặp (bấm 2 lần, mất mạng rồi gửi lại) không gây trừ tiền 2 lần | NFR-02 | Kiểm chứng được | [ĐÃ XÁC NHẬN] |  |
| UR-NF-05 | Thông tin nhạy cảm (CCCD, số TK) luôn hiển thị đã che, trừ khi người dùng chủ động xem và có quyền | BR-71 | Kiểm chứng được | [ĐÃ XÁC NHẬN] |  |
| UR-NF-06 | Thông báo lỗi luôn nói người dùng có thể làm gì tiếp (nạp thêm, chờ, gửi tra soát…) | Review UI | Kiểm chứng được | [ĐÃ XÁC NHẬN] |  |
| UR-NF-07 | Người lớn tuổi (P2) hoàn tất đăng ký → eKYC → liên kết trong ≤ 10 phút, không cần hỗ trợ | Kiểm thử người dùng | Chưa có quy trình kiểm thử người dùng (số người, kịch bản) | [CẦN XÁC NHẬN] | ISS-22 |

## C.6. Use case (UC)

| UC | Tên | File | Milestone | Trạng thái nguồn | Nhãn |
|---|---|---|---|---|---|
| UC-04.01 | Chuyển tiền ví–ví bằng SĐT | fsd/04 | M0 | Draft | [CẦN XÁC NHẬN] |
| UC-04.02 | Xác thực giao dịch bằng PIN | fsd/04 | M0 | Draft | [CẦN XÁC NHẬN] |
| UC-07.01 | Xem ví và số dư | fsd/07 | M0 | Draft | [CẦN XÁC NHẬN] |
| UC-07.02 | Xem hạn mức | fsd/07 | M0 | Draft | [CẦN XÁC NHẬN] |
| UC-07.03 | Xem lịch sử giao dịch | fsd/07 | M0 | Draft | [CẦN XÁC NHẬN] |
| UC-07.04 | Xem chi tiết và theo dõi trạng thái giao dịch | fsd/07 | M0 | Draft | [CẦN XÁC NHẬN] |
| UC-07.05 | Đóng ví theo yêu cầu | fsd/07 | M4 | Chưa viết | [TBD] |
| UC-07.06 | Đóng ví do không hoạt động | fsd/07 | M4 | Chưa viết | [TBD] |
| UC-07.07 | Nhận thông báo giao dịch | fsd/07 | M4 | Chưa viết | [TBD] |
| UC-09.01 | Tra cứu audit log | fsd/09 | M0 | Draft | [CẦN XÁC NHẬN] |
| UC-09.02 | Đăng nhập back-office | fsd/09 | M4 (M0 dùng tài khoản seed) | Chưa viết | [TBD] |
| UC-09.03 | Tra cứu khách hàng | fsd/09 | M4 | Chưa viết | [TBD] |
| UC-09.04 | Duyệt eKYC thủ công, xem ảnh giấy tờ | fsd/09 | M4 | Chưa viết | [TBD] |
| UC-09.05 | Phong tỏa / gỡ phong tỏa ví | fsd/09 | M4 | Chưa viết | [TBD] |
| UC-09.06 | Chốt giao dịch cần đối soát | fsd/09 | M1 | Chưa viết | [TBD] |
| UC-09.07 | Xem kết quả đối soát T+1 và TK đảm bảo | fsd/09 | M1 | Chưa viết | [TBD] |
| UC-09.08 | Tạo / duyệt điều chỉnh tài chính (maker–checker) | fsd/09 | M4 | Chưa viết | [TBD] |
| UC-09.09 | Xử lý cảnh báo AML | fsd/09 | M4 | Chưa viết | [TBD] |
| UC-09.10 | Xử lý khiếu nại | fsd/09 | M4 | Chưa viết | [TBD] |
| UC-09.11 | Quản trị tham số cấu hình | fsd/09 | M4 | Chưa viết | [TBD] |

UC đã viết có nhãn [CẦN XÁC NHẬN] vì các file fsd/ đang ở trạng thái DRAFT. File fsd/01, 02, 03, 05, 06, 08 chưa có UC nào (ISS-25).

## C.7. Hành vi dùng chung (CF) và yêu cầu audit (AU)

| ID | Tên | Nguồn | Nhãn | Vấn đề |
|---|---|---|---|---|
| CF-01 | Tiền và thời gian | fsd/README §4 | [CẦN XÁC NHẬN] | ISS-13 |
| CF-02 | Che dữ liệu | fsd/README §4 | [CẦN XÁC NHẬN] |  |
| CF-03 | Trạng thái giao dịch hiển thị | fsd/README §4 | [CẦN XÁC NHẬN] |  |
| CF-04 | Chống gửi trùng (idempotency) | fsd/README §4 | [MÂU THUẪN] | ISS-01 |
| CF-05 | Xác thực bổ sung (step-up) | fsd/README §4 | [CẦN XÁC NHẬN] |  |
| CF-06 | Lỗi mạng, timeout, lỗi hệ thống | fsd/README §4 | [CẦN XÁC NHẬN] |  |
| CF-07 | Phiên đăng nhập | fsd/README §4 | [CẦN XÁC NHẬN] |  |
| CF-08 | Nhãn trạng thái ví | fsd/README §4 | [CẦN XÁC NHẬN] |  |
| AU-1 | Ghi audit cho: mọi thao tác của **staff** (kể cả chỉ xem dữ liệu nhạy cảm), mọi thao tác **bảo mật** của khách hàng (khóa PIN, đổi thiết bị, đặt lại PIN, rút đồng ý), mọi quyết định của **hệ thống** làm thay đổi trạng thái ví / khách hàng. | fsd/09 §1.1 | [CẦN XÁC NHẬN] | |
| AU-2 | **Không** ghi audit cho từng giao dịch tiền thông thường: đã có `ledger.*`, `payment.payments`, `payment.status_history`. | fsd/09 §1.1 | [CẦN XÁC NHẬN] | |
| AU-3 | Ghi cả khi bị từ chối (`outcome = DENIED`, ví dụ sai role) hoặc lỗi (`ERROR`), không chỉ khi thành công. | fsd/09 §1.1 | [CẦN XÁC NHẬN] | |
| AU-4 | Thao tác **thay đổi dữ liệu** của staff bắt buộc có `reason` ≥ 10 ký tự; thiếu → `400 VALIDATION_ERROR`, thao tác không thực hiện. | fsd/09 §1.1 | [CẦN XÁC NHẬN] | |
| AU-5 | Audit của thao tác thay đổi dữ liệu được ghi **trong cùng DB transaction** với thao tác. Thao tác chỉ đọc (xem ảnh, tìm kiếm) ghi ngay khi trả kết quả. | fsd/09 §1.1 | [CẦN XÁC NHẬN] | |
| AU-6 | `audit.audit_logs` là append-only; không có API sửa hoặc xóa. Lưu theo erd §8. | fsd/09 §1.1 | [CẦN XÁC NHẬN] | |
| AU-7 | **Cấm** ghi vào bất kỳ trường nào (kể cả `metadata`): số CCCD đầy đủ, ảnh, dữ liệu khuôn mặt, OTP, PIN, token, số TK đầy đủ. Chỉ ghi dạng đã che (CF-02) hoặc ID. | fsd/09 §1.1 | [CẦN XÁC NHẬN] | |

## C.8. Thống kê

| Loại | Tổng | [ĐÃ XÁC NHẬN] | [CẦN XÁC NHẬN] | [MÂU THUẪN] | [TBD] |
|---|---|---|---|---|---|
| F | 23 | 19 | 2 | 2 | 0 |
| US | 43 | 30 | 10 | 3 | 0 |
| BR | 54 | 39 | 11 | 4 | 0 |
| UC | 20 | 0 | 7 | 0 | 13 |

US không có AC: US-11, US-44. BR không được US nào trích dẫn trực tiếp (vẫn hợp lệ vì là quy tắc hệ thống): BR-02, BR-08, BR-30, BR-55, BR-57, BR-72, BR-73.
