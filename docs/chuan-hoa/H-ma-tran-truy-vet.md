# H. Ma trận truy vết

> Trạng thái: **DRAFT** · Lập: 2026-10-21 · Nguồn: urd §2, prd §7, fsd/README §7, OpenAPI `x-br`, erd §9, architecture §14, mã `wallet-core` nhánh `development` commit `7332c9e`, cập nhật cột Triển khai theo `9a05355` (2026-10-25)
> Liên quan: [README](README.md) · [C. Danh mục yêu cầu](C-danh-muc-yeu-cau.md) · [D. Vấn đề](D-bao-cao-van-de.md)

Chuỗi truy vết theo quy ước của dự án (docs/README §2): **Persona → US → F → UC → BR → API `operationId` / bảng ERD → TC**.

Nguyên tắc lập ma trận: **chỉ ghi liên kết có trong tài liệu nguồn**. Ô "—" nghĩa là nguồn chưa có liên kết đó; không suy diễn để lấp chỗ trống. Vì vậy ma trận **đủ dữ liệu cho M0** và chỉ một phần cho M1–M4.

## H.1. Persona → Epic

Chép từ urd §4.

| Persona | Epic / User story |
|---|---|
| P1, P2 | E1–E6 (US-01…51) |
| P3 SUPPORT | US-70, 79 |
| P4 OPERATOR | US-71…76, 78, 79 |
| P5 CHECKER | US-75, 77, 81 |
| P6 ADMIN | US-80, 81 |
| P7 Merchant | US-60…63 |

## H.2. US → F → UC → BR → API → ERD → TC → Triển khai

- **UC, API, ERD**: chỉ lấy từ ma trận fsd/README §7 (UC đã viết).
- **TC**: chưa có — `failure-scenarios.md` chưa viết (ISS-24).
- **Triển khai**: đối chiếu với mã `E-Wallet-Wallet-Core` nhánh `development` (nhánh mặc định), commit `9a05355` (2026-10-25). Chỉ là quan sát, không phải nghiệm thu.

| US | F | Milestone | BR (urd) | UC | API (`operationId`) | Dữ liệu chính (erd) | TC | Triển khai |
|---|---|---|---|---|---|---|---|---|
| US-01 | F-01 | M3 (M0 dùng seed) | BR-03 | — | — | — | — | Chưa |
| US-02 | F-02 | M3 | BR-70 | — | — | — | — | Chưa |
| US-03 | F-01 | M3 (M0 dùng seed) | BR-35, 41 | — | — | — | — | Chưa |
| US-04 | F-03 | M3 | BR-01, 04…06 | — | — | — | — | Chưa |
| US-05 | F-01 | M3 (M0 dùng seed) | BR-38, 39 | — | — | — | — | Chưa |
| US-06 | F-01 | M3 (M0 dùng seed) | BR-38 | — | — | — | — | Chưa |
| US-07 | F-01 | M3 (M0 dùng seed) | BR-39 | — | — | — | — | Chưa |
| US-08 | F-03 | M3 | BR-07 | — | — | — | — | Chưa |
| US-10 | F-08 | M1 | BR-10…12, 14 | — | — | — | — | Chưa |
| US-11 | F-08 | M1 | — | — | — | — | — | Chưa |
| US-12 | F-08 | M1 | BR-13 | — | — | — | — | Chưa |
| US-20 | F-09 | M1 | BR-20, 49 | — | — | — | — | Chưa |
| US-21 | F-05 | M0 | BR-23…29, 47 | UC-04.01 | `lookupRecipient`, `createTransfer` | `payment.payments`, `ledger.*`, `limits.*`, `platform.idempotency_keys`, `platform.outbox_events` | — | Một phần — `POST /v1/transfers` đang chuyển sang tra theo `recipient.phone` (SCRUM-25) nhưng còn lỗi, nhánh `recipient.wallet_id` bị mất (ISS-41); chưa có PIN, hạn mức, `lookupRecipient` (ISS-39) |
| US-22 | F-10 | M1 | BR-21, 50 | — | — | — | — | Chưa |
| US-23 | F-11 | M1 | BR-22, 25, 50 | — | — | — | — | Chưa |
| US-24 | F-07 | M0 / M3 | BR-35…40 | UC-04.02 | (trong mọi lệnh tạo giao dịch) | `identity.credentials` | — | Chưa — trường `pin` được nhận nhưng bị bỏ qua (ISS-39) |
| US-25 | F-14 | M0 (API) / M4 (push) | BR-45, 46 | UC-07.04 | `getPayment` | `payment.payments` | — | Một phần — `GET /v1/payments/{id}` chỉ cho người khởi tạo (ISS-38) |
| US-30 | F-12 | M2 | BR-51, 52 | — | — | — | — | Chưa |
| US-31 | F-13 | M2 | BR-48 | — | — | — | — | Chưa |
| US-40 | F-04 | M0 | BR-24, 50 | UC-07.01 | `getWallet` | `wallet.wallets`, `ledger.accounts` | — | Một phần — `GET /v1/wallet`; `held_balance` cố định 0 (đúng ghi chú M0 của fsd/07) |
| US-41 | F-14 | M0 (API) / M4 (push) | — | UC-07.03 | `listWalletActivities` | `payment.wallet_activities` | — | Chưa — read-model `payment.wallet_activities` đã được ghi khi chuyển tiền, chưa có endpoint |
| US-42 | F-06 | M0 | BR-25…28 | UC-07.02 | `getWalletLimits` | `limits.usage` | — | Chưa — đã có migration `limits`, chưa có endpoint và chưa kiểm hạn mức (ISS-39) |
| US-43 | F-15 | M4 | BR-62 | — | — | — | — | Chưa |
| US-44 | F-14 | M0 (API) / M4 (push) | — | — | — | — | — | Chưa |
| US-45 | F-15 | M4 | BR-63 | — | — | — | — | Chưa |
| US-50 | F-16 | M4 | BR-47, 60 | — | — | — | — | Chưa |
| US-51 | F-16 | M4 | BR-60 | — | — | — | — | Chưa |
| US-60 | F-12, F-23 | M2 | BR-51…53 | — | — | — | — | Chưa |
| US-61 | F-12 | M2 | BR-51 | — | — | — | — | Chưa |
| US-62 | F-13 | M2 | BR-48 | — | — | — | — | Chưa |
| US-63 | F-23 | M2 | — | — | — | — | — | Chưa |
| US-70 | F-20 | M4 | BR-71 | — | — | — | — | Chưa |
| US-71 | F-20 | M4 | BR-06 | — | — | — | — | Chưa |
| US-72 | F-20 | M4 | BR-71 | — | — | — | — | Chưa |
| US-73 | F-20 | M4 | BR-06, 61 | — | — | — | — | Chưa |
| US-74 | F-17 | M1 | BR-46, 58 | — | — | — | — | Chưa |
| US-75 | F-17 | M1 | BR-56, 58 | — | — | — | — | Chưa |
| US-76 | F-18 | M4 | BR-58, 64 | — | — | — | — | Chưa |
| US-77 | F-18 | M4 | BR-64 | — | — | — | — | Chưa |
| US-78 | F-19 | M4 | BR-61, 74 | — | — | — | — | Chưa |
| US-79 | F-16 | M4 | BR-60 | — | — | — | — | Chưa |
| US-80 | F-21 | M4 | BR-12, 25…28, 36 | — | — | — | — | Chưa |
| US-81 | F-22 | M0 | BR-64, 71 | UC-09.01 | `adminListAuditLogs` | `audit.audit_logs` | — | Chưa — đã có migration `audit`, chưa có endpoint |

## H.3. BR → API → ERD → Module

- **API**: từ thuộc tính `x-br` của từng operation trong OpenAPI của wallet-core (nguồn có sẵn, không suy diễn). Từ 2026-10-21 tệp này nằm ở repo `E-Wallet-Wallet-Core` (`api/openapi.yaml`, Q-07).
- **ERD**: từ erd §9. **Module**: từ architecture §14 (theo nhóm BR).

| BR | API (`x-br`) | Bảng / ràng buộc (erd §9) | Module (arch §14) |
|---|---|---|---|
| BR-01 | — | `identity.customers.date_of_birth`, `wallet.wallets` UK `customer_id` | `identity`, `kyc`, `wallet`, MinIO, mock providers |
| BR-02 | `getWallet` | `identity.customers.date_of_birth`, `wallet.wallets` UK `customer_id` | `identity`, `kyc`, `wallet`, MinIO, mock providers |
| BR-03 | `requestRegisterOtp`, `verifyRegisterOtp` | `identity.phone_verifications`, `customers.phone` UK | `identity`, `kyc`, `wallet`, MinIO, mock providers |
| BR-04 | `createKycSession`, `submitKycSession` | `kyc.sessions`, `kyc.documents`, `kyc.checks`, `kyc.verified_identities` | `identity`, `kyc`, `wallet`, MinIO, mock providers |
| BR-05 | `getWallet`, `confirmBankLink` | Chuyển trạng thái ví §6 | `identity`, `kyc`, `wallet`, MinIO, mock providers |
| BR-06 | `createKycSession`, `submitKycSession`, `adminFreezeWallet`, `adminDecideKyc` | `kyc.sessions`, `kyc.documents`, `kyc.checks`, `kyc.verified_identities` | `identity`, `kyc`, `wallet`, MinIO, mock providers |
| BR-07 | `createKycSession` | `kyc.verified_identities.expiry_date`, `expiry_notified_at` | `identity`, `kyc`, `wallet`, MinIO, mock providers |
| BR-08 | — | `kyc.sessions`, `kyc.documents`, `kyc.checks`, `kyc.verified_identities` | `identity`, `kyc`, `wallet`, MinIO, mock providers |
| BR-10 | `createBankLink` | `banklink.links` (`source_type` không có CREDIT_CARD, partial UK), corebank `cards`, `wallet_links` | `banklink` ↔ corebank `/v1/accounts`, `/v1/links` |
| BR-11 | `createBankLink` | `banklink.links` (`source_type` không có CREDIT_CARD, partial UK), corebank `cards`, `wallet_links` | `banklink` ↔ corebank `/v1/accounts`, `/v1/links` |
| BR-12 | `createBankLink` | `banklink.links` (`source_type` không có CREDIT_CARD, partial UK), corebank `cards`, `wallet_links` | `banklink` ↔ corebank `/v1/accounts`, `/v1/links` |
| BR-13 | `deleteBankLink` | `banklink.links` (`source_type` không có CREDIT_CARD, partial UK), corebank `cards`, `wallet_links` | `banklink` ↔ corebank `/v1/accounts`, `/v1/links` |
| BR-14 | `createBankLink`, `confirmBankLink` | `banklink.links` (`source_type` không có CREDIT_CARD, partial UK), corebank `cards`, `wallet_links` | `banklink` ↔ corebank `/v1/accounts`, `/v1/links` |
| BR-20 | `createTopup` | `payment.payments`, ma trận §3.2, `ledger.accounts` CHECK số dư | `payment`, `ledger`, `limit`, `banklink` |
| BR-21 | `createWithdrawal` | `payment.payments`, ma trận §3.2, `ledger.accounts` CHECK số dư | `payment`, `ledger`, `limit`, `banklink` |
| BR-22 | `createInterbankTransfer` | `payment.payments`, ma trận §3.2, `ledger.accounts` CHECK số dư | `payment`, `ledger`, `limit`, `banklink` |
| BR-23 | `lookupRecipient`, `createTransfer` | `payment.payments`, ma trận §3.2, `ledger.accounts` CHECK số dư | `payment`, `ledger`, `limit`, `banklink` |
| BR-24 | `createTransfer` | `payment.payments`, ma trận §3.2, `ledger.accounts` CHECK số dư | `payment`, `ledger`, `limit`, `banklink` |
| BR-25 | `getWalletLimits`, `createTransfer`, `createInterbankTransfer`, `payQr` | `limits.usage`, `limits.consumptions`, `merchant.merchants.category`, `platform.settings` | `payment`, `ledger`, `limit`, `banklink` |
| BR-26 | `getWalletLimits`, `payQr` | `limits.usage`, `limits.consumptions`, `merchant.merchants.category`, `platform.settings` | `payment`, `ledger`, `limit`, `banklink` |
| BR-27 | — | `limits.usage`, `limits.consumptions`, `merchant.merchants.category`, `platform.settings` | `payment`, `ledger`, `limit`, `banklink` |
| BR-28 | `getWalletLimits`, `createTransfer`, `createWithdrawal`, `createInterbankTransfer` | `limits.usage`, `limits.consumptions`, `merchant.merchants.category`, `platform.settings` | `payment`, `ledger`, `limit`, `banklink` |
| BR-29 | `createTransfer` | `limits.usage`, `limits.consumptions`, `merchant.merchants.category`, `platform.settings` | `payment`, `ledger`, `limit`, `banklink` |
| BR-30 | — | `payment.payments.fee`, account `FEE_REVENUE` | `payment`, `ledger`, `limit`, `banklink` |
| BR-35 | `setPin`, `createTopup`, `createTransfer`, `createWithdrawal`, `createInterbankTransfer`, `payQr` | `identity.credentials`, `identity.devices`, `authz.challenges`, `payment.payments.auth_level` | `authz`, `identity` (device, PIN) |
| BR-36 | `getWalletLimits`, `createTopup`, `createTransfer`, `createWithdrawal`, `createInterbankTransfer`, `payQr` | `identity.credentials`, `identity.devices`, `authz.challenges`, `payment.payments.auth_level` | `authz`, `identity` (device, PIN) |
| BR-37 | `createTopup`, `createWithdrawal` | `limits.usage`, `limits.consumptions`, `merchant.merchants.category`, `platform.settings`; `identity.credentials`, `identity.devices`, `authz.challenges`, `payment.payments.auth_level` | `authz`, `identity` (device, PIN) |
| BR-38 | `verifyRegisterOtp`, `login`, `getWalletLimits` | `identity.credentials`, `identity.devices`, `authz.challenges`, `payment.payments.auth_level` | `authz`, `identity` (device, PIN) |
| BR-39 | `login`, `setPin`, `resetPin` | `identity.credentials`, `identity.devices`, `authz.challenges`, `payment.payments.auth_level` | `authz`, `identity` (device, PIN) |
| BR-40 | — | `identity.credentials`, `identity.devices`, `authz.challenges`, `payment.payments.auth_level` | `authz`, `identity` (device, PIN) |
| BR-41 | — | — | — |
| BR-45 | `getPayment` | `payment.payments`, `payment.status_history`, `banklink.operations`, `ledger.holds` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-46 | `getPayment`, `adminResolvePayment` | `payment.payments`, `payment.status_history`, `banklink.operations`, `ledger.holds` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-47 | `createTransfer`, `createDispute` | `payment.payments`, `payment.status_history`, `banklink.operations`, `ledger.holds` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-48 | `merchantCreateRefund` | `payment.payments`, `payment.status_history`, `banklink.operations`, `ledger.holds` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-49 | `createTopup` | `payment.payments`, `payment.status_history`, `banklink.operations`, `ledger.holds` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-50 | `getWallet`, `createWithdrawal`, `createInterbankTransfer` | `payment.payments`, `payment.status_history`, `banklink.operations`, `ledger.holds` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-51 | `payQr`, `merchantCreateOrder`, `merchantGetOrder` | `merchant.orders` (UK, `qr_token_hash`, `expires_at`), `merchant.callback_deliveries`, UK QR_PAYMENT theo `order_id` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-52 | `resolveQr`, `payQr`, `merchantCreateOrder` | `merchant.orders` (UK, `qr_token_hash`, `expires_at`), `merchant.callback_deliveries`, UK QR_PAYMENT theo `order_id` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-53 | `resolveQr`, `merchantCreateOrder` | `merchant.merchants.status` | `payment`, `merchant`, worker (bank runner, callback) |
| BR-55 | — | `SAFEGUARD_BANK`, `RECON_SUSPENSE`, `recon.runs/items/adjustments`, corebank `accounts.type = SAFEGUARD` | corebank (TK đảm bảo), `ledger.bank_settlement`, `reconciliation`, scheduler `recon.daily` |
| BR-56 | `adminListReconRuns` | `SAFEGUARD_BANK`, `RECON_SUSPENSE`, `recon.runs/items/adjustments`, corebank `accounts.type = SAFEGUARD` | corebank (TK đảm bảo), `ledger.bank_settlement`, `reconciliation`, scheduler `recon.daily` |
| BR-57 | — | `SAFEGUARD_BANK`, `RECON_SUSPENSE`, `recon.runs/items/adjustments`, corebank `accounts.type = SAFEGUARD` | corebank (TK đảm bảo), `ledger.bank_settlement`, `reconciliation`, scheduler `recon.daily` |
| BR-58 | `adminResolvePayment`, `adminListReconRuns`, `adminCreateAdjustment` | `SAFEGUARD_BANK`, `RECON_SUSPENSE`, `recon.runs/items/adjustments`, corebank `accounts.type = SAFEGUARD` | corebank (TK đảm bảo), `ledger.bank_settlement`, `reconciliation`, scheduler `recon.daily` |
| BR-60 | `createDispute`, `adminUpdateDispute` | `dispute.disputes` | `dispute`, `risk`, `wallet`, `reconciliation`, `backoffice`, `audit` |
| BR-61 | `adminReactivateWallet`, `adminUpdateAmlAlert` | `risk.aml_alerts` | `dispute`, `risk`, `wallet`, `reconciliation`, `backoffice`, `audit` |
| BR-62 | `closeWallet` | `wallet.wallets` (`last_activity_at`, `dormant_notified_at`, `closed_at`) | `dispute`, `risk`, `wallet`, `reconciliation`, `backoffice`, `audit` |
| BR-63 | — | `wallet.wallets` (`last_activity_at`, `dormant_notified_at`, `closed_at`) | `dispute`, `risk`, `wallet`, `reconciliation`, `backoffice`, `audit` |
| BR-64 | `adminFreezeWallet`, `adminCreateAdjustment`, `adminApproveAdjustment` | `recon.adjustments` (CHECK maker ≠ checker), `audit.audit_logs` | `dispute`, `risk`, `wallet`, `reconciliation`, `backoffice`, `audit` |
| BR-70 | `grantConsents`, `withdrawConsent`, `createKycSession` | `identity.consents` | `identity` (consent), `platform/crypto`, `platform/observability` (redaction), `audit`, `risk` |
| BR-71 | `adminCreateKycDocumentViewUrl` | Các cột `_enc` / `_hash`, quyền trên `audit` | `identity` (consent), `platform/crypto`, `platform/observability` (redaction), `audit`, `risk` |
| BR-72 | — | — | `identity` (consent), `platform/crypto`, `platform/observability` (redaction), `audit`, `risk` |
| BR-73 | — | §8 | `identity` (consent), `platform/crypto`, `platform/observability` (redaction), `audit`, `risk` |
| BR-74 | `adminListAmlAlerts`, `adminUpdateAmlAlert` | `risk.aml_alerts` | `identity` (consent), `platform/crypto`, `platform/observability` (redaction), `audit`, `risk` |

## H.4. Khoảng trống truy vết

| Khoảng trống | Số lượng | Chi tiết | Vấn đề |
|---|---|---|---|
| US chưa có UC | 36/43 | US đã có UC: US-21, US-24, US-40, US-42, US-41, US-25, US-81 | ISS-25 |
| US chưa có TC | 43/43 | Toàn bộ | ISS-24 |
| BR không có operation nào gắn `x-br` | 11/54 | BR-01, BR-08, BR-27, BR-30, BR-40, BR-41, BR-55, BR-57, BR-63, BR-72, BR-73 | Phần lớn hợp lệ (quy tắc nội bộ / job). Cần soát: BR-28 thiếu ở `payQr`, `createTopup` (ISS-15) |
| BR không có trong erd §9 | 2/54 | BR-41, BR-72 | Hợp lệ với quy tắc không cần lưu trữ riêng; BR-72 không có artefact (ISS-30) |
| Liên kết F ↔ US lệch giữa PRD và URD | 2 | F-23 liệt kê US-61, US-62; URD gắn US-61 → F-12, US-62 → F-13 | ISS-10 |

## H.5. Ma trận F → Milestone → trạng thái tài liệu

| F | Milestone | FSD | Có UC đã viết | Có TC |
|---|---|---|---|---|
| F-01 | M3 (M0 dùng seed) | fsd/01 | — | — |
| F-02 | M3 | fsd/01 | — | — |
| F-03 | M3 | fsd/01 | — | — |
| F-04 | M0 | fsd/07 | UC-07.01 | — |
| F-05 | M0 | fsd/04 | UC-04.01 | — |
| F-06 | M0 | fsd/07 | UC-07.02 | — |
| F-07 | M0 / M3 | fsd/01, fsd/04 | UC-04.02 | — |
| F-08 | M1 | fsd/02 | — | — |
| F-09 | M1 | fsd/03 | — | — |
| F-10 | M1 | fsd/05 | — | — |
| F-11 | M1 | fsd/05 | — | — |
| F-12 | M2 | fsd/06 | — | — |
| F-13 | M2 | fsd/06 | — | — |
| F-14 | M0 (API) / M4 (push) | fsd/07 | UC-07.03, UC-07.04 | — |
| F-15 | M4 | fsd/07 | — | — |
| F-16 | M4 | fsd/08 | — | — |
| F-17 | M1 | fsd/09 | — | — |
| F-18 | M4 | fsd/09 | — | — |
| F-19 | M4 | fsd/09 | — | — |
| F-20 | M4 | fsd/09 | — | — |
| F-21 | M4 | fsd/09 | — | — |
| F-22 | M0 | fsd/09 | UC-09.01 | — |
| F-23 | M2 | fsd/06 | — | — |
