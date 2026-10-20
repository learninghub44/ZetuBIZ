# ZetuBIZ Requirement-to-Test Traceability

**Status:** Initial test-planning draft; test IDs and evidence must be updated during delivery.  
**Rule:** A requirement is not verified by a screenshot or an implemented UI alone. Tests must cover the real domain/API/database behavior.

## 1. Traceability format

Each requirement should receive a stable ID in the format `AREA-NNN`, for example `AUTH-001`, `TENANT-001`, `QUOTE-001`, `INV-001`, or `PAY-001`. Link each requirement to:
- UX screen or workflow.
- API/service contract.
- Database constraints and RLS policies.
- Unit/integration/end-to-end test IDs.
- Reviewer and evidence.
- Status: proposed, approved, implemented, verified, released.

## 2. Initial risk-based test matrix

| Requirement ID | Requirement | Test level | Required scenarios | Release gate |
|---|---|---|---|---|
| AUTH-001 | Sign-in and recovery use Supabase Auth | Integration/E2E | valid login, invalid login, verified/unverified email, expired reset, revoked session | Blocker |
| TENANT-001 | Workspace records are isolated | RLS/integration | tenant A reads/writes own records; tenant B guesses IDs; no membership; revoked membership | Blocker |
| ROLE-001 | Role permissions apply server-side | Integration/E2E | allowed and denied direct API calls for every sensitive role/action | Blocker |
| ONB-001 | Onboarding saves/resumes | E2E | back, skip optional, refresh, sign-out/sign-in, interrupted setup, invalid required fields | Release 1 |
| WS-001 | Workspace preparation is truthful and retryable | Integration/E2E | each step succeeds, each step fails, retry does not duplicate records, partial recovery | Release 1 |
| CRM-001 | Customer records preserve tenant scope | Unit/integration/E2E | create/edit/archive, duplicate detection, invalid fields, import preview, denied cross-tenant access | Release 1 |
| QUOTE-001 | Quotation totals are deterministic | Unit | quantity, discounts, tax modes, rounding, zero/negative/large values, currency precision | Blocker |
| QUOTE-002 | Sent quotation snapshot is stable | Integration | change customer/template after sending; original sent version remains unchanged | Release 1 |
| QUOTE-003 | Conversion does not duplicate line items | Integration/E2E | accepted quote to invoice/project, repeated conversion, canceled/expired quote | Release 1 |
| INV-001 | Invoice numbering is concurrency-safe | Integration | simultaneous finalization, uniqueness, failed transaction retry | Blocker |
| INV-002 | Finalized invoice is immutable | Integration | attempted direct edit/delete, void, credit note, correction history | Blocker |
| PAY-001 | Payment status is based on verified provider events | Integration | forged callback, invalid signature, wrong amount/currency/reference, delayed callback | Blocker |
| PAY-002 | Webhook processing is idempotent | Integration | duplicate event, retry after timeout, out-of-order events, concurrent delivery | Blocker |
| PAY-003 | Payment allocation is accurate | Unit/integration | partial payment, overpayment, one payment to multiple invoices, credit balance, reversal | Blocker |
| SHARE-001 | Public document link is scoped and revocable | Security/E2E | valid token, expired/revoked token, guessed token, wrong tenant, rate limits | Blocker |
| EMAIL-001 | Sending documents reports actual provider state | Integration/E2E | queued, delivered if available, rejected, timeout, retry, duplicate prevention | Release 1 |
| EXP-001 | Expense approval is permissioned | Integration/E2E | submit, approve, reject, unauthorized approval, edit after approval | Release 1 |
| REPORT-001 | Reports match source records | Integration | date boundaries, timezone, currency, voids/credits, permission-denied exports | Release 1 |
| FILE-001 | Uploads are validated and private | Security/integration | oversized file, invalid MIME, unauthorized download, expired signed URL | Blocker |
| AUDIT-001 | Sensitive actions create audit events | Integration | role changes, exports, financial finalization, provider changes, denied actions policy | Blocker |
| AI-001 | AI respects caller permissions | Security/integration | prompt injection, cross-tenant request, unauthorized action, user confirmation, failed execution | Later module gate |
| PAYROLL-001 | Payroll rules are effective-dated and restricted | Unit/integration | approved official rule set, boundary cases, retroactive correction, role access | Separate compliance gate |

## 3. Required test layers

- **Unit:** pure calculations, state transitions, validators, permission evaluation, numbering, idempotency logic.
- **Integration:** database constraints/RLS, transaction boundaries, storage, email/payment adapters, job retries, webhook verification.
- **End-to-end:** complete user workflows from UI through server and persisted data.
- **Security:** tenant escape attempts, privilege escalation, injection, token misuse, secret exposure, abuse/rate limiting.
- **Accessibility:** keyboard-only use, focus order, accessible names, contrast, errors, zoom/reflow, screen-reader announcements.
- **Performance/reliability:** realistic record volumes, mobile device/network constraints, timeout/retry behavior, concurrent updates.
- **Recovery:** migration rollback plan, backup restore, failed job recovery, export/deletion workflows.

## 4. Evidence and reporting

For each run, record commit/PR, environment, command or test suite, pass/fail result, failures, known skips, and reviewer. Do not claim a test passed if it was not run. Never use live payment credentials or real customer data in automated tests.

## 5. Release gate

Release 1 cannot be approved while critical/high-severity security issues remain, tenant-isolation tests fail, financial calculations are unverified, payment callbacks can be forged or double-processed, finalized documents can be silently altered, or recovery and monitoring are undocumented.
