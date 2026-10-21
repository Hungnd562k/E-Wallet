# A. Báo cáo tổng quan

> Trạng thái: **DRAFT** · Lập: 2026-10-21
> Phạm vi: repo `Hungnd562k/E-Wallet` nhánh `master` commit `087c3ff` (2026-10-20) và repo `Hungnd562k/E-Wallet-Wallet-Core` nhánh `main` commit `7332c9e` (2026-10-21)
> Liên quan: [README](README.md) · [D. Vấn đề](D-bao-cao-van-de.md) · [F. Câu hỏi](F-cau-hoi-xac-nhan.md) · [I. Checklist](I-checklist-chat-luong.md)

## 1. Kết luận chính

1. **Bộ tài liệu có nền tảng tốt.** Đã có phân cấp PRD → URD → BR → FSD → kiến trúc / ERD / API, quy ước ID và chuỗi truy vết rõ, nhãn [L]/[P] tách luật khỏi quyết định sản phẩm, AC dạng Given/When/Then, tham số cấu hình tập trung. Không có ID trùng, không có tham chiếu tới ID không tồn tại.
2. **Đủ để làm M0, có điều kiện.** 6 story của Sprint 1 đều có UC, API và bảng dữ liệu. Trước khi đóng Sprint 1 cần chốt 5 câu hỏi P1 liên quan đến chuyển tiền (Q-01, Q-02, Q-12, Q-30, Q-31).
3. **Chưa đủ cho M1 trở đi.** 6/9 file FSD và 13/20 UC chưa viết; `failure-scenarios.md` chưa có nên SM-02 (Done của M1) và mắt xích TC của chuỗi truy vết chưa kiểm được.
4. **Phát hiện 40 vấn đề**: 6 mức Cao, 17 Trung bình, 17 Thấp. Các vấn đề Cao: idempotency khi lỗi nghiệp vụ (ISS-01), ADR monorepo không còn đúng (ISS-05), ngưỡng step-up chưa chắc về pháp lý (ISS-16), thiếu kịch bản kiểm thử (ISS-24), thiếu FSD M1 (ISS-25), tài liệu nằm ở nhánh không mặc định (ISS-34).
5. **31 câu hỏi cần chủ dự án trả lời**, trong đó 12 ưu tiên P1 và 18 có ảnh hưởng nghiệp vụ.

## 2. Kiểm kê nguồn

Không có tài liệu nào được gửi kèm yêu cầu, nên nguồn là toàn bộ tài liệu và mã trong hai repo của dự án [GIẢ ĐỊNH GD-01].

### 2.1. Repo `E-Wallet` (tài liệu thiết kế)

Nhánh mặc định `main` chỉ có `README.md` (2 dòng) và `.gitignore`. Toàn bộ tài liệu nằm ở nhánh `master` (ISS-34). Bảng dưới là nhánh `master`.

| # | Tệp | Loại | Dòng | Trạng thái ghi trong tệp | Cập nhật | Đọc được |
|---|---|---|---|---|---|---|
| 1 | `docs/README.md` | Mục lục, quy ước ID, chuỗi truy vết | 73 | — | 2026-10-17 (§4) | Có |
| 2 | `docs/prd.md` | PRD: bối cảnh, SM, F-xx, NFR, lộ trình, rủi ro | 168 | REVIEWED | 2026-10-18 | Có |
| 3 | `docs/urd.md` | URD: persona P1–P7, US-xx + AC, UR-NF | 308 | REVIEWED | 2026-10-18 | Có |
| 4 | `docs/business-rules.md` | BR-xx, căn cứ pháp lý, tham số cấu hình | 204 | REVIEWED | 2026-10-16 (lịch sử: 10-18) | Có |
| 5 | `docs/fsd/README.md` | Quy ước FSD, CF-xx, thông báo lỗi, ma trận US↔UC | 223 | DRAFT — M0 | 2026-10-18 | Có |
| 6 | `docs/fsd/04-p2p-transfer.md` | UC-04.01, UC-04.02 | 232 | DRAFT | 2026-10-18 | Có |
| 7 | `docs/fsd/07-wallet-history-limits.md` | UC-07.01…04 | 301 | DRAFT — phần M0 | 2026-10-18 | Có |
| 8 | `docs/fsd/09-backoffice-ops.md` | Yêu cầu audit AU-1…7, UC-09.01 | 137 | DRAFT — phần M0 | 2026-10-18 | Có |
| 9 | `docs/architecture.md` | C4, module, transaction, worker, bảo mật, ADR | 534 | DRAFT — chờ review (lịch sử: review done) | 2026-10-17 | Có |
| 10 | `docs/erd.md` | Schema, ma trận bút toán, trigger, máy trạng thái | 1240 | REVIEWED | 2026-10-17 (lịch sử: 10-18) | Có |
| 11 | `docs/api/wallet-core.openapi.yaml` | OpenAPI 3.1: 79 operation (gồm 2 webhook), 38 mã lỗi | 2855 | v0.1.0, "Draft, lint sạch" | — | Có |
| 12 | `docs/api/corebank.openapi.yaml` | OpenAPI 3.1 của corebank giả lập: 15 operation | 639 | v0.1.0 | — | Có |
| 13 | `docs/Q&A.md` | 3 câu hỏi thuật ngữ, chưa có câu trả lời | 4 | — | — | Có |
| 14 | `progression/jira-backlog-draft.md` | Ánh xạ epic / story / task sang Jira | 186 | "Đã đẩy lên Jira" | 2026-10-17 | Có |
| 15 | `progression/progression.md` | Nhật ký tiến độ (1 dòng) | 1 | — | — | Có |
| 16 | `redocly.yaml`, `.redocly.lint-ignore.yaml` | Cấu hình lint OpenAPI | 18 | — | — | Có |

### 2.2. Repo `E-Wallet-Wallet-Core` (mã nguồn)

| Nhóm | Nội dung | Dùng trong lần này |
|---|---|---|
| `README.md`, `CLAUDE.md` | Hướng dẫn chạy, quy ước kiến trúc mã, "khoảng trống có chủ đích" so với thiết kế | Đối chiếu với architecture, erd |
| `api/openapi.yaml` | Bản sao của `wallet-core.openapi.yaml` | So sánh `diff` (ISS-36) |
| `migrations/0001…0007` | Schema platform, ledger, wallet, payment, identity, limits, audit | Đối chiếu với erd |
| `internal/*` | Mã Go; module ngoài payment / wallet / ledger / platform chỉ là doc comment | Đối chiếu hành vi M0 (D §8) |
| `docs/go-map/`, `learn/` | Tài liệu học Go | Không đưa vào yêu cầu (GD-08) |

### 2.3. Nguồn được nhắc tới nhưng không có hoặc không truy cập

| Nguồn | Nhắc tại | Tình trạng |
|---|---|---|
| `failure-scenarios.md` | docs/README, prd SM-02, architecture §12 | Chưa viết (ISS-24) |
| `fsd/01, 02, 03, 05, 06, 08` | fsd/README §1 | Chưa viết (ISS-25) |
| `planning.md`, `scope.md` | Lịch sử docs/README §4 | Đã đổi tên thành `prd.md`, `business-rules.md` |
| Thảo luận ChatGPT | prd §11 | Không truy cập (GD-06, ISS-31) |
| Jira project SCRUM | jira-backlog-draft | Không truy cập (GD-06) |
| Văn bản pháp luật (TT40, TT41, TT45, TT50, TT77, PCRT, BVDLCN) | business-rules §2 | Không kiểm lại (GD-05) |

## 3. Đánh giá chất lượng

### 3.1. Điểm mạnh

- **Truy vết có chủ đích**: chuỗi Persona → US → F → UC → BR → API / ERD → TC được định nghĩa ngay ở docs/README; OpenAPI gắn `x-br` cho từng operation (43/54 BR được ít nhất một operation trích dẫn); erd §9 và architecture §14 truy vết BR về bảng và module.
- **Tách luật và quyết định sản phẩm**: 54/54 BR có nhãn [L] / [P]; quy tắc [L] có điều khoản và độ tin cậy; điểm chưa chắc được tự liệt kê ở business-rules §6.
- **Tiêu chí chấp nhận kiểm chứng được**: 41/43 US có AC Given/When/Then có số đo; UC M0 có thêm AC mức hệ thống (song song, idempotency, deadlock).
- **Thông báo lỗi nhất quán**: bảng thông báo ở fsd/README §5 khớp hoàn toàn 38 mã lỗi của OpenAPI.
- **Tính đúng của tiền được ép ở nhiều tầng**: app, ràng buộc DB, job kiểm tra; được mô tả nhất quán giữa architecture, erd và mã.

### 3.2. Điểm yếu

| Nhóm | Mô tả ngắn | Vấn đề |
|---|---|---|
| Độ phủ | Mới có FSD cho M0; chưa có kịch bản kiểm thử | ISS-24, ISS-25 |
| Nhất quán | 13 mâu thuẫn giữa tài liệu, phần lớn giữa architecture.md (2026-10-17) và các tài liệu viết sau | ISS-01…13 |
| Độ rõ | 10 điểm mơ hồ, tập trung ở hạn mức, xác thực và thời hạn | ISS-14…23 |
| Kiểm soát tài liệu | Trạng thái và ngày lệch; không có phiên bản, người duyệt; tài liệu ở nhánh không mặc định | ISS-04, ISS-28, ISS-34 |
| Đồng bộ với mã | Bản sao OpenAPI cũ; mã lỗi và quyền xem giao dịch khác đặc tả | ISS-35…40 |

### 3.3. Số liệu

| Chỉ số | Giá trị |
|---|---|
| Yêu cầu được gom vào danh mục C | 6 SM · 23 F · 43 US · 54 BR · 12 NFR · 7 UR-NF · 20 UC (7 đã viết) · 8 CF · 7 AU |
| Nhãn ở C (F / US / BR) | [ĐÃ XÁC NHẬN] 19 / 30 / 39 · [CẦN XÁC NHẬN] 2 / 10 / 11 · [MÂU THUẪN] 2 / 3 / 4 |
| US có AC | 41/43 |
| US có UC | 7/43 (đủ cho 6 story của Sprint 1) |
| US có TC | 0/43 |
| Vấn đề | 40 (Cao 6 · Trung bình 17 · Thấp 17) |
| Câu hỏi | 31 (P1 12 · P2 11 · P3 8) |

## 4. Cấu trúc đích và cách xử lý

Bộ tài liệu đã có cấu trúc BA phù hợp nên **giữ nguyên cấu trúc và nội dung nguồn**, không viết lại thành bản mới (lý do ở [B §1](B-bo-tai-lieu-chuan-hoa.md#1-cách-hiểu-bộ-tài-liệu-chuẩn-hóa-trong-dự-án-này)). Lần chuẩn hóa này bổ sung:

- Cấu trúc đích và việc cần làm cho từng tài liệu ([B §2](B-bo-tai-lieu-chuan-hoa.md#2-cấu-trúc-đích)).
- Quy ước nhãn, khối kiểm soát tài liệu, mẫu TC, bảng thuật ngữ ([B §3–6](B-bo-tai-lieu-chuan-hoa.md)).
- Danh mục yêu cầu gom một chỗ, có đánh giá ([C](C-danh-muc-yeu-cau.md)), và ma trận truy vết ([H](H-ma-tran-truy-vet.md)).

## 5. Việc tiếp theo được gợi ý

1. Trả lời 12 câu hỏi P1 ở [F](F-cau-hoi-xac-nhan.md), đặc biệt Q-01, Q-02, Q-06, Q-07 vì chúng chặn các sửa đổi khác.
2. Sau khi có câu trả lời: áp dụng các đề xuất biên tập ở [G §2.1](G-gia-dinh-de-xuat.md#21-đề-xuất-biên-tập) và ghi vào [E](E-nhat-ky-thay-doi.md).
3. Viết `failure-scenarios.md` phần M0, rồi fsd/02, 03, 05 trước khi lập kế hoạch sprint M1 (Q-24).
