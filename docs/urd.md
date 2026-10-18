# URD — User Requirements

> Trạng thái: **REVIEWED — Đã review** · Cập nhật: 2026-10-18
> Liên quan: [README](README.md) · [prd.md](prd.md) (`F-xx`) · [business-rules.md](business-rules.md) (`BR-xx`) · [fsd/](fsd/) (`UC-xx`)

Tài liệu này mô tả **người dùng cần làm gì và thế nào là đạt**, từ góc nhìn người dùng, không mô tả cách hệ thống xây dựng.

- Mỗi user story có mã `US-xx`, gắn với tính năng `F-xx` (PRD) và quy tắc `BR-xx`.
- Tiêu chí chấp nhận (AC) viết theo dạng **Given / When / Then**.
- Chi tiết màn hình, luồng ngoại lệ và mã lỗi thuộc về FSD.

---

## 1. Persona

Tất cả persona đều là nhân vật hư cấu.

### P1 — Khách hàng phổ thông: "Lan", 28 tuổi, nhân viên văn phòng tại TP.HCM

- **Bối cảnh:** có TK Vietcombank và CCCD gắn chip, dùng smartphone Android. Hằng ngày trả tiền cà phê, ăn trưa bằng QR; chuyển tiền góp quỹ cho bạn bè.
- **Mục tiêu:** thanh toán nhanh (< 10 giây từ lúc quét đến khi xong); biết chắc tiền đã đi hay chưa.
- **Lo ngại:** mất tiền khi mạng chập chờn; bị trừ tiền 2 lần; lộ thông tin CCCD.
- **Mức thành thạo:** trung bình. Không hiểu thuật ngữ như "đối soát" hay "hold".

### P2 — Khách hàng ít dùng: "Bác Hùng", 58 tuổi, tiểu thương

- **Bối cảnh:** mới dùng ví lần đầu, chủ yếu để nhận tiền và rút về ngân hàng.
- **Mục tiêu:** quy trình đăng ký rõ ràng từng bước; thông báo dễ hiểu bằng tiếng Việt.
- **Lo ngại:** chuyển nhầm người; quên PIN.

### P3 — Nhân viên chăm sóc khách hàng: "Hà" (role `SUPPORT`)

- **Nhiệm vụ:** trả lời "tiền của tôi đâu?", tra cứu giao dịch, tiếp nhận khiếu nại.
- **Quyền:** chỉ xem; **không** được thay đổi tiền.

### P4 — Nhân viên vận hành: "Tuấn" (role `OPERATOR`)

- **Nhiệm vụ:** duyệt eKYC thủ công, xử lý giao dịch treo, xử lý kết quả đối soát, tạo điều chỉnh tài chính (maker), xử lý cảnh báo AML, phong tỏa / gỡ phong tỏa ví.

### P5 — Kiểm soát viên / kế toán: "Phương" (role `CHECKER`)

- **Nhiệm vụ:** duyệt hoặc từ chối điều chỉnh tài chính do người khác tạo; xem đối soát và audit.

### P6 — Quản trị hệ thống: role `ADMIN`

- **Nhiệm vụ:** cấu hình tham số (hạn mức, ngưỡng); quản lý tài khoản nhân viên.

### P7 — Merchant: "Cà phê Sáng", chuỗi 5 quán, dùng POS tích hợp API

- **Mục tiêu:** hiển thị QR theo từng đơn; biết chắc đơn đã được trả tiền trước khi giao hàng; hoàn tiền khi khách trả món.
- **Lo ngại:** callback bị mất; khách thanh toán 2 lần.

---

## 2. User story theo epic

Cột **F** là tính năng trong PRD. Cột **BR** là quy tắc chính áp dụng.

### E1 — Onboarding & tài khoản (P1, P2)

| ID | User story | F | BR |
|---|---|---|---|
| US-01 | Là khách hàng mới, tôi muốn đăng ký bằng số điện thoại và mã OTP để có tài khoản ví | F-01 | BR-03 |
| US-02 | Là khách hàng, tôi muốn đọc và đồng ý từng loại điều khoản (riêng phần dữ liệu sinh trắc học) để biết dữ liệu của mình được dùng thế nào | F-02 | BR-70 |
| US-03 | Là khách hàng, tôi muốn đặt mã PIN 6 số để xác nhận giao dịch | F-01 | BR-35, 41 |
| US-04 | Là khách hàng, tôi muốn định danh bằng CCCD gắn chip và khuôn mặt ngay trên app để kích hoạt ví mà không cần ra quầy | F-03 | BR-01, 04…06 |
| US-05 | Là khách hàng, tôi muốn đăng nhập bằng SĐT + PIN trên điện thoại của mình | F-01 | BR-38, 39 |
| US-06 | Là khách hàng đổi điện thoại, tôi muốn đăng nhập trên máy mới một cách an toàn | F-01 | BR-38 |
| US-07 | Là khách hàng quên PIN hoặc bị khóa PIN, tôi muốn đặt lại PIN mà không phải liên hệ tổng đài | F-01 | BR-39 |
| US-08 | Là khách hàng, tôi muốn được nhắc trước khi CCCD hết hạn và định danh lại để ví không bị gián đoạn | F-03 | BR-07 |

**Tiêu chí chấp nhận**

- **US-01**
  - Given SĐT hợp lệ, When tôi yêu cầu OTP, Then tôi nhận OTP 6 số, hiệu lực 5 phút.
  - Given tôi nhập sai OTP 5 lần, Then OTP đó bị vô hiệu và tôi phải yêu cầu OTP mới.
  - Given SĐT đã có tài khoản, When tôi xác thực OTP, Then app hướng tôi sang đăng nhập (không lộ việc SĐT đã đăng ký ở bước gửi OTP).
- **US-02**
  - Given tôi chưa đồng ý `BIOMETRIC_DATA`, When tôi bắt đầu eKYC, Then app yêu cầu tôi đồng ý trước.
  - Given tôi đã đồng ý, Then tôi xem lại được phiên bản điều khoản và thời điểm đồng ý.
  - Given tôi rút lại đồng ý bắt buộc, Then app cảnh báo rằng ví sẽ không thể giao dịch.
- **US-03**
  - PIN là 6 chữ số, không được là chuỗi tăng/giảm hoặc lặp (ví dụ 123456, 111111) — BR-41.
  - Đổi PIN phải nhập PIN hiện tại.
- **US-04**
  - Given tôi dưới 18 tuổi theo CCCD, Then eKYC bị từ chối kèm lý do rõ ràng.
  - Given ảnh mờ hoặc nhà cung cấp lỗi, Then tôi được chụp lại; lỗi kỹ thuật không bị tính là "không đạt".
  - Given CCCD đã gắn với tài khoản khác, Then eKYC bị từ chối.
  - Given tôi thất bại 3 lần trong ngày, Then tôi phải chờ sang ngày hôm sau.
  - Given eKYC đạt, Then tôi thấy thông tin trích xuất (số CCCD đã che) và bước tiếp theo là liên kết ngân hàng.
- **US-05**
  - Sai PIN 5 lần liên tiếp thì bị khóa 30 phút, và app hiển thị thời điểm mở khóa.
  - Phiên đăng nhập tự gia hạn khi tôi đang dùng app; đăng xuất thì phiên trên máy đó bị thu hồi.
- **US-06**
  - Given máy mới, When đăng nhập, Then tôi phải xác thực OTP + khuôn mặt.
  - Sau khi đăng nhập trên máy mới, máy cũ bị đăng xuất.
  - Trong 24 giờ đầu trên máy mới, tổng giao dịch tối đa 10 triệu/ngày, và app thông báo rõ hạn chế này.
- **US-07**
  - Đặt lại PIN luôn cần OTP + khuôn mặt; xong thì mở khóa PIN ngay.
- **US-08**
  - Tôi nhận thông báo 30 ngày trước ngày hết hạn.
  - Khi CCCD đã hết hạn, ví chuyển sang tạm dừng: vẫn xem được và nhận được hoàn tiền, nhưng không giao dịch được cho tới khi eKYC lại thành công.

### E2 — Liên kết ngân hàng (P1, P2)

| ID | User story | F | BR |
|---|---|---|---|
| US-10 | Là khách hàng, tôi muốn liên kết tài khoản hoặc thẻ ghi nợ đứng tên mình để nạp và rút tiền | F-08 | BR-10…12, 14 |
| US-11 | Là khách hàng có nhiều liên kết, tôi muốn chọn một liên kết mặc định | F-08 | — |
| US-12 | Là khách hàng, tôi muốn hủy một liên kết không còn dùng | F-08 | BR-13 |

**Tiêu chí chấp nhận**

- **US-10**
  - Given TK không đứng tên tôi hoặc là thẻ tín dụng, Then liên kết bị từ chối kèm lý do.
  - Given đã có 3 liên kết, Then tôi không thể thêm liên kết mới.
  - Given ngân hàng chậm phản hồi, Then liên kết hiện "Đang xử lý", không báo lỗi giả.
  - Liên kết đầu tiên thành công (và đã eKYC) thì ví được kích hoạt.
  - App không bao giờ hỏi mật khẩu Internet Banking.
- **US-12**
  - Given đây là liên kết cuối cùng, Then tôi không thể hủy, và app giải thích rằng muốn hủy hết thì phải đóng ví.

### E3 — Chuyển & nhận tiền (P1, P2)

| ID | User story | F | BR |
|---|---|---|---|
| US-20 | Là khách hàng, tôi muốn nạp tiền từ ngân hàng đã liên kết vào ví | F-09 | BR-20, 49 |
| US-21 | Là khách hàng, tôi muốn chuyển tiền cho người dùng ví khác bằng số điện thoại | F-05 | BR-23…29, 47 |
| US-22 | Là khách hàng, tôi muốn rút tiền từ ví về tài khoản ngân hàng của mình | F-10 | BR-21, 50 |
| US-23 | Là khách hàng, tôi muốn chuyển tiền từ ví tới tài khoản ngân hàng bất kỳ | F-11 | BR-22, 25, 50 |
| US-24 | Là khách hàng, tôi muốn giao dịch lớn được bảo vệ thêm bằng OTP + khuôn mặt | F-07 | BR-35…40 |
| US-25 | Là khách hàng, tôi muốn biết rõ giao dịch đang xử lý, thành công hay thất bại, và tiền của tôi đang ở đâu | F-14 | BR-45, 46 |

**Tiêu chí chấp nhận**

- **US-20**
  - Số tiền tối thiểu 10.000đ.
  - Sau khi xác nhận, giao dịch hiện "Đang xử lý". Ví chỉ được cộng tiền khi ngân hàng xác nhận.
  - Given tôi bấm xác nhận 2 lần hoặc mạng gửi lại request, Then chỉ có 1 lần trừ tiền ngân hàng.
- **US-21**
  - Trước khi xác nhận, tôi thấy tên người nhận đã che một phần (ví dụ "NGUYEN V*** B").
  - Given số dư không đủ hoặc vượt hạn mức tháng, Then giao dịch bị từ chối kèm số còn lại.
  - Thành công thì cả hai bên thấy giao dịch ngay; giao dịch không thể hủy.
  - Không được chuyển cho chính mình.
- **US-22**
  - Số tiền được giữ lại ngay; số dư khả dụng giảm, và tôi thấy dòng "đang giữ".
  - Ngân hàng từ chối thì tiền giữ được trả lại ví.
  - Chưa rõ kết quả thì tiền vẫn được giữ, và app hiện "Đang xử lý" cho tới khi có kết luận.
- **US-23**
  - Như US-22, cộng thêm: giao dịch tính vào hạn mức 100 triệu/tháng; ngân hàng đích nhận được tên người gửi.
- **US-24**
  - Given số tiền > 10 triệu hoặc tổng trong ngày > 20 triệu, Then tôi phải xác thực OTP + khuôn mặt.
  - Xác thực chỉ dùng được cho đúng giao dịch đó, trong 5 phút.
  - Hủy xác thực thì giao dịch không được thực hiện.
- **US-25**
  - Giao dịch chỉ có 4 trạng thái hiển thị: Đang xử lý / Thành công / Thất bại / Đã hoàn.
  - Thất bại có lý do dễ hiểu.
  - Giao dịch đang xử lý quá 24 giờ thì app hướng dẫn tôi gửi tra soát.

### E4 — Thanh toán QR (P1, P7)

| ID | User story | F | BR |
|---|---|---|---|
| US-30 | Là khách hàng, tôi muốn quét QR tại quầy để trả tiền đơn hàng | F-12 | BR-51, 52 |
| US-31 | Là khách hàng, tôi muốn nhận lại tiền khi merchant hoàn tiền | F-13 | BR-48 |

**Tiêu chí chấp nhận**

- **US-30**
  - Sau khi quét, tôi thấy tên merchant và số tiền lấy từ hệ thống (không lấy từ nội dung QR).
  - QR quá 15 phút thì báo hết hạn.
  - Đơn đã thanh toán thì không thanh toán lại được.
  - Thành công thì tôi thấy ngay; merchant cũng nhận được kết quả.
- **US-31**
  - Hoàn tiền vào thẳng ví, có thông báo; tổng số hoàn không vượt quá số đã trả.

### E5 — Ví, lịch sử, hạn mức (P1, P2)

| ID | User story | F | BR |
|---|---|---|---|
| US-40 | Là khách hàng, tôi muốn xem số dư khả dụng và số tiền đang giữ | F-04 | BR-24, 50 |
| US-41 | Là khách hàng, tôi muốn xem lịch sử giao dịch, lọc theo loại và thời gian | F-14 | — |
| US-42 | Là khách hàng, tôi muốn biết hạn mức tháng còn lại | F-06 | BR-25…28 |
| US-43 | Là khách hàng, tôi muốn đóng ví khi không còn dùng | F-15 | BR-62 |
| US-44 | Là khách hàng, tôi muốn nhận thông báo khi tiền vào/ra hoặc giao dịch đổi trạng thái | F-14 | — |
| US-45 | Là khách hàng lâu không dùng, tôi muốn được báo trước khi ví bị đóng vì không hoạt động | F-15 | BR-63 |

**Tiêu chí chấp nhận**

- **US-40**: tổng số dư = khả dụng + đang giữ.
- **US-41**: lịch sử phân trang, mới nhất trước; mỗi dòng có loại, đối tác (đã che), số tiền, trạng thái.
- **US-42**: thấy đã dùng / còn lại trong tháng hiện tại; thấy ngưỡng cần xác thực mạnh.
- **US-43**:
  - Given còn số dư hoặc còn giao dịch đang xử lý, Then không đóng được, và app hướng dẫn rút hết.
  - Đóng ví cần PIN.
- **US-45**: được báo trước 30 ngày; số dư còn lại được hoàn về TK liên kết.

### E6 — Tra soát & khiếu nại (P1, P3)

| ID | User story | F | BR |
|---|---|---|---|
| US-50 | Là khách hàng, tôi muốn gửi khiếu nại cho một giao dịch (chuyển nhầm, chưa nhận được tiền…) | F-16 | BR-47, 60 |
| US-51 | Là khách hàng, tôi muốn theo dõi tiến độ và kết quả khiếu nại | F-16 | BR-60 |

**Tiêu chí chấp nhận**

- **US-50**
  - Chỉ gửi được trong 60 ngày kể từ ngày giao dịch.
  - Gửi xong thì nhận mã tra soát và hạn trả lời (15 ngày làm việc).
- **US-51**
  - Thấy trạng thái hiện tại và kết quả.
  - Nếu được chấp nhận và có hoàn tiền, thấy giao dịch hoàn tương ứng.

### E7 — Merchant (P7)

| ID | User story | F | BR |
|---|---|---|---|
| US-60 | Là merchant, tôi muốn tạo đơn hàng và nhận QR để khách quét | F-12, F-23 | BR-51…53 |
| US-61 | Là merchant, tôi muốn được báo ngay khi đơn đã thanh toán, và tự tra cứu được nếu không nhận được báo | F-12 | BR-51 |
| US-62 | Là merchant, tôi muốn hoàn tiền toàn bộ hoặc một phần cho đơn đã thanh toán | F-13 | BR-48 |
| US-63 | Là merchant, tôi muốn hủy đơn khách chưa trả | F-23 | — |

**Tiêu chí chấp nhận**

- **US-60**
  - Mã đơn của tôi không được trùng.
  - Gửi lại cùng request (cùng Idempotency-Key) thì nhận lại đúng đơn cũ.
  - QR có hạn 15 phút.
- **US-61**
  - Callback được ký HMAC; nếu tôi không trả 2xx thì được gửi lại trong tối đa 24 giờ.
  - API tra cứu luôn trả trạng thái mới nhất.
- **US-62**
  - Tổng hoàn ≤ số đã trả; quá 90 ngày thì không hoàn được.
  - Mỗi mã hoàn tiền chỉ xử lý một lần.
- **US-63**: chỉ hủy được đơn chưa thanh toán.

### E8 — Vận hành back-office (P3–P6)

| ID | User story | Role | F | BR |
|---|---|---|---|---|
| US-70 | Là nhân viên CSKH, tôi muốn tìm khách hàng theo SĐT / mã ví và xem giao dịch, liên kết, trạng thái eKYC để trả lời khách | SUPPORT+ | F-20 | BR-71 |
| US-71 | Là nhân viên vận hành, tôi muốn duyệt hoặc từ chối hồ sơ eKYC cần kiểm tra thủ công | OPERATOR | F-20 | BR-06 |
| US-72 | Là nhân viên vận hành, tôi muốn xem ảnh CCCD / selfie khi cần, và mọi lần xem đều được ghi lại | OPERATOR | F-20 | BR-71 |
| US-73 | Là nhân viên vận hành, tôi muốn phong tỏa / gỡ phong tỏa ví theo yêu cầu hợp lệ | OPERATOR | F-20 | BR-06, 61 |
| US-74 | Là nhân viên vận hành, tôi muốn thấy và chốt kết quả các giao dịch treo cần đối soát | OPERATOR | F-17 | BR-46, 58 |
| US-75 | Là nhân viên vận hành / kiểm soát, tôi muốn xem kết quả đối soát hằng ngày và tình trạng TK đảm bảo | OPERATOR, CHECKER | F-17 | BR-56, 58 |
| US-76 | Là nhân viên vận hành, tôi muốn tạo đề nghị điều chỉnh tài chính có lý do để xử lý chênh lệch / khiếu nại | OPERATOR | F-18 | BR-58, 64 |
| US-77 | Là kiểm soát viên, tôi muốn duyệt hoặc từ chối điều chỉnh do người khác tạo | CHECKER | F-18 | BR-64 |
| US-78 | Là nhân viên vận hành, tôi muốn xử lý cảnh báo AML, và tạm dừng ví khi cần | OPERATOR | F-19 | BR-61, 74 |
| US-79 | Là nhân viên CSKH / vận hành, tôi muốn xử lý khiếu nại đúng hạn | SUPPORT, OPERATOR | F-16 | BR-60 |
| US-80 | Là quản trị, tôi muốn chỉnh tham số hạn mức / ngưỡng mà không cần deploy | ADMIN | F-21 | BR-12, 25…28, 36 |
| US-81 | Là kiểm soát viên, tôi muốn tra cứu audit log của mọi thao tác nhạy cảm | CHECKER, ADMIN | F-22 | BR-64, 71 |

**Tiêu chí chấp nhận**

- **US-70**
  - SUPPORT chỉ xem; số CCCD luôn được che; không thấy ảnh.
- **US-71 / US-73 / US-76 / US-78 / US-80**
  - Mọi thao tác thay đổi đều bắt buộc nhập lý do (≥ 10 ký tự) và được ghi audit.
- **US-72**
  - Ảnh chỉ mở được qua link có hạn 5 phút; mỗi lần mở là một dòng audit.
- **US-74**
  - Danh sách giao dịch `NEEDS_RECONCILIATION` xếp theo thời gian treo.
  - Hệ thống tra cứu lại ngân hàng trước khi chấp nhận kết luận của nhân viên.
- **US-75**
  - Thấy số dư TK đảm bảo (từ ngân hàng) so với tổng số dư ví; lệch thì có cảnh báo nổi bật.
- **US-76 / US-77**
  - Không có chức năng sửa số dư trực tiếp.
  - Người duyệt phải khác người tạo.
  - Điều chỉnh chỉ thay đổi tiền sau khi được duyệt.
- **US-79**
  - Có danh sách khiếu nại quá hạn.
  - Hoàn tiền cho khiếu nại phải đi qua điều chỉnh (US-76/77).
- **US-81**
  - Lọc được theo người thực hiện, đối tượng, thời gian.
  - Log không chứa CCCD đầy đủ, OTP, PIN.

---

## 3. Yêu cầu người dùng phi chức năng

| ID | Yêu cầu | Đo bằng |
|---|---|---|
| UR-NF-01 | Toàn bộ giao diện và thông báo bằng tiếng Việt, tránh thuật ngữ kỹ thuật ("Đang xử lý" thay vì `PENDING`) | Review UI |
| UR-NF-02 | Giao dịch đồng bộ (chuyển ví–ví, QR) có phản hồi trong ~1 giây | NFR-07 |
| UR-NF-03 | Không bao giờ báo "thất bại" khi hệ thống chưa chắc chắn | BR-46 |
| UR-NF-04 | Thao tác lặp (bấm 2 lần, mất mạng rồi gửi lại) không gây trừ tiền 2 lần | NFR-02 |
| UR-NF-05 | Thông tin nhạy cảm (CCCD, số TK) luôn hiển thị đã che, trừ khi người dùng chủ động xem và có quyền | BR-71 |
| UR-NF-06 | Thông báo lỗi luôn nói người dùng có thể làm gì tiếp (nạp thêm, chờ, gửi tra soát…) | Review UI |
| UR-NF-07 | Người lớn tuổi (P2) hoàn tất đăng ký → eKYC → liên kết trong ≤ 10 phút, không cần hỗ trợ | Kiểm thử người dùng |

---

## 4. Truy vết

| Persona | Epic / User story |
|---|---|
| P1, P2 | E1–E6 (US-01…51) |
| P3 SUPPORT | US-70, 79 |
| P4 OPERATOR | US-71…76, 78, 79 |
| P5 CHECKER | US-75, 77, 81 |
| P6 ADMIN | US-80, 81 |
| P7 Merchant | US-60…63 |

Ma trận đầy đủ US ↔ UC ↔ API sẽ nằm trong `fsd/README.md`.

## 5. Lịch sử

- 2026-10-18: Review `urd.md` done