# Business Rules — E-Wallet MVP

> Trạng thái: **DRAFT — chờ review** · Cập nhật: 2026-10-16
> Đổi tên từ `scope.md`. Liên quan: [README](README.md) · [prd.md](prd.md) · [urd.md](urd.md) · [fsd/](fsd/)

## 1. Mục đích & disclaimer

Tài liệu này chốt **phạm vi MVP** và **quy tắc nghiệp vụ** cho `wallet-core`, là nguồn duy nhất cho mọi quy tắc `BR-xx` được URD, FSD, ERD và API tham chiếu.

- Mỗi quy tắc có mã `BR-xx` và nhãn:
  - **[L]** — xuất phát từ quy định pháp luật hiện hành (có căn cứ).
  - **[P]** — quyết định sản phẩm (luật không quy định cụ thể hoặc đã được đơn giản hóa cho MVP).
- Mọi con số (hạn mức, ngưỡng, thời hạn) là **tham số cấu hình** (§5), không hard-code.
- Đây là project học tập/demo, **không xử lý tiền thật, không phải tư vấn pháp lý**. Căn cứ được tổng hợp từ văn bản hợp nhất và nguồn thứ cấp (tháng 10/2026); trước khi dùng thật phải đối chiếu văn bản gốc trên Công báo / vbpl.vn.

## 2. Căn cứ pháp lý

| Mã | Văn bản | Nội dung áp dụng | Độ tin cậy |
|---|---|---|---|
| **TT40** | Thông tư 40/2024/TT-NHNN về dịch vụ trung gian thanh toán, sửa đổi bởi **TT 41/2025/TT-NHNN** (hợp nhất tại VBHN 76/VBHN-NHNN 2026) | Đối tượng mở ví (Đ17), hồ sơ & eKYC (Đ18, 22), liên kết TK/thẻ ghi nợ chính chủ (Đ24), nạp/rút/sử dụng ví, cấm tiền mặt/tín dụng/trả lãi (Đ25), hạn mức (Đ26), tài khoản đảm bảo (Đ27), giấy tờ hết hạn & quản lý rủi ro (Đ28) | Cao |
| **TT45** | Thông tư 45/2025/TT-NHNN sửa TT 18/2024 về thẻ ngân hàng (hiệu lực 05/01/2026) | Không dùng thẻ tín dụng nạp tiền vào ví | Trung bình |
| **TT50** | Thông tư 50/2024/TT-NHNN về an toàn bảo mật dịch vụ trực tuyến, sửa đổi bởi **TT 77/2025/TT-NHNN** (phần KH cá nhân hiệu lực 01/07/2026) | Phân loại giao dịch A/B/C/D và hình thức xác thực tối thiểu; yêu cầu bảo mật thiết bị/ứng dụng | **Thấp–trung bình** (nguồn lệch nhau về ngưỡng) |
| **PCRT** | Luật Phòng, chống rửa tiền 2022; QĐ 11/2023/QĐ-TTg; TT 27/2025/TT-NHNN | Trung gian thanh toán là đối tượng báo cáo; lưu hồ sơ 5 năm (Đ38); ngưỡng báo cáo chuyển tiền điện tử | Trung bình |
| **BVDLCN** | Luật Bảo vệ dữ liệu cá nhân 2025 + NĐ 356/2025/NĐ-CP (hiệu lực 01/01/2026, thay NĐ 13/2023) | Sinh trắc học là dữ liệu nhạy cảm; đồng ý rõ ràng; mã hóa; thông báo sự cố trong 72 giờ | Trung bình |

Nguồn tham khảo:

- [VBHN 76/VBHN-NHNN 2026 — hợp nhất TT40 (LuatVietnam)](https://luatvietnam.vn/tai-chinh/van-ban-hop-nhat-76-vbhn-nhnn-2026-quy-dinh-hoat-dong-cung-ung-dich-vu-trung-gian-thanh-toan-440839-d5.html)
- [Thông tư 41/2025/TT-NHNN (Thư viện Pháp luật)](https://thuvienphapluat.vn/van-ban/Tien-te-Ngan-hang/Thong-tu-41-2025-TT-NHNN-sua-doi-Thong-tu-40-2024-TT-NHNN-679853.aspx)
- [Quy định mới về ví điện tử: tăng an toàn, mở rộng hạn mức, siết định danh (VNBA)](https://vnba.org.vn/vi/quy-dinh-moi-ve-vi-dien-tu--tang-an-toan--mo-rong-han-muc--siet-dinh-danh-19550.htm)
- [Từ 1/1/2026 mở ví điện tử phải xác minh sinh trắc học (Tuổi Trẻ)](https://tuoitre.vn/plo/tu-1-1-2026-muon-mo-vi-dien-tu-phai-xac-minh-thong-tin-sinh-trac-hoc-post880385.html)
- [Không được chuyển tiền từ thẻ tín dụng vào ví điện tử từ 2026 (Luật Việt An)](https://luatvietan.vn/tai-sao-khong-duoc-chuyen-tien-tu-the-tin-dung-vao-vi-dien-tu-tu-2026.html)
- [Thông tư 50/2024/TT-NHNN (Thư viện Pháp luật)](https://thuvienphapluat.vn/van-ban/Tien-te-Ngan-hang/Thong-tu-50-2024-TT-NHNN-an-toan-bao-mat-cung-cap-dich-vu-truc-tuyen-nganh-Ngan-hang-327954.aspx)
- [Thông tư 77/2025/TT-NHNN (Thư viện Pháp luật)](https://thuvienphapluat.vn/van-ban/Tien-te-Ngan-hang/Thong-tu-77-2025-TT-NHNN-sua-doi-Thong-tu-50-2024-TT-NHNN-688630.aspx)
- [Phân tích TT 77/2025 — bảng phân loại giao dịch](https://impact.boss.io.vn/2026/03/thong-tu-772025tt-nhnn-buoc-chuyen-tu.html)
- [Luật Phòng, chống rửa tiền 2022](https://thuvienphapluat.vn/van-ban/Tien-te-Ngan-hang/Luat-Phong-chong-rua-tien-2022-519327.aspx)
- [TT 27/2025/TT-NHNN — thay đổi trong PCRT (Luật Việt An)](https://luatvietan.vn/thong-tu-27-2025-tt-nhnn-thay-doi-lon-trong-phong-chong-rua-tien-tai-viet-nam.html)
- [NĐ 356/2025/NĐ-CP hướng dẫn Luật BVDLCN](https://thuvienphapluat.vn/van-ban/Quyen-dan-su/Nghi-dinh-356-2025-ND-CP-huong-dan-Luat-Bao-ve-du-lieu-ca-nhan-687428.aspx)
- [Luật BVDLCN 2025 và tác động tới ngành ngân hàng (Tạp chí Ngân hàng)](https://tapchinganhang.gov.vn/luat-bao-ve-du-lieu-ca-nhan-nam-2025-va-nhung-tac-dong-doi-voi-linh-vuc-ngan-hang-tai-viet-nam-17211.html)

## 3. Phạm vi MVP

### Trong phạm vi

| Nhóm | Chức năng |
|---|---|
| Onboarding | Đăng ký SĐT + OTP, eKYC mô phỏng (CCCD gắn chip + khuôn mặt), đồng ý xử lý dữ liệu, thiết lập PIN, gắn thiết bị |
| Liên kết NH | Liên kết/hủy liên kết TK thanh toán / thẻ ghi nợ chính chủ tại corebank |
| Luồng tiền | Nạp tiền, chuyển ví–ví, rút về TK liên kết, chuyển ra TK ngân hàng bất kỳ, thanh toán QR động cho merchant, hoàn tiền do merchant khởi tạo |
| Tra cứu | Số dư, lịch sử, trạng thái giao dịch, hạn mức còn lại trong tháng |
| Vận hành | Đối soát T+1 với corebank, kiểm tra bất biến tài khoản đảm bảo, tra soát/khiếu nại, AML alert cơ bản, audit |

### Ngoài phạm vi MVP

Ví tổ chức / hộ kinh doanh; ví cho người dưới 18 tuổi hoặc người nước ngoài; merchant là cá nhân; QR tĩnh; VietQR/NAPAS thật; thanh toán hóa đơn thiết yếu (điện, nước, viễn thông…); ngoại tệ, thanh toán xuyên biên giới; voucher, cashback, loyalty; mọi tính năng tín dụng/trả sau; chữ ký số, FIDO (loại giao dịch D).

## 4. Quy tắc nghiệp vụ

### A. Khách hàng & mở ví

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú |
|---|---|---|---|
| BR-01 | Chỉ mở ví cho **cá nhân, công dân Việt Nam, từ đủ 18 tuổi**, có CCCD gắn chip còn hiệu lực. | [P] | TT40 Đ17 cho phép người từ đủ 15 tuổi và trẻ dưới 15 tuổi qua người đại diện; MVP đơn giản hóa còn từ đủ 18 tuổi. |
| BR-02 | Mỗi khách hàng có **1 ví cá nhân**, chỉ dùng VND, số tiền là số nguyên (đồng). | [P] | |
| BR-03 | Số điện thoại phải được xác thực bằng OTP khi đăng ký và khi đổi số. | [L] | TT41 — xác thực SĐT khi mở ví/giao dịch. |
| BR-04 | eKYC bắt buộc: OCR CCCD → kiểm tra liveness → **so khớp khuôn mặt với ảnh trong chip CCCD** (hoặc VNeID). Tất cả là provider mock. | [L] | TT40 Đ22 (sửa bởi TT41, hiệu lực 01/01/2026). |
| BR-05 | **Không có ví "chưa xác minh" được giao dịch.** Ví chỉ `ACTIVE` khi đã có eKYC đạt **và** ít nhất 1 liên kết ngân hàng. Trước đó chỉ được đăng nhập và xem. | [L] | TT40 Đ24.3 (chưa liên kết thì không được dùng ví), Đ25.6 (phải đối chiếu sinh trắc trước khi giao dịch). Trả lời câu hỏi "ví chưa xác minh có được nạp tiền?" → **Không**. |
| BR-06 | Vòng đời: `REGISTERED → KYC_PENDING → KYC_VERIFIED → ACTIVE`; ngoài ra có `SUSPENDED` (giấy tờ hết hạn, nghi gian lận), `FROZEN` (bị khóa theo yêu cầu của KH hoặc cơ quan có thẩm quyền), `CLOSED`. eKYC lỗi kỹ thuật → giữ `KYC_PENDING` (retry); eKYC không đạt → `KYC_REJECTED` (cho làm lại tối đa 3 lần/ngày) hoặc chuyển `MANUAL_REVIEW`. | [P] | |
| BR-07 | Giấy tờ sắp hết hạn: thông báo trước **30 ngày**; tới ngày hết hạn ví tự chuyển `SUSPENDED` cho tới khi eKYC lại. Khi `SUSPENDED` vẫn được xem nhưng **không** nạp/chuyển/thanh toán; được nhận hoàn tiền. | [L] | TT40 Đ28.3. |
| BR-08 | Lưu đầy đủ dữ liệu nhận biết KH: ảnh CCCD, dữ liệu sinh trắc, thông tin thiết bị, nhật ký eKYC. | [L] | TT40 Đ22.2c, TT41. |

### B. Liên kết ngân hàng

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú |
|---|---|---|---|
| BR-10 | Chỉ liên kết **tài khoản thanh toán VND hoặc thẻ ghi nợ đứng tên chính chủ ví**. corebank xác minh họ tên + số CCCD khớp với hồ sơ ví. Không chấp nhận TK chung. | [L] | TT40 Đ17, Đ24. |
| BR-11 | **Không** liên kết thẻ tín dụng làm nguồn nạp. | [L] | TT45. |
| BR-12 | Mỗi khách hàng được liên kết tối đa **3** nguồn. | [P] | Luật cho phép nhiều; giới hạn để MVP gọn. |
| BR-13 | Phải duy trì ≥ 1 liên kết: **không cho hủy liên kết cuối cùng** khi ví còn hoạt động; muốn bỏ hết thì phải đóng ví (BR-62). | [L] | TT40 Đ24 — duy trì liên kết suốt thời gian sử dụng ví. |
| BR-14 | Liên kết qua OTP do corebank (giả lập) gửi; wallet **không** lưu mật khẩu Internet Banking. Corebank timeout → liên kết giữ `PENDING`, tra cứu lại. | [P] | fsd/02. |

### C. Luồng tiền & hạn mức

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú |
|---|---|---|---|
| BR-20 | **Nạp tiền** chỉ từ nguồn đã liên kết của chính chủ. Không nhận tiền mặt. Tối thiểu **10.000đ**/lần. | [L]/[P] | TT40 Đ25.5. Mức tối thiểu là [P]. |
| BR-21 | **Rút tiền** chỉ về TK/thẻ đã liên kết. Không cho rút tiền mặt. | [L] | TT40 Đ25. |
| BR-22 | **Chuyển ra ngân hàng** (TK bất kỳ, kể cả ngân hàng khác qua mạng giả lập): bắt buộc gửi kèm thông tin tối thiểu của người gửi (họ tên, mã ví) và nội dung chuyển. | [L] | TT41 — tổ chức chuyển đi phải cung cấp thông tin tối thiểu cho bên thụ hưởng. |
| BR-23 | **Chuyển ví–ví** chỉ tới ví `ACTIVE` khác trong hệ thống; hiển thị tên người nhận (đã che một phần) trước khi xác nhận. | [P] | |
| BR-24 | Số dư khả dụng **không bao giờ âm**: không thấu chi, không cấp tín dụng, không trả lãi trên số dư. | [L] | TT40 Đ25 (sửa bởi TT41). |
| BR-25 | **Hạn mức tháng**: tổng (chuyển ví–ví + chuyển ra ngân hàng **khác TK liên kết** + thanh toán) ≤ **100.000.000đ/tháng/khách hàng**. **Không** tính: nạp tiền, rút về TK liên kết chính chủ, nhận tiền, hoàn tiền. | [L] | TT40 Đ26 (sửa bởi TT41). Cách phân loại giao dịch tính hạn mức → **cần đối chiếu** (§6). |
| BR-26 | Thanh toán cho merchant thuộc nhóm `ESSENTIAL` (dịch vụ công, điện, nước, viễn thông, học phí, viện phí, bảo hiểm, trả nợ tại TCTD) có hạn mức cộng thêm; **tổng tối đa 300.000.000đ/tháng**. MVP chưa có merchant thiết yếu nhưng vẫn có cột `merchant.category` và bộ đếm riêng. | [L] | TT41 sửa Đ26. |
| BR-27 | "Tháng" = tháng dương lịch theo múi giờ `Asia/Ho_Chi_Minh`; "ngày" tương tự. | [P] | |
| BR-28 | Theo lần: chuyển/thanh toán tối thiểu **1.000đ**, tối đa **50.000.000đ**/lần. | [P] | |
| BR-29 | Hạn mức được kiểm tra **ở backend, trong cùng DB transaction với bút toán** (khóa bộ đếm hạn mức của KH), không tin client. Giao dịch bị từ chối/đảo thì hoàn lại hạn mức đã dùng. | [P] | Tránh race giữa 2 giao dịch đồng thời. |
| BR-30 | Phí: **miễn phí toàn bộ trong MVP**. API và ledger vẫn có trường `fee` và tài khoản `fee_revenue`. | [P] | |

### D. Xác thực giao dịch

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú |
|---|---|---|---|
| BR-35 | Mọi giao dịch làm thay đổi tiền (nạp, chuyển, rút, thanh toán) phải xác nhận bằng **PIN 6 số**. | [P] | Tối thiểu cho loại A/B theo TT50; PIN dùng chung cho mọi loại để đơn giản. |
| BR-36 | Nếu **G > 10.000.000đ** hoặc **G + tổng đã giao dịch trong ngày cùng nhóm > 20.000.000đ** → bắt buộc **so khớp khuôn mặt (mock) + OTP** (giao dịch loại C). | [L] | TT50 Phụ lục 01–02, sửa bởi TT77. **Ngưỡng cần đối chiếu** (§6). |
| BR-37 | Nhóm giao dịch để cộng dồn theo ngày: (a) nạp/rút với TK liên kết chính chủ; (b) các giao dịch còn lại. | [L] | TT50 tách riêng nhóm nạp/rút với TK chính chủ. |
| BR-38 | Mỗi KH chỉ có **1 thiết bị đang hoạt động**. Đăng nhập trên thiết bị mới phải qua OTP + so khớp khuôn mặt; thiết bị cũ bị hủy gắn. Trong 24 giờ đầu sau khi đổi thiết bị, áp hạn mức 10.000.000đ/ngày. | [L]/[P] | TT50/TT77 có yêu cầu bảo mật thiết bị; các con số là [P]. |
| BR-39 | Sai PIN **5 lần liên tiếp** → khóa giao dịch **30 phút**; sai liên tiếp sau đó → khóa tới khi đặt lại PIN (qua OTP + khuôn mặt). | [P] | |
| BR-40 | Xác thực phải gắn với đúng giao dịch: challenge (OTP/face) chứa `transaction_id`, số tiền, người nhận; dùng một lần, hết hạn sau 5 phút. | [P] | Chống replay. |
| BR-41 | PIN là 6 chữ số, **không** chấp nhận chuỗi lặp (`111111`) hoặc tăng/giảm liên tiếp (`123456`, `654321`) và không trùng ngày sinh dạng `DDMMYY`. | [P] | Bổ sung từ URD US-03. |

### E. Trạng thái giao dịch & xử lý lỗi

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú |
|---|---|---|---|
| BR-45 | Trạng thái payment: `CREATED → PENDING → SUCCEEDED / FAILED`, cùng `NEEDS_RECONCILIATION` và `REVERSED`. Chỉ backend được chuyển trạng thái. | [P] | |
| BR-46 | Ngân hàng timeout / không rõ kết quả → hiển thị **"Đang xử lý" (`PENDING`)**, **không bao giờ** tự báo thất bại khi chưa có xác nhận. Worker tra cứu lại với backoff; sau **24 giờ** chưa rõ → `NEEDS_RECONCILIATION`, chuyển cho vận hành. | [P] | Trả lời câu "ngân hàng timeout thì hiển thị gì". |
| BR-47 | Chuyển ví–ví là **tức thời, đồng bộ và không hủy được**. Chuyển nhầm → xử lý qua tra soát (BR-60), người nhận phải đồng ý hoàn. | [P] | |
| BR-48 | KH **không được hủy** giao dịch sau khi đã ghi nợ. Sai sót chỉ được sửa bằng **bút toán đảo** (hệ thống/vận hành, maker–checker) hoặc **hoàn tiền** (merchant khởi tạo, ≤ số đã thanh toán, trong 90 ngày). | [P] | Trả lời câu "có cho phép hủy sau khi ghi nợ". |
| BR-49 | Nạp tiền: chỉ ghi có ví khi corebank **xác nhận** đã ghi nợ TK nguồn. Retry/tra cứu luôn dùng cùng `bank_operation_id`. | [P] | architecture §5.3. |
| BR-50 | Rút/chuyển ra NH: giữ chỗ (hold) số tiền khi tạo lệnh. Thất bại **chắc chắn** → giải phóng hold ngay. Kết quả không rõ → **giữ nguyên hold** cho tới khi tra cứu hoặc đối soát có kết luận. | [P] | erd §3.2 (dòng 3a–3c). |
| BR-51 | Thanh toán QR: `SUCCEEDED` ngay khi ledger commit (ví là nguồn sự thật). Callback cho merchant được retry với backoff tối đa **24 giờ**; merchant có API tra cứu theo `order_id`. `(merchant_id, order_id)` là **unique**, nên một đơn hàng không thể bị thanh toán 2 lần. | [P] | Trả lời câu "merchant đã nhận nhưng ví chưa nhận callback". |
| BR-52 | QR MVP chỉ là **QR động**: có số tiền cố định, `order_id`, hết hạn sau **15 phút**. App lấy thông tin merchant/đơn hàng **từ backend** theo mã trong QR, không tin nội dung QR. | [P] | |
| BR-53 | Merchant phải là tổ chức đã ký hợp đồng (seed sẵn trong DB), trạng thái `ACTIVE`. | [P] | |

### F. Tài khoản đảm bảo & đối soát

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú |
|---|---|---|---|
| BR-55 | Công ty ví có **tài khoản đảm bảo thanh toán** tại corebank, tách riêng, chỉ dùng cho dịch vụ ví. Mọi dòng tiền thật đi qua tài khoản này: nạp = TK KH → TK đảm bảo; rút/chuyển ra NH = TK đảm bảo → TK đích. | [L] | TT40 Đ27. |
| BR-56 | Bất biến: **số dư TK đảm bảo ≥ tổng số dư (kể cả phần hold) của mọi ví**. Kiểm tra tự động **hằng ngày**; vi phạm → cảnh báo mức nghiêm trọng, tạm dừng rút/chuyển ra NH cho tới khi xử lý. | [L] | TT40 Đ27.1. |
| BR-57 | Tài khoản ledger `bank_settlement` phản ánh TK đảm bảo; `wallet_hold` cho tiền giữ chỗ; `merchant_payable` cho tiền phải trả merchant (quyết toán merchant: ngoài MVP, chỉ ghi nhận công nợ). | [P] | |
| BR-58 | **Đối soát T+1** với corebank theo danh sách bank operations trong ngày. Xử lý chênh lệch: NH đã ghi nợ mà ví chưa ghi có → **ghi có bù**; ví đã ghi có mà NH không ghi nợ → **bút toán đảo**; payout không rõ → chốt theo kết quả của NH. Mọi điều chỉnh có audit (người làm, người duyệt, lý do). | [P] | Trả lời câu "ghi nợ nhưng chưa ghi có". |

### G. Vận hành, khiếu nại, đóng ví

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú |
|---|---|---|---|
| BR-60 | Tra soát/khiếu nại: KH gửi trong vòng **60 ngày** kể từ ngày giao dịch; trả lời trong tối đa **15 ngày làm việc**; có mã yêu cầu và trạng thái theo dõi. | [L]/[P] | TT40 yêu cầu thỏa thuận phải có quy trình tra soát; các thời hạn là [P]. |
| BR-61 | Từ chối/tạm dừng giao dịch khi nghi gian lận (theo AML rule hoặc bộ tiêu chí rủi ro) → ví `SUSPENDED` và tạo case cho vận hành. | [L] | TT40 Đ19, Đ28. |
| BR-62 | **Đóng ví** theo yêu cầu: không còn giao dịch `PENDING`/hold; số dư phải rút hết về TK liên kết trước khi đóng. | [L]/[P] | TT40 Đ25 (hoàn trả số dư). |
| BR-63 | Ví **không phát sinh giao dịch trong 12 tháng** → thông báo trước 30 ngày rồi đóng; số dư tự hoàn về TK liên kết. | [P] | Luật cho phép quy định trong thỏa thuận. |
| BR-64 | Back-office **không** sửa số dư hoặc trạng thái giao dịch trực tiếp. Mọi điều chỉnh tài chính qua nghiệp vụ điều chỉnh, **maker–checker** (2 người khác nhau), bắt buộc ghi lý do. | [P] | prd F-18. |

### H. Dữ liệu cá nhân, PCRT, lưu trữ

| Mã | Quy tắc | Nhãn | Căn cứ / ghi chú |
|---|---|---|---|
| BR-70 | Màn hình đồng ý xử lý dữ liệu nêu rõ mục đích, loại dữ liệu, thời hạn lưu, bên nhận chia sẻ (corebank); **đồng ý riêng** cho dữ liệu sinh trắc học; lưu phiên bản điều khoản đã đồng ý. | [L] | BVDLCN 2025, NĐ 356/2025. |
| BR-71 | Mã hóa khi lưu: ảnh CCCD, dữ liệu khuôn mặt, số CCCD (lưu thêm hash để tra cứu). Ảnh lưu trong private object storage. Log/audit không chứa số CCCD đầy đủ, ảnh, OTP, PIN, token. | [L] | BVDLCN. |
| BR-72 | Có runbook xử lý sự cố lộ dữ liệu, thông báo trong **72 giờ**. | [L] | BVDLCN. |
| BR-73 | Thời hạn lưu: hồ sơ KYC **5 năm** kể từ khi đóng ví; dữ liệu giao dịch tối thiểu 5 năm, mặc định **10 năm** cho ledger vì là chứng từ kế toán. | [L]/[P] | PCRT Đ38; 10 năm là [P]. |
| BR-74 | AML cơ bản: bảng `aml_alerts` với các rule: (a) chuyển tiền điện tử ≥ ngưỡng báo cáo (mặc định 500.000.000đ, cộng dồn theo ngày); (b) nạp xong rút/chuyển ra gần như toàn bộ trong 24 giờ; (c) ≥ 20 giao dịch nhận từ ≥ 10 ví khác nhau trong 24 giờ. Alert chỉ tạo case để vận hành xem, không tự khóa ví (trừ khi vận hành quyết định). | [L]/[P] | PCRT, TT27/2025; các rule cụ thể là [P]. |

## 5. Tham số cấu hình (giá trị mặc định)

| Key | Mặc định | BR |
|---|---|---|
| `customer.min_age` | 18 | BR-01 |
| `kyc.max_attempts_per_day` | 3 | BR-06 |
| `kyc.id_expiry_notice_days` | 30 | BR-07 |
| `banklink.max_per_customer` | 3 | BR-12 |
| `topup.min_amount` | 10_000 | BR-20 |
| `transfer.min_amount` / `payment.min_amount` | 1_000 | BR-28 |
| `txn.max_amount_per_txn` | 50_000_000 | BR-28 |
| `limit.monthly_general` | 100_000_000 | BR-25 |
| `limit.monthly_with_essential` | 300_000_000 | BR-26 |
| `limit.timezone` | `Asia/Ho_Chi_Minh` | BR-27 |
| `auth.strong_single_threshold` | 10_000_000 | BR-36 |
| `auth.strong_daily_threshold` | 20_000_000 | BR-36 |
| `auth.new_device_daily_limit` / `auth.new_device_window_hours` | 10_000_000 / 24 | BR-38 |
| `auth.pin_max_failures` / `auth.pin_lock_minutes` | 5 / 30 | BR-39 |
| `auth.challenge_ttl_seconds` | 300 | BR-40 |
| `payment.pending_max_hours` | 24 | BR-46 |
| `refund.window_days` | 90 | BR-48 |
| `merchant.callback_retry_max_hours` | 24 | BR-51 |
| `qr.ttl_minutes` | 15 | BR-52 |
| `dispute.submit_window_days` / `dispute.resolve_business_days` | 60 / 15 | BR-60 |
| `wallet.dormant_close_months` / `wallet.dormant_notice_days` | 12 / 30 | BR-63 |
| `retention.kyc_years_after_close` / `retention.ledger_years` | 5 / 10 | BR-73 |
| `aml.ewire_report_threshold` | 500_000_000 | BR-74 |

## 6. Cần đối chiếu khi review

1. **Ngưỡng xác thực (BR-36)**: theo TT50 gốc, với ví là G ≤ 10 tr và tổng trong ngày ≤ 20 tr. Một số bài phân tích TT77/2025 lại nêu ngưỡng loại A là 5 tr và loại C từ trên 100 tr. Cần đọc Phụ lục 01–02 của TT77 bản gốc.
2. **Phạm vi hạn mức 100 tr (BR-25)**: Đ26 dẫn chiếu "các điểm b, c, d, đ khoản 2 Điều 25". Cần xác định chính xác việc chuyển tới TK ngân hàng khác (không phải TK liên kết) có thuộc phạm vi hay không. Hiện tại tôi **tính vào hạn mức**, là cách an toàn hơn.
3. **Ngưỡng báo cáo PCRT (BR-74)**: con số 500 tr theo TT27/2025 mới lấy từ nguồn thứ cấp.
4. **TT45/2025** (cấm thẻ tín dụng nạp ví): mới xác nhận qua nguồn thứ cấp.
5. Các quy tắc **[P]** là đề xuất của tôi, bạn có thể đổi tùy ý, đặc biệt BR-01 (tuổi), BR-12, BR-28, BR-38, BR-60, BR-63.

## 7. Truy vết các câu hỏi nghiệp vụ ban đầu (từ `planning.md` cũ)

| Câu hỏi | Trả lời |
|---|---|
| Ví chưa xác minh có được nạp/chuyển? Hạn mức? | Không — BR-05 |
| Tối đa bao nhiêu TK liên kết? | 3 — BR-12 |
| Hạn mức theo giao dịch/ngày/loại? | BR-25…BR-29, BR-36 |
| Giao dịch nào cần xác thực bổ sung? | BR-35…BR-40 |
| NH timeout hiển thị gì? | "Đang xử lý" — BR-46 |
| Merchant/ví lệch callback? | BR-51 |
| Hủy sau khi ghi nợ? | Không — BR-47, BR-48 |
| Ghi nợ nhưng chưa ghi có? | BR-49, BR-58 |
| Phí hạch toán vào đâu? | Miễn phí MVP, tài khoản `fee_revenue` — BR-30 |
