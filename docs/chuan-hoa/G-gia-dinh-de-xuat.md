# G. Giả định và đề xuất

> Trạng thái: **DRAFT** · Lập: 2026-10-21
> Liên quan: [README](README.md) · [D. Vấn đề](D-bao-cao-van-de.md) · [F. Câu hỏi](F-cau-hoi-xac-nhan.md)

## 1. Giả định của lần chuẩn hóa này [GIẢ ĐỊNH]

Các giả định dưới đây là của **người thực hiện chuẩn hóa**, dùng để tiến hành công việc khi thiếu dữ liệu. Chúng không phải giả định của sản phẩm (giả định sản phẩm nằm ở prd §10).

| ID | Giả định | Lý do | Nếu sai thì |
|---|---|---|---|
| GD-01 | Không có tài liệu nào khác ngoài hai repo `Hungnd562k/E-Wallet` và `Hungnd562k/E-Wallet-Wallet-Core`. Nguồn = tài liệu + mã trong hai repo này. | Yêu cầu không kèm tệp riêng | Cần kiểm kê lại và đối chiếu thêm nguồn mới |
| GD-02 | Nhánh `master` của `E-Wallet` (commit `087c3ff`, 2026-10-20) là phiên bản tài liệu hiện hành. | `main` chỉ có README 2 dòng; `master` có toàn bộ `docs/` và commit mới nhất (ISS-34) | Toàn bộ báo cáo phải đối chiếu lại với nhánh đúng |
| GD-03 | Nhánh `main` của `E-Wallet-Wallet-Core` (commit `7332c9e`, 2026-10-21) phản ánh tiến độ triển khai hiện tại. | Là nhánh mặc định, có commit mới nhất | Cột "Triển khai" ở H và mục 8 của D có thể lỗi thời |
| GD-04 | Trạng thái "REVIEWED" là tự review của tác giả, **không phải** phê duyệt chính thức. | Không có tên người duyệt hay biên bản (ISS-28) | Nhãn [ĐÃ XÁC NHẬN] ở C có thể nâng thành "đã phê duyệt" |
| GD-05 | Không kiểm lại tính chính xác của căn cứ pháp lý; dùng đúng như business-rules §2, §6. | business-rules §1 đã ghi "không phải tư vấn pháp lý"; ngoài phạm vi chuẩn hóa | Có thể phát sinh thêm vấn đề ở nhóm [L] |
| GD-06 | Không truy cập các liên kết ngoài: thảo luận ChatGPT (prd §11), Jira (SCRUM), các trang văn bản pháp luật. | Không cần để đối chiếu nội bộ; tránh dựa vào nguồn không lưu trong repo | Có thể có quyết định nằm ngoài repo chưa được phản ánh |
| GD-07 | Không chạy lại `redocly lint`, test, hay khởi động hệ thống. | Lần này chỉ xử lý tài liệu | "Lint sạch" ở docs/README chưa được kiểm chứng độc lập |
| GD-08 | `docs/go-map/` và `learn/` của wallet-core là tài liệu học Go, không chứa yêu cầu sản phẩm, nên không đưa vào danh mục yêu cầu. | Nội dung là bài học cú pháp, liên kết `file:line` | Không ảnh hưởng |
| GD-09 | `progression/jira-backlog-draft.md` là bản ánh xạ US/F sang Jira, không phải nguồn yêu cầu. | Tự ghi "Nguồn: urd.md, prd.md…" | Không ảnh hưởng |
| GD-10 | Các con số trong FSD được ghi [P] là quyết định sản phẩm mới, chưa duyệt. | fsd/README §2 quy ước như vậy | — |

## 2. Đề xuất [ĐỀ XUẤT]

Đề xuất chia làm hai loại: **biên tập** (không đổi nghiệp vụ, có thể làm ngay sau khi chủ dự án đồng ý) và **nghiệp vụ** (cần câu trả lời ở F trước).

### 2.1. Đề xuất biên tập

| ID | Đề xuất | Giải quyết | Công sức |
|---|---|---|---|
| DX-01 | Thêm **khối kiểm soát tài liệu** (phiên bản, trạng thái, người soạn, người review, ngày) ở đầu mỗi tài liệu theo mẫu [B §4](B-bo-tai-lieu-chuan-hoa.md#4-khối-kiểm-soát-tài-liệu). Chỉ điền người review / phê duyệt khi chủ dự án cung cấp. | ISS-04, ISS-28 | Nhỏ |
| DX-02 | Dùng bộ trạng thái thống nhất: `DRAFT` → `IN REVIEW` → `REVIEWED` → `APPROVED`, thêm `SUPERSEDED`. | ISS-04 | Nhỏ |
| DX-03 | Đồng bộ dòng trạng thái đầu file với lịch sử cuối file và bảng docs/README §1 (sau khi có câu trả lời Q-04). | ISS-04 | Nhỏ |
| DX-04 | Tạo `docs/glossary.md` từ bản nháp [B §6](B-bo-tai-lieu-chuan-hoa.md#6-bảng-thuật-ngữ); trả lời 3 câu trong `Q&A.md` bằng link sang glossary. | ISS-29 | Nhỏ |
| DX-05 | Sửa các tham chiếu lỗi thời trong wallet-core: `scope.md` → `business-rules.md` ở CLAUDE.md; đồng bộ `api/openapi.yaml` với bản gốc. **Đã làm** (CHG-06, 07). | ISS-35, ISS-36 | Nhỏ |
| DX-06 | ~~Thêm bước CI ở wallet-core so sánh `api/openapi.yaml` với bản ở repo docs (theo Q-07).~~ **Không cần**: Q-07 chọn (b), docs không còn bản sao. | ISS-36 | Nhỏ |
| DX-07 | Thống nhất tên gọi theo erd/OpenAPI ở các mục ISS-13 (b)–(g). | ISS-13 | Nhỏ |
| DX-08 | Áp dụng đúng quy tắc docs/README §3: các tài liệu ngoài business-rules ghi **tên key** (ví dụ `qr.ttl_minutes`) thay vì số. Với AC trong urd, có thể ghi "15 phút (`qr.ttl_minutes`)" để vẫn dễ đọc. | ISS-32 | Trung bình |
| DX-09 | Thêm cột "Triển khai" vào ma trận fsd/README §7, cập nhật cuối mỗi sprint (H.2 là bản đầu). | ISS-37…40 | Nhỏ |
| DX-10 | Bổ sung ma trận truy vết vào `failure-scenarios.md` khi viết: mỗi TC trỏ về UC/AC hoặc NFR/SM. Mẫu TC ở [B §5.3](B-bo-tai-lieu-chuan-hoa.md#53-mẫu-kịch-bản-kiểm-thử-tc). | ISS-24 | — |

### 2.2. Đề xuất nghiệp vụ (chờ F)

| ID | Đề xuất | Câu hỏi |
|---|---|---|
| DX-11 | Sửa architecture §5.2 theo CF-04 (lưu lỗi nghiệp vụ) và theo thứ tự kiểm tra của fsd/04. **Đã làm** (CHG-04, 05). | Q-01, Q-02 |
| DX-12 | Thêm ADR-16 "Polyrepo" thay ADR-13; cập nhật architecture §11 và phiên bản Go. | Q-05 |
| DX-13 | Merge nhánh `master` vào `main` của repo `E-Wallet`. **Đã làm** (CHG-03); xóa `master` chờ chủ dự án. | Q-06 |
| DX-14 | Tách BR-06 thành vòng đời khách hàng và vòng đời ví; bổ sung nhánh đồng bộ vào BR-45. | Q-08 |
| DX-15 | Ghi rõ phạm vi BR-28 và cập nhật `x-br` của `payQr`, `createTopup`. | Q-12 |
| DX-16 | Thêm `lookup.max_per_hour` vào business-rules §5. | Q-20 |
| DX-17 | Viết AC cho US-11, US-44. | Q-28 |
| DX-18 | Viết `failure-scenarios.md` phần M0 trước khi kết thúc Sprint 1, rồi fsd/02, 03, 05 trước Sprint M1. | Q-24 |
| DX-19 | Ghi "Giới hạn ở M0" vào fsd/04 cho các bước chưa làm trong Sprint 1, như CF-05 đã làm với step-up. | Q-31 |

### 2.3. Đề xuất cách viết yêu cầu mới

Áp dụng cho yêu cầu **viết mới** từ nay; không viết lại yêu cầu cũ (để giữ nguyên nghĩa nguồn).

1. Mỗi yêu cầu một câu, một ý, có chủ ngữ rõ: "Hệ thống phải…", "Khách hàng có thể…", "Khi … thì hệ thống …".
2. Tránh từ khó kiểm chứng: "nhanh", "thân thiện", "~", "tối thiểu", "phù hợp". Thay bằng số và tên key cấu hình.
3. Mỗi US có ít nhất một AC Given/When/Then; mỗi AC kiểm được bằng một TC.
4. Quy tắc mới có nhãn [L] hoặc [P]; [L] phải có căn cứ và độ tin cậy như business-rules §2.
5. Khi bộ tài liệu tham khảo ISO/IEC/IEEE 29148 (đặc tính của yêu cầu tốt: cần thiết, rõ ràng, kiểm chứng được, nhất quán, truy vết được), chỉ dùng như hướng dẫn; bộ tài liệu **chưa được đánh giá** theo chuẩn này.
