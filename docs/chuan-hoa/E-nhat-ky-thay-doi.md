# E. Nhật ký thay đổi

> Trạng thái: **DRAFT** · Lập: 2026-10-21
> Liên quan: [README](README.md) · [G. Đề xuất](G-gia-dinh-de-xuat.md)

## 1. Thay đổi đã thực hiện trong lần chuẩn hóa này

Nguyên tắc: **không sửa nội dung nghiệp vụ** của tài liệu nguồn khi chưa có xác nhận. Lần này chỉ thêm tệp mới và một dòng mục lục.

| ID | Ngày | Tệp | Loại | Nội dung | Nhãn | Ảnh hưởng nghiệp vụ |
|---|---|---|---|---|---|---|
| CHG-01 | 2026-10-21 | `docs/chuan-hoa/*` (10 tệp) | Thêm mới | Báo cáo chuẩn hóa A–I và README của thư mục | — | Không |
| CHG-02 | 2026-10-21 | `docs/README.md` §1 | Thêm 1 dòng | Thêm dòng mục lục trỏ tới `chuan-hoa/` | [ĐÃ BIÊN TẬP] | Không |
| CHG-03 | 2026-10-21 | Repo `E-Wallet` | Nhánh | Gộp `main` (README.md, .gitignore) vào lịch sử `master` để mở PR vào `main` (Q-06). Xóa `master` chờ chủ dự án | [ĐÃ XÁC NHẬN] | Không |
| CHG-04 | 2026-10-21 | `docs/architecture.md` §5.2 | Sửa câu | Lỗi nghiệp vụ `422` được lưu theo `Idempotency-Key`; `401/423/409/429/5xx` không chiếm key, theo CF-04 (Q-01) | [ĐÃ XÁC NHẬN] | Có, khớp với CF-04 và mã |
| CHG-05 | 2026-10-21 | `docs/architecture.md` §5.2 | Sửa sequence | Thứ tự: xác thực token → idempotency → PIN → khoảng số tiền → step-up → khóa ví → hạn mức → bút toán, theo fsd/04 (Q-02); thêm link tới bảng thứ tự của fsd/04 | [ĐÃ XÁC NHẬN] | Có, khớp với fsd/04 |
| CHG-06 | 2026-10-21 | `docs/api/wallet-core.openapi.yaml` (xóa), `redocly.yaml`, `.redocly.lint-ignore.yaml` (xóa), link ở `docs/README.md`, `architecture.md` §7, §11, `fsd/README.md`; wallet-core `api/openapi.yaml`, `redocly.yaml`, `.redocly.lint-ignore.yaml` | Chuyển nguồn chuẩn | OpenAPI của wallet-core chuyển về repo `E-Wallet-Wallet-Core` (Q-07). Bản ở wallet-core được đồng bộ với bản mới nhất của docs trước, kèm cấu hình lint | [ĐÃ XÁC NHẬN] | Không |
| CHG-07 | 2026-10-21 | wallet-core `CLAUDE.md` | Sửa tham chiếu | `scope.md` → `business-rules.md`, thêm `fsd/`; mô tả `api/openapi.yaml` là nguồn chuẩn (ISS-35, Q-07) | [ĐÃ BIÊN TẬP] | Không |

CHG-03…05 và phần docs của CHG-06 nằm trong PR vào `main` của repo `E-Wallet`; phần wallet-core của CHG-06 và CHG-07 nằm trong PR của repo `E-Wallet-Wallet-Core`. Nên merge PR wallet-core trước, để contract luôn có ở ít nhất một nơi.

## 2. Thay đổi đề xuất, chờ xác nhận

Các thay đổi dưới đây **chưa được thực hiện**. Mỗi dòng sẽ chuyển sang §1 với ID `CHG-xx` khi được làm.

### 2.1. Biên tập (không đổi nghĩa)

| Đề xuất | Tệp | Nội dung thay đổi | Phụ thuộc |
|---|---|---|---|
| DX-03 | architecture.md, business-rules.md, erd.md | Đồng bộ dòng trạng thái / ngày với lịch sử | Q-04 |
| DX-04 | `docs/glossary.md` (mới), `Q&A.md` | Tạo bảng thuật ngữ từ B §6 | Q-23 |
| DX-07 | architecture.md, fsd/README, BR-40 | Thống nhất tên gọi theo ISS-13 (b)–(g) | Q-25 |
| DX-08 | urd.md, architecture.md, erd.md, OpenAPI | Ghi tên key cạnh con số | Q-20 |

### 2.2. Nghiệp vụ (có thể đổi hành vi hoặc câu chữ của quy tắc)

| Đề xuất | Tệp | Nội dung thay đổi | Phụ thuộc |
|---|---|---|---|
| DX-12 | architecture.md §3, §11, §13 | ADR-16 polyrepo; Go ≥ 1.26; milestone back-office | Q-03, Q-05 |
| DX-14 | business-rules.md BR-06, BR-45 | Tách vòng đời khách hàng / ví; nhánh đồng bộ | Q-08 |
| DX-15 | business-rules.md BR-28; OpenAPI `x-br` | Phạm vi áp dụng khoảng số tiền | Q-12 |
| DX-16 | business-rules.md §5 | Thêm `lookup.max_per_hour` | Q-20 |
| DX-17 | urd.md US-11, US-44 | Thêm AC | Q-28 |
| — | prd.md F-23 hoặc urd.md US-61, US-62 | Sửa liên kết F ↔ US | Q-21 |
| — | prd.md §7, business-rules.md §3 | Thống nhất danh sách ngoài phạm vi | Q-22 |
| — | business-rules.md BR-57 | Tên tài khoản ledger theo erd | Q-25 |
| DX-19 | fsd/04 | Ghi "Giới hạn ở M0" cho các bước chưa làm trong Sprint 1 | Q-31 |

### 2.3. Ngoài tài liệu

| Đề xuất | Nơi | Nội dung | Phụ thuộc |
|---|---|---|---|
| — | Mã `E-Wallet-Wallet-Core` | Mã lỗi (ISS-37), `getPayment` (ISS-38), `GET /v1/wallet` (ISS-40) | Q-19, Q-30 |

## 3. Lịch sử của thư mục `chuan-hoa/`

- 2026-10-21: bản đầu A–I, dựa trên `E-Wallet@087c3ff` và `E-Wallet-Wallet-Core@7332c9e`.
- 2026-10-21: áp dụng câu trả lời Q-01, Q-02, Q-06, Q-07 (CHG-03…07); cập nhật D, E, F, G, I và các tham chiếu OpenAPI ở B, H. A, C giữ nguyên là ảnh chụp lúc rà soát.
