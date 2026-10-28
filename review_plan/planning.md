## Quy trình review PR sử dụng Claude AI

Có. Và với cách bạn đang phát triển dự án bằng các file `.md` mô tả spec cho AI đọc và code, bạn hoàn toàn có thể nâng cấp quy trình đó thành một AI Code Reviewer hiểu nghiệp vụ, hiểu kiến trúc và hiểu quy tắc của toàn bộ repository, sử dụng Claude của bạn để review mỗi Pull Request trước khi merge.

Điểm quan trọng nhất: đừng chỉ yêu cầu Claude review một đoạn diff. Hãy thiết kế để nó review thay đổi code trong ngữ cảnh của ticket Jira, tài liệu nghiệp vụ, kiến trúc và quy tắc dự án.

Ví dụ, developer tạo một PR có thông tin:

- Jira: `WALLET-142` — Chuyển tiền giữa hai ví.
- Commit: cập nhật `TransferService`, `WalletRepository` và xử lý số dư.
- Tài liệu: `docs/business/transfer.md`, `docs/architecture/ledger.md`.
- Quy tắc: `CLAUDE.md`, quy tắc transaction và idempotency.

Claude cần kiểm tra không chỉ code có chạy hay không, mà còn những câu hỏi như:

- Có thể trừ tiền ví A thành công nhưng cộng tiền ví B thất bại không?
- Hai request đồng thời có thể khiến ví bị trừ tiền hai lần không?
- Retry cùng một request có tạo hai giao dịch không?
- Code có vi phạm state machine hoặc quy tắc ledger đã mô tả trong tài liệu không?
- Có thiếu test cho các trường hợp nghiệp vụ quan trọng không?

Đây là loại review đáng xây dựng cho dự án e-wallet của bạn.

## 1. Có 3 cách triển khai — nên chọn cách nào?

[Claude](https://images.openai.com/static-rsc-4/cLHBfiXuOV35_PSb5wC1jXivsAXbOR_8j6QIuMUVJNmHhvqDTZSAjip3DZXEZE8rrigO3zrBPXPTm3OPFqtXG1oYbfxtfR2epfKwRiaR-r0auw26KWwnnPiRgtOfu6qqeScD6KjrINNkb8STcXPBIHcixXdk1xpiKuwC8UdBT_8?purpose=inline)

Phương án A — Claude Code Review chính thức

Nhanh nhất để thử

Cài GitHub App của Anthropic, bật Code Review cho repository. Claude tự review PR, phân tích codebase, rồi đăng nhận xét trực tiếp trên các dòng code. Có thể kích hoạt khi mở PR, mỗi lần push hoặc thủ công. Hỗ trợ tùy chỉnh bằng `CLAUDE.md` và `REVIEW.md`.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://support.claude.com\&sz=32)

Claude Help Center



Lưu ý: tính năng Code Review chính thức được tài liệu hiện tại giới hạn cho gói Team và Enterprise; chi phí review tính riêng theo usage.

[Phase 5: Setting up CI/CD with Github Actions | Cloud Connections](https://images.openai.com/static-rsc-4/EtjTqatLhTQ4emhM-y4Stkjuc5ub0rUWQCfQAOIvT7l0pgs8Oc-4JIirAvq1K1bdX4Y9LkGbE-aN81YYWxwhru716W-y9hZ0U5L3FVGvNrIbG1OzF7IjsJx3o3dXtgNN0Unw34Nzz5_eksWfem8pNzB1F0_NZnBMCOPdnTMOhL8?purpose=inline)

Phương án B — Claude Code + GitHub Actions

Khuyến nghị cho yêu cầu của bạn

Dùng GitHub Actions để tự động chạy Claude mỗi khi có PR hoặc commit mới. Bạn tự định nghĩa prompt, cách đọc Jira, tài liệu `.md`, quy tắc nghiệp vụ, cách chấm mức độ nghiêm trọng và điều kiện cho phép merge. Anthropic có action chính thức làm nền tảng cho workflow tùy chỉnh.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub+1



[Claude Code Auto Mode Pricing, Availability](https://images.openai.com/static-rsc-4/93Fgb7BsPJvrIp59xXINQT1IKIEAeDLtVmu3KF5Yz9ZEU-c1TKLCZp0nxdZyNnQinU-J96-KiBDQihN6G4obHOCTNlIOLtqymbV4eUVMNbyISlAH7ijtOtzCRwOCbUvpUS5K7CImD9ADiy8kgNUy8jm4pX6zD6QXbQmDF19O9wU?purpose=inline)

Phương án C — Xây dựng AI Reviewer riêng

Tạo một service riêng điều phối Claude API, GitHub API, Jira API và hệ thống tài liệu. Phù hợp nếu sau này muốn có nhiều agent review độc lập, lưu lịch sử đánh giá, đo tỷ lệ lỗi bị bỏ sót và áp dụng cho nhiều repository.

Đổi lại, bạn phải tự vận hành, quản lý quyền truy cập, retry, chi phí và bảo trì hệ thống.

Tôi sẽ chọn phương án B cho bạn. Bạn có thể bắt đầu bằng một workflow đơn giản, tận dụng chính những file Markdown đang dùng để AI code, sau đó mở rộng thành reviewer nhiều lớp mà không phải xây một nền tảng riêng ngay từ đầu.

## 2. Kiến trúc tôi đề xuất

GitHub Pull Request

Diff · Commit SHA · PR description · Jira key

GitHub Actions — Review Orchestrator

Lấy diff, xác định ticket, tải context và gọi Claude

Repository context

CLAUDE.md, REVIEW\.md, PRD, FSD, ADR, state machine, ERD

Jira context

Description, acceptance criteria, trạng thái và các yêu cầu liên quan

Claude — Multi-perspective Review

Code correctness · Business logic · Architecture · Project rules · Tests

Kết quả review trên PR

Inline comments · Severity · Evidence · Review status · CI check

Một điểm cần phân biệt: GitHub PR là đơn vị review chính, còn commit là đơn vị xác định chính xác thay đổi. Một PR có thể chứa nhiều commit; review nên đánh giá toàn bộ thay đổi so với base branch, đồng thời ghi nhận commit SHA để biết chính xác phiên bản code nào đã được kiểm tra.

## 3. Chuẩn hóa repository để Claude hiểu đúng dự án

Đây là phần quyết định chất lượng review. Model tốt đến đâu cũng không thể kiểm tra nghiệp vụ chính xác nếu tài liệu mơ hồ, mâu thuẫn hoặc không có quy tắc rõ ràng.

Tôi sẽ tổ chức các file theo hướng sau:

Repository root

`CLAUDE.md`

Quy tắc làm việc với codebase, kiến trúc, các điều cấm và nguyên tắc chung.

`REVIEW.md`

Checklist review, tiêu chí nghiệp vụ, severity, điều kiện báo lỗi và những vấn đề không được bỏ qua.

`docs/business/`

PRD, URD, FSD, acceptance criteria, business rules, state machine.

`docs/architecture/`

ADR, ERD, module boundaries, transaction boundaries, integration contracts.

`.github/workflows/`

Workflow chạy reviewer khi có PR hoặc commit mới.

### Vai trò của từng loại tài liệu

| File           | Claude phải dùng để làm gì?                                           |
| -------------- | --------------------------------------------------------------------- |
| `CLAUDE.md`    | Hiểu cách dự án được tổ chức, quy tắc coding và cách làm việc         |
| `REVIEW.md`    | Xác định những gì reviewer bắt buộc phải kiểm tra                     |
| `PRD` / `FSD`  | Đối chiếu hành vi được implement với yêu cầu nghiệp vụ                |
| `ADR`          | Kiểm tra quyết định kiến trúc có bị vi phạm không                     |
| `ERD` / schema | Kiểm tra quan hệ dữ liệu, constraint và invariant                     |
| State machine  | Kiểm tra transition, trạng thái hợp lệ và điều kiện chuyển trạng thái |
| Jira ticket    | Xác định mục tiêu, phạm vi và acceptance criteria của thay đổi        |

Theo hướng dẫn chính thức, Claude Code Review tự đọc `CLAUDE.md` theo cấu trúc thư mục và hỗ trợ `REVIEW.md` ở root để bổ sung quy tắc review. Tuy nhiên, với workflow tùy chỉnh, bạn vẫn phải chủ động đưa đúng tài liệu nghiệp vụ vào ngữ cảnh hoặc chỉ dẫn Claude đọc chúng.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://support.claude.com\&sz=32)

Claude Help Center+1



### Ví dụ `REVIEW.md` cho hệ thống e-wallet

Bạn có thể bắt đầu với bộ quy tắc như sau:

```

# AI Code Review Policy

## 1. Review scope
- Review all changes introduced by the PR against its base branch.
- Read the relevant Jira ticket and acceptance criteria.
- Read relevant business specifications and architecture documents.
- Review only issues introduced by the changes, unless an existing issue
  directly affects the correctness of the new implementation.

## 2. Business correctness
- Verify every applicable acceptance criterion.
- Verify valid and invalid state transitions.
- Verify authorization and ownership of wallet operations.
- Verify transaction atomicity for related database updates.
- Verify idempotency and retry behavior for money-moving operations.
- Verify concurrent requests cannot violate balance or ledger invariants.
- Verify failures and partial external-service failures are handled safely.
- Flag behavior that conflicts with documented business rules.

## 3. Architecture and project rules
- Respect module boundaries and dependency direction.
- Do not bypass domain services to modify critical business state.
- Follow repository conventions for errors, logging and transactions.
- Do not introduce undocumented changes to public API contracts.
- Flag implementation changes that require corresponding documentation updates.

## 4. Evidence requirements
- Every finding must identify the changed code and explain the failure scenario.
- Cite the relevant Jira acceptance criterion or project document when applicable.
- Distinguish confirmed defects from assumptions or missing information.
- Do not invent business requirements.
- If the requirements are ambiguous or contradictory, flag the ambiguity.

## 5. Severity
- P0: Critical security or financial integrity issue.
- P1: Major business correctness, data consistency or authorization issue.
- P2: Significant functional defect or missing required behavior.
- P3: Minor, non-blocking issue.

## 6. Signal quality
- Do not report stylistic preferences as bugs.
- Do not report a finding without a concrete rationale.
- Avoid duplicate findings for the same underlying issue.
- Report only issues introduced by the PR, except relevant regressions.

```

Các severity ở đây là quy ước bạn tự định nghĩa, không phải severity mặc định của Anthropic. Bạn có thể chỉnh lại cho phù hợp với quy trình phát hành của team.

## 4. Kết nối Jira như thế nào?

Có hai hướng triển khai.

Cách A — Jira REST API

Đơn giản, dễ kiểm soát

Workflow lấy Jira key từ tên branch, PR title hoặc PR description, ví dụ `WALLET-142`. Sau đó gọi Jira API để lấy ticket và acceptance criteria.

Ưu điểm: rõ ràng, dễ log, dễ kiểm soát dữ liệu gửi sang Claude. Bạn cần cấu hình Jira URL, tài khoản/service account và API token trong GitHub Secrets.

Cách B — Jira MCP server

Cho Claude truy cập Jira qua một lớp công cụ MCP. Hướng này phù hợp khi muốn Claude truy vấn nhiều loại thông tin, ví dụ ticket liên quan, yêu cầu liên kết và lịch sử quyết định.

Đổi lại, bạn phải quản lý MCP server, quyền truy cập và giới hạn những thao tác Claude được phép thực hiện.

Tôi khuyên dùng REST API ở giai đoạn đầu. Không cần cho Claude quyền sửa Jira; chỉ cần đọc ticket. Nếu ticket không tìm thấy, không có acceptance criteria hoặc nội dung mâu thuẫn với tài liệu, reviewer phải báo thiếu context thay vì tự suy diễn.

Bạn cũng nên bắt buộc liên kết Jira với PR, ví dụ:

```
Branch: feature/WALLET-142-transfer
PR title: [WALLET-142] Implement wallet transfer
```

Đừng để AI tự đoán ticket chỉ từ tên các file code.

## 5. Quy trình review thực tế

Tôi sẽ chia review thành năm bước logic. Ban đầu chúng có thể là các phần trong một workflow, chưa cần triển khai thành năm agent riêng biệt.

1. Lấy đúng context

   Xác định base SHA, head SHA, diff, Jira key, tài liệu liên quan và quy tắc áp dụng cho các file bị thay đổi.
2. Review code correctness

   Tìm lỗi logic, lỗi xử lý dữ liệu, concurrency, transaction, security và các trường hợp biên.
3. Review nghiệp vụ

   Đối chiếu từng acceptance criterion và business invariant với implementation. Nêu rõ tiêu chí nào bị vi phạm và code nào gây ra vi phạm.
4. Review kiến trúc và project conventions

   Kiểm tra dependency giữa các module, quy tắc coding, hợp đồng API, migration và tài liệu có cần cập nhật hay không.
5. Xác minh findings và xuất kết quả

   Kiểm tra lại các lỗi được phát hiện, loại bỏ nhận xét trùng lặp hoặc không đủ bằng chứng, chạy các kiểm tra xác định được bằng máy và đăng kết quả lên PR.

Với nghiệp vụ tài chính, bước xác minh rất quan trọng. Ví dụ, chỉ thấy code thực hiện hai câu SQL không có nghĩa chắc chắn là lỗi. Reviewer phải kiểm tra chúng có cùng transaction hay không, có cơ chế bù trừ không và trạng thái cuối cùng có thể vi phạm invariant nào.

## 6. Tự động review và chặn merge ra sao?

Bạn cần phân biệt hai việc:

- AI review: tìm lỗi, viết nhận xét, đưa ra bằng chứng.
- Merge gate: quyết định PR có được merge hay không.

Claude Code Review chính thức có thể đăng findings nhưng không tự block merge. Với phương án GitHub Actions, bạn có thể thiết kế thêm CI check và cấu hình GitHub branch protection hoặc rulesets để bắt buộc check đó phải pass.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://support.claude.com\&sz=32)

Claude Help Center+1



Tôi đề xuất quy tắc ban đầu:

| Điều kiện                                               | Kết quả                                                       |
| ------------------------------------------------------- | ------------------------------------------------------------- |
| Build hoặc test thất bại                                | Block merge                                                   |
| Claude xác nhận có lỗi P0/P1                            | Block merge                                                   |
| Thiếu Jira ticket hoặc không tải được tài liệu bắt buộc | Fail closed hoặc yêu cầu người review bổ sung context         |
| Chỉ có P2/P3                                            | Cho phép người review quyết định                              |
| Không phát hiện lỗi                                     | Chỉ là một tín hiệu tốt, không đồng nghĩa code chắc chắn đúng |

Không nên để Claude tự merge hoặc tự approve chính PR của nó trong giai đoạn đầu. Nếu cho AI quyết định gate, cần có cơ chế xác minh và quy trình xử lý khi AI không chắc chắn. Với các thay đổi ảnh hưởng đến số dư, ledger hoặc luồng chuyển tiền, vẫn nên có human review.

### Workflow mẫu

Đây là cấu trúc để bạn hình dung phần GitHub Actions cần tạo, chưa phải file hoàn chỉnh để copy chạy ngay:

```

name: AI Code Review

on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]

permissions:
  contents: read
  pull-requests: read
  issues: read

jobs:
  review:
    if: github.event.pull_request.draft == false
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v6
        with:
          ref: ${{ github.event.pull_request.head.sha }}
          fetch-depth: 0

      - name: Run Claude review
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Review this pull request against its base branch.

            Read CLAUDE.md and REVIEW.md.
            Identify the Jira ticket and retrieve its
            acceptance criteria using the configured integration.
            Read all relevant business and architecture documents.

            Review:
            1. Code correctness and security.
            2. Business rules and acceptance criteria.
            3. Architecture and project conventions.
            4. Tests, edge cases and failure scenarios.

            Report only actionable findings with evidence,
            severity and affected code locations.
            Do not approve or merge the pull request.

```

Lưu ý khi hiện thực hóa:

- Cần xác nhận phiên bản action, input và cơ chế xác thực theo tài liệu chính thức trước khi triển khai; đoạn trên là khung minh họa.
- Cần bổ sung bước lấy Jira ticket và truyền nội dung ticket cho Claude. Prompt tự nó không tạo ra kết nối Jira.
- Cần thêm bước xuất findings thành GitHub review comments hoặc check result; chỉ trả về Markdown chưa đủ để tạo merge gate.
- Không cấp quyền ghi code nếu reviewer chỉ cần đọc và nhận xét.

Tham khảo implementation và hướng dẫn bảo mật của Anthropic: [Claude Code Action trên GitHub](https://github.com/anthropics/claude-code-action?utm_source=chatgpt.com). Action chạy Claude trong ngữ cảnh repository nên bạn phải đặc biệt chú ý quyền truy cập và nội dung không đáng tin cậy từ PR, tránh để prompt injection khiến agent thực hiện các thao tác ngoài ý muốn.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub+1



## 7. Một số vấn đề quan trọng để reviewer thực sự hữu ích

### Không cần nạp toàn bộ tài liệu vào mọi lần review

Nếu PR chỉ sửa module audit, không nhất thiết phải đọc toàn bộ tài liệu chuyển tiền. Hãy dùng đường dẫn file thay đổi, module ownership và các liên kết trong tài liệu để chọn đúng context.

Với dự án modular monolith, bạn có thể tổ chức tài liệu theo module, ví dụ `docs/modules/wallet/`, `docs/modules/transfer/` và `docs/modules/ledger/`. Điều này giúp giảm chi phí và hạn chế việc Claude tham chiếu nhầm tài liệu không liên quan.

### Phân biệt lỗi thực sự với yêu cầu chưa rõ

Một reviewer tốt không chỉ tìm lỗi. Nó phải biết khi nào chưa đủ thông tin để kết luận.

Ví dụ: Jira ghi rằng chuyển tiền phải đảm bảo an toàn, nhưng không nói rõ chính sách hoàn tiền khi ngân hàng đích timeout. AI không nên tự khẳng định implementation sai chỉ vì chưa có cơ chế bù trừ theo một giả định của nó. Nó nên nêu rõ quyết định nghiệp vụ còn thiếu và yêu cầu người phụ trách xác nhận.

### Chống review sai phiên bản code

Mỗi lần push lên PR, review cũ có thể không còn áp dụng cho code mới. Hãy lưu base SHA, head SHA và kết quả kiểm tra tương ứng; chỉ sử dụng kết quả review cho đúng phiên bản đã kiểm tra. Nếu code thay đổi, phải chạy lại các bước cần thiết.

### Đo chất lượng AI reviewer

Sau một thời gian, hãy theo dõi:

- Tỷ lệ findings được developer xác nhận là lỗi thật.
- Tỷ lệ lỗi nghiệp vụ quan trọng bị bỏ sót.
- Tỷ lệ nhận xét trùng lặp hoặc không có bằng chứng.
- Chi phí trung bình trên mỗi PR.
- Thời gian từ khi mở PR đến khi có kết quả review.

Không nên đánh giá chất lượng chỉ bằng số lượng comment Claude tạo ra.

## 8. Lộ trình triển khai tôi khuyên bạn

Giai đoạn 1 — Chuẩn hóa tài liệu

Tạo `CLAUDE.md`, `REVIEW.md`, quy tắc nghiệp vụ, state machine và checklist review. Chạy thử Claude thủ công trên 3–5 PR cũ để xem nó có phát hiện được lỗi thật không.

Giai đoạn 2 — Tự động hóa GitHub

Cấu hình Claude Code Action, tự lấy diff và comment trên PR. Ban đầu chỉ dùng chế độ advisory, chưa chặn merge.

Giai đoạn 3 — Kết nối Jira

Bắt buộc PR có ticket key, lấy acceptance criteria, kiểm tra mức độ đầy đủ của tài liệu và đối chiếu từng yêu cầu với code.

Giai đoạn 4 — Merge gate và xác minh

Bổ sung CI checks, xác minh findings, cấu hình branch protection và quy tắc bắt buộc người review đối với các thay đổi nhạy cảm.

## Tôi cần biết một điều để chọn đúng cách cài đặt cho bạn

Bạn đang sử dụng loại Claude nào? Câu trả lời này quyết định liệu có thể dùng ngay GitHub App chính thức hay nên xây workflow riêng dùng API key.

1\. Bạn có tài khoản Claude nào?

Claude Pro (thuê bao cá nhân)

Claude Max

Claude Team / Enterprise

Anthropic API key / Console

Chưa rõ, tôi chỉ đang dùng Claude để code

2\. Repository đang ở đâu?

GitHub.com

GitHub Enterprise Server tự host

GitLab / nền tảng khác

3\. Jira của bạn là loại nào?

Jira Cloud

Jira Data Center / Server

Chưa dùng Jira hoặc chưa rõ

Thiết kế hướng dẫn triển khai cho tôi