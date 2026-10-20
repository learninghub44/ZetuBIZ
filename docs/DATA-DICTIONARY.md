# ZetuBIZ Data Dictionary and Relationship Baseline

**Status:** Conceptual draft — requires database/security review before migrations.  
**Purpose:** Define the shared business concepts and relationships so modules do not create duplicate sources of truth.

## 1. Core conventions

- Primary identifiers use UUIDs unless an approved integration contract requires another identifier.
- Store timestamps as UTC-aware timestamps; display them in the workspace timezone.
- Use exact decimal/numeric types or integer minor units for money; never binary floating-point arithmetic.
- Store currency with every monetary transaction and document. Do not assume all records always use KES.
- Distinguish immutable business/document identifiers from editable display names and references.
- Tenant-owned tables must carry `business_id` or have a rigorously enforced tenant-scoped parent relationship with matching constraints and RLS.
- Use foreign keys, uniqueness constraints, check constraints, and transactions for invariants.
- Finalized financial records must preserve snapshots and correction history.
- Supabase Auth is the identity source. Application profiles reference `auth.users(id)`; do not store passwords or session bearer tokens in app tables.
- This is a conceptual dictionary, not an approved SQL migration. See `DATABASE.md` for the proposed schema examples and known review requirements.

## 2. Identity and tenancy

| Entity | Purpose | Key fields | Relationships and rules |
|---|---|---|---|
| User profile | App-specific preferences for an authenticated identity | auth user ID, display name, avatar path, language, timestamps | One-to-one with `auth.users`; profile deletion must not accidentally delete unrelated business records |
| Business/workspace | Tenant boundary and configuration owner | ID, public/legal name, category, country, currency, timezone, owner ID, status | Has members, settings, branding, customers, documents, finance records, and enabled modules |
| Business membership | User membership and role in a workspace | business ID, user ID, role, status, invited/accepted timestamps | Unique business/user pair; revoked membership must immediately affect authorization |
| Invitation | Pending access request | business ID, email, invited role, token hash, expiry, invited by, accepted/revoked timestamps | Token is high-entropy and stored safely; invitation cannot grant access after expiry/revocation |
| Business settings | Workspace-specific configuration | business ID, typed settings, updated by, timestamps | Prefer validated structured fields for critical settings over arbitrary unvalidated JSON |
| Branding/template preferences | Document visual defaults | business ID, logo asset, colors, font/theme, default template | Does not alter snapshots of already-finalized documents |
| Audit event | Security and business action history | business ID, actor, action, target type/ID, timestamp, outcome, safe metadata | Append-only by application policy; exclude secrets and unnecessary personal data |

## 3. Customer and sales

| Entity | Purpose | Key fields | Relationships and rules |
|---|---|---|---|
| Lead/opportunity | Potential sale before qualification | business ID, title, source, stage, owner, estimated value, next action, close date | Conversion to customer preserves history; value carries currency |
| Customer | Individual or organization buying goods/services | business ID, type, display/legal name, email, phone, billing details, tax ID, status | Has contacts, quotations, invoices, payments, notes, files, activities |
| Customer contact | Person associated with a customer organization | customer ID, name, email, phone, role, primary flag | Must be scoped through the same tenant as the customer |
| Activity/task | Follow-up, note, call/message record, or reminder | business ID, related entity, owner, due date, status, type | Access follows related record and assigned scope |
| Quotation | Offer to a customer | business ID, customer ID, number, status, currency, dates, totals, terms, version | Number unique per workspace/period policy; finalization captures a snapshot |
| Quotation line | Item/service and pricing on a quotation | quotation ID, description, quantity, unit, unit price, discount, tax, amount, sort order | Totals calculated by server-side domain rules |
| Quotation revision/acceptance | Immutable revision and customer decision evidence | quotation ID, revision, snapshot, acceptance status, timestamp, evidence | Evidence collection must minimize personal data and be reviewed for legal appropriateness |
| Invoice | Request for payment | business ID, customer ID, number, status, currency, dates, totals, balance, snapshot reference | Finalized invoice cannot be silently edited or deleted; corrections use credit/void workflows |
| Invoice line | Item/service billed | invoice ID, description, quantity, unit, price, tax, discount, amount | Must preserve the finalized snapshot, including relevant customer/business details |
| Credit note | Traceable correction reducing invoice amount | business ID, invoice ID, number, amount, reason, status, issued date | Cannot exceed remaining allowable amount; does not delete original invoice history |

## 4. Money and reconciliation

| Entity | Purpose | Key fields | Relationships and rules |
|---|---|---|---|
| Payment | Money movement/receipt event | business ID, provider, provider reference, amount, currency, status, method, timestamps, idempotency key | Provider event IDs unique in appropriate provider/workspace scope; payment may be allocated across multiple invoices |
| Payment allocation | Amount of a payment applied to an invoice | payment ID, invoice ID, amount, created by, timestamp | Sum of allocations cannot exceed permitted payment amount; corrections are traceable |
| Payment provider connection | Workspace provider configuration | business ID, provider, mode, status, secret reference, metadata | Secrets are not readable from client-side app; rotate/revoke safely |
| Webhook event | Provider callback receipt and processing state | provider, event ID, received time, signature result, processing status, attempt count | Idempotent processing; store sanitized payload or safe normalized fields per retention policy |
| Expense | Business cost record | business ID, category, amount, currency, date, supplier, status, attachment | Approval/reimbursement states are explicit; finalized entries corrected via traceable workflow |
| Supplier | Vendor relationship | business ID, name, contacts, tax details, status | Can be related to bills, purchase orders, expenses, payments |
| Supplier bill | Amount owed to supplier | business ID, supplier ID, number, due date, total, balance, status | Related payments and credits must be traceable |
| Statement import | Imported bank/provider statement | business ID, source, period, file, status, creator | Import preview, duplicate detection, and safe reprocessing |
| Statement transaction | Normalized transaction row | import ID, external reference, date, amount, currency, direction, description | Preserve original reference and import provenance |
| Reconciliation match | Match between statement and platform records | statement transaction ID, target type/ID, matched amount, status, actor | Support partial, unmatched, duplicate, and manually reviewed outcomes |

## 5. Operations and workforce (later modules)

| Entity | Purpose | Key fields | Relationships and rules |
|---|---|---|---|
| Product/service | Sellable item catalogue | business ID, SKU, name, unit, price, cost, tax code, status | Price changes must not rewrite old quotations/invoices |
| Stock location | Branch/store/warehouse | business ID, name, address, status | Membership permissions may be limited by location |
| Stock movement | Immutable stock quantity change | business ID, product ID, location, quantity delta, reason, source, actor, timestamp | Stock balance derives from movements or is reconciled against a controlled balance |
| Purchase order/receipt | Purchase intent and goods received | business ID, supplier ID, number, status, expected/received dates | Purchase approval and goods receipt are distinct states |
| Project/task/time entry | Work delivery and billable effort | business ID, customer/project IDs, assignee, status, duration, billable flag | Approved billable items can be invoiced once; prevent duplicate billing |
| Employee | Workforce profile | business ID, user ID if applicable, employee number, department, manager, status | Personal and salary data need narrower permissions than general team records |
| Leave request/attendance | Time away and attendance evidence | business ID, employee ID, dates, type, status, approver | Approved changes must be auditable; rules vary by policy and jurisdiction |
| Payroll run/line | Pay calculation and employee result | business ID, period, status, employee, earnings, deductions, net amount | Restricted access, effective-dated rules, approval, locked runs, corrections and audit |

## 6. Shared entities and relationships

Conceptual relationship outline:

- Auth identity 1 → 0..1 app profile.
- Business 1 → many memberships, invitations, settings, customers, leads, quotations, invoices, payments, expenses, audit events, and module-specific records.
- Customer 1 → many contacts, activities, quotations, invoices, and projects.
- Quotation 1 → many lines and revisions; accepted quotation may create an invoice or project through an explicit conversion workflow.
- Invoice 1 → many lines, snapshots, credit notes, and payment allocations.
- Payment 1 → many allocations; allocation connects a payment to an invoice. A payment is not required to belong to exactly one invoice.
- Supplier 1 → many purchase orders, supplier bills, and supplier-related expenses.
- Product 1 → many stock movements across locations.
- Project 1 → many tasks, milestones, time entries, and approved billable expenses.
- Employee 1 → many leave requests, attendance entries, and payroll lines.
- All tenant-owned relationships must prevent cross-business references.

## 7. Required integrity rules

1. Composite tenant integrity: where practical, use composite unique keys and foreign keys (for example, `(business_id, id)`) so a record in one workspace cannot reference a parent from another workspace.
2. RLS: enable on tenant-owned tables; test reads and writes for users with no membership, revoked membership, and membership in another tenant.
3. Status transitions: define allowed transitions in domain rules; do not allow arbitrary client-supplied status updates.
4. Financial calculations: document tax order, discount order, precision, rounding point, and currency precision.
5. Idempotency: protect payment callbacks, payment initiation, recurring invoice generation, imports, and retryable background jobs.
6. Deletion: use restrictive deletion for finalized financial records; prefer archive, void, reversal, and correction records.
7. Numbering: enforce uniqueness under concurrency; define periods, reset rules, and gaps policy.
8. Audit: record actor, business, target, action, result, timestamp, and safe metadata for sensitive operations.
9. Retention: define lifecycle for public-link tokens, webhook payloads, imports, files, logs, and personal records.
10. Imports/exports: enforce tenant permissions, preview changes, and report partial failures.
11. Attachments: record ownership, visibility, MIME type, size, checksum where useful, and retention state; use private storage by default.
12. Reporting: specify source records, time range, currency, timezone, and formula for each financial metric.

## 8. Review checklist before migrations

- [ ] Every entity has an owner and purpose.
- [ ] All tenant-scoped foreign keys prevent cross-tenant references.
- [ ] Supabase Auth references are correct; no duplicate password/session system.
- [ ] Payment allocations support partial and multi-invoice allocation.
- [ ] Credit notes, refunds, voids, and reversals preserve original records.
- [ ] Numbering and idempotency constraints handle concurrent requests.
- [ ] All RLS policies are defined and tested, not merely described.
- [ ] Indexes are based on expected query patterns and validated with query plans.
- [ ] Data retention, deletion, export, and backup are specified.
- [ ] Schema examples are converted into reviewed, ordered, versioned migrations.
