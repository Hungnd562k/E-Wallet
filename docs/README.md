# Tài liệu dự án E-Wallet

Mục lục và quy ước của bộ tài liệu. Đọc theo thứ tự từ trên xuống: **vì sao → cho ai → làm gì → chạy chi tiết ra sao → xây thế nào**.

## 1. Phân cấp tài liệu

```mermaid
flowchart TD
    PRD[prd.md<br/>Sản phẩm: mục tiêu, tính năng F-xx, NFR, lộ trình]
    URD[urd.md<br/>Người dùng: persona, user story US-xx, AC]
    BR[business-rules.md<br/>Quy tắc BR-xx + căn cứ pháp lý]
    FSD[fsd/<br/>Đặc tả chức năng: use case UC-xx]
    ARCH[architecture.md]
    ERD[erd.md]
    API[api/*.openapi.yaml]
    TEST[failure-scenarios.md<br/>Kịch bản kiểm thử TC-xx]

    PRD --> URD
    PRD --> BR
    URD --> FSD
    BR --> FSD
    FSD --> ARCH
    FSD --> ERD
    FSD --> API
    FSD --> TEST
```

| Tài liệu | Loại (chuẩn BA) | Trả lời câu hỏi | Người đọc chính | Trạng thái |
|---|---|---|---|---|
| [prd.md](prd.md) | **PRD** (gộp BRD) | Sản phẩm giải quyết vấn đề gì, gồm tính năng nào, ưu tiên ra sao, khi nào xong, đo thành công thế nào | Tất cả | Draft |
| [urd.md](urd.md) | **URD** | Từng nhóm người dùng cần làm gì; thế nào là đạt | BA, dev, tester | Draft |
| [business-rules.md](business-rules.md) | Business rules catalogue (một phần của SRS) | Quy tắc nghiệp vụ nào bắt buộc, căn cứ pháp lý nào | BA, dev | Draft |
| [fsd/](fsd/) | **FSD / SRS** | Mỗi chức năng chạy chi tiết ra sao: luồng chính/thay thế/ngoại lệ, màn hình, validation, thông báo | Dev, tester | **Chưa viết** |
| [architecture.md](architecture.md) | TDD / SAD | Hệ thống được chia thành những thành phần nào, giao tiếp ra sao | Dev | Draft |
| [erd.md](erd.md) | TDD — mô hình dữ liệu | Dữ liệu lưu thế nào, ràng buộc gì, bút toán ra sao | Dev | Draft |
| [api/wallet-core.openapi.yaml](api/wallet-core.openapi.yaml), [api/corebank.openapi.yaml](api/corebank.openapi.yaml) | Interface spec | Hợp đồng API giữa các hệ thống | Dev (BE, FE, mobile) | Draft, lint sạch |
| `failure-scenarios.md` | Test spec | Các kịch bản lỗi và cách kiểm chứng | Dev, tester | Chưa viết |
| [Q&A.md](Q&A.md) | Ghi chú cá nhân | Giải thích thuật ngữ | — | — |

## 2. Quy ước ID và chuỗi truy vết

| Tiền tố | Ý nghĩa | Định nghĩa tại |
|---|---|---|
| `SM-xx` | Chỉ số thành công | prd.md §3 |
| `F-xx` | Tính năng | prd.md §7 |
| `NFR-xx` | Yêu cầu phi chức năng (hệ thống) | prd.md §8 |
| `P1…P7` | Persona | urd.md §1 |
| `US-xx` | User story | urd.md §2 |
| `UR-NF-xx` | Yêu cầu phi chức năng (người dùng) | urd.md §3 |
| `BR-xx` | Quy tắc nghiệp vụ, nhãn **[L]** luật / **[P]** quyết định sản phẩm | business-rules.md §4 |
| `UC-xx` | Use case | fsd/*.md |
| `ADR-xx` | Quyết định kiến trúc | architecture.md §13 |
| `TC-xx` | Kịch bản kiểm thử | failure-scenarios.md |

Chuỗi truy vết: **Persona → US → F → UC → BR → API `operationId` / bảng ERD → TC**.

Ví dụ: P1 Lan → US-21 chuyển ví–ví → F-05 → UC (fsd/04) → BR-24, 25, 29, 36 → `createTransfer`, `ledger.entries`, `limits.usage` → TC chuyển song song.

## 3. Quy ước viết

- Tiếng Việt, thuật ngữ kỹ thuật giữ nguyên tiếng Anh khi không có từ tương đương phổ biến.
- Mỗi tài liệu có dòng đầu: **Trạng thái · Ngày cập nhật · Liên quan**.
- Không lặp lại nội dung: tài liệu sau **trỏ tới** tài liệu trước bằng ID, không chép lại quy tắc.
- Mọi con số (hạn mức, ngưỡng, thời hạn) là tham số cấu hình — xem business-rules.md §5.

## 4. Lịch sử cấu trúc

- 2026-10-17: chuẩn hóa theo URD / PRD / FSD.
  - `planning.md` → `prd.md`; phần kỹ thuật của file cũ chuyển sang architecture / erd / api.
  - `scope.md` → `business-rules.md`.
  - `money-flows.md` (đã dự kiến trước đó) không tạo riêng nữa: sequence diagram và bút toán của từng luồng tiền nằm trong file FSD tương ứng.
