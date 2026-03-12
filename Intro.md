Chuẩn mực hiện tại (2025-2026) cho tài liệu dự án dùng để "feed" cho AI agents.

---

## Tài liệu chuẩn cho một dự án AI Agent

### Bức tranh tổng quan

Hiện nay, industry đã hình thành một **bộ tài liệu gồm nhiều lớp** — từ tài liệu sản phẩm (dành cho con người) đến tài liệu hướng dẫn agent (dành cho AI đọc trực tiếp). Các công ty Đức (SAP, Siemens, Bosch, Deutsche Telekom...) có xu hướng kết hợp chuẩn quốc tế với văn hóa tài liệu hóa rất kỹ lưỡng (_Gründlichkeit_ — tính cẩn thận, triệt để).

---

### Lớp 1 — Tài liệu sản phẩm & yêu cầu (cho con người)

**PRD — Product Requirements Document**

Đây là tài liệu nền tảng. Các developer có kinh nghiệm đối xử với PRD cho AI agent như một PRD truyền thống hoặc System Design doc — toàn diện, có cấu trúc rõ ràng, dễ để một AI "literal-minded" có thể đọc và hiểu.

Một PRD chuẩn thường bao gồm:

- **Problem Statement** — vấn đề cần giải quyết, bối cảnh kinh doanh
- **Solution Alignment** — giải pháp được chọn và lý do
- **User Stories + Acceptance Criteria** — điều kiện hoàn thành rõ ràng
- **Scope & Out-of-scope** — ranh giới rõ ràng
- **Dependencies & Risks** — phụ thuộc và rủi ro
- **Success Metrics** — KPI có thể đo được

Viết PRD theo kiểu PRD giúp đảm bảo có context "why behind each feature" để AI không tối ưu hóa sai mục tiêu. Mở rộng thêm theo kiểu SRS (System Requirements Specification) giúp xác định rõ các chi tiết kỹ thuật mà AI cần để tạo ra code đúng.

---

### Lớp 2 — Tài liệu hướng dẫn agent đọc trực tiếp

Đây là lớp **quan trọng nhất** và mang tính đặc thù của thời đại AI agent.

#### `AGENTS.md` / `CLAUDE.md` / `CURSOR_RULES`

`AGENTS.md` là một format đơn giản và mở để hướng dẫn coding agents — như một README dành riêng cho AI, cung cấp context và hướng dẫn để AI coding agents làm việc hiệu quả trong project của bạn.

File này chứa hai loại hướng dẫn: Personal scope (style commit, coding pattern cá nhân) và Project scope (project làm gì, package manager nào, các quyết định kiến trúc).

**Cấu trúc chuẩn của `AGENTS.md`:**

```markdown
# Project Overview

[Một câu mô tả dự án — cực kỳ quan trọng, anchors mọi quyết định]

# Build & Test

- npm run build
- npm test
- pytest -v tests/

# Architecture Overview

[Mô tả các module chính — không liệt kê file path cụ thể vì chúng thay đổi]

# Conventions & Patterns

[Naming, folder layout, code style]

# Security

[Auth flows, API keys, sensitive data]

# Git Workflows

[Branching, commit conventions, PR requirements]
```

Giới hạn khoảng 150 dòng — file quá dài sẽ làm chậm agent và chôn vùi thông tin quan trọng.

Lưu ý: Claude Code không dùng `AGENTS.md` — nó dùng `CLAUDE.md`. Bạn có thể dùng symlink để giữ cả hai tool hoạt động đồng thời.

---

#### `CLAUDE.md` (dành cho Claude Code)

File `CLAUDE.md` cung cấp project context được load vào mỗi session — coding conventions, cấu trúc thư mục, các lệnh thường dùng, và workflow preferences. Best practice là giữ file này dưới 500 dòng để đảm bảo context efficiency.

---

### Lớp 3 — Context Engineering (quan trọng không kém)

Context engineering là hành động có chủ đích trong việc lựa chọn và trình bày thông tin nền, dữ liệu, và các tham số để định hướng lý luận của AI. Phần lớn các thất bại của AI agent không đến từ model kém năng lực, mà đến từ "context failures" — AI không được cung cấp đúng thông tin hoặc công cụ để thành công.

Điều này nghĩa là ngoài `AGENTS.md`, bạn cần chuẩn bị:

- **`tech_spec.md`** — đặc tả kỹ thuật chi tiết
- **`plan.md`** — kế hoạch triển khai theo phase
- **`prd.md`** — tóm tắt yêu cầu sản phẩm
- **`llms.txt`** — tóm tắt documentation cho LLM tiêu thụ (chuẩn đang nổi lên)
- **OpenAPI schemas** — cho bất kỳ API nào agent sẽ sử dụng

---

### Lớp 4 — Governance & Guardrails (đặc biệt quan trọng với công ty Đức)

Công ty Đức nổi tiếng với văn hóa tuân thủ (compliance) và kiểm soát rủi ro. Trong một spec AI agent chuẩn, cần có phần định nghĩa các rules, constraints, và guardrails điều chỉnh hành vi agent — đảm bảo các hành động của agent không chỉ đúng về chức năng mà còn an toàn, đạo đức, và phù hợp với chuẩn của project.

Cụ thể cần bao gồm:

- **Human-in-the-loop gates** — điều kiện nào cần human approve trước khi agent tiếp tục
- **Pre-deployment checks** — agent không được tự deploy lên production
- **Failure protocols** — phải làm gì khi agent thất bại liên tục
- **GDPR/Data Privacy** — hướng dẫn xử lý PII (đặc biệt quan trọng tại Đức theo DSGVO)

---

### Tóm tắt — Stack tài liệu chuẩn cho một dự án

| Tài liệu                  | Audience       | Mục đích                      |
| ------------------------- | -------------- | ----------------------------- |
| `PRD.md`                  | Con người + AI | Yêu cầu sản phẩm, "why"       |
| `tech_spec.md`            | Con người + AI | Đặc tả kỹ thuật, "how"        |
| `AGENTS.md` / `CLAUDE.md` | AI agent       | Hướng dẫn làm việc trong repo |
| `plan.md`                 | Con người + AI | Kế hoạch theo phase           |
| `llms.txt`                | AI agent       | Tóm tắt doc cho LLM           |
| OpenAPI / schemas         | AI agent       | Định nghĩa API và data        |
| Guardrails spec           | Con người      | Governance & compliance       |

Hãy xem spec như một "living document" — đừng viết xong rồi quên. Cập nhật khi bạn và agent đưa ra quyết định mới hoặc khám phá thông tin mới. Hãy coi nó như version-controlled documentation.

---

## Bổ sung khuyến nghị để áp dụng ngay

### 1) Checklist MVP cho tuần đầu

Nếu bắt đầu một dự án mới, tối thiểu nên có đủ các file sau trước khi giao việc lớn cho agent:

- `PRD.md` (1-2 trang, có scope/out-of-scope rõ)
- `tech_spec.md` (kiến trúc + luồng dữ liệu + ràng buộc kỹ thuật)
- `AGENTS.md` hoặc `CLAUDE.md` (lệnh build/test/lint + conventions)
- `plan.md` (milestone theo tuần hoặc theo phase)
- `guardrails.md` (approve gate, quyền hạn agent, xử lý sự cố)

Khi thiếu 1 trong các file trên, chất lượng output của agent thường giảm rõ rệt.

### 2) Cadence cập nhật tài liệu

Để giữ tài liệu luôn hữu ích cho cả người và AI:

- Cập nhật **ngay sau mỗi quyết định kiến trúc quan trọng**
- Review tài liệu theo nhịp cố định (ví dụ: mỗi sprint)
- Khi PR thay đổi behavior lớn, bắt buộc cập nhật `AGENTS.md`/`CLAUDE.md`
- Nếu phát sinh incident, thêm mục “postmortem rules” vào guardrails

### 3) Ownership rõ ràng

Mỗi tài liệu nên có owner để tránh tình trạng “không ai giữ”:

- `PRD.md` — Product Owner / PM
- `tech_spec.md` — Tech Lead / Architect
- `AGENTS.md` / `CLAUDE.md` — Engineering team (owner chính: Tech Lead)
- `guardrails.md` — Engineering + Security/Compliance

### 4) Definition of Done cho tài liệu agent

Một tài liệu hướng dẫn agent chỉ nên coi là “done” khi:

- Một dev mới vào team có thể chạy được project từ tài liệu
- Agent có thể hoàn thành task cơ bản mà không cần hỏi lại nhiều vòng
- Lệnh build/test/lint trong tài liệu chạy đúng trên môi trường CI
- Các policy bảo mật và quyền hạn được mô tả đủ cụ thể để enforce

### 5) Anti-pattern nên tránh

- Viết tài liệu quá dài nhưng thiếu ví dụ cụ thể
- Chỉ mô tả "what", không mô tả "why" và "constraints"
- Để lệnh cũ/đã lỗi trong `AGENTS.md` hoặc `CLAUDE.md`
- Không phân biệt rõ việc nào agent được tự làm và việc nào cần approval

---

## Kết luận

Khung tài liệu tốt cho AI agent không cần phức tạp, nhưng phải **rõ ràng, cập nhật, và có thể vận hành được ngay**. Nếu cần ưu tiên theo mức tối thiểu: bắt đầu từ `PRD.md` + `tech_spec.md` + `AGENTS.md`/`CLAUDE.md` + `guardrails.md`, sau đó mở rộng dần theo độ trưởng thành của dự án.

Đây là cách bố trí chuẩn mà industry đang dùng:

```
my-project/
│
├── AGENTS.md              ← Root-level: rules cho toàn bộ project
├── CLAUDE.md              ← Symlink hoặc copy của AGENTS.md (cho Claude Code)
├── README.md              ← Cho con người
├── llms.txt               ← Tóm tắt doc cho LLM (chuẩn mới nổi)
│
├── docs/
│   ├── prd.md             ← Product Requirements
│   ├── tech_spec.md       ← Technical Specification
│   ├── plan.md            ← Implementation plan theo phase
│   ├── architecture.md    ← Sơ đồ kiến trúc, ADR (Architecture Decision Records)
│   └── guardrails.md      ← Governance, compliance rules (quan trọng với Đức)
│
├── .cursor/
│   └── rules/             ← Rules cho Cursor IDE
│       ├── general.mdc
│       └── backend.mdc
│
├── src/
│   ├── AGENTS.md          ← Sub-level: rules riêng cho /src
│   │
│   ├── frontend/
│   │   └── AGENTS.md      ← Rules riêng: React conventions, styling...
│   │
│   ├── backend/
│   │   └── AGENTS.md      ← Rules riêng: API patterns, DB conventions...
│   │
│   └── infra/
│       └── AGENTS.md      ← Rules riêng: Terraform, K8s, security...
│
└── tests/
    └── AGENTS.md          ← Rules riêng: test framework, coverage expectations
```

---

### Nguyên tắc hoạt động — **Nearest file wins**

Agent tìm `AGENTS.md` theo thứ tự từ gần đến xa — file gần nhất với file đang được edit sẽ được ưu tiên. Nhiều file có thể cùng tồn tại, và file nào gần hơn thì thắng.

Ví dụ khi agent đang làm việc trong `src/frontend/Button.tsx`:

1. Đọc `src/frontend/AGENTS.md` → React conventions, Tailwind rules
2. Đọc `src/AGENTS.md` → general coding style
3. Đọc `AGENTS.md` (root) → project-wide rules

---

### Nội dung mỗi sub-level `AGENTS.md` nên khác nhau

**Root `AGENTS.md`** — big picture:

```markdown
# Project: Payment Processing System

This is a PCI-DSS compliant payment gateway for B2B clients.

## Package Manager: pnpm workspaces

## Build: pnpm run build

## Test: pnpm test (must pass before commit)

## Language: TypeScript strict mode throughout
```

**`src/backend/AGENTS.md`** — domain-specific:

```markdown
# Backend Rules

- All DB queries go through the Repository pattern, never direct ORM calls
- Never log PII — use maskPII() helper before any logging
- All endpoints require JWT validation via authMiddleware
- Error responses must follow RFC 7807 (Problem Details)
```

**`src/frontend/AGENTS.md`** — frontend-specific:

```markdown
# Frontend Rules

- Components in /components must be pure — no direct API calls
- Use React Query for all server state, Zustand for client state
- Accessibility: all interactive elements need aria-labels
```

---

### Lưu ý quan trọng

**`CLAUDE.md` vs `AGENTS.md`**: Claude Code dùng `CLAUDE.md`, không dùng `AGENTS.md`. Cách đơn giản nhất là tạo symlink: `ln -s AGENTS.md CLAUDE.md` để cả hai tool cùng đọc một file.

**Tránh document file paths cụ thể** trong các file này — file paths thay đổi liên tục, và stale paths sẽ "đầu độc" context của agent, khiến nó tự tin tìm đến chỗ sai. Thay vào đó, mô tả **capabilities** và **domain concepts**.

**`docs/`** là dành cho con người đọc — PRD, tech spec, plan. Agent có thể được chỉ dẫn để đọc các file này khi cần thiết, nhưng không tự động load như `AGENTS.md`.
