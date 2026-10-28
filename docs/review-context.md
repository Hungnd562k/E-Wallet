# Chỉ mục context cho reviewer

> Trạng thái: **DRAFT** · Cập nhật: 2026-10-28
> Mục đích: với mỗi thư mục code bị đổi trong PR, chỉ ra các mục tài liệu reviewer cần đọc, để không phải nạp toàn bộ tài liệu. Dùng cùng [review-policy.md](review-policy.md).
> Đường dẫn tài liệu bên dưới tính từ thư mục `docs/` của repo `E-Wallet` (trong workflow review nằm ở `e-wallet-docs/docs/`).

## 0. Luôn đọc

- `CLAUDE.md` và `REVIEW.md` của repo code.
- [fsd/README.md](fsd/README.md) §4 (`CF-01`…`CF-08`) khi PR đụng tới API hoặc luồng tiền.
- Ticket Jira trong `.ai-review/jira.md`; với `US-xx` thì đọc AC trong [urd.md](urd.md) §2 và tìm `UC-xx` qua [fsd/README.md](fsd/README.md) §7.

## 1. Thành phần

| Thành phần | Repo | Trạng thái |
|---|---|---|
| `wallet-core` | `Hungnd562k/E-Wallet-Wallet-Core` (nhánh `development`) | Đang phát triển |
| `corebank-sim` | chưa có repo | Kế hoạch M1 (architecture §3, §7.1, ADR-10, ADR-12) |
| `merchant-sim` | chưa có repo | Kế hoạch M2 (architecture §7.2, ADR-11) |
| `app-mobile` | chưa có repo | Kế hoạch M4 (architecture §7.3) |
| `back-office-web` | chưa có repo | Kế hoạch (architecture §7.3) |

Khi tạo repo mới: thêm dòng ở bảng trên và một mục §N bên dưới.

## 2. `wallet-core`

| Đường dẫn bị đổi | Đọc thêm |
|---|---|
| `internal/ledger/`, `migrations/*ledger*` | architecture §4.1, §5.2, ADR-08, ADR-09; erd §3 (tài khoản, ma trận bút toán §3.2), §4.4; business-rules BR-24, BR-50, BR-57 |
| `internal/wallet/`, `migrations/*wallet*` | erd §4.3, §6 (máy trạng thái ví); business-rules BR-05…07, BR-62, BR-63; fsd/07 UC-07.01; fsd/README CF-08 |
| `internal/payment/`, `migrations/*payment*` | architecture §5.2, §5.3; erd §4.7, §6; business-rules BR-20…23, BR-45…52; fsd/04 UC-04.01, UC-04.02; fsd/07 UC-07.03, UC-07.04; fsd/README CF-03, CF-04 |
| `internal/limit/`, `migrations/*limits*` | erd §4.5; business-rules BR-25…29, BR-37, §5 (tham số); fsd/07 UC-07.02 |
| `internal/identity/`, `migrations/*identity*` | architecture §8.1; erd §4.1; business-rules BR-01…03, BR-38, BR-39, BR-70; fsd/04 UC-04.02 (PIN) |
| `internal/authz/` | architecture §8.1; erd §4.6; business-rules BR-35…40; fsd/README CF-05 |
| `internal/audit/`, `migrations/*audit*` | erd §4.12; business-rules BR-64, BR-71; fsd/09 UC-09.01; fsd/README CF-02 |
| `internal/kyc/` | erd §4.2; business-rules BR-04, BR-06…08 |
| `internal/banklink/` | architecture §5.3, §7.1; erd §4.8; business-rules BR-10…14, BR-22, BR-49; [api/corebank.openapi.yaml](api/corebank.openapi.yaml) |
| `internal/merchant/` | architecture §7.2; erd §4.9; business-rules BR-51…53 |
| `internal/reconciliation/` | erd §4.10; business-rules BR-56, BR-58, BR-64 |
| `internal/risk/`, `internal/dispute/`, `internal/notification/` | erd §4.11; business-rules BR-47, BR-60, BR-61, BR-74 |
| `internal/backoffice/` | erd §4.13; business-rules BR-64; fsd/09 |
| `internal/platform/db/`, `internal/platform/idempotency/` | architecture §5; erd §4.14; fsd/README CF-04 |
| `internal/platform/outbox/`, `cmd/worker/` | architecture §5.1, §6, ADR-03; erd §4.14 |
| `internal/platform/observability/` | architecture §8.2, §10; business-rules BR-70…73; fsd/README CF-02 |
| `internal/platform/auth/`, `internal/platform/httpx/` | architecture §7.3, §8; fsd/README CF-06 |
| `internal/archtest/`, import mới giữa các module | architecture §4.2, §4.3 |
| `migrations/` (bất kỳ) | architecture §9; erd §1, §2, §7, §8 |
| `api/openapi.yaml` | architecture §7.3; fsd tương ứng với `operationId` (fsd/README §7) |
| `docs/go-map/`, `learn/` | Không cần tài liệu nghiệp vụ; chỉ kiểm tra anchor `file:line` còn đúng |

## 3. Lịch sử

- 2026-10-28: Bản đầu (ADR-21).
