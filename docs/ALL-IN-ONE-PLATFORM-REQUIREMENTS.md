# ZetuBIZ — All-in-One Business Platform Requirements

**Document type:** Product-scope expansion and requirements baseline  
**Product:** ZetuBIZ  
**Owner:** CodeFamily  
**Market entry:** Kenya; designed for configurable expansion across Africa and international markets  
**Status:** Proposed requirements for review — not implemented  
**Version:** 1.0  
**Last updated:** 20 October 2026

> This document expands the existing master specification. It is a requirements document, not evidence that any feature exists in code. Product research may inform the team’s understanding of customer expectations, but all ZetuBIZ requirements, designs, copy, and implementation decisions must be independently documented and owned by the ZetuBIZ team.

## 1. Purpose and product direction

ZetuBIZ is intended to become a modular business operating platform where a business can manage customers, sales, documents, money, stock, people, projects, communication, public web content, and reporting from one secure workspace.

The platform must not be a collection of disconnected mini-apps. Modules should share customer, supplier, employee, product, document, payment, permission, and audit concepts where appropriate. A single business event should update the relevant records through explicit, testable workflows rather than duplicated manual entry.

### 1.1 Product principles

1. **One workspace, connected modules.** A customer, quotation, invoice, payment, project, and report should use consistent identifiers and history.
2. **Kenya-first, not Kenya-only.** Default to KES, Africa/Nairobi, Kenyan phone formatting, and relevant local workflows, while allowing country, currency, timezone, language, and tax configuration.
3. **Mobile-first and low-friction.** Core daily tasks must work on a phone and tolerate slow or intermittent connections gracefully.
4. **Secure by default.** Tenant isolation, least privilege, auditability, safe document sharing, and secrets management are foundational requirements.
5. **Financial integrity.** Use decimal-safe calculations, explicit rounding rules, immutable finalized-document snapshots, verified payment events, and traceable corrections.
6. **Configurable, not overwhelming.** Businesses choose their industry, team size, and enabled workflows; hide irrelevant navigation without deleting or weakening permissions.
7. **Real functionality only.** No mock financial records or buttons that pretend to work in production.
8. **AI with approval.** AI can explain, summarize, and prepare actions; consequential actions require explicit user confirmation and must be logged.
9. **Accessible and usable.** Keyboard support, clear focus states, readable contrast, helpful errors, and semantic controls are required.
10. **Incremental delivery.** The full vision is documented, but implementation is sequenced by dependencies, risk, and validated customer demand.

## 2. Target customer segments

Initial customer discovery should focus on a small number of segments instead of claiming equal fit for every industry.

- Freelancers and consultants: customers, quotations, invoices, expenses, projects, billable time.
- Agencies and service firms: leads, proposals, contracts, projects, tasks, team permissions, recurring invoices.
- Retail and wholesale SMEs: products, inventory, suppliers, purchases, sales, customer credit, branches.
- Small employers: employee records, leave, attendance, payroll preparation, documents.
- Growing multi-branch businesses: role controls, consolidated reports, location-level operations.

**Validation requirement:** Interview at least 15–20 business owners across candidate segments. Record the current process, most costly recurring problem, existing tools, willingness to pay, and must-have workflow. Select the first launch segment based on evidence.

## 3. Roles and access model

A user may belong to multiple business workspaces. A person’s role in one business must not grant access to another business.

### 3.1 Workspace roles

- **Owner:** Full workspace authority, billing, ownership transfer, exports, deletion requests, and member management.
- **Administrator:** Workspace settings and delegated management, excluding restricted owner-only actions.
- **Finance:** Approved financial records, invoices, payments, expenses, and finance reports.
- **Sales:** Leads, customers, quotations, sales activity, and permitted invoices.
- **Operations/Manager:** Assigned projects, tasks, inventory or operational functions granted by permissions.
- **HR:** Employee records and workforce functions; salary data is separately restricted.
- **Staff:** Only assigned work and explicitly granted modules.
- **Read-only/Auditor:** Read access to explicitly permitted data, with exports restricted by policy.
- **External customer:** Only their own shared documents or portal records.
- **Platform support administrator:** Limited operational access, no routine access to tenant business content by default; privileged access must be justified and audited.

Roles are starting presets, not the only authorization mechanism. Use explicit permission checks at API and database layers. Sensitive permissions such as payroll viewing, exports, provider credential management, refunds, and role assignment must be separately configurable.

### 3.2 Authorization requirements

- Deny by default.
- Validate membership and permission on every server-side request.
- Enforce tenant scope in database policies and queries.
- Never trust a client-supplied business ID without validating membership.
- Protect against insecure direct object references.
- Test both allowed and denied cases for every sensitive endpoint.
- Record actor, tenant, action, target, timestamp, and safe metadata in audit logs.
- Avoid logging passwords, session tokens, payment secrets, or unnecessary personal data.

## 4. Full module catalogue

Each module below is part of the long-term product scope. The labels indicate proposed sequencing, not current implementation.

### 4.1 Platform foundation — Release 0 / mandatory

- Registration, email verification, sign-in, sign-out, password recovery, session revocation, and optional two-factor authentication.
- Workspace creation, invitations, membership, role management, workspace switching, and onboarding progress.
- Business profile, legal name, registration/tax identifiers, address, contacts, logo, brand colors, language, currency, timezone, fiscal year, numbering preferences, and notification settings.
- Module activation and configuration by business type and subscription entitlements.
- Search, filters, pagination, saved views where useful, import/export permissions, global command/search interface, and consistent activity timelines.
- In-app notifications, notification preferences, support access, help content, status/error handling, and account settings.
- Platform-level tenant lifecycle, plans, subscription status, feature flags, usage limits, operational events, support cases, and audited administrative actions.

### 4.2 CRM and customer relationships — Release 1

- Leads, prospects, customers, organizations, contacts, contact roles, communication preferences, and duplicate detection.
- Lead source, pipeline stages, owner, value, expected close date, next action, follow-up reminders, and loss reasons.
- Notes, tasks, files, interaction timeline, calls/messages metadata, consent preferences, and opt-out handling.
- Customer statements, open quotations, invoices, receipts, payments, credit balance, and overdue amounts.
- Convert qualified leads into customers without losing history.
- Import/export with column mapping, validation preview, duplicate policy, and import results.
- Customer portal access must be separately invited and limited to that customer’s records.

### 4.3 Quotations, proposals and contracts — Release 1

- Draft, edit, duplicate, preview, send, expire, accept, decline, revise, cancel, and archive quotations.
- Line items, quantity, unit, rate, discount, tax treatment, notes, terms, validity, delivery assumptions, currency, and optional milestones.
- Multiple branded templates with typography, logo, layout, payment instructions, and live preview.
- Version history and immutable sent/finalized snapshots.
- Secure public links using high-entropy, revocable tokens; optional expiry and access logging.
- Customer acceptance/decline with timestamp and appropriate evidence.
- Convert accepted quotations to invoices or projects without retyping customer and line-item data.
- Contract/document records with owner, dates, status, attachment, renewal reminder, and access restrictions. Electronic signatures require a separately evaluated provider and legal review.

### 4.4 Invoicing, receipts and customer credit — Release 1

- Draft, finalize, send, view, partially paid, paid, overdue, voided, and credited states with controlled transitions.
- Invoice numbering scoped by workspace and configurable financial period; prevent collisions under concurrent requests.
- Line items, discounts, tax treatment, currency, due date, payment instructions, terms, and customer references.
- Snapshot finalized documents so later changes to a customer or product do not rewrite historical invoices.
- Partial payments, overpayments, unapplied credits, allocations, refunds, credit notes, and payment reversals with explicit audit history.
- Receipt generation only when supported by a recorded and verified payment event or authorized manual receipt workflow.
- Recurring invoice schedules with preview, failure handling, pause/resume, and duplicate prevention.
- Customer statements and aging buckets.
- PDF rendering, email delivery, printable views, and revocable secure links.
- Customer credit/debt book with credit limits, due dates, payment history, reminders, and permission-controlled adjustments.

### 4.5 Payments and reconciliation — Release 1, integrations staged

- Provider-neutral payment model supporting M-Pesa Daraja, PayHero, Paystack, and future providers only after API and commercial verification.
- Per-workspace provider configuration, test/live mode distinction, connection status, credential rotation, and secure secret storage.
- Payment links and checkout where supported by provider capabilities.
- Verify provider webhook signatures or authenticity mechanisms; validate amount, currency, reference, and expected state.
- Idempotent event processing, retry policy, event history, duplicate callback protection, and manual review queue.
- Payment allocation to one or multiple invoices; support unapplied payments and reconciliation.
- Never mark an invoice paid because a browser redirect or screenshot says payment succeeded.
- Refunds and reversals must reflect provider capability, authorization, status, and audit history.
- CSV statement import with mapping preview, duplicate detection, suggested matches, manual matching, unmatched items, and reconciliation reports.
- Platform subscription payments must be separated from each tenant’s customer payments.
- Do not imply that provider credentials can be connected through an API unless the provider explicitly supports it.

### 4.6 Expenses and financial operations — Release 1

- Expense entry, category, date, amount, currency, tax fields, payment method, supplier, project/client association, receipt attachment, notes, and creator.
- Draft, submitted, approved, rejected, reimbursed, and voided states where applicable.
- Approval thresholds and separation of duties for larger expenses.
- Supplier bills, due dates, payment status, and supplier statements.
- Recurring expense reminders and budgets by category, project, or period.
- Income/expense summaries and cash-flow estimates clearly distinguished from formal accounting statements.
- Audit-safe corrections rather than silent deletion of finalized financial records.
- CSV/PDF exports with date range, filters, currency, timezone, and access controls.

### 4.7 Accounting and tax records — Later, specialist-reviewed

- Chart of accounts, journal entries, double-entry ledger, posting rules, trial balance, general ledger, profit and loss, balance sheet, and cash-flow statements.
- Accounts receivable/payable controls, period close, reversals, adjustments, and traceability from reports to source transactions.
- Tax configuration by country, tax code, inclusive/exclusive pricing, exemptions, rounding, and effective dates.
- Kenya-specific tax and eTIMS integration research and implementation only after current official requirements, provider access, and certification obligations are verified.
- Clear distinction between management reports and statutory/accountant-approved reports.
- No claim of tax or accounting compliance until reviewed and tested by qualified professionals.

### 4.8 Products, inventory, purchasing and POS — Release 2

- Product/service catalogue, SKU, barcode, category, unit of measure, variants, images, cost and selling price, tax treatment, and active/archive state.
- Stock locations, branches, opening balances, receipts, issues, transfers, adjustments, returns, stock counts, and movement ledger.
- Stock valuation policy documented and consistently applied.
- Low-stock thresholds, reorder suggestions, batch/serial/expiry support where required by validated segments.
- Suppliers, purchase orders, receiving, supplier bills, returns, and purchase approval.
- Sales orders, delivery status, sales returns, discounts, customer credit, and stock deduction rules.
- POS sales, cash drawer sessions, receipt printing, cashier roles, shift close, and reconciliation.
- Multi-branch stock transfers and branch-level permissions.
- Offline operation is a separate requirement: define permitted offline actions, local storage protections, conflict resolution, and sync recovery before claiming offline support.
- Restaurant-specific table, kitchen, order, recipe, and ingredient-cost management only if market research validates the need.

### 4.9 Projects, tasks and time tracking — Release 2

- Projects linked to customers, quotation, contract, and invoices.
- Project owner, team, status, priority, start/due dates, budget, milestones, dependencies, and files.
- Tasks with assignment, due date, status, comments, attachments, checklist, and notifications.
- Timesheets with start/end or duration, project/task, billable status, review, approval, and correction history.
- Convert approved billable time and expenses into draft invoice lines without duplication.
- Compare budget, estimated hours, actual hours, billed amount, and project margin.
- Recurring tasks and calendar views if supported by customer evidence.

### 4.10 HR and employee records — Release 3

- Employee profiles, employment status, department, job title, manager, start/end dates, emergency contact, and restricted personal records.
- Secure document uploads, expiry reminders, access logs, and retention rules.
- Leave types, entitlements, requests, approvals, cancellations, balances, holidays, and audit history.
- Attendance, shifts, timesheets, late/absence records, and corrections with an audit trail.
- Employee self-service for permitted profile fields, leave requests, payslips, and policy documents.
- Performance reviews and training records only after privacy, access, and workflow requirements are specified.
- HR access must not automatically grant payroll, medical, or all employee-document access.

### 4.11 Payroll — Release 3, compliance gate

- Employee compensation components, pay frequency, allowances, deductions, benefits, overtime rules, and effective-dated changes.
- Payroll run lifecycle: draft, calculation, validation, approval, locked/finalized, payslip delivery, and reversal/correction.
- Configurable statutory rules and effective dates, with test cases for edge cases and retroactive changes.
- Kenya PAYE, NSSF, SHIF and other applicable statutory treatment must be researched against current official guidance before implementation; never hardcode unverified rates as permanent truth.
- Separation of payroll preparer and approver where configured.
- Restricted salary visibility, encrypted transport/storage where appropriate, audit logs, and controlled exports.
- Bank/payment export where supported, but do not claim that salary has been paid until verified through the relevant payment workflow.
- Payroll reports, payslips, annual summaries, and filing support only after professional compliance review.

### 4.12 Communication and notification centre — staged

- Email for invitations, quotations, invoices, receipts, reminders, approvals, and system security events.
- SMS and WhatsApp through approved providers, with delivery status, retries, template/consent management, and opt-out controls.
- In-app notifications with read/unread state and user preferences.
- Notification templates with workspace branding, language, preview, and test send.
- Scheduled jobs for recurring invoices, due reminders, leave reminders, and report delivery.
- Delivery failure queues, idempotency, rate limits, monitoring, and safe retry behavior.
- Keep provider cost visibility and per-workspace usage limits.

### 4.13 Website Studio and public API — Later

- Per-workspace public website/content configuration with drafts, preview, publish/unpublish, version history, and rollback.
- Pages, blog posts, categories, media library, SEO metadata, social preview metadata, navigation, FAQs, testimonials, team profiles, portfolio, and careers content.
- Forms for contact, leads, newsletter subscriptions, and job applications, with spam controls and consent notices.
- Custom domain/subdomain setup, DNS/SSL status, publishing status, and domain ownership verification.
- REST API with scoped keys, permission scopes, rotation/revocation, rate limits, audit logs, and documentation.
- Signed webhooks with retries, replay protection, event history, and secret rotation.
- Public content must never expose private customer, employee, financial, or workspace data.
- Define a safe page/template model; do not execute arbitrary customer-supplied JavaScript on the platform origin.

### 4.14 AI assistant and automation — Later, safety-gated

- Answer questions from the current user’s authorized workspace data and cite relevant records/periods in the response.
- Summarize revenue, outstanding invoices, expenses, customer history, project status, and stock alerts with clear date ranges and calculation definitions.
- Draft quotations, invoices, emails, reports, job posts, and website content for human review.
- Prepare supported actions such as creating a draft invoice or adding a customer; show a plain-language preview before execution.
- Require explicit confirmation for sending messages, finalizing financial documents, changing roles, exporting sensitive records, initiating refunds, or other consequential actions.
- Enforce permissions again when an AI action is executed; model output never bypasses authorization.
- Log proposed action, confirmer, executed result, and failure safely.
- Treat retrieved documents, messages, web content, and user-supplied files as untrusted input; resist prompt injection and data exfiltration.
- Define AI provider, data retention, privacy, model limits, cost budgets, rate limits, fallback behavior, and user-visible disclosure.
- Voice features are optional and require separate evaluation for privacy, latency, accessibility, and cost.

### 4.15 Call centre — optional advanced module

- Provider integration for browser-based inbound/outbound calling, agent presence, queues, IVR, routing, call outcomes, notes, and missed-call follow-up.
- Link calls to customer records only with authorized access.
- Recording must be disabled unless legal basis, notice/consent, retention, access controls, and deletion rules have been reviewed.
- Track provider availability, call cost, failed calls, and queue metrics.
- Treat as a separately scoped module due to telephony complexity and recurring costs.

### 4.16 Analytics and reporting — staged

- Role-appropriate dashboards for sales, finance, operations, HR, stock, projects, and platform health.
- Consistent definitions for revenue, collected cash, outstanding receivables, expenses, gross margin, stock valuation, and payroll totals.
- Filters by period, branch, currency, owner, project, and category where meaningful.
- Reports must be derived from persisted source records, not UI counters or fabricated data.
- Export to CSV/PDF with access controls, timezone, currency, filter summary, and generated timestamp.
- Sensitive reports require explicit permissions; export activity must be logged.
- Large reports should be paginated or generated asynchronously with status and expiry.

### 4.17 Subscription and platform administration — platform-wide

- Plans, trials, renewals, grace periods, failed payment handling, cancellation, plan changes, invoices, and entitlements.
- Usage meters for seats, storage, messages, AI credits, public API, and other metered features where applicable.
- Clear tenant-level subscription status and feature access; never rely only on frontend hiding.
- Platform support tools for tenant status, incident tracking, safe diagnostics, and customer support.
- Elevated support access should be time-limited, reason-coded, permission-limited, and audited.
- Tenant export, retention, deletion, suspension, reactivation, and ownership transfer workflows.
- Platform audit log, job/webhook health, rate-limit events, error monitoring, incident response, and feature flags.
- Separate platform administration from business workspace administration.

## 5. Shared end-to-end workflows

These cross-module workflows are core requirements and must be represented in UX flows, APIs, tests, and documentation.

### 5.1 Lead-to-cash

Lead → qualified opportunity → customer → quotation → acceptance → invoice → payment initiation → verified provider event → payment allocation → receipt → customer statement/report.

Each transition must preserve the source record, avoid duplicate creation, validate permissions, and expose failures that require user action.

### 5.2 Purchase-to-stock-to-payment

Supplier → purchase order → approval → goods received → stock movement → supplier bill → payment record → reconciliation. Receiving, bill approval, and payment are distinct events.

### 5.3 Project-to-invoice

Customer → project → task/time/expense entries → review and approval → draft invoice lines → invoice review/finalization → payment allocation → project profitability report.

### 5.4 Employee-to-payroll

Employee setup → approved attendance/leave and compensation inputs → draft payroll calculation → validation → authorized approval → locked payroll → payslip → payment/export status → audit and reports.

### 5.5 Website lead capture

Published form → spam/rate-limit checks → consent capture where applicable → CRM lead → assignment/follow-up → communication history. Public forms must not expose internal records.

### 5.6 AI-assisted action

User request → identity and permission check → retrieve scoped records → prepare explanation/action preview → user confirms → revalidate permission and data → execute idempotently → log outcome → report result.

## 6. Cross-cutting non-functional requirements

### Security and privacy
- Threat model and data classification before implementation of sensitive modules.
- Row Level Security for tenant-owned data and server-side authorization for all privileged operations.
- MFA for privileged accounts; secure recovery and session revocation.
- Rate limiting and abuse prevention beyond IP-only controls where appropriate.
- Secure secret storage, key rotation, webhook verification, CSRF/XSS/SQL injection protections as relevant to the chosen architecture.
- File type/size validation, malware scanning strategy where feasible, signed or authorized download URLs, and private-by-default storage.
- Encryption in transit; evaluate encryption at rest and field-level protection for highly sensitive records.
- Privacy notices, purpose limitation, retention, export, correction, deletion, and processor/vendor documentation.
- Incident response, breach escalation, access review, and backup restoration drills.

### Reliability and performance
- Define performance budgets and test on low-end Android devices and constrained networks.
- Paginate large tables and avoid unbounded queries.
- Add indexes based on query patterns and inspect query plans.
- Use idempotency for payment callbacks, recurring jobs, imports, and other retryable operations.
- Monitor API failures, job failures, email/payment delivery, database health, and latency.
- Define service targets only after measurement and capacity planning.
- Graceful error states; never silently lose a user’s draft or financial entry.

### Accessibility and localization
- Target WCAG 2.2 AA as a design and test goal.
- Keyboard navigation, visible focus, semantic labels, contrast, accessible validation, and screen-reader-friendly status updates.
- English first with an architecture ready for Kiswahili and additional languages.
- Use locale-aware dates, phone numbers, number formatting, currency, and timezone; store timestamps consistently in UTC and display in the workspace timezone.
- Never assume all customers use KES or Africa/Nairobi once international settings are enabled.

### Data integrity
- Define ownership and deletion behavior for every relationship.
- Use foreign keys, unique constraints, check constraints, and transactions for critical multi-record operations.
- Avoid hard deletion of finalized financial records; use void/reversal/correction workflows.
- Use optimistic concurrency or conflict handling for collaborative editing where needed.
- Track schema versions and use reversible, reviewed migrations where feasible.
- Test backup restoration, tenant export, and deletion workflows.

## 7. Kenya-first requirements and compliance gates

- M-Pesa integration must use the selected provider’s current official API and webhook requirements.
- PayHero and Paystack integration capabilities, terms, callbacks, and credential models must be verified against current documentation before being promised.
- Tax/eTIMS, payroll deductions, data protection, employee records, and communications must be reviewed against current official Kenyan requirements before release.
- Do not store customer mobile money PINs, full payment-card data, or provider secrets in browser storage.
- Record the source, verification date, owner, and review date for any statutory rate or legal rule.
- Provide a way to update effective-dated rates/configuration without silently recalculating historical finalized documents.
- Obtain qualified legal/accounting review where the platform makes compliance-sensitive calculations or claims.

## 8. UX and design requirements

- Workspace switcher and clear business identity in the shell.
- Responsive desktop sidebar and compact mobile navigation with consistent information architecture.
- Role- and module-aware navigation; hide irrelevant modules while keeping authorization independent.
- Consistent page titles, breadcrumbs where useful, primary action placement, filters, table actions, bulk actions, pagination, and empty states.
- Forms must preserve draft input when validation fails and clearly distinguish save, finalize, send, publish, approve, and delete.
- Destructive and consequential actions require clear consequences and confirmation.
- Financial documents need preview before sending/finalization and stable PDF layout.
- Design system must document color tokens, typography, spacing, density, borders, status colors, components, interaction states, and accessibility.
- Avoid decorative dashboards that obscure the most useful actions; prioritize actual business tasks and understandable data.

## 9. Product analytics and success measures

Collect only data necessary for product improvement, with appropriate disclosure and privacy controls.

- Activation: proportion of workspaces completing setup and their first real business workflow.
- Time to value: time from workspace creation to first sent quotation/invoice or recorded transaction.
- Workflow completion and failure rates.
- Weekly active workspaces and adoption of enabled modules.
- Invoice collection outcomes and overdue follow-up usage, without claiming causality without evidence.
- Retention, support requests, error rates, and user-reported satisfaction.
- Unit economics: infrastructure, email/SMS/WhatsApp, payment, AI, storage, and support cost per active workspace.

Do not optimize for vanity metrics at the expense of data safety or customer outcomes.

## 10. Release sequencing and dependency rules

### Release 0 — Documentation and foundation
Complete product decisions, screen map, data dictionary, permissions matrix, architecture decisions, threat model, migration plan, acceptance criteria, and operational plan before feature implementation begins.

### Release 1 — Service-business core
Foundation, CRM, quotations, invoices, document templates, secure sharing, email, expenses, payment recording, one verified payment provider, basic reports, team roles, and admin/operations minimums.

### Release 2 — Operational business
Products, inventory, suppliers, purchase orders, sales workflows, branch support, POS scope based on validation, projects, tasks, and time tracking.

### Release 3 — People operations
Employee records, leave, attendance, payroll after compliance validation, payslips, and restricted HR permissions.

### Release 4 — Platform expansion
Website Studio, public API/webhooks, AI assistant, advanced automation, call centre, deeper accounting, and advanced analytics based on validated demand and operational readiness.

The sequence can be adjusted only through a documented decision that records customer evidence, dependencies, risk, and impact.

## 11. Acceptance criteria for any module

A module is not complete merely because a page exists. Its definition of done must include:

- Approved requirements and screen-level flows.
- Data model, migration, indexes, constraints, and ownership rules.
- Role/permission matrix and tenant-isolation tests.
- API contract, validation, error behavior, idempotency and rate-limit requirements where applicable.
- Responsive, accessible UI states: loading, empty, success, validation error, permission denied, network/server error, and recovery.
- Automated unit, integration, and end-to-end tests proportionate to risk.
- Audit history for sensitive or financial actions.
- Logging and monitoring without secrets or unnecessary personal data.
- Documentation, support notes, and migration/rollback plan.
- Review by the module owner and QA; no unresolved critical/high-severity security issue.
- Status updated honestly as proposed, in progress, implemented, tested, or released.

## 12. Open decisions to resolve before coding

1. Which first customer segment will be served based on interviews?
2. Is the initial release strictly service-business-first, or must stock/POS be included from day one?
3. Which payment provider will be implemented first after API and account-access verification?
4. What is the final subscription model: free tier, paid tiers, usage limits, setup services, and trial policy?
5. Which design direction and brand tokens are approved?
6. What is the chosen PDF generation strategy compatible with the deployment runtime?
7. Which email/WhatsApp/SMS providers and sender verification requirements are available?
8. What are the support, backup, recovery, retention, and incident response targets?
9. Which payroll/tax/eTIMS scope is legally and technically supportable for the first release?
10. What data export/deletion guarantees and customer support access policy will be offered?
11. Which modules are optional and which are included in each subscription tier?
12. What team members own each domain and who approves architecture/security decisions?

Record each decision with owner, date, rationale, alternatives, and review trigger. Do not silently treat an unanswered question as a confirmed decision.

## 13. Relationship to existing documentation

This file expands, but does not replace, the existing master specification. Keep the following documents aligned:

- `docs/ZetuBIZ-MASTER-SPEC.md` — product-level single source of truth and navigation to supporting specifications.
- `docs/ARCHITECTURE.md` — architecture, boundaries, runtime, integrations, and deployment decisions.
- `docs/DATABASE.md` — data dictionary, relationships, constraints, indexes, migrations, RLS, and retention.
- `docs/IMPLEMENTATION-STATUS.md` — evidence-based status of each requirement.
- `TASKS.md` — sequenced implementation backlog after documentation review.
- `docs/TESTING.md` — risk-based test strategy and acceptance gates.
- `docs/DEPLOYMENT.md` and `docs/ENVIRONMENT.md` — deployment and secret configuration.
- `AGENTS.md` — instructions for coding agents, including the requirement to read approved specifications first.

**Current truth:** All requirements in this document are proposed/planned until implemented and verified. Do not claim feature availability based on this document alone.
