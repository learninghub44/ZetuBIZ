# ZetuBIZ Consolidated Documentation Handbook

> **Purpose:** A single reading document that brings the project's existing documentation together in one place for product-owner review, team coordination, and future handover.
>
> **Documentation-first gate:** This handbook is a consolidation of the source documents, not approval to implement. Application implementation, production migrations, and deployment remain blocked until the product owner explicitly reviews and approves the documentation gate.
>
> **Source-of-truth rule:** The individual files linked in the repository remain the editable source documents for their respective subjects. This consolidated handbook is a reading and review copy. When content conflicts, record the conflict in `docs/DECISIONS.md`; do not silently choose a version. Rebuild this handbook after source documents change.
>
> **Included files:** `docs/ZetuBIZ-MASTER-SPEC.md`, `docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md`, `docs/ARCHITECTURE.md`, `docs/DATABASE.md`, `docs/DATA-DICTIONARY.md`, `docs/DECISIONS.md`, `docs/DEPLOYMENT.md`, `docs/DEVELOPMENT.md`, `docs/ENVIRONMENT.md`, `docs/HANDOVER.md`, `docs/IMPLEMENTATION-STATUS.md`, `docs/PERMISSIONS-MATRIX.md`, `docs/TASK-HANDOVER-TEMPLATE.md`, `docs/TEAM-ASSIGNMENT-MATRIX.md`, `docs/TEAM-WORKFLOW.md`, `docs/TEST-TRACEABILITY.md`, `docs/TESTING.md`, `docs/UX-SPECIFICATION.md`.

## Contents

1. [ZetuBIZ MASTER SPEC](#document-1)
2. [ALL IN ONE PLATFORM REQUIREMENTS](#document-2)
3. [ARCHITECTURE](#document-3)
4. [DATABASE](#document-4)
5. [DATA DICTIONARY](#document-5)
6. [DECISIONS](#document-6)
7. [DEPLOYMENT](#document-7)
8. [DEVELOPMENT](#document-8)
9. [ENVIRONMENT](#document-9)
10. [HANDOVER](#document-10)
11. [IMPLEMENTATION STATUS](#document-11)
12. [PERMISSIONS MATRIX](#document-12)
13. [TASK HANDOVER TEMPLATE](#document-13)
14. [TEAM ASSIGNMENT MATRIX](#document-14)
15. [TEAM WORKFLOW](#document-15)
16. [TEST TRACEABILITY](#document-16)
17. [TESTING](#document-17)
18. [UX SPECIFICATION](#document-18)

---



<a id="document-1"></a>

# Document 1: `docs/ZetuBIZ-MASTER-SPEC.md`

> Source path: [`docs/ZetuBIZ-MASTER-SPEC.md`](../ZetuBIZ-MASTER-SPEC.md)

---

# ZetuBIZ — Master Product Specification

**Document type:** Single source of truth for product design and development  
**Product:** ZetuBIZ  
**Product category:** Customizable business management, quotations, invoicing, payments and financial operations platform  
**Primary market:** Kenya, with architecture supporting international expansion  
**Primary users:** Developers, freelancers, consultants, agencies, SMEs and service-based businesses  
**Development approach:** Design-first, security-first, incremental implementation  
**Version:** 1.0  
**Last updated:** October 20, 2026  
**Status:** Initial specification

---

# Table of Contents

1. [Repository Audit](#1-repository-audit)
2. [Proposed Architecture](#2-proposed-architecture)
3. [Navigation Architecture](#3-navigation-architecture)
4. [Core Workflows](#4-core-workflows)
5. [Database Schema](#5-database-schema)
6. [Payment-Provider Integration Strategy](#6-payment-provider-integration-strategy)
7. [Security and Compliance Risks](#7-security-and-compliance-risks)
8. [UI Reference Resources](#8-ui-reference-resources)
9. [Implementation Milestones](#9-implementation-milestones)
10. [Supporting Specifications](#10-supporting-specifications)

---

## Expanded platform scope

The initial specification below remains part of the project history and core workflow definition. The full all-in-one platform scope, including additional CRM, inventory, purchasing, HR/payroll, communications, website studio, AI, call centre, accounting, subscription administration, cross-module workflows, non-functional requirements, release sequencing, and acceptance criteria, is maintained in **[ALL-IN-ONE-PLATFORM-REQUIREMENTS.md](ALL-IN-ONE-PLATFORM-REQUIREMENTS.md)**.

That requirements document is the companion scope reference for future revisions. Requirements remain planned until implementation and verification are recorded in [IMPLEMENTATION-STATUS.md](IMPLEMENTATION-STATUS.md).

---

# 1. Repository Audit

## 1.1 Current State

- **Repository:** `learninghub44/ZetuBIZ`
- **Current repository contents:** Product and technical documentation, proposed schema examples, task lists, and handover guidance.
- **Application code:** No verified application implementation was found during the repository audit.
- **Database migrations and tests:** Not present/verified at the audit point; inspect the current tree before making claims.

## 1.2 Finding

The GitHub repository exists, but the product is still at the documentation and planning stage. Specifications describe intended behavior and do not prove that application features exist. Implementation remains paused until the documentation approval gate in `docs/TEAM-WORKFLOW.md` is completed and approved.

---

# 2. Proposed Architecture

## 2.1 Technology Stack

**Frontend:**
- HTML5, CSS3, JavaScript (ES6+)
- TailwindCSS for styling
- Inter font family (Google Fonts)

**Backend:**
- Cloudflare Workers for serverless functions
- Cloudflare Pages for static hosting
- Cloudflare R2 for file storage

**Database & Authentication:**
- Supabase PostgreSQL
- Supabase Auth
- Supabase Row Level Security (RLS)

**Additional Services:**
- Resend/SendGrid for email
- PDF generation (Puppeteer)
- Payment APIs (PayHero, Paystack, M-Pesa Daraja)

## 2.2 Module Boundaries

1. Authentication Module
2. Business Module
3. Client Module
4. Project Module
5. Quotation Module
6. Invoice Module
7. Payment Module
8. Expense Module
9. Reconciliation Module
10. Report Module
11. Notification Module
12. Team Module
13. Platform-Admin Module

## 2.3 Deployment Environments

- Development: Local + Supabase local
- Staging: Cloudflare Workers + Supabase staging
- Production: Cloudflare Workers + Supabase production

## 2.4 Secret Handling

- Environment variables via Cloudflare Workers secrets
- Supabase service role keys in Cloudflare secrets
- Payment credentials encrypted in database
- No secrets in client-side code

---

# 3. Navigation Architecture

## 3.1 Business Sidebar

**Overview**
- Dashboard

**Work**
- Clients
- Projects
- Tasks and milestones

**Sales**
- Quotations
- Invoices
- Payment links
- Customers

**Finance**
- Transactions
- Expenses
- Reconciliation
- Reports

**Settings**
- Business profile
- Branding and documents
- Payment connections
- Team and permissions
- Notifications
- Security

## 3.2 Platform-Admin Navigation (Separate)

- Platform overview
- Tenants
- Plans and subscriptions
- Integrations
- Jobs and webhooks
- Support and incidents
- Security and audit
- Administrator access
- Platform configuration

---

# 4. Core Workflows

## 4.1 Quotation-to-Payment Flow

1. Create quotation with line items
2. Send quotation to client
3. Client accepts quotation
4. Convert accepted quotation to invoice
5. Finalize invoice
6. Send invoice to client
7. Initiate payment via provider
8. Client pays
9. Webhook verifies payment
10. Payment allocated to invoice
11. Receipt generated

## 4.2 Client Onboarding Flow

1. User registers
2. Email verification
3. Business onboarding wizard (8 steps)
4. Workspace preparation
5. Personalized dashboard

## 4.3 Payment Reconciliation Flow

1. Import bank statement CSV
2. Map columns
3. Auto-match transactions
4. Manual resolution of ambiguities
5. Generate reconciliation report

---

# 5. Database Schema

## 5.1 Core Entities

**Users & Authentication**
- users (id, email, password_hash, created_at)
- user_identities (provider, provider_id)
- sessions (id, user_id, token, expires_at)

**Business & Tenancy**
- businesses (id, name, legal_name, category, country, currency, owner_id)
- business_memberships (id, business_id, user_id, role)
- business_settings (id, business_id, settings_json)
- branding (id, business_id, logo_url, colors, fonts)

**Clients**
- clients (id, business_id, name, type, email, phone, address, tax_id)
- client_contacts (id, client_id, name, email, phone)

**Projects**
- projects (id, business_id, client_id, name, description, status, start_date, end_date)
- milestones (id, project_id, name, description, status, due_date, completed_at)
- project_assignments (id, project_id, user_id, role)

**Quotations**
- quotations (id, business_id, client_id, project_id, number, status, issue_date, validity_date, subtotal, tax, total, currency)
- quotation_line_items (id, quotation_id, description, quantity, rate, discount, tax, amount)
- quotation_revisions (id, quotation_id, revision_number, snapshot_json)
- quotation_acceptances (id, quotation_id, accepted_at, accepted_by, evidence)

**Invoices**
- invoices (id, business_id, client_id, project_id, number, status, issue_date, due_date, subtotal, discount, tax, total, currency, finalized_at)
- invoice_line_items (id, invoice_id, description, quantity, rate, discount, tax, amount)
- invoice_snapshots (id, invoice_id, snapshot_json, created_at)
- credit_notes (id, invoice_id, number, amount, reason, created_at)

**Payments**
- payments (id, business_id, invoice_id, provider, provider_transaction_id, amount, currency, status, initiated_at, completed_at)
- payment_allocations (id, payment_id, invoice_id, amount)
- provider_configs (id, business_id, provider, credentials_encrypted, test_mode, status)

**Expenses**
- expenses (id, business_id, client_id, description, amount, currency, date, category, payment_method, document_url)
- expense_categories (id, business_id, name, description)

**Reconciliation**
- reconciliation_imports (id, business_id, file_name, imported_at, status)
- imported_transactions (id, import_id, date, description, amount, reference, matched_payment_id, match_status)
- match_decisions (id, transaction_id, payment_id, decided_by, decided_at, reason)

**Reports & Exports**
- report_exports (id, business_id, type, parameters, file_url, created_at, created_by)

**Notifications**
- notifications (id, business_id, user_id, type, title, message, read_at, created_at)
- notification_preferences (id, business_id, user_id, preferences_json)

**Audit**
- audit_events (id, actor, tenant_id, action, target_type, target_id, timestamp, outcome, metadata_json)

**Platform Admin**
- admin_users (id, email, password_hash, role)
- tenant_status (id, business_id, status, restricted_at, restricted_by, reason)
- feature_flags (id, name, enabled, description)
- background_jobs (id, type, payload, status, attempts, error_message, created_at, completed_at)
- webhook_deliveries (id, provider, payload, status, attempts, delivered_at, error_message)

## 5.2 Tenant Isolation Strategy

- All business tables include `business_id` foreign key
- Supabase RLS policies enforce `business_id = auth.uid()::text` or membership check
- Server-side authorization validates business membership on every operation
- Indexes on `business_id` for query performance
- No cross-tenant queries allowed

---

# 6. Payment-Provider Integration Strategy

## 6.1 Supported Providers

1. **PayHero** - Kenyan payment gateway
2. **Paystack** - Pan-African payment processor
3. **M-Pesa Daraja** - Safaricom mobile money

## 6.2 Common Adapter Interface

```typescript
interface PaymentProvider {
  validateConnection(credentials): Promise<ConnectionStatus>;
  initiatePayment(request): Promise<PaymentInitiationResponse>;
  verifyTransaction(transactionId): Promise<TransactionStatus>;
  verifyWebhook(payload, signature): boolean;
  mapProviderStatus(providerStatus): PaymentStatus;
  processRefund(request): Promise<RefundResponse>;
  getReconciliationData(startDate, endDate): Promise<ReconciliationData>;
  supportsTestMode(): boolean;
}
```

## 6.3 Implementation Order

1. Paystack (best documentation, easiest integration)
2. M-Pesa Daraja (critical for Kenya market)
3. PayHero (additional Kenyan option)

## 6.4 Security

- Credentials encrypted at rest
- Webhook signature verification
- Idempotency keys for all payment operations
- Never trust client-side payment confirmation
- Verify via webhook or API polling

---

# 7. Security and Compliance Risks

## 7.1 Critical Risks

1. **Cross-tenant data leakage** - Prevented by RLS + server-side auth
2. **Broken access control** - Role-based permissions on all operations
3. **Payment spoofing** - Webhook signature verification
4. **Secret exposure** - Environment variables, no logging of secrets
5. **SQL injection** - Parameterized queries only
6. **XSS/CSRF** - CSP, CSRF tokens, input sanitization

## 7.2 Compliance Considerations

- Kenyan tax requirements: Research eTIMS before claiming compliance
- Data protection: Implement data retention and deletion policies
- Audit trail: Complete logging of financial and security events

---

# 8. UI Reference Resources

## 8.1 Free Resources

1. **SaaS Dashboard UI Kit (Free)**
   - URL: https://www.figma.com/community/file/1561384239599771178/saas-dashboard-ui-kit-free
   - License: Free
   - Use: Component structure, responsive layouts

2. **TailAdmin - Free Design System**
   - URL: https://www.figma.com/community/file/1463141366275764364/tailadmin-free-design-system-and-dashboard-kit-for-tailwind-css
   - License: CC BY 4.0
   - Use: Design system structure, TailwindCSS integration

3. **Spora - Mobile Dashboard UI Kit**
   - URL: https://www.figma.com/community/file/1629707974434651841/spora-simple-mobile-dashboard-app-ui-kit
   - License: Free
   - Use: Mobile navigation patterns

## 8.2 Paid References (for study only)

- Ledger Invoice & Billing Dashboard UI Kit - Invoice lifecycle patterns
- ChatFin Dashboard - Finance dashboard layouts
- Onion Dashboard Pro - Project management patterns

## 8.3 Design Adaptation

- Study patterns, don't copy pixel-for-pixel
- Create original ZetuBIZ design system
- Verify all license terms before use
- Use free resources where possible

---

# 9. Implementation Milestones

## Milestone 0 — Repository Audit ✅
- Inspect existing repository
- Document current state
- Create master specification
- **Status:** Complete

## Milestone 1 — UX and Foundations
- Establish design system
- Define navigation
- Design database schema
- Set up environment configuration
- **Acceptance:** Design system documented, schema defined, environment configured

## Milestone 2 — Business Workspace
- Authentication and onboarding
- Business settings and branding
- Client management
- Projects and milestones
- Dashboard with real data
- **Acceptance:** Users can register, set up business, manage clients and projects

## Milestone 3 — Quotations and Invoicing
- Quotation creation and acceptance
- Conversion to invoice
- Financial calculations
- Finalization and snapshots
- PDF generation
- **Acceptance:** Complete quotation-to-invoice workflow with accurate calculations

## Milestone 4 — Payments
- Provider settings
- First provider integration (Paystack)
- Payment initiation
- Webhook processing
- Receipts
- **Acceptance:** End-to-end payment collection with one verified provider

## Milestone 5 — Finance
- Expenses and attachments
- Transaction history
- Reconciliation imports
- Financial reports
- **Acceptance:** Complete expense tracking and reconciliation

## Milestone 6 — Teams and Platform Admin
- Team roles and permissions
- Separate platform-admin console
- Audit history
- Tenant management
- **Acceptance:** Role-based access and separate admin interface

## Milestone 7 — Production Readiness
- Security review
- Tenant isolation verification
- Automated tests
- Mobile layouts
- Backup and recovery
- **Acceptance:** Production-ready with all features tested

---

---

# 10. Adaptive Onboarding and Workspace Preparation

## 10.1 Core Principle

Onboarding choices must have practical consequences. Every answer configures the user's workspace, dashboard, navigation, terminology, document templates, business settings and recommended next actions.

Users can change answers later in Settings without losing business data.

## 10.2 Onboarding Flow

### Screen 1: Welcome to ZetuBIZ

**Purpose:** Professional introduction to ZetuBIZ capabilities

**Content:** Explain that ZetuBIZ helps manage clients, quotations, invoices, payments, expenses and business operations in one place.

**Actions:**
- Primary: "Get started" - begins onboarding flow
- Secondary: "Sign in" - for existing users

**Behavior:**
- Save progress as user moves through flow
- Show progress indicator and Back button
- Allow resume after leaving or refreshing

### Screen 2: What Best Describes You?

**Options (one primary, optional secondary):**
- Software developer
- Freelancer
- Designer or creative professional
- Consultant
- Digital agency
- Marketing agency
- Retail or trading business
- Contractor or construction business
- Professional services
- Education or training
- Other

**Adaptive Consequences:**

**Software developer:**
- Prioritize: Client projects, development milestones, retainers, hourly/fixed-price quotations, support agreements, project-linked invoices
- Dashboard: Active projects, milestones, retainers, quotations awaiting approval

**Freelancer:**
- Prioritize: Quick quotations, simple invoices, client management, expenses, payment reminders
- Dashboard: Outstanding invoices, quotations awaiting response, recent clients, monthly income/expenses

**Designer:**
- Prioritize: Creative service packages, design milestones, revision terms, branded proposals
- Dashboard: Active projects, quotations awaiting approval, recent clients

**Consultant:**
- Prioritize: Consultation packages, day rates, retainers, deliverables, engagement proposals
- Dashboard: Active engagements, retainers, quotations awaiting approval

**Agency:**
- Prioritize: Multiple clients, team members, project budgets, recurring services, approval workflows, consolidated reporting
- Dashboard: Active projects, team workload, outstanding client balances, revenue and expenses

**Retail or trading business:**
- Prioritize: Products, quantities, stock-related workflows, receipts, sales invoices
- Dashboard: Sales invoices, recent transactions, outstanding balances

**Contractor:**
- Prioritize: Milestone billing, project budgets, staged payments, variation quotations
- Dashboard: Active projects, milestone progress, outstanding invoices

**Education or training:**
- Prioritize: Course/service fees, grouped line items, client payment tracking
- Dashboard: Active courses, student payments, upcoming sessions

**Note:** These are configurable defaults, not permanent restrictions. All features remain accessible per permissions.

### Screen 3: Tell Us About Your Business

**Fields:**
- Business or trading name (required)
- Business category (required)
- Country (required, default Kenya)
- County/region (optional)
- Business email (optional)
- Business phone (optional)
- Business address (optional)
- Website (optional)
- Tax identification details (optional, only when relevant)
- Preferred currency (required, default KES for Kenya)
- Time zone (required, default Africa/Nairobi for Kenya)
- Date format (required)

**Kenya-Specific Defaults:**
- Currency: Kenyan shillings (KES)
- Time zone: Africa/Nairobi
- User can change these values

**Tax Handling:**
- Never assume VAT-registration or tax-compliance
- Tax settings must be explicit and configurable
- Do not collect tax registration unless user indicates relevance

### Screen 4: How Big Is Your Team?

**Options:**
- Just me
- 2–5 people
- 6–10 people
- 11–25 people
- 26–50 people
- More than 50 people

**Adaptive Behavior:**

**Solo user:**
- Simple owner workspace
- No team-management prompts
- Simplified navigation

**Team:**
- Optional team invitation step
- Explain roles: owner, administrator, accountant, manager, staff member
- Enforce permissions on server, not just UI
- Do not create fake team members
- Do not send invitations without confirmation

### Screen 5: What Do You Want to Manage?

**Options (multiple selection):**
- Clients and contacts
- Projects and tasks
- Quotations and proposals
- Invoices and receipts
- Online payments
- Expenses
- Financial reports
- Team and permissions
- Business branding
- Email and client communication

**Adaptive Behavior:**
- Configure dashboard shortcuts
- Set onboarding recommendations
- Prioritize navigation items
- Do not hide core features permanently

### Screen 6: How Did You Hear About ZetuBIZ?

**Options:**
- Google or another search engine
- Facebook
- Instagram
- TikTok
- LinkedIn
- WhatsApp
- YouTube
- Friend or colleague
- Community or event
- Blog or article
- Developer community
- Other
- Prefer not to say

**Handling:**
- Save attribution source with privacy controls
- Must not block onboarding
- Must not make unsupported assumptions
- Optional text field if "Other" selected

### Screen 7: Customize Your Workspace

**Options:**
- Upload business logo (optional)
- Choose accent color (from accessible presets)
- Select document style (template)
- Choose default quotation validity
- Choose default payment terms
- Configure invoice numbering
- Add standard business contact details
- Select preferred language (where supported)

**Live Preview:**
- Show how business name, logo, colors, contact details appear on quotation and invoice
- Update preview in real-time as user makes changes

**Defaults:**
- Provide sensible defaults
- "Skip for now" option available

### Screen 8: Review and Prepare

**Content:**
- Concise summary of all choices
- Each section editable (navigate back to specific step)

**Actions:**
- Primary: "Prepare my workspace"
- Secondary: "Save and finish later" (where supported)

**Workspace Preparation:**
1. Validate required information on server
2. Create or complete business workspace
3. Associate authenticated owner with workspace
4. Save business preferences and onboarding choices
5. Apply selected business profile and default navigation
6. Set up initial document numbering and defaults safely
7. Prepare personalized dashboard
8. Return real success or failure result

**Technical Requirements:**
- Use transactions where practical
- Idempotency for safe retries
- Prevent duplicate workspaces
- Prevent duplicate initialization

### Screen 9: Workspace Preparation Screen

**Content:** Branded, polished progress screen

**Example Steps:**
- Saving business profile
- Setting up workspace preferences
- Preparing quotation and invoice defaults
- Personalizing your dashboard
- Finalizing your workspace

**Requirements:**
- Show only progress supported by actual operations
- No fake percentages
- No claiming completion before actual completion

**Failure Handling:**
- Explain issue in plain language
- Preserve saved answers
- Provide Retry button
- Avoid endless loading screen

### Screen 10: Workspace Ready

**Content:**
- Success message: "Your workspace is ready"
- Explanation of what has been configured
- Clear next actions based on selections

**Next Actions (contextual):**
- Create your first quotation (for developers/freelancers who prioritized quotations)
- Create your first invoice
- Add your first client
- Customize your business documents

**Behavior:**
- Primary action depends on user's onboarding selections
- Clicking action navigates to actual working feature
- No decorative buttons
- No unrelated dashboard redirects

---

# 11. Visual Identity and Design System

## 11.1 Logo Requirements

**ZetuBIZ Platform Logo:**
- Clear wordmark: ZetuBIZ
- Compact symbol for small sizes
- Suitable for light and dark backgrounds
- Works in: application sidebar, login page, document headers, email templates, favicon
- Vector format (SVG) for production
- Light and dark variants where necessary
- Avoid: generic clip art, excessive detail, complicated gradients
- Distinct from customer business logos

**Separation:**
- ZetuBIZ platform logo and customer business logo are separate assets
- Customer branding appears on their documents
- ZetuBIZ brand remains throughout application

## 11.2 Color Palette

**Direction:**
- Restrained, trustworthy business palette
- Deep teal or forest green as primary
- Dark charcoal for text
- White and subtle neutral surfaces
- Consistent use for: primary actions, links, success, warning, error, informational states
- Sufficient text contrast (WCAG AA)

**Avoid:**
- Generic purple-heavy SaaS styling
- Random gradients
- Excessive decorative colors
- Unnecessary glass effects

## 11.3 Typography

**Font Evaluation:**
- Inter or DM Sans for readability and professional appearance
- Choose one primary interface font
- Complementary document typography strategy

**Usage:**
- Strong, consistent heading hierarchy
- Readable body text
- Clearly aligned monetary values
- Tabular numerals for totals and financial tables
- Appropriate line height and spacing
- Legible fonts in generated PDFs and email templates

## 11.4 Layout and Responsiveness

**Approach:**
- Mobile-first design
- Works well on phones, tablets, desktops

**Components:**
- Responsive sidebar on desktop
- Accessible mobile navigation pattern
- Clear page titles and breadcrumbs where useful
- Well-designed tables with useful mobile alternatives
- Restrained borders and shadows
- Consistent spacing and form layouts
- Accessible focus states and keyboard navigation
- Clear loading, empty, error, success states

**Layout Philosophy:**
- Avoid filling every page with cards
- Prefer clean layouts, well-organized sections
- Flexible rows and tables where they communicate information better

**Design Tokens:**
- Reusable components for: buttons, inputs, dropdowns, dialogs, tables, status labels, notifications, navigation, document previews, form validation
- Established before full product implementation

---

# 12. Document Studio and Template Library

## 12.1 Document Studio Purpose

Dedicated workspace where users can design, preview, manage and reuse professional business document templates.

## 12.2 Template Library

**Provided Templates:**
1. Modern Minimal
2. Professional Corporate
3. Clean Freelancer
4. Creative Studio
5. Software Development Proposal
6. Agency Services
7. Consulting and Professional Services
8. Product and Retail Sales
9. Milestone-Based Project
10. Simple Tax and Payment Summary

**Implementation:**
- Shared, tested document rendering engine
- Configurable layouts
- Not separate hard-coded invoice systems

**User Capabilities:**
- Preview template before selection
- Sample layouts with demonstration data (never fake user records)
- Set default quotation template
- Set default invoice template
- Use different templates for different document types
- Preview on desktop and mobile
- Upload or choose business logo
- Configure brand colors
- Configure header and footer information
- Control optional business details
- Choose page sizes and layouts
- Save branding and template preferences
- Restore original template defaults

**Advanced Customization:**
- Add only where reliably implementable
- Do not claim drag-and-drop editor if only fixed templates supported

---

# 13. Quotation Creation Workflow

## 13.1 End-to-End Flow

1. Click "Create quotation"
2. Choose existing client or create new client without losing draft
3. Enter quotation title, reference, issue date, validity date
4. Add line items (descriptions, quantities, units, unit prices)
5. Configure discounts, taxes, other charges
6. Add scope of work, deliverables, exclusions, assumptions, milestones, payment terms
7. Select template
8. Preview final quotation
9. Save as draft or finalize
10. Download, email, or share secure link

## 13.2 Autosave and Calculations

**Autosave:**
- Useful autosave or draft-saving behavior
- Visible save status

**Calculations:**
- Validate inputs
- Calculate totals accurately
- Use decimal-safe monetary calculations
- Server-side or reliable shared calculation layer
- Show subtotal, discounts, taxes, additional charges, final total clearly
- Avoid silently applying unconfigured taxes

## 13.3 Button Behaviors

**Save Draft:**
- Persist draft
- Return clear confirmation

**Preview:**
- Render document using current saved/draft values
- Do not finalize

**Finalize Quotation:**
- Validate required fields
- Assign unique reference safely
- Preserve finalized document snapshot
- Move to appropriate status

**Download PDF:**
- Generate and return real PDF

**Email Quotation:**
- Open reviewable email composition
- Recipient, subject, message, document attachment or secure link

**Copy Share Link:**
- Generate or retrieve valid, secure public-sharing link
- Copy to clipboard

**Share via WhatsApp:**
- Open WhatsApp share flow
- Prepared message and secure link
- Do not claim message sent unless delivery verified

**Duplicate:**
- Create new draft from existing document
- Do not reuse finalized number

**Edit:**
- Permit edits per document status and permissions

**Accept Quotation:**
- Record valid client acceptance event and timestamp
- Subject to configured approval process

**Reject Quotation:**
- Record rejection and optional reason

**Convert to Invoice:**
- Create draft invoice from accepted quotation
- Preserve line items, quantities, agreed prices, taxes, client details
- No re-entry required

**Archive or Cancel:**
- Require confirmation where appropriate
- Retain audit trail

## 13.4 Versioning

- If quotation finalized or accepted, do not silently overwrite
- Use versioning or explicit revision workflow

## 13.5 Quotation Statuses

**States:**
- Draft
- Sent
- Viewed (when view event reliably recorded)
- Accepted
- Rejected
- Expired
- Converted to invoice
- Cancelled

**Transitions:**
- Define allowed status transitions
- Prevent invalid transitions on server

## 13.6 Client Acceptance

**Public Links:**
- Allow clients to view without account when appropriate
- Clear acceptance action
- Record event
- Display relevant terms

**Electronic Signatures:**
- Implement only with appropriate consent and evidence workflow
- Do not represent simple click as legally verified signature

---

# 14. Invoice Creation and Payment Tracking

## 14.1 Invoice Creation

**Support:**
- Invoice number and issue date
- Due date
- Client billing details
- Optional reference to quotation or project
- Line items, quantity, unit prices
- Discounts, applicable taxes, additional charges
- Payment instructions
- Configurable terms and notes
- Partial payments
- Outstanding balance
- Relevant business and tax details

**Numbering:**
- Safe invoice numbering
- Unique under concurrent requests
- Do not allocate permanent numbers to abandoned drafts unless policy requires

## 14.2 Finalization and Snapshots

**Finalized Invoices:**
- Preserve immutable snapshot of issued contents
- Corrections via documented revision, voiding, or credit-note workflow
- Do not silently change original issued document

## 14.3 Invoice Statuses

**States:**
- Draft
- Issued
- Sent
- Partially paid
- Paid
- Overdue
- Void or cancelled (where permitted)

**Status Derivation:**
- Derived from persisted invoice and payment data
- Not merely from button click

## 14.4 Payment Tracking

**Record:**
- Payment amount, date, currency, reference
- Allocation to relevant invoice

**Partial Payments:**
- Allow multiple partial payments
- Calculate remaining balance accurately

**Payment Verification:**
- Do not mark paid solely because customer clicked payment link
- Do not mark paid from unverified provider redirect
- Verify via documented server-side provider workflow
- Use trusted webhook or verification response

**Payment History:**
- Clear payment history for each invoice

## 14.5 PDF Requirements

**Professional PDFs:**
- Professional, printable
- Consistent with on-screen preview

**Include:**
- Business logo and contact information
- Client details
- Document reference and dates
- Line-item table
- Totals and applicable tax breakdown
- Payment terms and instructions
- Page numbers where appropriate
- Footer and business notes

**Quality:**
- Prevent clipped text
- Prevent broken tables
- Prevent overlapping totals
- Prevent missing logos
- Prevent unreadable mobile previews
- Handle long descriptions
- Handle multipage documents properly

**Actions:**
- Download and print
- Test with short and long documents

---

# 15. Secure Document Sharing and Client Portal

## 15.1 Secure Share Links

**Requirements:**
- High-entropy, unguessable public tokens (not sequential IDs)
- Scope every document to owning business
- Support token revocation or link expiration
- Avoid exposing internal database IDs
- Avoid exposing private notes
- Avoid exposing unrelated client information

**Event Recording:**
- Record: sent, viewed, accepted, rejected, payment-confirmed
- Distinguish: email queued, sent, delivered, opened
- Only display states supported by actual provider evidence

**Access Control:**
- Respect business privacy and access permissions
- Prevent cross-tenant access via URL modification

## 15.2 Public Document Page

**Features:**
- Clean, mobile-friendly
- Business branding
- Document summary
- Downloadable PDF
- Appropriate acceptance or payment actions

**Client Accounts:**
- Client should not need ZetuBIZ account to view shared quotation/invoice
- Unless specific protected workflow requires authentication

---

# 16. Email and Client Communication

## 16.1 Email Provider

**Implementation:**
- Real email workflow using supported transactional email provider
- Verify provider's current API and account requirements before implementation

## 16.2 Email Features

**User Capabilities:**
- Email quotations
- Email invoices
- Send payment reminders
- Send overdue notices
- Send receipts after verified payments
- Customize message accompanying document
- Preview email before sending
- View email activity and failure states
- Retry eligible failed sends safely

## 16.3 Email Composition

**Trigger:** User clicks "Email quotation" or "Email invoice"

**Composition Screen/Dialog:**
- Recipient email
- Optional CC where supported
- Subject
- Editable message
- Document attachment or secure share link
- Send button
- Cancel button

**Prefill:**
- Sensible values
- Let user review
- Never send to invented or unverified recipient

**Example Subject:**
`Quotation {{quotation_number}} from {{business_name}}`

**Example Message:**
```
Hello {{client_name}},

Please find your quotation attached, or review it securely using the link below.

If you have any questions about the scope, pricing or terms, please contact us.

Kind regards,
{{business_name}}
```

**Template Variables:**
- Use actual template variables
- Safe escaping
- Replace with validated values at send time

## 16.4 Email Delivery Implementation

**Security:**
- Store provider credentials securely on server
- Never expose secret keys in frontend code

**Configuration:**
- Use supported email provider
- Document configuration requirements

**Tracking:**
- Record send attempts
- Record provider message identifiers where available
- Use retry rules and idempotency where supported
- Avoid duplicate sends

**Error Handling:**
- Handle invalid addresses
- Handle provider outages
- Handle rate limits
- Handle temporary failures
- Provide visible sending state
- Provide clear success or failure result
- Avoid claiming delivery/opened without reliable evidence

**Sender Domain:**
- Use verified sender domain when configured
- Provide setup guide for sender identity, DNS records, domain verification
- Do not assume sender domain already verified

## 16.5 Reminders

**Implementation:**
- Offer manual reminders first
- Optional automated reminders with explicit business settings

**User Control:**
- Control reminder timing
- Control message templates
- Control which invoices qualify
- Disable automation
- View reminder history

**Prevent:**
- Duplicate reminders
- Spamming clients
- Reminders for paid or cancelled invoices

---

# 17. Dashboard Personalization

## 17.1 Adaptive Dashboard

Use onboarding choices to configure dashboard rather than showing generic interface.

## 17.2 Dashboard Examples

**Solo Freelancer:**
- Outstanding invoices
- Quotations awaiting response
- Recent clients
- Quick-create quotation
- Quick-create invoice
- Monthly income and expenses

**Software Developer:**
- Active client projects
- Project milestones
- Retainers and recurring work (if implemented)
- Quotations awaiting approval
- Unpaid invoices
- Recently active clients

**Agency:**
- Active projects and team workload
- Outstanding client balances
- Quotations awaiting approval
- Recent business activity
- Revenue and expenses
- Team invitations and permissions

**Retail Business:**
- Sales invoices
- Recent transactions
- Outstanding balances
- Product/inventory summaries (only if relevant functionality exists)

## 17.3 Data Requirements

- Must use real persisted data
- For new workspace: show useful empty state with primary next action
- Example: "Create your first quotation"
- Never show fake revenue, sample clients, fabricated charts

## 17.4 Customization

- Allow users to customize dashboard shortcuts later

---

# 18. Updated Navigation and Settings

## 18.1 Main Workspace Navigation

**Overview**
- Dashboard

**Clients**
- Client list
- Client creation
- Client profiles

**Projects**
- Project list
- Project creation
- Project details
- Milestones

**Quotations**
- Quotation list
- Quotation creation
- Quotation details

**Invoices**
- Invoice list
- Invoice creation
- Invoice details

**Payments**
- Payment history
- Payment links

**Expenses**
- Expense list
- Expense creation

**Reports**
- Financial reports
- Exports

**Team** (when relevant)
- Team members
- Permissions

**Document Studio**
- Template library
- Template customization
- Branding

**Settings**
- Business profile
- Branding
- Quotation defaults
- Invoice defaults
- Document templates
- Payment connections
- Email and notifications
- Team and permissions
- Security
- Workspace preferences

## 18.2 Settings Capabilities

**Progressive Disclosure:**
- Keep default experience simple
- Make advanced configuration discoverable

**Changeable Without Recreation:**
- Business type
- Team size
- Preferred modules
- Branding
- Document defaults

## 18.3 Platform Administration

**Separation:**
- Keep platform administration separate from normal business workspaces

**Access Control:**
- Platform administrators do not automatically gain unrestricted access to every business's private information
- Apply explicit authorization
- Apply audit logging
- Apply controlled access

---

# 19. Testing and Acceptance Criteria

## 19.1 End-to-End Flows

The implementation is not complete until these flows work end to end:

1. New user completes onboarding and receives exactly one correctly configured workspace
2. Selected business profile changes appropriate defaults and recommendations
3. User creates and saves quotation with accurate calculations
4. User previews and downloads professional PDF
5. User emails quotation to real, user-specified recipient through configured provider
6. User copies secure link and opens it in separate browser session
7. Client can accept quotation where acceptance is enabled
8. Business converts accepted quotation into invoice without re-entering line items
9. User emails or downloads invoice
10. Partial payments update invoice balance accurately
11. Verified payment events update relevant invoice safely
12. Failed email or payment operations display useful message and support safe recovery
13. Tenant-isolation tests prove users cannot access another business's documents
14. Mobile and desktop navigation, forms, previews, dialogs work correctly
15. Empty states, loading states, validation errors, permission errors are tested
16. Document history and audit records remain available for important changes

## 19.2 Testing Approach

**Live Payments:**
- Do not use live payments in automated tests
- Use provider sandbox environments
- Use test adapters where available

---

# 20. Updated Implementation Milestones

## Milestone 0 — Repository Audit ✅
- Inspect existing repository
- Document current state
- Create master specification
- **Status:** Complete

## Milestone 1 — Brand and Design System
- Create ZetuBIZ logo and brand identity
- Establish design tokens and components
- Define color palette and typography
- Create responsive layout patterns
- **Acceptance:** Brand assets created, design system documented, components defined

## Milestone 2 — Authentication and Onboarding
- Implement adaptive onboarding (10 screens)
- Workspace preparation with real progress
- Dashboard personalization based on choices
- Business profile and settings
- **Acceptance:** Complete onboarding flow with adaptive workspace configuration

## Milestone 3 — Clients and Business Settings
- Client management (CRUD)
- Business settings with changeable preferences
- Team size handling
- **Acceptance:** Users can manage clients and change business settings

## Milestone 4 — Document Studio and Templates
- Template library (10 templates)
- Document rendering engine
- Template preview and selection
- Branding customization
- **Acceptance:** Users can select, preview, and customize document templates

## Milestone 5 — Quotation Workflow
- Quotation creation with line items
- Autosave and calculations
- Template-based preview
- PDF generation
- Draft/finalize/status management
- **Acceptance:** Complete quotation creation with accurate calculations and PDF

## Milestone 6 — Invoice Workflow
- Invoice creation from scratch
- Invoice creation from quotation
- Safe numbering and finalization
- Immutable snapshots
- Partial payment tracking
- **Acceptance:** Complete invoice workflow with conversion from quotation

## Milestone 7 — Secure Sharing and Email
- Secure share links with high-entropy tokens
- Public document page
- Email composition and sending
- Email provider integration
- **Acceptance:** Documents can be shared securely and emailed reliably

## Milestone 8 — Payment Tracking and Integration
- Payment recording and allocation
- First provider integration (Paystack)
- Webhook verification
- Idempotency and retry
- **Acceptance:** End-to-end payment collection with verified provider

## Milestone 9 — Expenses, Reports, and Teams
- Expense management with attachments
- Financial reports
- Team roles and permissions
- **Acceptance:** Complete expense tracking, reporting, and team management

## Milestone 10 — Platform Administration
- Separate admin console
- Tenant management
- Audit logs
- **Acceptance:** Platform administrators can manage tenants and view audits

## Milestone 11 — Security Review and Testing
- Security review
- Tenant isolation verification
- Automated tests for all critical workflows
- Mobile layout testing
- **Acceptance:** All acceptance criteria met, security verified, tests passing

## Milestone 12 — Production Readiness
- Backup and recovery procedures
- Monitoring and observability
- Documentation completion
- **Acceptance:** Production-ready with complete documentation and operational procedures

---

# Document Status

This is the expanded master specification for ZetuBIZ, incorporating:
- Adaptive onboarding with practical consequences
- Visual identity and design system
- Document Studio and template library
- Complete quotation and invoice workflows
- Secure document sharing and client portal
- Email and client communication
- Dashboard personalization
- Updated navigation and settings
- Detailed testing and acceptance criteria
- Updated implementation milestones

**Location:** `docs/ZetuBIZ-MASTER-SPEC.md`


# 10. Supporting Specifications

Use these companion documents for details that should not be duplicated in this master file:

- [All-in-One Platform Requirements](ALL-IN-ONE-PLATFORM-REQUIREMENTS.md)
- [Screen-by-Screen UX Specification](UX-SPECIFICATION.md)
- [Role and Permission Matrix](PERMISSIONS-MATRIX.md)
- [Data Dictionary](DATA-DICTIONARY.md)
- [Team Workflow](TEAM-WORKFLOW.md)
- [Decision Log](DECISIONS.md)
- [Test Traceability](TEST-TRACEABILITY.md)

These documents are proposed until reviewed. Resolve contradictions through the decision log and update implementation status only after evidence is available.

---



<a id="document-2"></a>

# Document 2: `docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md`

> Source path: [`docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md`](../ALL-IN-ONE-PLATFORM-REQUIREMENTS.md)

---

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

---



<a id="document-3"></a>

# Document 3: `docs/ARCHITECTURE.md`

> Source path: [`docs/ARCHITECTURE.md`](../ARCHITECTURE.md)

---

# Architecture

This document describes the planned architecture for ZetuBIZ.

## System Overview

ZetuBIZ is a multi-tenant SaaS application built on a serverless architecture with strong tenant isolation and security-first design.

## Technology Stack

### Frontend
- **Language:** HTML5, CSS3, JavaScript (ES6+)
- **Framework:** Vanilla JavaScript (no framework initially)
- **Styling:** TailwindCSS via CDN or build process
- **Typography:** Inter (Google Fonts)
- **Build Tool:** To be determined (likely Vite or similar)

### Backend
- **Runtime:** Cloudflare Workers (serverless)
- **API:** RESTful endpoints via Workers
- **Authentication:** Supabase Auth
- **File Storage:** Cloudflare R2

### Database
- **Database:** Supabase PostgreSQL
- **ORM:** Direct SQL or lightweight ORM (to be determined)
- **Migrations:** Supabase CLI
- **Security:** Row Level Security (RLS)

### Additional Services
- **Email:** Resend or SendGrid
- **PDF Generation:** Puppeteer or similar server-side rendering
- **Payment Providers:** Paystack, M-Pesa Daraja, PayHero

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                         Client Browser                        │
│  (HTML/CSS/JS + TailwindCSS)                                 │
└──────────────────────┬──────────────────────────────────────┘
                       │ HTTPS
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                    Cloudflare Pages                           │
│  (Static Asset Hosting)                                       │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                  Cloudflare Workers                           │
│  (Serverless API Endpoints)                                   │
│  - Authentication                                             │
│  - Business Logic                                             │
│  - Payment Processing                                         │
│  - PDF Generation                                             │
└──────────┬──────────────────────────┬────────────────────────┘
           │                          │
           ▼                          ▼
┌──────────────────────┐   ┌──────────────────────┐
│   Supabase           │   │   Cloudflare R2       │
│   - PostgreSQL        │   │   - File Storage      │
│   - Auth             │   │   - Documents         │
│   - RLS Policies     │   │   - Logos             │
└──────────────────────┘   └──────────────────────┘
           │
           ▼
┌──────────────────────┐
│  External Services   │
│  - Email Provider     │
│  - Payment Providers  │
└──────────────────────┘
```

## Module Architecture

### 1. Authentication Module
**Location:** `workers/auth/` or `functions/auth/`
**Responsibilities:**
- User registration and login
- Email verification
- Password recovery
- Session management
- Token validation

**Dependencies:** Supabase Auth

### 2. Business Module
**Location:** `workers/business/`
**Responsibilities:**
- Business creation and configuration
- Business settings management
- Branding configuration
- Onboarding workflow

**Dependencies:** Supabase Database

### 3. Client Module
**Location:** `workers/clients/`
**Responsibilities:**
- Client CRUD operations
- Client search and filtering
- Client contact management

**Dependencies:** Supabase Database

### 4. Project Module
**Location:** `workers/projects/`
**Responsibilities:**
- Project CRUD operations
- Milestone management
- Team assignment
- Project financial summaries

**Dependencies:** Supabase Database

### 5. Quotation Module
**Location:** `workers/quotations/`
**Responsibilities:**
- Quotation creation and editing
- Line item management
- Calculations (server-side)
- PDF generation
- Public link generation
- Client acceptance tracking

**Dependencies:** Supabase Database, PDF Service

### 6. Invoice Module
**Location:** `workers/invoices/`
**Responsibilities:**
- Invoice creation from scratch or quotation
- Safe numbering
- Finalization and snapshots
- Credit notes
- PDF generation
- Public link generation

**Dependencies:** Supabase Database, PDF Service

### 7. Payment Module
**Location:** `workers/payments/`
**Responsibilities:**
- Payment initiation via providers
- Webhook processing
- Payment verification
- Payment allocation
- Idempotency handling

**Dependencies:** Payment Provider APIs, Supabase Database

### 8. Expense Module
**Location:** `workers/expenses/`
**Responsibilities:**
- Expense CRUD operations
- Document upload
- Category management
- Expense reports

**Dependencies:** Supabase Database, Cloudflare R2

### 9. Reconciliation Module
**Location:** `workers/reconciliation/`
**Responsibilities:**
- CSV import
- Transaction matching
- Manual resolution
- Reconciliation reports

**Dependencies:** Supabase Database

### 10. Report Module
**Location:** `workers/reports/`
**Responsibilities:**
- Financial summaries
- Invoice reports
- Payment reports
- Export generation

**Dependencies:** Supabase Database

### 11. Notification Module
**Location:** `workers/notifications/`
**Responsibilities:**
- Email composition and sending
- Reminder scheduling
- Notification preferences
- Delivery tracking

**Dependencies:** Email Provider, Supabase Database

### 12. Team Module
**Location:** `workers/team/`
**Responsibilities:**
- Team member invitations
- Role assignment
- Permission enforcement
- Access revocation

**Dependencies:** Supabase Database, Supabase Auth

### 13. Platform-Admin Module
**Location:** `workers/admin/` (separate deployment)
**Responsibilities:**
- Tenant management
- System monitoring
- Audit log access
- Feature flags
- Background job management

**Dependencies:** Supabase Database (with elevated access)

## Data Flow

### Authentication Flow
```
Client → Cloudflare Pages → Cloudflare Worker (Auth) → Supabase Auth
                                                      ↓
                                              Session Token
                                                      ↓
                                                  Client
```

### Quotation Creation Flow
```
Client → Cloudflare Worker (Quotations) → Supabase Database
                                      ↓
                              PDF Generation Service
                                      ↓
                                  Cloudflare R2
                                      ↓
                                  Client
```

### Payment Flow
```
Client → Cloudflare Worker (Payments) → Payment Provider API
                                      ↓
                              Payment Initiation
                                      ↓
                                  Client
                                      ↓
                              Provider Webhook
                                      ↓
                      Cloudflare Worker (Webhook Handler)
                                      ↓
                              Supabase Database
                                      ↓
                              Email Notification
```

## Tenant Isolation Strategy

### Database Level
- Every table includes `business_id` foreign key
- All queries filter by `business_id`
- Indexes on `business_id` for performance

### Application Level
- Server-side validation of business membership on every operation
- Session tokens include business context
- Authorization checks before data access

### RLS Policies
- Supabase Row Level Security policies enforce tenant isolation
- Policies check user's business membership
- No direct table access without RLS check

Example RLS policy:
```sql
CREATE POLICY "Users can view own business data"
ON clients
FOR SELECT
USING (
  business_id IN (
    SELECT business_id 
    FROM business_memberships 
    WHERE user_id = auth.uid()
  )
);
```

## Security Architecture

### Secret Management
- Secrets stored in Cloudflare Workers secrets
- Never exposed to client-side code
- Payment credentials encrypted in database
- Rotation procedures documented

### Authentication
- Supabase Auth for user authentication
- JWT tokens for session management
- Token expiration and refresh
- Secure cookie flags (HttpOnly, Secure, SameSite)

### Authorization
- Role-based permissions (owner, admin, finance, manager, staff, viewer)
- Server-side permission checks
- No client-side permission enforcement

### Payment Security
- Webhook signature verification
- Idempotency keys for all payment operations
- Never trust client-side payment confirmation
- Verify via server-side API or webhook

### Audit Trail
- All sensitive operations logged
- Audit events include actor, timestamp, action, outcome
- Audit logs append-only for ordinary users
- Platform administrators can view all audit logs

## File Structure (Planned)

```
ZetuBIZ/
├── public/                 # Static assets
│   ├── index.html
│   ├── css/
│   ├── js/
│   └── assets/
├── workers/                # Cloudflare Workers
│   ├── auth/
│   ├── business/
│   ├── clients/
│   ├── projects/
│   ├── quotations/
│   ├── invoices/
│   ├── payments/
│   ├── expenses/
│   ├── reconciliation/
│   ├── reports/
│   ├── notifications/
│   ├── team/
│   └── admin/
├── supabase/               # Database migrations
│   └── migrations/
├── docs/                   # Documentation
│   ├── ZetuBIZ-MASTER-SPEC.md
│   ├── ARCHITECTURE.md
│   ├── DATABASE.md
│   ├── DEVELOPMENT.md
│   ├── ENVIRONMENT.md
│   ├── TESTING.md
│   ├── DEPLOYMENT.md
│   ├── IMPLEMENTATION-STATUS.md
│   └── HANDOVER.md
├── src/                    # Shared code (if needed)
│   ├── types/
│   ├── utils/
│   └── constants/
├── tests/                  # Test files
├── .env.example
├── .gitignore
├── AGENTS.md
├── README.md
├── TASKS.md
├── package.json
└── wrangler.toml
```

## API Design

### RESTful Conventions
- Use HTTP verbs correctly (GET, POST, PUT, DELETE)
- Use plural nouns for resources (/clients, /invoices)
- Use query parameters for filtering and pagination
- Return appropriate HTTP status codes
- Use JSON for request/response bodies

### Authentication
- Require valid session token for protected endpoints
- Include token in Authorization header
- Return 401 for invalid or expired tokens
- Return 403 for insufficient permissions

### Error Responses
Standard error format:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": {
      "field": "email",
      "issue": "Invalid format"
    }
  }
}
```

### Pagination
Use cursor-based or offset-based pagination:
```
GET /invoices?limit=20&offset=0
```

Response:
```json
{
  "data": [...],
  "pagination": {
    "limit": 20,
    "offset": 0,
    "total": 150,
    "has_more": true
  }
}
```

## Performance Considerations

### Database
- Index foreign keys and frequently queried columns
- Use connection pooling
- Optimize complex queries
- Consider read replicas for scaling

### Caching
- Cache static assets on Cloudflare CDN
- Cache frequently accessed data (Redis, if needed)
- Use HTTP caching headers appropriately

### Workers
- Keep worker execution time under limits
- Use background jobs for long-running tasks
- Implement retry logic for external API calls

## Scalability Strategy

### Horizontal Scaling
- Cloudflare Workers auto-scale
- Database can scale vertically or with read replicas
- File storage scales with Cloudflare R2

### Multi-Region
- Deploy Cloudflare Workers to multiple regions
- Consider database read replicas for global access
- CDN for static assets

### Rate Limiting
- Implement rate limiting on API endpoints
- Per-tenant rate limits
- Global rate limits for abuse prevention

## Monitoring and Observability

### Logging
- Structured JSON logs
- Correlation IDs for request tracing
- Log levels (debug, info, warn, error)
- Never log secrets or sensitive data

### Metrics
- Request latency
- Error rates
- Throughput
- Provider availability
- Database query performance

### Alerts
- High error rate
- Slow responses
- Provider downtime
- Failed webhooks
- Security events

## Disaster Recovery

### Backups
- Automated daily database backups
- Point-in-time recovery capability
- Backup retention policy (30 days)

### Rollback
- Database migration rollback procedures
- Application deployment rollback
- Documented recovery procedures

### High Availability
- Multi-region deployment
- Database failover (if supported)
- CDN for static assets

---

**Status:** Planned architecture - implementation pending  
**Last updated:** October 20, 2026

---



<a id="document-4"></a>

# Document 4: `docs/DATABASE.md`

> Source path: [`docs/DATABASE.md`](../DATABASE.md)

---

# Database Schema

This document describes the database schema, relationships, migrations, and Row Level Security (RLS) policies for ZetuBIZ.

## Database System

- **Database:** Supabase PostgreSQL
- **Schema Version:** 1.0
- **Migration Tool:** Supabase CLI

## Schema Overview

The database is designed with multi-tenancy as a core principle. Every tenant-owned table must include a `business_id` foreign key (or be demonstrably tenant-scoped through a parent relation with matching constraints and policies). Tenant isolation must be enforced by RLS and server-side authorization, not by naming conventions alone.

**Important:** This document contains proposed schema examples, not executable or verified migrations. Before implementation, reconcile every table, foreign key, tenant boundary, deletion rule, index, status transition, and RLS policy into reviewed migrations. Do not run the illustrative SQL as a single production migration.

## Core Tables

### Users & Authentication

**Authoritative decision:** Supabase Auth owns authentication identities, password handling, credential recovery, and auth sessions. Do not create a second application-managed `users` table containing passwords or a `sessions` table containing bearer tokens.

Application-owned user profile data may live in a profile table linked to the Supabase identity:

```sql
CREATE TABLE public.user_profiles (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  display_name TEXT,
  avatar_path TEXT,
  preferred_language TEXT NOT NULL DEFAULT 'en',
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
```

This is a design example, not an applied migration. Use `auth.users(id)` as the foreign-key target for user identity references. Never copy password hashes, access tokens, refresh tokens, or session secrets into application tables. If Supabase Auth behavior changes, record and approve an architecture decision before changing this rule.


### Business & Tenancy

#### businesses
```sql
CREATE TABLE businesses (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  legal_name TEXT,
  category TEXT NOT NULL,
  business_type TEXT NOT NULL,
  country TEXT NOT NULL DEFAULT 'KE',
  region TEXT,
  business_email TEXT,
  business_phone TEXT,
  business_address TEXT,
  website TEXT,
  tax_id TEXT,
  default_currency TEXT NOT NULL DEFAULT 'KES',
  timezone TEXT NOT NULL DEFAULT 'Africa/Nairobi',
  date_format TEXT NOT NULL DEFAULT 'DD/MM/YYYY',
  owner_id UUID NOT NULL REFERENCES auth.users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(name, owner_id)
);
```

#### business_memberships
```sql
CREATE TABLE business_memberships (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  role TEXT NOT NULL CHECK (role IN ('owner', 'admin', 'finance', 'manager', 'staff', 'viewer')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, user_id)
);
```

#### business_settings
```sql
CREATE TABLE business_settings (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  settings_json JSONB NOT NULL DEFAULT '{}',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id)
);
```

#### branding
```sql
CREATE TABLE branding (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  logo_url TEXT,
  primary_color TEXT,
  secondary_color TEXT,
  font_family TEXT,
  document_style TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id)
);
```

#### onboarding_progress
```sql
CREATE TABLE onboarding_progress (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  business_id UUID REFERENCES businesses(id) ON DELETE SET NULL,
  current_step INTEGER NOT NULL DEFAULT 1,
  completed_steps JSONB NOT NULL DEFAULT '[]',
  business_category TEXT,
  team_size TEXT,
  workflow_selections JSONB NOT NULL DEFAULT '[]',
  referral_source TEXT,
  referral_details TEXT,
  status TEXT NOT NULL DEFAULT 'in_progress',
  last_updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  completed_at TIMESTAMP WITH TIME ZONE,
  UNIQUE(user_id)
);
```

### Clients

#### clients
```sql
CREATE TABLE clients (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  client_type TEXT NOT NULL CHECK (client_type IN ('individual', 'business')),
  email TEXT,
  phone TEXT,
  billing_address TEXT,
  tax_id TEXT,
  notes TEXT,
  archived_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### client_contacts
```sql
CREATE TABLE client_contacts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  client_id UUID NOT NULL REFERENCES clients(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  email TEXT,
  phone TEXT,
  is_primary BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Projects

#### projects
```sql
CREATE TABLE projects (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  client_id UUID REFERENCES clients(id) ON DELETE SET NULL,
  name TEXT NOT NULL,
  description TEXT,
  status TEXT NOT NULL DEFAULT 'active' CHECK (status IN ('draft', 'active', 'on_hold', 'completed', 'cancelled')),
  start_date DATE,
  end_date DATE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### milestones
```sql
CREATE TABLE milestones (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  description TEXT,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'in_progress', 'completed', 'cancelled')),
  due_date DATE,
  completed_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### project_assignments
```sql
CREATE TABLE project_assignments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  role TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(project_id, user_id)
);
```

### Quotations

#### quotations
```sql
CREATE TABLE quotations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  client_id UUID NOT NULL REFERENCES clients(id) ON DELETE CASCADE,
  project_id UUID REFERENCES projects(id) ON DELETE SET NULL,
  number TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'sent', 'viewed', 'accepted', 'rejected', 'expired', 'converted', 'cancelled')),
  title TEXT,
  reference TEXT,
  issue_date DATE NOT NULL,
  validity_date DATE NOT NULL,
  subtotal DECIMAL(19,4) NOT NULL DEFAULT 0,
  discount_amount DECIMAL(19,4) NOT NULL DEFAULT 0,
  tax_amount DECIMAL(19,4) NOT NULL DEFAULT 0,
  total DECIMAL(19,4) NOT NULL DEFAULT 0,
  currency TEXT NOT NULL,
  notes TEXT,
  terms TEXT,
  public_token TEXT UNIQUE,
  template_id TEXT,
  finalized_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, number)
);
```

#### quotation_line_items
```sql
CREATE TABLE quotation_line_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  quotation_id UUID NOT NULL REFERENCES quotations(id) ON DELETE CASCADE,
  description TEXT NOT NULL,
  quantity DECIMAL(19,4) NOT NULL,
  unit TEXT,
  unit_price DECIMAL(19,4) NOT NULL,
  discount_percent DECIMAL(5,2) DEFAULT 0,
  tax_percent DECIMAL(5,2) DEFAULT 0,
  amount DECIMAL(19,4) NOT NULL,
  sort_order INTEGER DEFAULT 0
);
```

#### quotation_revisions
```sql
CREATE TABLE quotation_revisions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  quotation_id UUID NOT NULL REFERENCES quotations(id) ON DELETE CASCADE,
  revision_number INTEGER NOT NULL,
  snapshot_json JSONB NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(quotation_id, revision_number)
);
```

#### quotation_acceptances
```sql
CREATE TABLE quotation_acceptances (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  quotation_id UUID NOT NULL REFERENCES quotations(id) ON DELETE CASCADE,
  accepted_by TEXT,
  accepted_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  evidence TEXT,
  ip_address TEXT,
  user_agent TEXT
);
```

### Invoices

#### invoices
```sql
CREATE TABLE invoices (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  client_id UUID NOT NULL REFERENCES clients(id) ON DELETE CASCADE,
  project_id UUID REFERENCES projects(id) ON DELETE SET NULL,
  quotation_id UUID REFERENCES quotations(id) ON DELETE SET NULL,
  number TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'issued', 'sent', 'partially_paid', 'paid', 'overdue', 'void', 'cancelled')),
  issue_date DATE NOT NULL,
  due_date DATE NOT NULL,
  purchase_order_ref TEXT,
  subtotal DECIMAL(19,4) NOT NULL DEFAULT 0,
  discount_amount DECIMAL(19,4) NOT NULL DEFAULT 0,
  tax_amount DECIMAL(19,4) NOT NULL DEFAULT 0,
  total DECIMAL(19,4) NOT NULL DEFAULT 0,
  balance DECIMAL(19,4) NOT NULL DEFAULT 0,
  currency TEXT NOT NULL,
  notes TEXT,
  terms TEXT,
  payment_instructions TEXT,
  public_token TEXT UNIQUE,
  template_id TEXT,
  finalized_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, number)
);
```

#### invoice_line_items
```sql
CREATE TABLE invoice_line_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
  description TEXT NOT NULL,
  quantity DECIMAL(19,4) NOT NULL,
  unit TEXT,
  unit_price DECIMAL(19,4) NOT NULL,
  discount_percent DECIMAL(5,2) DEFAULT 0,
  tax_percent DECIMAL(5,2) DEFAULT 0,
  amount DECIMAL(19,4) NOT NULL,
  sort_order INTEGER DEFAULT 0
);
```

#### invoice_snapshots
```sql
CREATE TABLE invoice_snapshots (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
  snapshot_json JSONB NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### credit_notes
```sql
CREATE TABLE credit_notes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE RESTRICT,
  number TEXT NOT NULL,
  amount DECIMAL(19,4) NOT NULL,
  reason TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, number)
);
```

### Payments

**Relationship rule:** A payment is a money event and must not be structurally restricted to exactly one invoice. Link it to one or more invoices through `payment_allocations`. The illustrative `payments` table therefore must not require `invoice_id`; the allocation table is the authoritative relationship. Reconcile all payment examples and tests against this rule before migration design.

#### payments
```sql
CREATE TABLE payments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  provider TEXT NOT NULL,
  provider_transaction_id TEXT,
  amount DECIMAL(19,4) NOT NULL,
  currency TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'processing', 'successful', 'failed', 'cancelled', 'expired', 'uncertain')),
  payment_method TEXT,
  initiated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  completed_at TIMESTAMP WITH TIME ZONE,
  failure_reason TEXT,
  idempotency_key TEXT UNIQUE
);
```

#### payment_allocations
```sql
CREATE TABLE payment_allocations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  payment_id UUID NOT NULL REFERENCES payments(id) ON DELETE CASCADE,
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
  amount DECIMAL(19,4) NOT NULL,
  allocated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### provider_configs
```sql
CREATE TABLE provider_configs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  provider TEXT NOT NULL,
  credentials_encrypted TEXT NOT NULL,
  test_mode BOOLEAN DEFAULT TRUE,
  status TEXT NOT NULL DEFAULT 'not_configured' CHECK (status IN ('not_configured', 'testing', 'connected', 'degraded', 'authentication_failed', 'disconnected')),
  last_verified_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, provider)
);
```

### Expenses

#### expenses
```sql
CREATE TABLE expenses (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  client_id UUID REFERENCES clients(id) ON DELETE SET NULL,
  description TEXT NOT NULL,
  amount DECIMAL(19,4) NOT NULL,
  currency TEXT NOT NULL,
  date DATE NOT NULL,
  category_id UUID REFERENCES expense_categories(id) ON DELETE SET NULL,
  payment_method TEXT,
  document_url TEXT,
  document_name TEXT,
  notes TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### expense_categories
```sql
CREATE TABLE expense_categories (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  description TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Reconciliation

#### reconciliation_imports
```sql
CREATE TABLE reconciliation_imports (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  file_name TEXT NOT NULL,
  file_url TEXT,
  status TEXT NOT NULL DEFAULT 'processing' CHECK (status IN ('processing', 'completed', 'failed')),
  imported_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  imported_by UUID REFERENCES auth.users(id)
);
```

#### imported_transactions
```sql
CREATE TABLE imported_transactions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  import_id UUID NOT NULL REFERENCES reconciliation_imports(id) ON DELETE CASCADE,
  transaction_date DATE NOT NULL,
  description TEXT,
  amount DECIMAL(19,4) NOT NULL,
  reference TEXT,
  matched_payment_id UUID REFERENCES payments(id),
  match_status TEXT NOT NULL DEFAULT 'unmatched' CHECK (match_status IN ('unmatched', 'matched', 'duplicate', 'ambiguous')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### match_decisions
```sql
CREATE TABLE match_decisions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  transaction_id UUID NOT NULL REFERENCES imported_transactions(id) ON DELETE CASCADE,
  payment_id UUID REFERENCES payments(id),
  decided_by UUID NOT NULL REFERENCES auth.users(id),
  decided_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  reason TEXT
);
```

### Reports & Exports

#### report_exports
```sql
CREATE TABLE report_exports (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  type TEXT NOT NULL,
  parameters JSONB NOT NULL,
  file_url TEXT,
  file_name TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  created_by UUID NOT NULL REFERENCES auth.users(id)
);
```

### Notifications

#### notifications
```sql
CREATE TABLE notifications (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  type TEXT NOT NULL,
  title TEXT NOT NULL,
  message TEXT NOT NULL,
  read_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### notification_preferences
```sql
CREATE TABLE notification_preferences (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  preferences_json JSONB NOT NULL DEFAULT '{}',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, user_id)
);
```

### Audit

#### audit_events
```sql
CREATE TABLE audit_events (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  actor UUID REFERENCES auth.users(id),
  tenant_id UUID REFERENCES businesses(id),
  action TEXT NOT NULL,
  target_type TEXT,
  target_id UUID,
  timestamp TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  outcome TEXT NOT NULL CHECK (outcome IN ('success', 'failure')),
  metadata_json JSONB NOT NULL DEFAULT '{}',
  ip_address TEXT,
  user_agent TEXT
);
```

### Platform Admin

#### admin_users
```sql
CREATE TABLE admin_users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  role TEXT NOT NULL CHECK (role IN ('super_admin', 'admin', 'support')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### tenant_status
```sql
CREATE TABLE tenant_status (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  status TEXT NOT NULL DEFAULT 'active' CHECK (status IN ('active', 'suspended', 'restricted')),
  restricted_at TIMESTAMP WITH TIME ZONE,
  restricted_by UUID REFERENCES admin_users(id),
  reason TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id)
);
```

#### feature_flags
```sql
CREATE TABLE feature_flags (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT UNIQUE NOT NULL,
  enabled BOOLEAN NOT NULL DEFAULT FALSE,
  description TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### background_jobs
```sql
CREATE TABLE background_jobs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  type TEXT NOT NULL,
  payload JSONB NOT NULL,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'processing', 'completed', 'failed')),
  attempts INTEGER DEFAULT 0,
  error_message TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  completed_at TIMESTAMP WITH TIME ZONE
);
```

#### webhook_deliveries
```sql
CREATE TABLE webhook_deliveries (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  provider TEXT NOT NULL,
  payload JSONB NOT NULL,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'delivered', 'failed')),
  attempts INTEGER DEFAULT 0,
  delivered_at TIMESTAMP WITH TIME ZONE,
  error_message TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

## Indexes

Create indexes for performance:

```sql
-- Business isolation indexes
CREATE INDEX idx_businesses_owner ON businesses(owner_id);
CREATE INDEX idx_clients_business ON clients(business_id);
CREATE INDEX idx_projects_business ON projects(business_id);
CREATE INDEX idx_quotations_business ON quotations(business_id);
CREATE INDEX idx_invoices_business ON invoices(business_id);
CREATE INDEX idx_payments_business ON payments(business_id);
CREATE INDEX idx_expenses_business ON expenses(business_id);

-- Foreign key indexes
CREATE INDEX idx_quotations_client ON quotations(client_id);
CREATE INDEX idx_invoices_client ON invoices(client_id);
CREATE INDEX idx_payments_invoice ON payments(invoice_id);

-- Status indexes
CREATE INDEX idx_quotations_status ON quotations(business_id, status);
CREATE INDEX idx_invoices_status ON invoices(business_id, status);
CREATE INDEX idx_payments_status ON payments(business_id, status);

-- Date indexes
CREATE INDEX idx_invoices_due_date ON invoices(due_date);
CREATE INDEX idx_quotations_validity ON quotations(validity_date);

-- Search indexes
CREATE INDEX idx_clients_name ON clients(business_id, name);
CREATE INDEX idx_quotations_number ON quotations(business_id, number);
CREATE INDEX idx_invoices_number ON invoices(business_id, number);
```

## Row Level Security (RLS) Policies

### Enable RLS

```sql
ALTER TABLE businesses ENABLE ROW LEVEL SECURITY;
ALTER TABLE clients ENABLE ROW LEVEL SECURITY;
ALTER TABLE quotations ENABLE ROW LEVEL SECURITY;
ALTER TABLE invoices ENABLE ROW LEVEL SECURITY;
-- Enable on all business tables
```

### Example RLS Policies

#### Business Access
```sql
-- Users can view businesses they are members of
CREATE POLICY "Users can view own businesses"
ON businesses
FOR SELECT
USING (
  id IN (
    SELECT business_id 
    FROM business_memberships 
    WHERE user_id = auth.uid()
  )
);

-- Users can insert businesses (for onboarding)
CREATE POLICY "Users can create businesses"
ON businesses
FOR INSERT
WITH CHECK (
  owner_id = auth.uid()
);
```

#### Client Access
```sql
-- Users can view clients from their businesses
CREATE POLICY "Users can view business clients"
ON clients
FOR SELECT
USING (
  business_id IN (
    SELECT business_id 
    FROM business_memberships 
    WHERE user_id = auth.uid()
  )
);

-- Users can insert clients for their businesses
CREATE POLICY "Users can create clients"
ON clients
FOR INSERT
WITH CHECK (
  business_id IN (
    SELECT business_id 
    FROM business_memberships 
    WHERE user_id = auth.uid()
  )
);
```

## Migration Instructions

### Initial Setup

```bash
# Install Supabase CLI
npm install -g supabase

# Initialize Supabase
supabase init

# Link to your project
supabase link --project-ref YOUR_PROJECT_REF

# Push migrations
supabase db push

# Generate types
supabase gen types typescript --local > src/types/supabase.ts
```

### Creating New Migrations

```bash
# Create a new migration
supabase migration new add_new_feature

# Edit the migration file
# Add your SQL changes

# Push the migration
supabase db push
```

### Resetting Database (Development Only)

```bash
# WARNING: This deletes all data
supabase db reset
```

### Rolling Back Migrations

```bash
# View migration history
supabase migration list

# Roll back to specific migration
supabase migration down
```

## Data Integrity

### Tenant-scoped relationships
- Add composite tenant constraints where practical so a quotation/invoice/payment/project/customer relationship cannot point across workspaces.
- Validate parent-child business ownership in database constraints and/or trusted transaction functions; application checks alone are insufficient.
- Review cascade rules carefully. Finalized invoices, payments, credit notes, and audit evidence must not be erased by deleting a customer or workspace record without an approved retention/deletion policy.

### Constraints
- All foreign keys have ON DELETE CASCADE or SET NULL as appropriate
- Unique constraints on critical fields (business name per owner, invoice numbers)
- CHECK constraints on enums and status fields
- NOT NULL constraints on required fields

### Transactions
- Use transactions for multi-table operations
- Example: Creating a quotation with line items

```sql
BEGIN;
INSERT INTO quotations (...) VALUES (...);
INSERT INTO quotation_line_items (...) VALUES (...);
COMMIT;
```

## Backup and Recovery

### Backups
- Supabase provides automated daily backups
- Point-in-time recovery available (7 days)
- Export database schema and data regularly

### Recovery
```bash
# Restore from backup via Supabase dashboard
# Or use point-in-time recovery
```

---

**Status:** Schema designed - implementation pending  
**Last updated:** October 20, 2026

---



<a id="document-5"></a>

# Document 5: `docs/DATA-DICTIONARY.md`

> Source path: [`docs/DATA-DICTIONARY.md`](../DATA-DICTIONARY.md)

---

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

---



<a id="document-6"></a>

# Document 6: `docs/DECISIONS.md`

> Source path: [`docs/DECISIONS.md`](../DECISIONS.md)

---

# ZetuBIZ Decision Log

Use this file as the index for product and architecture decisions. Detailed architecture decisions should be added as individual records in `docs/decisions/` when that folder is created. Do not treat proposals below as approved choices.

## Decision status

- **Proposed:** Option raised, not approved.
- **Accepted:** Accountable owner approved the decision.
- **Rejected:** Considered and not selected.
- **Superseded:** Replaced by a newer decision.

## Proposed baseline (requires review)

| ID | Topic | Current proposal | Status | Owner |
|---|---|---|---|---|
| P-001 | Product direction | Modular, multi-tenant business operations platform, Kenya-first | Proposed | Product owner |
| A-001 | Frontend | HTML/CSS/JavaScript; build tool not yet finalized | Proposed | Architecture lead |
| A-002 | Runtime | Cloudflare Pages/Workers | Proposed | Architecture lead |
| A-003 | Database | Supabase PostgreSQL with RLS | Proposed | Architecture/security |
| A-004 | Identity | Supabase Auth; app tables reference `auth.users(id)` | Proposed | Architecture/security |
| A-005 | File storage | Cloudflare R2 with private-by-default access | Proposed | Architecture lead |
| A-006 | PDF generation | Server-side generation compatible with selected runtime; library TBD | Proposed | Architecture lead |
| A-007 | Email | Provider selection pending sender/domain and cost verification | Proposed | Integrations lead |
| A-008 | Payments | Verify each provider's current API, webhook, credentials, and commercial capabilities before commitment | Proposed | Payments lead |
| S-001 | Tenant security | Database RLS plus server-side authorization and negative isolation tests | Proposed | Security lead |
| F-001 | Financial records | Finalized documents are snapshot-based; corrections use traceable credit/void/reversal workflows | Proposed | Finance domain owner |
| T-001 | Delivery process | Documentation-first; implementation blocked until gate approval is recorded | Accepted as current workflow | Product owner |

## Required record format

For every decision, record:

- **ID and title**
- **Status**
- **Date and decision owner**
- **Context and problem**
- **Constraints**
- **Options considered**
- **Decision and rationale**
- **Security/privacy impact**
- **Cost and operational impact**
- **UX/data migration impact**
- **Consequences and follow-up work**
- **Evidence/source and date verified**
- **Review trigger**

## Open decisions to prioritize

1. Initial target customer segment and Release 1 exclusions.
2. Final screen architecture and onboarding behavior.
3. Build tooling and project structure.
4. API organization and runtime boundaries.
5. Database conventions, composite tenant constraints, and RLS policy patterns.
6. PDF generation approach and document rendering consistency.
7. First payment provider after account/API capability verification.
8. Email, SMS, and WhatsApp providers, costs, and consent handling.
9. Subscription plans, limits, and entitlement enforcement.
10. Retention, backup, recovery, incident response, and support access.
11. Tax/payroll/eTIMS scope after current official guidance and qualified review.
12. Branch protection, CI checks, review requirements, and release approval.

Never place secrets, customer data, or credentials in decision records.

---



<a id="document-7"></a>

# Document 7: `docs/DEPLOYMENT.md`

> Source path: [`docs/DEPLOYMENT.md`](../DEPLOYMENT.md)

---

# Deployment Guide

This document describes the deployment process for ZetuBIZ to Cloudflare Pages and Workers.

## Deployment Overview

ZetuBIZ uses:
- **Cloudflare Pages** for static frontend hosting
- **Cloudflare Workers** for serverless API endpoints
- **Supabase** for database and authentication
- **Cloudflare R2** for file storage

## Prerequisites

- Cloudflare account with Workers and Pages enabled
- Supabase production project
- Configured environment variables
- DNS configured (if using custom domain)

## Environment Configuration

### Production Environment Variables

Set these in Cloudflare Workers secrets:

```bash
wrangler secret put SUPABASE_SERVICE_ROLE_KEY
wrangler secret put ENCRYPTION_KEY
wrangler secret put RESEND_API_KEY
wrangler secret put PAYSTACK_SECRET_KEY
wrangler secret put R2_ACCESS_KEY_ID
wrangler secret put R2_SECRET_ACCESS_KEY
```

Set these in Cloudflare Pages environment variables:
- SUPABASE_URL
- SUPABASE_ANON_KEY
- APP_URL
- EMAIL_PROVIDER
- CLOUDFLARE_ACCOUNT_ID

### Staging Environment

Create a separate Cloudflare Workers/Pages project for staging:
- Use test/sandbox payment providerKeys
- Use test email provider
- Use staging Supabase project

## Deployment Process

### 1. Build for Production

```bash
npm run build
```

This creates optimized production assets in `dist/` or `public/`.

### 2. Deploy to Cloudflare Pages

```bash
npm run deploy:pages
```

Or using Wrangler:

```bash
wrangler pages deploy ./public --project-name=zetubiz
```

### 3. Deploy Cloudflare Workers

```bash
wrangler deploy
```

This deploys all workers in the project.

### 4. Verify Deployment

- Access the deployed application
- Test critical user flows
- Check worker logs for errors
- Verify database connectivity

## Deployment Environments

### Development

- **URL:** http://localhost:3000
- **Database:** Local Supabase or development project
- **Payment:** Sandbox/test mode
- **Purpose:** Local development

### Staging

- **URL:** https://staging.zetubiz.com (or similar)
- **Database:** Staging Supabase project
- **Payment:** Sandbox/test mode
- **Purpose:** Pre-production testing

### Production

- **URL:** https://zetubiz.com
- **Database:** Production Supabase project
- **Payment:** Live mode
- **Purpose:** Live production application

## Database Migrations

### Apply Migrations to Production

```bash
supabase db push --db-url $PRODUCTION_DATABASE_URL
```

**Important:**
- Test migrations in staging first
- Backup database before major migrations
- Have rollback plan ready
- Run during low-traffic period

### Migration Rollback

If a migration fails:

1. Identify the failing migration
2. Roll back to previous version:
```bash
supabase migration down
```
3. Fix the migration
4. Re-apply:
```bash
supabase db push
```

## Rollback Procedures

### Application Rollback

If deployment causes issues:

**Cloudflare Pages:**
```bash
wrangler pages deploy ./public --project-name=zetubiz --branch=previous-branch
```

**Cloudflare Workers:**
```bash
wrangler deploy --version previous-version
```

### Database Rollback

If database migration causes issues:

1. Restore from backup via Supabase dashboard
2. Or use point-in-time recovery (available within 7 days)
3. Document the issue
4. Fix the migration
5. Re-apply when ready

## Monitoring

### Cloudflare Analytics

Monitor via Cloudflare dashboard:
- Request volume
- Error rates
- Response times
- Geographic distribution

### Worker Logs

View logs:
```bash
wrangler tail
```

Or via Cloudflare dashboard:
- Workers & Pages > Your Worker > Logs

### Database Monitoring

Monitor via Supabase dashboard:
- Query performance
- Connection pool
- Storage usage
- Backup status

### Error Tracking

Configure Sentry (if using):
```bash
wrangler secret put SENTRY_DSN
```

View errors at https://sentry.io

## Health Checks

### Application Health

Create a health check endpoint:
```
GET /health
```

Returns:
```json
{
  "status": "healthy",
  "timestamp": "2026-10-20T12:00:00Z",
  "database": "connected",
  "services": {
    "email": "connected",
    "payments": "connected"
  }
}
```

### Database Health

Check database connectivity:
- Run test query
- Check connection pool
- Verify RLS policies

### External Service Health

Monitor:
- Email provider status
- Payment provider status
- Cloudflare status

## Backup and Recovery

### Automated Backups

Supabase provides:
- Daily automated backups
- 7-day point-in-time recovery
- 30-day backup retention

### Manual Backup

Before major changes:
```bash
# Export database
supabase db dump -f backup-$(date +%Y%m%d).sql
```

### Recovery Procedures

**Database Recovery:**
1. Access Supabase dashboard
2. Go to Settings > Database
3. Select restore point
4. Confirm recovery

**Application Recovery:**
1. Rollback to previous deployment
2. Verify application works
3. Investigate root cause
4. Fix and redeploy

## Security in Production

### Secrets Management

- Never commit secrets to Git
- Use Cloudflare Workers secrets for sensitive data
- Rotate secrets regularly
- Monitor for secret leaks

### HTTPS Only

- Force HTTPS via Cloudflare settings
- Use HSTS headers
- No HTTP access to production

### Security Headers

Set security headers:
```
Content-Security-Policy: default-src 'self'
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=()
```

### Rate Limiting

Implement rate limiting on:
- API endpoints
- Authentication attempts
- Payment initiation
- Email sending

## Performance Optimization

### Caching

- Cache static assets on Cloudflare CDN
- Cache API responses where appropriate
- Use cache headers correctly
- Implement cache invalidation

### Database Optimization

- Use indexes on frequently queried columns
- Optimize slow queries
- Use connection pooling
- Consider read replicas for scaling

### Worker Optimization

- Keep worker execution time under limits
- Use caching for expensive operations
- Implement retry logic for external APIs
- Minimize cold starts

## Scaling

### Horizontal Scaling

Cloudflare Workers auto-scale automatically.

### Database Scaling

- Vertical scaling via Supabase dashboard
- Consider read replicas for global access
- Optimize queries before scaling

### CDN Scaling

Cloudflare CDN automatically scales globally.

## Deployment Checklist

Before deploying to production:

- [ ] All tests passing
- [ ] Code reviewed
- [ ] Security review completed
- [ ] Database migrations tested in staging
- [ ] Environment variables configured
- [ ] Secrets set in Cloudflare
- [ ] Backup taken before deployment
- [ ] Rollback plan documented
- [ ] Monitoring configured
- [ ] Documentation updated
- [ ] Stakeholders notified

## Troubleshooting

### Deployment Fails

**Problem:** Deployment command fails

**Solutions:**
- Check Cloudflare credentials
- Verify wrangler.toml configuration
- Check build output exists
- View error logs
- Try deploying individual workers

### Application Not Working After Deployment

**Problem:** Application deployed but not functional

**Solutions:**
- Check environment variables
- Verify database connectivity
- Check worker logs for errors
- Test health check endpoint
- Rollback if necessary

### Database Migration Fails

**Problem:** Migration fails in production

**Solutions:**
- Check migration syntax
- Verify database state
- Rollback to previous migration
- Fix migration
- Re-apply

### High Error Rate After Deployment

**Problem:** Error rate increases after deployment

**Solutions:**
- Check worker logs
- Identify failing endpoints
- Rollback if critical
- Investigate root cause
- Fix and redeploy

## Continuous Deployment

### GitHub Actions (Example)

```yaml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm install
      - run: npm test
      - run: npm run build
      - run: wrangler deploy
```

### Deployment Pipeline

1. Code pushed to main branch
2. Tests run automatically
3. Build production assets
4. Deploy to staging
5. Run smoke tests
6. Deploy to production
7. Run health checks
8. Notify team

## Post-Deployment Verification

After deploying to production:

1. **Health Check:** Verify health endpoint returns healthy
2. **Critical Flows:** Test quotation creation, invoice creation, payment flow
3. **Database:** Verify database connectivity and performance
4. **Logs:** Check for errors in worker logs
5. **Monitoring:** Verify metrics are being collected
6. **User Testing:** Have team members test key features
7. **Rollback Plan:** Confirm rollback procedure is ready

## Incident Response

If production issue detected:

1. **Assess:** Determine severity and impact
2. **Communicate:** Notify stakeholders
3. **Mitigate:** Rollback if critical
4. **Investigate:** Review logs and metrics
5. **Fix:** Implement fix
6. **Test:** Test in staging
7. **Deploy:** Deploy fix to production
8. **Verify:** Confirm issue resolved
9. **Document:** Document incident and learnings

---

**Status:** Deployment guide documented  
**Last updated:** October 20, 2026

---



<a id="document-8"></a>

# Document 8: `docs/DEVELOPMENT.md`

> Source path: [`docs/DEVELOPMENT.md`](../DEVELOPMENT.md)

---

# Development Guide

This document provides exact steps for setting up and running ZetuBIZ in a local development environment.

## Prerequisites

- **Node.js:** 18 or higher
- **npm:** 9 or higher
- **Git:** Latest version
- **Supabase CLI:** Latest version
- **Wrangler CLI:** Latest version (for Cloudflare Workers)
- **Code Editor:** VS Code or similar

## Initial Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd ZetuBIZ
```

### 2. Install Node Dependencies

```bash
npm install
```

This installs:
- Development dependencies
- Build tools
- Testing frameworks
- Supabase CLI
- Wrangler CLI

### 3. Install Supabase CLI

```bash
npm install -g supabase
```

Verify installation:
```bash
supabase --version
```

### 4. Install Wrangler CLI

```bash
npm install -g wrangler
```

Verify installation:
```bash
wrangler --version
```

## Environment Configuration

### 1. Create Environment File

```bash
cp .env.example .env
```

### 2. Configure Supabase

Create a Supabase project at https://supabase.com

Add to `.env`:
```bash
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

### 3. Configure Email Provider

Choose Resend or SendGrid and add to `.env`:

**Resend:**
```bash
EMAIL_PROVIDER=resend
RESEND_API_KEY=your-resend-api-key
RESEND_FROM_EMAIL=noreply@yourdomain.com
```

**SendGrid:**
```bash
EMAIL_PROVIDER=sendgrid
SENDGRID_API_KEY=your-sendgrid-api-key
SENDGRID_FROM_EMAIL=noreply@yourdomain.com
```

### 4. Configure Payment Providers

Add to `.env` (only for providers you're using):

```bash
# Paystack
PAYSTACK_SECRET_KEY=your-paystack-secret-key
PAYSTACK_PUBLIC_KEY=your-paystack-public-key

# M-Pesa Daraja
MPESA_CONSUMER_KEY=your-consumer-key
MPESA_CONSUMER_SECRET=your-consumer-secret
MPESA_PASSKEY=your-passkey
MPESA_SHORTCODE=your-shortcode
MPESA_ENVIRONMENT=sandbox

# PayHero
PAYHERO_API_KEY=your-payhero-api-key
PAYHERO_MERCHANT_ID=your-merchant-id
```

### 5. Configure Cloudflare

```bash
wrangler login
```

Add to `.env`:
```bash
CLOUDFLARE_ACCOUNT_ID=your-account-id
CLOUDFLARE_API_TOKEN=your-api-token
```

## Database Setup

### 1. Link to Supabase Project

```bash
supabase link --project-ref YOUR_PROJECT_REF
```

### 2. Run Initial Migrations

```bash
supabase db push
```

This creates all tables, indexes, and RLS policies defined in `docs/DATABASE.md`.

### 3. Seed Development Data (Optional)

Create a seed file in `supabase/seed.sql`:
```bash
supabase db seed
```

### 4. Generate TypeScript Types

```bash
supabase gen types typescript --local > src/types/supabase.ts
```

## Running the Application

### Start Development Server

```bash
npm run dev
```

This starts:
- Frontend development server (typically on http://localhost:3000)
- Cloudflare Workers local environment
- Hot module reloading

### Start Cloudflare Workers Locally

```bash
wrangler dev
```

### Start Supabase Local (Optional)

```bash
supabase start
```

This runs a local Supabase instance for offline development.

## Development Workflow

### 1. Create a Feature Branch

```bash
git checkout -b feature/your-feature-name
```

### 2. Make Changes

- Edit code
- Add tests
- Update documentation

### 3. Run Tests

```bash
npm test
```

### 4. Run Linting

```bash
npm run lint
```

### 5. Run Type Checking (if using TypeScript)

```bash
npm run type-check
```

### 6. Commit Changes

```bash
git add .
git commit -m "Descriptive commit message"
```

### 7. Push Branch

```bash
git push origin feature/your-feature-name
```

## Database Development

### Create a New Migration

```bash
supabase migration new describe_your_change
```

Edit the generated migration file in `supabase/migrations/`.

### Apply Migration

```bash
supabase db push
```

### View Migration Status

```bash
supabase migration list
```

### Reset Database (Development Only)

```bash
supabase db reset
```

**Warning:** This deletes all data in the local database.

### Access Database Directly

```bash
supabase db reset
supabase db shell
```

## Testing

### Run All Tests

```bash
npm test
```

### Run Specific Test File

```bash
npm test -- path/to/test.spec.js
```

### Run Tests in Watch Mode

```bash
npm test -- --watch
```

### Run Tests with Coverage

```bash
npm test -- --coverage
```

See `docs/TESTING.md` for detailed testing strategy.

## Debugging

### Debug Cloudflare Workers

```bash
wrangler dev --local
```

Add console.log statements in your worker code.

### Debug Frontend

Use browser DevTools:
- Open DevTools (F12)
- Check Console for errors
- Use Network tab to inspect API calls
- Use Sources tab for debugging JavaScript

### Debug Database

Use Supabase dashboard:
- Go to https://supabase.com/dashboard
- Select your project
- Use SQL Editor to run queries
- Use Table Editor to inspect data

## Common Development Tasks

### Add a New API Endpoint

1. Create worker file in `workers/your-module/`
2. Implement handler function
3. Add route in main worker or router
4. Add tests
5. Update documentation

### Add a New Database Table

1. Create migration: `supabase migration new add_table_name`
2. Define table schema
3. Add RLS policies
4. Apply migration: `supabase db push`
5. Generate types: `supabase gen types typescript --local`
6. Update documentation

### Add a New Frontend Page

1. Create HTML file in `public/` or `src/`
2. Add styling with TailwindCSS
3. Add JavaScript for interactivity
4. Test responsive layout
5. Update documentation

## Code Style

### JavaScript

- Use ES6+ syntax
- Use const/let, not var
- Use arrow functions for callbacks
- Use template literals for strings
- Add JSDoc comments for functions

### SQL

- Use uppercase for SQL keywords
- Use lowercase for table/column names
- Add comments for complex queries
- Use prepared statements

### Git Commits

Use conventional commits:
```
feat: add quotation creation
fix: resolve invoice calculation error
docs: update API documentation
test: add payment integration tests
refactor: simplify payment module
```

## Troubleshooting

### Supabase Connection Issues

**Problem:** Cannot connect to Supabase

**Solution:**
- Check SUPABASE_URL in .env
- Verify network connectivity
- Check Supabase service status
- Try: `supabase status`

### Migration Conflicts

**Problem:** Migration fails with conflict

**Solution:**
- Check migration history: `supabase migration list`
- Resolve conflict manually
- Or reset: `supabase db reset` (development only)

### Cloudflare Workers Errors

**Problem:** Worker fails to start

**Solution:**
- Check wrangler.toml configuration
- Verify CLOUDFLARE_ACCOUNT_ID
- Check worker syntax
- View logs: `wrangler tail`

### Environment Variables Not Loading

**Problem:** Process.env values undefined

**Solution:**
- Verify .env file exists
- Check .env file syntax (no spaces around =)
- Restart development server
- Check .gitignore (ensure .env is ignored)

## Performance Tips

### Database Queries
- Use indexes on frequently queried columns
- Avoid SELECT * - select only needed columns
- Use pagination for large result sets
- Use connection pooling

### Frontend
- Lazy load images
- Minimize JavaScript bundle size
- Use code splitting for large apps
- Enable browser caching

### Workers
- Keep execution time under limits
- Use caching for expensive operations
- Implement retry logic for external APIs
- Use background jobs for long tasks

## Security in Development

### Never Commit Secrets
- Ensure .env is in .gitignore
- Never commit real API keys
- Use .env.example as template only

### Use Test Environments
- Use provider sandbox/test modes
- Use test payment cards
- Use test email addresses

### Validate Inputs
- Never trust client-side validation
- Validate all inputs server-side
- Sanitize user inputs
- Use parameterized queries

## IDE Configuration

### VS Code Extensions (Recommended)

- ESLint
- Prettier
- PostgreSQL
- Tailwind CSS IntelliSense
- GitLens

### VS Code Settings

Create `.vscode/settings.json`:
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

## Next Steps

Before any implementation:
1. Read `AGENTS.md` and `docs/TEAM-WORKFLOW.md`.
2. Read `docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md` and `docs/ZetuBIZ-MASTER-SPEC.md`.
3. Review architecture, database, testing, deployment, environment, implementation status, and handover documents.
4. Complete the documentation-first approval gate in `docs/TEAM-WORKFLOW.md`.
5. Obtain and record explicit product-owner approval before initializing or implementing the application.
6. Until approval is recorded, contribute only to assigned documentation and review tasks.

---

**Status:** Development guide documented - implementation pending  
**Last updated:** October 20, 2026

---



<a id="document-9"></a>

# Document 9: `docs/ENVIRONMENT.md`

> Source path: [`docs/ENVIRONMENT.md`](../ENVIRONMENT.md)

---

# Environment Variables

This document lists all environment variables used by ZetuBIZ, their purpose, where they belong, and whether they are required.

## Required Variables

### Supabase

**SUPABASE_URL**
- **Purpose:** Supabase project URL
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes
- **Example:** `https://your-project.supabase.co`

**SUPABASE_ANON_KEY**
- **Purpose:** Supabase anonymous/public key for client-side access
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes
- **Example:** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`

**SUPABASE_SERVICE_ROLE_KEY**
- **Purpose:** Supabase service role key for server-side admin access
- **Where:** Cloudflare Workers secrets only (never client-side)
- **Required:** Yes
- **Example:** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`

**Security Note:** Never expose service role key to client-side code.

## Optional Variables

### Email Provider

Choose one email provider:

**EMAIL_PROVIDER**
- **Purpose:** Email provider selection (resend or sendgrid)
- **Where:** .env
- **Required:** Yes (if using email features)
- **Values:** `resend` or `sendgrid`

**RESEND_API_KEY**
- **Purpose:** Resend API key for sending emails
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (if using Resend)
- **Example:** `re_1234567890abcdef`

**RESEND_FROM_EMAIL**
- **Purpose:** Default sender email address
- **Where:** .env
- **Required:** Yes (if using Resend)
- **Example:** `noreply@yourdomain.com`

**SENDGRID_API_KEY**
- **Purpose:** SendGrid API key for sending emails
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (if using SendGrid)
- **Example:** `SG.1234567890abcdef`

**SENDGRID_FROM_EMAIL**
- **Purpose:** Default sender email address
- **Where:** .env
- **Required:** Yes (if using SendGrid)
- **Example:** `noreply@yourdomain.com`

### Payment Providers

Configure only the providers you're using:

**PAYSTACK_SECRET_KEY**
- **Purpose:** PayStack secret key for API authentication
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using Paystack)
- **Example:** `sk_test_1234567890abcdef`

**PAYSTACK_PUBLIC_KEY**
- **Purpose:** PayStack public key for client-side payment initiation
- **Where:** .env (client-side)
- **Required:** No (only if using Paystack)
- **Example:** `pk_test_1234567890abcdef`

**MPESA_CONSUMER_KEY**
- **Purpose:** M-Pesa Daraja consumer key
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using M-Pesa)
- **Example:** `your-consumer-key`

**MPESA_CONSUMER_SECRET**
- **Purpose:** M-Pesa Daraja consumer secret
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using M-Pesa)
- **Example:** `your-consumer-secret`

**MPESA_PASSKEY**
- **Purpose:** M-Pesa Daraja passkey for STK push
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using M-Pesa)
- **Example:** `your-passkey`

**MPESA_SHORTCODE**
- **Purpose:** M-Pesa business short code
- **Where:** .env
- **Required:** No (only if using M-Pesa)
- **Example:** `174379`

**MPESA_ENVIRONMENT**
- **Purpose:** M-Pesa environment (sandbox or live)
- **Where:** .env
- **Required:** No (only if using M-Pesa)
- **Values:** `sandbox` or `live`
- **Default:** `sandbox`

**PAYHERO_API_KEY**
- **Purpose:** PayHero API key
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using PayHero)
- **Example:** `your-payhero-api-key`

**PAYHERO_MERCHANT_ID**
- **Purpose:** PayHero merchant ID
- **Where:** .env
- **Required:** No (only if using PayHero)
- **Example:** `your-merchant-id`

### Cloudflare

**CLOUDFLARE_ACCOUNT_ID**
- **Purpose:** Cloudflare account ID for Workers deployment
- **Where:** .env, wrangler.toml
- **Required:** Yes (for deployment)
- **Example:** `1234567890abcdef`

**CLOUDFLARE_API_TOKEN**
- **Purpose:** Cloudflare API token for authentication
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (for deployment)
- **Example:** `your-api-token`

### Application

**NODE_ENV**
- **Purpose:** Application environment
- **Where:** .env
- **Required:** No
- **Values:** `development`, `staging`, `production`
- **Default:** `development`

**APP_URL**
- **Purpose:** Base URL for the application
- **Where:** .env
- **Required:** Yes (for email links, webhooks)
- **Example:** `https://your-domain.com`

**API_BASE_URL**
- **Purpose:** Base URL for API endpoints
- **Where:** .env
- **Required:** No (defaults to relative paths)
- **Example:** `https://api.your-domain.com`

### File Storage

**R2_BUCKET_NAME**
- **Purpose:** Cloudflare R2 bucket name for file storage
- **Where:** .env, wrangler.toml
- **Required:** Yes (for file uploads)
- **Example:** `zetubiz-documents`

**R2_ACCESS_KEY_ID**
- **Purpose:** R2 access key ID
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (for file uploads)
- **Example:** `your-access-key-id`

**R2_SECRET_ACCESS_KEY**
- **Purpose:** R2 secret access key
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (for file uploads)
- **Example:** `your-secret-access-key`

### Security

**JWT_SECRET**
- **Purpose:** Secret for JWT token signing (if custom JWT implementation)
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (using Supabase Auth)
- **Example:** `your-jwt-secret`

**SESSION_SECRET**
- **Purpose:** Secret for session encryption
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (using Supabase Auth)
- **Example:** `your-session-secret`

**ENCRYPTION_KEY**
- **Purpose:** Key for encrypting sensitive data (payment credentials)
- **Where:** Cloudflare Workers secrets only
- **Required:** Yes (for credential encryption)
- **Example:** `your-32-character-encryption-key`

### Monitoring

**SENTRY_DSN**
- **Purpose:** Sentry DSN for error tracking
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No
- **Example:** `https://your-sentry-dsn`

**LOG_LEVEL**
- **Purpose:** Logging verbosity
- **Where:** .env
- **Required:** No
- **Values:** `debug`, `info`, `warn`, `error`
- **Default:** `info`

## Environment-Specific Configuration

### Development (.env.development)

```bash
NODE_ENV=development
APP_URL=http://localhost:3000
LOG_LEVEL=debug
MPESA_ENVIRONMENT=sandbox
```

### Staging (.env.staging)

```bash
NODE_ENV=staging
APP_URL=https://staging.zetubiz.com
LOG_LEVEL=info
MPESA_ENVIRONMENT=sandbox
```

### Production (.env.production)

```bash
NODE_ENV=production
APP_URL=https://zetubiz.com
LOG_LEVEL=warn
MPESA_ENVIRONMENT=live
```

## Cloudflare Workers Secrets

Set these using Wrangler CLI:

```bash
wrangler secret put SUPABASE_SERVICE_ROLE_KEY
wrangler secret put ENCRYPTION_KEY
wrangler secret put RESEND_API_KEY
wrangler secret put PAYSTACK_SECRET_KEY
```

Or set in Cloudflare dashboard:
1. Go to Workers & Pages
2. Select your worker
3. Settings > Variables and Secrets
4. Add secrets

## Security Best Practices

### Never Commit Secrets
- Add `.env` to `.gitignore`
- Never commit `.env` file
- Never commit secrets in code
- Use `.env.example` as template only

### Use Different Keys per Environment
- Development keys should be test/sandbox keys
- Staging keys should be test/sandbox keys
- Production keys should be live keys
- Never use production keys in development

### Rotate Keys Regularly
- Rotate API keys every 90 days
- Rotate encryption keys annually
- Document rotation procedures
- Test key rotation in staging first

### Limit Key Permissions
- Use minimum required permissions
- Use scoped keys where available
- Revoke unused keys
- Monitor key usage

## Variable Validation

The application should validate:
- Required variables are present
- Variable formats are correct (URLs, emails)
- No empty strings where values are expected
- Environment-specific configurations are appropriate

## Troubleshooting

### Variables Not Loading

**Problem:** Environment variables undefined

**Solutions:**
- Check .env file exists
- Check .env file syntax (no spaces around =)
- Restart development server
- Check Cloudflare Workers secrets are set
- Verify variable names match exactly

### Invalid API Keys

**Problem:** API authentication failing

**Solutions:**
- Verify API key is correct
- Check key has required permissions
- Verify key is for correct environment (test/live)
- Check key hasn't expired or been revoked

### Cloudflare Workers Secrets Not Working

**Problem:** Workers can't access secrets

**Solutions:**
- Verify secrets are set in Cloudflare dashboard
- Check secret names match exactly
- Use `wrangler secret list` to verify
- Redeploy worker after adding secrets

## .env.example Template

```bash
# Supabase
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Email Provider
EMAIL_PROVIDER=resend
RESEND_API_KEY=your-resend-api-key
RESEND_FROM_EMAIL=noreply@yourdomain.com

# Payment Providers
PAYSTACK_SECRET_KEY=your-paystack-secret-key
PAYSTACK_PUBLIC_KEY=your-paystack-public-key
MPESA_CONSUMER_KEY=your-consumer-key
MPESA_CONSUMER_SECRET=your-consumer-secret
MPESA_PASSKEY=your-passkey
MPESA_SHORTCODE=174379
MPESA_ENVIRONMENT=sandbox
PAYHERO_API_KEY=your-payhero-api-key
PAYHERO_MERCHANT_ID=your-merchant-id

# Cloudflare
CLOUDFLARE_ACCOUNT_ID=your-account-id
CLOUDFLARE_API_TOKEN=your-api-token

# Application
NODE_ENV=development
APP_URL=http://localhost:3000

# File Storage
R2_BUCKET_NAME=zetubiz-documents
R2_ACCESS_KEY_ID=your-access-key-id
R2_SECRET_ACCESS_KEY=your-secret-access-key

# Security
ENCRYPTION_KEY=your-32-character-encryption-key

# Monitoring (Optional)
SENTRY_DSN=https://your-sentry-dsn
LOG_LEVEL=debug
```

---

**Status:** Environment variables documented  
**Last updated:** October 20, 2026

---



<a id="document-10"></a>

# Document 10: `docs/HANDOVER.md`

> Source path: [`docs/HANDOVER.md`](../HANDOVER.md)

---

# Handover Notes for OpenCode

This document provides a concise handover for the next coding agent (OpenCode).

## Current State

**Project Status:** Documentation/specification work in progress; implementation blocked pending product-owner approval

**What ZetuBIZ Currently Does:**
- Nothing implemented yet (greenfield project)
- Complete specification documented
- Database schema designed
- Architecture planned
- All documentation created

**What Is Implemented and Verified:**
- None (greenfield project)

**What Is Partially Implemented:**
- None

**What Is Still Missing:**
- All features (see IMPLEMENTATION-STATUS.md for complete list)
- Codebase initialization
- Database setup
- Infrastructure configuration

## Current Architecture

**Planned Stack:**
- Frontend: HTML5, CSS3, JavaScript (ES6+), TailwindCSS
- Backend: Cloudflare Workers (serverless)
- Database: Supabase PostgreSQL
- Authentication: Supabase Auth
- Storage: Cloudflare R2
- Email: Resend or SendGrid
- PDF: Puppeteer or similar

**Important File Locations:**
- Specification: `docs/ZetuBIZ-MASTER-SPEC.md`
- Expanded platform requirements: `docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md`
- Team workflow: `docs/TEAM-WORKFLOW.md`
- Architecture: `docs/ARCHITECTURE.md`
- Database: `docs/DATABASE.md`
- Development: `docs/DEVELOPMENT.md`
- Implementation Status: `docs/IMPLEMENTATION-STATUS.md`
- Tasks: `TASKS.md`
- Agent Instructions: `AGENTS.md`

## Database and Migration Status

**Status:** Schema designed, not implemented

- 30+ tables designed with relationships
- RLS policies designed
- Indexes designed
- No migrations created yet
- No Supabase project created yet

**Next Database Step:** Review and correct the data model, tenant boundaries, auth references, deletion rules, indexes, and RLS policy design. Creating a Supabase project or running migrations is blocked until the documentation gate is approved.

## Environment Setup Requirements

**Required:**
- Node.js 18+
- Supabase account
- Cloudflare account
- Email provider account (Resend or SendGrid)

**Environment Variables:** See `docs/ENVIRONMENT.md` and `.env.example`

## Tests Executed and Results

**Status:** No tests executed (no code to test)

## Known Bugs and Technical Risks

**Known Bugs:** None (greenfield project)

**Technical Risks:**
1. Payment provider integrations are complex - implement one at a time
2. Multi-tenancy security is critical - implement RLS early
3. PDF generation quality requires manual review
4. Email delivery requires provider-specific testing

## Exact Next Task

**Task:** Complete and review the documentation package. Do not initialize the application or create database migrations yet.

**Required reading:**
1. `AGENTS.md`
2. `docs/TEAM-WORKFLOW.md`
3. `docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md`
4. `docs/ZetuBIZ-MASTER-SPEC.md`
5. `docs/ARCHITECTURE.md`
6. `docs/DATABASE.md`
7. `docs/IMPLEMENTATION-STATUS.md`
8. `docs/UX-SPECIFICATION.md`
9. `docs/PERMISSIONS-MATRIX.md`
10. `docs/DATA-DICTIONARY.md`
11. `docs/DECISIONS.md`
12. `docs/TEST-TRACEABILITY.md`
13. `TASKS.md`

**Acceptance criteria:**
1. UX screen map and onboarding flow are documented and reviewed.
2. Release 1 scope and exclusions are approved.
3. Permissions and tenant-isolation matrix is documented.
4. Data model and architecture decisions are reviewed.
5. API/integration contracts and failure states are specified.
6. Test strategy and requirement-to-test traceability are documented.
7. Operations, backups, support, and incident procedures are specified.
8. All documentation contradictions are resolved or logged as explicit decisions.
9. Named owners and reviewers are assigned.
10. Product-owner approval is recorded before implementation begins.

**First action:** Review `TASKS.md` and take an assigned documentation task. No `npm init`, database project setup, migrations, or feature implementation until the approval is recorded.

## Important Reminders for OpenCode

1. **Documentation first:** Implementation is blocked until the product owner records approval of the documentation gate.
2. **Read First:** Read AGENTS.md, TEAM-WORKFLOW.md, ALL-IN-ONE-PLATFORM-REQUIREMENTS.md, ZetuBIZ-MASTER-SPEC.md, ARCHITECTURE.md, IMPLEMENTATION-STATUS.md, and TASKS.md before starting
3. **Verify Reality:** Inspect actual code and Git status - don't trust documentation blindly
4. **Follow Stack:** Use the planned technology stack - don't introduce new frameworks without approval
5. **Security First:** Implement tenant isolation from the start, never expose secrets
6. **Real Data Only:** Never use fake financial data or sample clients
7. **One Milestone at a Time:** Complete Milestone 1 before moving to Milestone 2
8. **Test Everything:** Write tests for financial calculations, tenant isolation, and critical workflows
9. **Update Documentation:** Update IMPLEMENTATION-STATUS.md and TASKS.md as you implement
10. **Ask for Approval:** Ask before destructive database operations or major architecture changes
11. **No Secrets:** Never commit secrets or credentials to Git

## Commands to Get Started

Do not run setup or migration commands yet. Follow the documentation gate in `docs/TEAM-WORKFLOW.md`. Implementation commands will be added after the stack, package manager, build process, migration plan, and product scope are approved.

## Quick Reference

- **Master Spec:** docs/ZetuBIZ-MASTER-SPEC.md
- **Architecture:** docs/ARCHITECTURE.md
- **Database:** docs/DATABASE.md
- **Development:** docs/DEVELOPMENT.md
- **Environment:** docs/ENVIRONMENT.md
- **Testing:** docs/TESTING.md
- **Deployment:** docs/DEPLOYMENT.md
- **Implementation Status:** docs/IMPLEMENTATION-STATUS.md
- **Tasks:** TASKS.md
- **Agent Instructions:** AGENTS.md

## Handover Summary

- **Project:** ZetuBIZ - Business management platform
- **Status:** Greenfield, specification complete
- **Next Task:** Map T01–T15 to actual team members, confirm skills and availability, and continue the documentation review.
- **Priority:** Complete and approve the documentation gate; do not start application implementation yet.
- **Timeline:** Implementation milestones remain blocked pending approval.
- **Documentation:** Drafts exist and require review, reconciliation, and formal product-owner approval.

---

**Handed over by:** Cascade  
**Date:** October 20, 2026  
**To:** OpenCode  
**Status:** Documentation handover only; implementation is blocked pending recorded product-owner approval

---



<a id="document-11"></a>

# Document 11: `docs/IMPLEMENTATION-STATUS.md`

> Source path: [`docs/IMPLEMENTATION-STATUS.md`](../IMPLEMENTATION-STATUS.md)

---

# Implementation Status

This document tracks the implementation status of ZetuBIZ requirements.

**Last updated:** October 20, 2026  
**Project status:** Documentation expansion in progress — implementation explicitly paused pending product-owner approval

## Milestone Status

### Milestone D0 — Documentation and Product Readiness
- [x] Add expanded all-in-one platform requirements
- [x] Add team workflow and documentation approval gate
- [x] Draft AI-tool-agnostic task handover template
- [x] Draft 15-member team assignment and ownership matrix
- [ ] Map team slots to actual members and confirm skills/availability
- [x] Link expanded requirements from README and master spec
- [x] Correct Supabase Auth guidance in proposed database documentation
- [x] Draft screen-by-screen UX specification
- [ ] Review and approve screen-by-screen UX specification
- [x] Draft data dictionary and permissions matrix
- [ ] Review data dictionary and permissions matrix; produce/approve ERD
- [x] Add decision log and initial test traceability draft
- [ ] Resolve architecture decisions and integration contracts
- [ ] Complete requirement-to-test traceability and operational readiness review
- [ ] Record product-owner approval of documentation gate
- **Status:** In progress
- **Implementation remains blocked until approval is recorded**

### Milestone 0 — Repository Audit ✅
- [x] Inspect existing repository
- [x] Document current state
- [x] Create master specification
- **Status:** Complete

### Milestone 1 — Brand and Design System
- [ ] Create ZetuBIZ logo and brand identity
- [ ] Establish design tokens and components
- [ ] Define color palette and typography
- [ ] Create responsive layout patterns
- **Status:** Not started
- **Blockers:** None

### Milestone 2 — Authentication and Onboarding
- [ ] Implement adaptive onboarding (10 screens)
- [ ] Workspace preparation with real progress
- [ ] Dashboard personalization based on choices
- [ ] Business profile and settings
- **Status:** Not started
- **Blockers:** Milestone 1 must complete first

### Milestone 3 — Clients and Business Settings
- [ ] Client management (CRUD)
- [ ] Business settings with changeable preferences
- [ ] Team size handling
- **Status:** Not started
- **Blockers:** Milestone 2 must complete first

### Milestone 4 — Document Studio and Templates
- [ ] Template library (10 templates)
- [ ] Document rendering engine
- [ ] Template preview and selection
- [ ] Branding customization
- **Status:** Not started
- **Blockers:** Milestone 1 must complete first

### Milestone 5 — Quotation Workflow
- [ ] Quotation creation with line items
- [ ] Autosave and calculations
- [ ] Template-based preview
- [ ] PDF generation
- [ ] Draft/finalize/status management
- **Status:** Not started
- **Blockers:** Milestones 1-4 must complete first

### Milestone 6 — Invoice Workflow
- [ ] Invoice creation from scratch
- [ ] Invoice creation from quotation
- [ ] Safe numbering and finalization
- [ ] Immutable snapshots
- [ ] Partial payment tracking
- **Status:** Not started
- **Blockers:** Milestone 5 must complete first

### Milestone 7 — Secure Sharing and Email
- [ ] Secure share links with high-entropy tokens
- [ ] Public document page
- [ ] Email composition and sending
- [ ] Email provider integration
- **Status:** Not started
- **Blockers:** Milestone 6 must complete first

### Milestone 8 — Payment Tracking and Integration
- [ ] Payment recording and allocation
- [ ] First provider integration (Paystack)
- [ ] Webhook verification
- [ ] Idempotency and retry
- **Status:** Not started
- **Blockers:** Milestone 7 must complete first

### Milestone 9 — Expenses, Reports, and Teams
- [ ] Expense management with attachments
- [ ] Financial reports
- [ ] Team roles and permissions
- **Status:** Not started
- **Blockers:** Milestone 8 must complete first

### Milestone 10 — Platform Administration
- [ ] Separate admin console
- [ ] Tenant management
- [ ] Audit logs
- **Status:** Not started
- **Blockers:** Milestone 9 must complete first

### Milestone 11 — Security Review and Testing
- [ ] Security review
- [ ] Tenant isolation verification
- [ ] Automated tests for all critical workflows
- [ ] Mobile layout testing
- **Status:** Not started
- **Blockers:** All previous milestones must complete

### Milestone 12 — Production Readiness
- [ ] Backup and recovery procedures
- [ ] Monitoring and observability
- [ ] Documentation completion
- **Status:** Not started
- **Blockers:** All previous milestones must complete

## Module Status

### Authentication Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Business Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Client Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Project Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Quotation Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Invoice Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Payment Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented
- **Integrations:** None

### Expense Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Reconciliation Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Report Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Notification Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented
- **Integrations:** None

### Team Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

### Platform-Admin Module
- **Status:** Not implemented
- **Database:** Schema designed
- **API:** Not implemented
- **Frontend:** Not implemented
- **Tests:** Not implemented

## Database Status

### Schema
- **Status:** Designed, not implemented
- **Migrations:** Not created
- **Tables:** 30+ tables designed
- **Indexes:** Designed
- **RLS Policies:** Designed

### Migration Status
- **Initial migration:** Not created
- **Test migrations:** Not created
- **Production migrations:** Not created

## Infrastructure Status

### Cloudflare
- **Workers:** Not configured
- **Pages:** Not configured
- **R2:** Not configured
- **Account:** Not linked

### Supabase
- **Project:** Not created
- **Database:** Not created
- **Auth:** Not configured
- **Storage:** Not configured

### Email Provider
- **Provider:** Not selected
- **Account:** Not created
- **Configuration:** Not configured

### Payment Providers
- **Paystack:** Not configured
- **M-Pesa Daraja:** Not configured
- **PayHero:** Not configured

## Documentation Status

### Product Documentation
- [x] Master specification (ZetuBIZ-MASTER-SPEC.md)
- [x] Expanded platform requirements (ALL-IN-ONE-PLATFORM-REQUIREMENTS.md)
- [x] Team workflow (TEAM-WORKFLOW.md)
- [x] Task handover template draft (TASK-HANDOVER-TEMPLATE.md)
- [x] 15-member assignment matrix draft (TEAM-ASSIGNMENT-MATRIX.md)
- [x] UX specification draft (UX-SPECIFICATION.md)
- [x] Permissions matrix draft (PERMISSIONS-MATRIX.md)
- [x] Data dictionary draft (DATA-DICTIONARY.md)
- [x] Decision log (DECISIONS.md)
- [x] Test traceability draft (TEST-TRACEABILITY.md)
- [ ] Screen-by-screen UX specification
- [ ] Data dictionary and ERD
- [ ] Permissions matrix
- [ ] Architecture decision log
- [x] Architecture (ARCHITECTURE.md)
- [x] Database schema (DATABASE.md)
- [x] Development guide (DEVELOPMENT.md)
- [x] Environment variables (ENVIRONMENT.md)
- [x] Testing strategy (TESTING.md)
- [x] Deployment guide (DEPLOYMENT.md)
- [x] Implementation status (this file)
- [x] Handover notes (HANDOVER.md)
- [x] Agent instructions (AGENTS.md)
- [x] README (README.md)
- [x] Task list (TASKS.md)

### Code Documentation
- [ ] API documentation
- [ ] Component documentation
- [ ] Database documentation (in-code)
- [ ] Architecture decision records (ADRs)

## Known Issues

### Documentation and schema risks

1. The schema examples remain unverified and must not be run as migrations without review.
2. Composite tenant-scoped foreign keys and matching RLS policies need explicit design and negative tests.
3. Payment schema and payment allocation model need reconciliation so one payment can be allocated across multiple invoices.
4. Architecture choices remain proposed; the build tool, PDF strategy, provider selection, and operational targets are not approved.
5. UX, permissions, data dictionary, and test traceability documents are drafts awaiting review.

## Technical Debt

### None

No technical debt at this time (greenfield project).

## Dependencies

### Current Dependencies
- None (project not initialized)

### Planned Dependencies
- Node.js packages (to be determined)
- Supabase CLI
- Wrangler CLI
- Testing framework (Jest or similar)
- E2E testing framework (Playwright or Cypress)
- PDF generation library (Puppeteer or similar)

## Risks and Blockers

### Current Risks

1. **Greenfield Project:** No application codebase is verified; the repository is currently specification-focused.
   - **Mitigation:** Complete and approve documentation before implementation.

2. **Team assignments are provisional:** Actual member names, skills, and availability have not yet been mapped to the proposed 15 slots.
   - **Mitigation:** Product owner confirms the roster, capacity, and reviewers before assigning work.

3. **Payment provider integration:** Capabilities and operational requirements must be verified before choosing an implementation path.
   - **Mitigation:** Record provider evidence and decisions in the integration contracts/decision log.

4. **Multi-tenancy security:** Tenant isolation is a critical design requirement and remains subject to architecture review and negative tests.
   - **Mitigation:** Complete threat modelling, tenant-scoped data design, RLS review, and test requirements before implementation.

### Current Blockers

- Product-owner approval of the documentation gate has not been recorded.
- The proposed 15-member matrix still needs mapping to actual team members.

## Next Steps

1. Continue documentation work using `TASKS.md` and `docs/TEAM-WORKFLOW.md`.
2. Complete the screen-by-screen UX and onboarding specification.
3. Complete the data dictionary, ERD, and permissions matrix.
4. Review architecture decisions, integrations, testing, and operations documentation.
5. Reconcile contradictions across all repository documents.
6. Obtain and record product-owner approval of the documentation gate.
7. Only after approval, create the implementation plan and begin project initialization.

## Notes

- This is a greenfield project - no existing code to preserve
- All requirements are documented in ZetuBIZ-MASTER-SPEC.md
- Architecture is designed but not implemented
- Database schema is designed but not created
- Implementation should follow the milestone order
- Each milestone must meet acceptance criteria before proceeding

---

**Status:** Documentation expansion in progress; implementation paused pending approval  
**Last updated:** October 20, 2026

---



<a id="document-12"></a>

# Document 12: `docs/PERMISSIONS-MATRIX.md`

> Source path: [`docs/PERMISSIONS-MATRIX.md`](../PERMISSIONS-MATRIX.md)

---

# ZetuBIZ Role and Permission Matrix

**Status:** Proposed authorization baseline; must be reviewed before implementation.  
**Security rule:** This matrix is a product-level guide. API authorization and database RLS must independently enforce the approved permissions.

## 1. Permission legend

- **Full:** May perform the action within the authorized workspace.
- **Scoped:** May perform only assigned/owned records or specific granted actions.
- **View:** Read-only access to permitted records.
- **Request:** May submit a request that requires another role's approval.
- **No:** No access by default.
- **Configurable:** Must be explicitly granted; never assume it from a broad role.

## 2. Proposed role presets

| Capability | Owner | Admin | Finance | Sales | Manager/Ops | HR | Staff | Read-only/Auditor |
|---|---|---|---|---|---|---|---|---|
| View general workspace profile | Full | Full | View | View | View | View | View | View |
| Edit business settings/branding | Full | Full | No | No | Configurable | No | No | No |
| Manage workspace members | Full | Scoped | No | No | No | No | No | No |
| Transfer ownership/delete workspace | Full | No | No | No | No | No | No | No |
| View all customers | Full | Full | View | Full | Scoped | No | Scoped | View |
| Edit customers/leads | Full | Full | Scoped | Full | Scoped | No | Scoped | No |
| View quotations | Full | Full | View | Full | Scoped | No | Scoped | View |
| Create/edit draft quotations | Full | Full | No | Full | Scoped | No | Scoped | No |
| Finalize/send quotations | Full | Full | No | Configurable | Configurable | No | No by default | No |
| View invoices/payment status | Full | Full | Full | Scoped | Scoped | No | Scoped | View |
| Create/edit draft invoices | Full | Full | Full | Configurable | Configurable | No | No by default | No |
| Finalize/void invoices | Full | Configurable | Configurable | No by default | No by default | No | No | No |
| Record/reconcile payments | Full | Configurable | Full | No by default | No by default | No | No | View |
| Initiate refunds/provider actions | Full | Configurable | Configurable | No | No | No | No | No |
| Create expenses | Full | Full | Full | Scoped | Full | No | Request | View |
| Approve expenses | Full | Configurable | Configurable | No by default | Configurable | No | No | View |
| View financial reports | Full | Full | Full | Scoped | Scoped | No | No | View |
| Export financial data | Full | Configurable | Configurable | No by default | Configurable | No | No | Configurable |
| Manage products/stock | Full | Full | Scoped | Scoped | Full | No | Scoped | View |
| Manage projects/tasks | Full | Full | Scoped | Scoped | Full | No | Scoped | View |
| View employee directory | Full | Configurable | No | No | Scoped | Full | Self only | Configurable |
| View salary/payroll details | Full | No by default | Configurable | No | No | Configurable | Self payslip only | No |
| Run/approve payroll | Full | No by default | Configurable | No | No | Configurable | No | No |
| Manage integrations/secrets | Full | Configurable | Configurable | No | No | No | No | No |
| Configure website/public API | Full | Full | No | Configurable | Configurable | No | No | No |
| Use AI assistant | Scoped to own permissions | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped read-only |
| Access platform admin console | No by default | No | No | No | No | No | No | No |

The platform support administrator is a separate platform-level role, not a workspace role. Access to tenant content is denied by default; temporary break-glass access requires a reason, approval where feasible, time limit, narrow scope, and audit log.

## 3. Sensitive action controls

Require explicit permission checks and audit events for:
- Inviting, removing, deactivating, or changing a member's role.
- Ownership transfer and workspace deletion.
- Exporting customer, financial, HR, or payroll data.
- Finalizing, voiding, crediting, or reversing financial documents.
- Recording manual payments or initiating refunds.
- Changing payment-provider credentials or webhook settings.
- Changing tax, currency, numbering, or financial-period configuration.
- Viewing/editing payroll, employee documents, or sensitive personal data.
- Publishing websites or changing public API keys/webhooks.
- Granting temporary support access or using platform-admin tools.

For high-risk financial workflows, separate creator and approver where feasible and configurable. The owner role must not bypass audit logging.

## 4. Tenant and record-scope rules

1. A user may belong to several workspaces, each with separate membership and permissions.
2. Every request must resolve the active workspace from an authorized server-side membership context.
3. Never trust a submitted `business_id`, record ID, role, or owner ID without checking authorization.
4. Public document links are a separate restricted access path, scoped to one document and limited actions.
5. A manager's access to a project does not imply access to all customer finances or employee salary records.
6. HR access does not automatically include payroll, medical, disciplinary, or all employee attachments.
7. Finance access does not automatically include workspace member administration or secrets.
8. Read-only access does not automatically include export, download of sensitive attachments, or bulk search.
9. AI retrieval and tool execution must apply the same record-level permissions as normal UI/API requests.
10. Platform administrators and support tools must be isolated from tenant workspace roles.

## 5. Implementation requirements

- Convert approved capabilities into explicit permission keys, not scattered role-name conditionals.
- Define permission inheritance and deny precedence.
- Enforce access in server-side route/service layer and database RLS.
- Use safe database functions for operations that require atomic state changes.
- Add negative tests for cross-tenant access, guessed IDs, revoked memberships, expired invitations, disabled users, and public-link misuse.
- Test every sensitive action for each role, including direct API calls that bypass the UI.
- Audit actor, workspace, action, target, outcome, timestamp, and minimal metadata.
- Never log secrets, access tokens, or unnecessary personal data.
- Review permission changes as security-sensitive schema/API changes.

## 6. Required permission test cases

- [ ] User A cannot read or update User B's workspace records by changing an ID.
- [ ] A removed member loses access promptly, including active sessions as policy requires.
- [ ] A user with read-only access cannot mutate records through direct API calls.
- [ ] A sales user cannot view restricted payroll data or provider secrets.
- [ ] An HR user cannot run payroll unless explicitly granted that permission.
- [ ] A finance user cannot manage workspace roles unless separately granted.
- [ ] A public share token grants access only to its exact document and allowed actions.
- [ ] Revoked or expired public tokens stop working.
- [ ] An AI action cannot read or modify records outside the caller's permissions.
- [ ] Platform support access is denied by default and any approved elevation is time-limited and audited.
- [ ] Exports and bulk operations enforce the same permissions as record-by-record access.
- [ ] Branch/location restrictions are enforced if multi-branch support is enabled.

## 7. Approval

This matrix is a proposal. The product owner, domain leads, and security reviewer must approve it before migrations or authorization code are written. Record changes in `docs/DECISIONS.md` and link them to test cases.

---



<a id="document-13"></a>

# Document 13: `docs/TASK-HANDOVER-TEMPLATE.md`

> Source path: [`docs/TASK-HANDOVER-TEMPLATE.md`](../TASK-HANDOVER-TEMPLATE.md)

---

# ZetuBIZ Task Handover Template

**Purpose:** Make every task transferable between people, AI models, coding assistants, and development environments without relying on private conversation history.

Copy this template into the relevant issue, pull request, or task record. Delete instructional notes before finalizing where appropriate.

---

## 1. Task identity

- **Task ID / issue:**
- **Title:**
- **Task owner:**
- **Reviewer(s):**
- **Status:** Backlog / Ready for documentation / In progress / Needs decision / In review / Changes requested / Approved / Blocked / Done / Deferred
- **Priority:**
- **Related requirements and documents:**
- **Current branch:**
- **Latest commit SHA or PR:**

## 2. Goal and scope

**Problem to solve:**  
Describe the user or business problem, not just the proposed technical change.

**Expected outcome:**  
Describe what should be true when the task is complete.

**In scope:**
- 

**Out of scope:**
- 

**Assumptions:**
- Mark each assumption as unverified until evidence confirms it.

## 3. Acceptance criteria

- [ ] 
- [ ] 
- [ ] 

Criteria must be observable and testable. Link each criterion to requirement IDs where available.

## 4. Context read

List the authoritative files, sections, issues, or decisions reviewed. Do not assume the next contributor has the previous conversation.

- AGENTS.md:
- docs/TEAM-WORKFLOW.md:
- Task-specific specifications:
- Relevant decision records:
- Relevant database/API contracts:
- Related issues or PRs:

## 5. Work completed

For each change, state what changed, why, and link the exact file, commit, or PR.

| File or resource | Change made | Reason / requirement |
|---|---|---|
|  |  |  |

Do not describe proposed work as completed work.

## 6. Verification and evidence

- **Checks/tests run:**
- **Exact command or procedure:**
- **Result:** Pass / Fail / Not run
- **Evidence or logs:**
- **Manual checks performed:**
- **Checks not performed and why:**

Only report results actually observed. If no test was run, say so explicitly.

## 7. Decisions and unresolved questions

**Decisions made:**  
Include a link to the approved decision record. A conversation message alone is not a permanent decision record.

**Open questions:**
- Question:
- Impact if unresolved:
- Person or role needed to decide:

**Conflicts with existing documentation:**  
List any contradictions discovered. Do not silently choose between conflicting specifications.

## 8. Risks, defects, and dependencies

- **Known defects:**
- **Security/privacy/tenant-isolation impact:**
- **Data migration or compatibility impact:**
- **External dependencies or provider assumptions:**
- **Blocked by:**
- **Rollback or recovery considerations:**

Never include passwords, API keys, tokens, production secrets, or unnecessary real customer data.

## 9. Next action

- **Exact next task:**
- **Next owner:**
- **Acceptance criteria for next task:**
- **Dependencies that must be resolved first:**
- **Suggested starting files or sections:**

The next action must be specific enough for a new contributor to start without asking the previous contributor to reconstruct their context.

## 10. AI-tool portability checklist

- [ ] Relevant findings are recorded in the repository or task record.
- [ ] No critical requirement exists only in an AI conversation or hidden prompt.
- [ ] No provider-specific feature is required unless explicitly approved.
- [ ] Generated suggestions were checked against the approved specifications.
- [ ] Secrets and unnecessary personal/customer data were not supplied to an AI tool or committed.
- [ ] A human reviewer is identified where review is required.
- [ ] The current documentation approval gate has been respected.

## 11. Final handover summary

**Current state:**  
Summarize the verified state in 2–5 sentences.

**What the next contributor should do first:**  
Give one concrete first action.

**Prepared by:**  
**Date:**  
**Reviewer / receiver acknowledgement:**

---

## Example of a good status update

> Completed the draft customer lifecycle requirements and linked them to the UX specification. Reviewed the existing master specification and recorded two unresolved questions in the decision log. No application code was changed. The document is awaiting product-owner review. Next action: the product owner decides the customer-merge policy before the data model is approved.

## Completion rule

A handover is not complete if it only says “done,” “continue,” or “the AI knows.” The repository/task record must contain enough context, evidence, and next steps for another authorized person or AI tool to continue independently.

---



<a id="document-14"></a>

# Document 14: `docs/TEAM-ASSIGNMENT-MATRIX.md`

> Source path: [`docs/TEAM-ASSIGNMENT-MATRIX.md`](../TEAM-ASSIGNMENT-MATRIX.md)

---

# ZetuBIZ 15-Member Team Assignment and Ownership Matrix

**Status:** Proposed for product-owner review  
**Project stage:** Documentation and specification only  
**Implementation gate:** Application implementation remains blocked until the product owner records approval of the documentation gate.  
**Assignment method:** Numbered team slots are placeholders. Replace them with actual member names after confirming skills, availability, and conflicts of interest.

## 1. Assignment principles

1. Every work item has one accountable owner and at least one reviewer.
2. Owners are responsible for producing the deliverable and keeping its status current; reviewers independently check quality and consistency.
3. No contributor approves their own work.
4. For authentication, payments, financial records, permissions, payroll, and tenant isolation, include a reviewer with relevant security or domain competence.
5. A member may support another workstream, but their primary ownership and capacity must be visible.
6. AI tools are optional and interchangeable. The approved repository documentation is the source of truth.
7. All work must use the [Task Handover Template](TASK-HANDOVER-TEMPLATE.md).
8. No member should be assigned by name until the product owner confirms the actual team roster.

## 2. Proposed allocation of all 15 team slots

| Slot | Primary role | Accountable area | Initial deliverables | Primary reviewer |
|---|---|---|---|---|
| T01 | Product owner / product lead | Product direction and approval | Target segment, success measures, Release 1 scope, decision ownership, documentation gate approval | Independent team reviewer for clarity; product owner retains final approval |
| T02 | Product analyst | Customer discovery and requirements | Interview plan, user problems, personas, requirement IDs, measurable outcomes | T01 |
| T03 | UX and information architecture lead | Onboarding and navigation | Onboarding questions and branches, sitemap, role-aware navigation, workspace setup states | T02 |
| T04 | UI design and accessibility lead | Design system and responsive UX | Tokens, typography, components, responsive rules, keyboard and contrast criteria | T03 |
| T05 | Solution architect | System boundaries and architecture | Architecture overview, module boundaries, deployment options, ADR proposals | T01 + T06 |
| T06 | Security and tenancy lead | Threat model and tenant boundaries | Threat model, authorization principles, tenant isolation requirements, security review checklist | T05; T01 for scope decisions |
| T07 | Data model lead | ERD and database integrity | ERD, reviewed data dictionary, tenant-scoped relationships, deletion and retention rules | T06 + T05 |
| T08 | API and integration lead | API contracts and provider behaviour | API contracts, error model, webhook verification, idempotency, retry and timeout rules | T05 + T06 |
| T09 | Finance domain lead | Quotations, invoices, receipts, credit notes | Financial workflows, numbering, immutable snapshots, rounding and correction rules | T02 + T07 |
| T10 | Payments and reconciliation lead | Payment lifecycle | Payment states, allocations, reconciliation, provider capability verification, exception handling | T08 + T09 + T06 |
| T11 | Inventory and operations lead | Products, stock, purchasing, POS | Inventory workflows, stock movements, purchasing and stock-count requirements | T02 + T07 |
| T12 | HR and reporting lead | Employee workflows and reports | HR requirements, leave/attendance boundaries, report definitions, compliance questions | T02 + T07; qualified compliance review where needed |
| T13 | QA and traceability lead | Test strategy and acceptance evidence | Requirement-to-test map, test levels, negative security tests, release acceptance criteria | T06 + domain owners |
| T14 | Operations and reliability lead | Deployment, monitoring, recovery, support | Environment separation, secrets handling, backups, recovery drills, monitoring and incident procedures | T05 + T06 |
| T15 | Documentation integration and customer feedback coordinator | Cross-document consistency and feedback loop | Link checks, terminology consistency, contradiction register, review notes, customer feedback tracking | T01 + relevant domain reviewer |

**Capacity note:** This is a starting allocation, not a claim that every person has the same skill or available hours. The product owner should adjust roles after a short skills and availability check. T01 is accountable for approval, but must not self-review their own authored technical or product documents.

## 3. Initial documentation work packages

Work packages are sequenced to reduce rework. Parallel work is allowed only where dependencies are clear.

| ID | Deliverable | Owner | Required reviewers | Depends on | Acceptance evidence |
|---|---|---|---|---|---|
| D0-01 | Team roster, skills, availability, primary assignments | T01 | T15 | None | All 15 slots mapped to real members; capacity and backup owner recorded |
| D0-02 | Target segment, customer problem, success measures | T02 | T01 | D0-01 | Evidence and assumptions separated; measurable outcomes agreed |
| D0-03 | Release 1 scope and exclusions | T01 | T02, T05, T09, T10, T13 | D0-02 | In-scope, out-of-scope, dependencies and deferrals recorded |
| D0-04 | Requirements catalogue with stable IDs | T02 | T09, T10, T11, T12, T13 | D0-02 | Each requirement has priority, actor, outcome, acceptance criteria and status |
| D0-05 | Information architecture and onboarding specification | T03 | T02, T04 | D0-03 | Screen inventory, conditional questions, save/resume, failure and permission states |
| D0-06 | Design system and accessibility requirements | T04 | T03, T13 | D0-05 | Tokens and component rules documented; keyboard, contrast and responsive criteria specified |
| D0-07 | Architecture and ADR proposals | T05 | T06, T08, T14 | D0-03 | Decisions, alternatives, consequences, unresolved risks and verification sources recorded |
| D0-08 | Threat model and security requirements | T06 | T05, T07, T08, T10, T13 | D0-03 | Trust boundaries, abuse cases, mitigations and negative tests documented |
| D0-09 | ERD and data dictionary review | T07 | T05, T06, T09, T10, T11, T12 | D0-04, D0-08 | Relationships, tenant keys, constraints, indexes, retention and financial integrity reconciled |
| D0-10 | API and integration contracts | T08 | T05, T06, T10, T14 | D0-07, D0-08 | Request/response shapes, authorization, errors, idempotency, retries and webhook verification defined |
| D0-11 | Finance domain workflows | T09 | T02, T07, T10, T13 | D0-04, D0-09 | Draft/final states, numbering, calculations, snapshots, corrections and acceptance cases specified |
| D0-12 | Payment and reconciliation workflows | T10 | T06, T08, T09, T13 | D0-10, D0-11 | Provider assumptions verified; payment allocation, duplicates, reversals and unmatched events addressed |
| D0-13 | Inventory and operations requirements | T11 | T02, T07, T13 | D0-04, D0-09 | Stock movement rules, adjustment authority, purchasing and inventory acceptance cases documented |
| D0-14 | HR and reporting requirements | T12 | T02, T07, T13 | D0-04, D0-09 | Metric definitions and privacy boundaries documented; legal/compliance claims flagged for verification |
| D0-15 | Test traceability and release acceptance | T13 | T06, T09, T10, T11, T12, T14 | D0-04, D0-08 through D0-14 | Critical requirements map to observable tests; security and failure cases included |
| D0-16 | Operations and recovery specification | T14 | T05, T06, T08, T13 | D0-07, D0-08 | Environments, secret handling, backup/restore, monitoring, incident response and rollback documented |
| D0-17 | Cross-document consistency review | T15 | All workstream owners | D0-05 through D0-16 | Broken links, conflicting terms, duplicate rules and contradictory status claims logged and resolved |
| D0-18 | Documentation gate review and decision record | T01 | All leads; T15 records review | D0-01 through D0-17 | Approved scope/version, exclusions, unresolved deferred decisions, approver and date recorded |

The IDs above are planning IDs. Link them to GitHub issues or the team's chosen tracker before work begins; do not imply that issues already exist.

## 4. Review and collaboration rules

- Every work package has a single owner. Contributors can assist, but the owner maintains the final document and resolves review feedback.
- Reviewers should comment on requirement correctness, cross-document consistency, security/privacy, testability, and scope. They must not approve solely because the document was AI-generated.
- For a sensitive area, the domain lead and security lead should both review where applicable.
- If a decision is blocked, change the task status to **Needs decision** and record the question, options, impact, and decision owner.
- If two contributors need the same file, agree on section ownership or submit separate changes and reconcile them in review.
- Do not mark a work package done until acceptance evidence and the handover are recorded.
- Any external provider capability or compliance claim must have an authoritative source and a last-verified date.

## 5. Weekly team update format

Every active contributor posts the following update asynchronously:

- **Slot / owner:**
- **Task ID and status:**
- **Completed since last update:**
- **Next deliverable:**
- **Review needed from:**
- **Blocker or decision required:**
- **Risks/dependencies:**
- **Links to evidence and handover:**

Updates should be factual and concise. “AI is working on it” is not a deliverable or status.

## 6. Owner handover and absence coverage

For each workstream, the owner names a backup reviewer or temporary handover recipient. If the owner becomes unavailable, the recipient uses the latest repository and task handover rather than relying on private messages. Transfer ownership in the tracker and record the transfer date.

## 7. Product-owner setup checklist

Before the team starts this allocation:

- [ ] Replace T01–T15 with the actual team members.
- [ ] Confirm each member's skills, availability, preferred work, and primary responsibility.
- [ ] Identify backup coverage for each critical area.
- [ ] Create or link the corresponding task records.
- [ ] Confirm reviewers have the knowledge and authority needed for their assigned reviews.
- [ ] Agree on check-in cadence and response expectations.
- [ ] Confirm the documentation-only boundary is understood by every member.
- [ ] Keep application implementation blocked until D0-18 is formally approved and recorded.

## 8. Definition of success

This matrix is successful when every deliverable has a real owner and reviewer, work is sequenced around documented dependencies, open decisions are visible, cross-document conflicts are resolved, and any authorized person or AI tool can continue from repository evidence without needing another contributor's conversation history.

---



<a id="document-15"></a>

# Document 15: `docs/TEAM-WORKFLOW.md`

> Source path: [`docs/TEAM-WORKFLOW.md`](../TEAM-WORKFLOW.md)

---

# ZetuBIZ Team Workflow and Delivery Handbook

**Purpose:** Define how the product owner, documentation contributors, designers, developers, QA, and operations collaborators work together without duplicated effort or unreviewed changes.  
**Project stage:** Documentation first. Implementation is blocked until the product owner explicitly approves the documentation gate.  
**Applies to:** All team members, contractors, coding agents, and AI tools.

## 1. Working agreements

1. One shared source of truth: requirements, decisions, tasks, status, and handovers live in this repository.
2. No work by assumption: unclear requirements become questions or documented proposals, not silent product decisions.
3. One owner per task: contributors may collaborate, but one person is accountable for delivery.
4. Small, reviewable changes: each pull request should have one clear purpose and a manageable diff.
5. Main branch stays protected: work in branches; merge only after review and required checks.
6. Documentation first: do not implement a feature until its requirements, UX flow, data impact, permission rules, error states, tests, and acceptance criteria are approved.
7. Evidence over claims: report what changed, what was checked, what failed, and what remains.
8. Security and privacy are everyone’s responsibility. Raise sensitive issues privately to the project owner/security lead; do not publish exploit details unnecessarily.
9. No secrets, real customer data, or production credentials in issues, screenshots, test fixtures, commits, or AI prompts.
10. Respectful collaboration: critique the work and evidence, not the person. Raise blockers early.

## 2. Suggested team structure

The product owner may adjust assignments based on actual skills and availability. Roles below describe responsibilities, not fixed job titles.

- **Product owner (1):** Sets priorities, approves scope, resolves product trade-offs, accepts releases, and authorizes the documentation gate.
- **Product/business analysis (2):** Customer research, user journeys, requirements, terminology, acceptance criteria, and open-decision tracking.
- **UX/UI and accessibility (2):** Information architecture, onboarding questions, wireframes, screen specifications, design tokens, responsive states, accessibility.
- **Architecture and security (2):** Architecture decisions, integration boundaries, tenancy model, threat model, data access rules, technical risk review.
- **Domain/documentation leads (3):** Own cross-module requirements, data dictionary, workflow documentation, and consistency across the specification set.
- **Engineering leads/contributors (3):** Review technical feasibility, API contracts, migration approach, coding standards, and later implementation plans. Do not start implementation before approval.
- **QA/reliability (1):** Test strategy, traceability, acceptance scenarios, regression approach, accessibility checks, release gates.
- **Operations/release and customer feedback (1):** Deployment/recovery plan, support workflows, observability, user research logistics, and feedback tracking.

One person may hold multiple responsibilities. The product owner must name actual task owners in the task tracker; do not infer names from this role map. Security-sensitive decisions require a designated reviewer even when the team is small.

## 3. Workstreams and ownership

Create a work item for each deliverable with: ID, title, problem, scope, owner, reviewers, dependencies, priority, status, acceptance criteria, evidence, and due date if genuinely agreed.

Recommended workstreams:

1. Product and customer discovery.
2. UX, onboarding, navigation, and design system.
3. Domain requirements and end-to-end workflows.
4. Architecture and architecture decision records (ADRs).
5. Data model, permissions, retention, and migrations.
6. Integrations and provider capability verification.
7. Security, privacy, and compliance review.
8. QA, accessibility, performance, and acceptance criteria.
9. Operations, deployment, backup, recovery, and support.
10. Documentation quality, consistency, and release readiness.

Every document has one maintainer and at least one reviewer. Avoid multiple people editing the same file simultaneously. If work overlaps, agree on section ownership or submit separate branches and reconcile in review.

## 4. Task lifecycle

Use these statuses consistently:

- **Backlog:** Captured but not yet prioritized.
- **Ready for documentation:** Problem and intended outcome are understood; owner assigned.
- **In progress:** Owner is actively working and has stated scope.
- **Needs decision:** Blocked on a product, architecture, security, or compliance decision.
- **In review:** Deliverable is ready and acceptance criteria/evidence are supplied.
- **Changes requested:** Reviewer identified concrete gaps.
- **Approved:** Required reviewers accepted the deliverable.
- **Blocked:** External dependency or unresolved issue prevents progress; blocker and next action are recorded.
- **Done:** Acceptance criteria are met, links/evidence are present, related documents are updated, and required reviewers approved.
- **Deferred:** Product owner explicitly moved it out of current scope with a reason.

“Done” must not mean “code generated,” “page exists,” or “document drafted.” It means the agreed acceptance criteria have been verified.

## 5. GitHub workflow

### Before work
1. Pull the latest default branch.
2. Read `AGENTS.md`, this handbook, the master specification, relevant domain docs, current status, and handover.
3. Check existing issues/PRs and the file history to avoid duplicate work.
4. Create or claim a task and state the intended deliverable.
5. Create a focused branch, for example:
   - `docs/ux-onboarding-spec`
   - `docs/team-workflow`
   - `docs/data-permissions-matrix`
   - Later, after approval: `feat/customer-management` or `fix/invoice-rounding`

### While working
1. Keep changes limited to the assigned scope.
2. Do not mix formatting-only changes with major content changes unless required.
3. Do not overwrite another contributor’s unmerged work.
4. Update related links and version/status metadata.
5. For a product decision, propose options and consequences; do not bury the decision in code.
6. For uncertain provider or compliance behavior, record the official source, verification date, and remaining uncertainty.

### Pull request requirements
Every PR must include:
- Purpose and problem being solved.
- Scope and explicit out-of-scope items.
- Files changed and why.
- Related task/issue.
- Acceptance criteria checklist.
- Tests/checks run and actual results.
- Screenshots or flow evidence for UX changes when useful.
- Security, tenant-isolation, data migration, privacy, and accessibility impact.
- Risks, limitations, and rollback/recovery plan when relevant.
- Follow-up tasks and documentation links.

Reviewers should check correctness, consistency with approved requirements, security/privacy, scope creep, maintainability, test evidence, and whether the status claims are truthful. Request changes when requirements are unclear or important evidence is missing.

### Merge rules
- No direct pushes to the protected default branch for normal work.
- No self-approval for high-risk financial, auth, permissions, payment, payroll, or tenant-isolation changes.
- Require at least one independent review; require a security/domain reviewer for sensitive changes.
- Required automated checks must pass once CI exists.
- Resolve review comments or record why a comment is not being applied.
- Squash or merge according to the repository’s approved convention and use a descriptive commit/PR title.
- Do not merge known critical/high-severity security issues.
- If GitHub branch protection is not configured, the team must still follow these rules manually until protection can be enabled.

## 6. Meetings and asynchronous communication

Keep meetings short and decision-focused. Suggested cadence:

- **Weekly planning (30 minutes):** Agree top outcomes, owners, dependencies, and what will not be done.
- **Async daily update:** Each active contributor posts: completed, next, blocked, review needed.
- **Weekly documentation review (30–45 minutes):** Check cross-document consistency, open decisions, scope conflicts, and readiness.
- **Release/readiness review:** Only when a milestone is near approval.

Meeting notes must capture decisions, owner, due date if agreed, and affected docs. A chat message is not the authoritative record until the decision is copied into the relevant document or decision log.

Escalate blockers as soon as they threaten another person’s work. Do not wait for the next meeting.

## 7. Decision-making and conflict resolution

Use `docs/decisions/` for Architecture Decision Records (ADRs) and significant product decisions. Each record contains:
- ID, title, date, status, decision owner.
- Context/problem and constraints.
- Options considered.
- Decision and rationale.
- Security, cost, UX, operations, and migration implications.
- Consequences and revisit trigger.

For a disagreement:
1. State the decision to be made and the evidence.
2. Separate confirmed facts from assumptions.
3. Compare no more than a few viable options.
4. Ask the accountable owner to decide.
5. Record the decision and notify affected contributors.

Security or compliance concerns cannot be overruled merely to meet a date. Escalate for qualified review.

## 8. Documentation-first approval gate

Implementation remains blocked until all of the following are reviewed and accepted by the product owner:

- [ ] Product vision, target segment, problem statement, and success measures.
- [ ] Release 1 scope and explicit exclusions.
- [ ] Onboarding questions, conditional logic, workspace preparation, and recovery from interrupted setup.
- [ ] Full navigation map, screen inventory, and role-specific experiences.
- [ ] UX specification for each Release 1 screen, including loading, empty, error, success, and permission-denied states.
- [ ] Design system: typography, color tokens, spacing, components, responsive behavior, and accessibility.
- [ ] Domain workflows for CRM, quotations, invoices, payments, expenses, notifications, and reports.
- [ ] Role/permission matrix with tenant boundaries and privileged actions.
- [ ] Reviewed data dictionary and ERD, including relationships, uniqueness, deletion behavior, indexes, and audit history.
- [ ] Architecture decision records for frontend build, API/runtime, authentication, database/RLS, file storage, PDF generation, email, payment provider, jobs, and deployment.
- [ ] Threat model, privacy/retention rules, backup/recovery, monitoring, and incident handling.
- [ ] API contracts and integration failure/retry/idempotency behavior.
- [ ] Test strategy and acceptance criteria mapped to requirements.
- [ ] Team task breakdown, owners, dependencies, review rules, and initial milestone estimates.
- [ ] All open decisions are either resolved or explicitly approved as safe to defer.
- [ ] Contradictions across README, master spec, architecture, database, status, tasks, and handover are reconciled.

Approval must be recorded in the repository with date, approver, approved scope/version, and any exclusions. A chat acknowledgement alone should not be treated as a durable release approval.

## 9. Documentation quality and consistency

- Use stable terms for business/workspace, customer/client, supplier, quotation, invoice, payment, receipt, and employee.
- Mark requirements as **Proposed**, **Approved**, **In progress**, **Implemented**, **Verified**, or **Released**.
- Every requirement should have a unique ID once traceability is introduced.
- Link UX requirements to domain rules, data fields, permissions, and tests.
- Do not put secrets, personal customer information, or live credentials in docs.
- Use relative links within the repository.
- Update the master specification when a product decision changes.
- Update the implementation status only with verifiable evidence.
- Record legal/compliance claims with official source and last-verified date.

## 10. Team assignment and ownership matrix

Use [TEAM-ASSIGNMENT-MATRIX.md](TEAM-ASSIGNMENT-MATRIX.md) as the proposed role allocation for the 15-person team. It uses numbered slots until actual names, skills, and availability are confirmed. The matrix defines accountable owners, reviewers, dependencies, and acceptance evidence for documentation work packages. The product owner must map the slots to real members and update the task tracker before work is assigned. Do not infer individual identities or treat this proposed allocation as confirmed staffing.

## 11. Handover template

Every handover must include:
- Current branch and latest commit/PR.
- Work completed and links to changed files.
- Requirements/tasks approved or still under review.
- Checks/tests run and exact results.
- Decisions made and where they are recorded.
- Known defects, risks, and unresolved questions.
- Dependencies and who owns the next action.
- Exact next task and its acceptance criteria.
- Confirmation that secrets and customer data were not added.

## 12. Definition of done

A task is done only when:
- Scope and acceptance criteria are met.
- An independent reviewer approved it.
- Relevant documentation, status, and links are updated.
- Tests/checks were run where applicable and results are truthful.
- Security, privacy, accessibility, and tenant impact were considered.
- No untracked temporary workaround or unresolved critical blocker remains.
- The next contributor can continue without guessing.

## 13. Immediate team actions

1. Map the proposed slots in `TEAM-ASSIGNMENT-MATRIX.md` to named members after confirming skills and availability.
2. Review the current repository and mark contradictions instead of assuming existing docs are correct.
3. Complete the missing UX, permission, data dictionary, ADR, compliance, operations, and test traceability documents.
4. Create a prioritized documentation backlog with dependencies.
5. Run the documentation gate review with the product owner.
6. Keep implementation paused until explicit approval is recorded.


## AI model and development-tool independence

ZetuBIZ is AI-tool agnostic. Contributors may use any authorized AI model, coding assistant, IDE, terminal agent, local model, or no AI, provided they follow the same approved requirements, security rules, and review process.

AI tools are assistants, not sources of truth. The repository's approved specifications and decisions take precedence. Every agent must inspect the current repository, read AGENTS.md and the relevant documentation, and work only within the assigned task. Do not rely on hidden prompts, model memory, or prior chat history.

Do not provide AI tools with production secrets, credentials, tokens, or unnecessary personal/customer data. Verify generated work, test claims, and tool output. Until the product owner records approval of the documentation gate, agents may review and improve documentation but must not start application implementation or deploy features.

Every task handover must use [Task Handover Template](TASK-HANDOVER-TEMPLATE.md). Record files changed, decisions, checks and exact results, risks, unresolved questions, dependencies, and a concrete next action. A task must be transferable to another person or AI tool without requiring access to the original conversation.

---



<a id="document-16"></a>

# Document 16: `docs/TEST-TRACEABILITY.md`

> Source path: [`docs/TEST-TRACEABILITY.md`](../TEST-TRACEABILITY.md)

---

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

---



<a id="document-17"></a>

# Document 17: `docs/TESTING.md`

> Source path: [`docs/TESTING.md`](../TESTING.md)

---

# Testing Strategy

This document describes the testing approach, commands, coverage requirements, and test data for ZetuBIZ.

## Testing Philosophy

- Test critical paths end-to-end
- Test financial calculations with unit tests
- Test tenant isolation with integration tests
- Test payment integrations with sandbox environments
- Never use live payments in automated tests
- All features must have passing tests before completion

## Test Types

### Unit Tests

**Purpose:** Test individual functions and modules in isolation

**What to Test:**
- Financial calculations (invoice totals, tax calculations, discounts)
- Status transition logic
- Data validation functions
- Utility functions
- Payment provider adapters (with test mocks)

**Tools:** Jest or similar (to be determined)

**Run:**
```bash
npm run test:unit
```

### Integration Tests

**Purpose:** Test module interactions and database operations

**What to Test:**
- Database CRUD operations
- Authentication flows
- Authorization checks
- API endpoint responses
- RLS policy enforcement
- Tenant isolation

**Tools:** Jest with Supabase test database

**Run:**
```bash
npm run test:integration
```

### End-to-End Tests

**Purpose:** Test complete user workflows

**What to Test:**
- User registration and onboarding
- Quotation creation to PDF download
- Quotation acceptance and invoice conversion
- Invoice creation and payment initiation
- Payment webhook processing
- Secure document sharing
- Email sending

**Tools:** Playwright or Cypress (to be determined)

**Run:**
```bash
npm run test:e2e
```

## Test Commands

### Run All Tests

```bash
npm test
```

### Run Unit Tests Only

```bash
npm run test:unit
```

### Run Integration Tests Only

```bash
npm run test:integration
```

### Run E2E Tests Only

```bash
npm run test:e2e
```

### Run Tests in Watch Mode

```bash
npm test -- --watch
```

### Run Tests with Coverage

```bash
npm test -- --coverage
```

### Run Specific Test File

```bash
npm test path/to/test.spec.js
```

## Test Coverage Requirements

### Minimum Coverage Targets

- **Overall:** 80%
- **Critical paths (financial calculations, auth):** 95%
- **API endpoints:** 85%
- **Database operations:** 90%

### Coverage Report

```bash
npm test -- --coverage
```

Coverage report generated in `coverage/` directory.

## Test Data

### Test Fixtures

Test fixtures are clearly marked and isolated from production data.

**Location:** `tests/fixtures/`

**Example fixtures:**
- Test users
- Test businesses
- Test clients
- Test quotations
- Test invoices
- Test payments

### Test Database

Use a separate Supabase project for testing:

```bash
# In .env.test
SUPABASE_URL=https://your-test-project.supabase.co
SUPABASE_ANON_KEY=your-test-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-test-service-role-key
```

Reset test database before each test run:

```bash
supabase db reset --db-url $TEST_DATABASE_URL
```

### Safe Test Accounts

**Payment Providers:**

**Paystack Test:**
- Use test mode keys
- Use test cards from Paystack documentation
- Example test card: `4084 0640 0000 0000` (success)
- Example test card: `4084 0640 0000 0001` (fail)

**M-Pesa Sandbox:**
- Use sandbox credentials
- Use sandbox shortcodes
- Test with small amounts (KES 1-10)

**Email Providers:**
- Use test mode if available
- Send to test email addresses only
- Example: `test@yourdomain.com`

## Critical Test Scenarios

### 1. Financial Calculations

**Test Cases:**
- Line item subtotal calculation
- Discount application (percentage and fixed)
- Tax calculation
- Rounding behavior
- Multi-line item totals
- Currency conversion (if implemented)

**Example:**
```javascript
test('calculates invoice total correctly', () => {
  const lineItems = [
    { quantity: 2, unitPrice: 100 },
    { quantity: 1, unitPrice: 50 }
  ];
  const result = calculateTotal(lineItems, 0.1, 0.16); // 10% discount, 16% tax
  expect(result.total).toBe(270); // (250 - 25) + 36
});
```

### 2. Tenant Isolation

**Test Cases:**
- User cannot access another business's clients
- User cannot access another business's invoices
- RLS policies enforced correctly
- Cross-tenant API calls blocked

**Example:**
```javascript
test('user cannot access another business data', async () => {
  const user1 = await createTestUser();
  const business1 = await createTestBusiness(user1.id);
  const user2 = await createTestUser();
  const business2 = await createTestBusiness(user2.id);
  
  const clients = await getClients(user1, business2.id);
  expect(clients).toEqual([]);
});
```

### 3. Authentication and Authorization

**Test Cases:**
- User can register and login
- Invalid credentials rejected
- Session tokens validated
- Permissions enforced on API endpoints
- Role-based access control

### 4. Payment Integration

**Test Cases:**
- Payment initiation with test provider
- Webhook signature verification
- Idempotency key prevents duplicate charges
- Payment status updates correctly
- Failed payments handled gracefully

**Important:** Use provider sandbox/test mode only.

### 5. Quotation to Invoice Flow

**Test Cases:**
- Quotation creation saves correctly
- Quotation can be finalized
- Quotation acceptance recorded
- Accepted quotation converts to invoice
- Line items preserved during conversion
- Invoice numbering unique

### 6. PDF Generation

**Test Cases:**
- PDF generates successfully
- PDF matches preview
- PDF includes all required elements
- Multi-page PDFs render correctly
- PDF is downloadable

### 7. Secure Document Sharing

**Test Cases:**
- Public token is high-entropy
- Public link works for valid token
- Invalid token rejected
- Token scoped to correct business
- Token can be revoked

### 8. Email Sending

**Test Cases:**
- Email composed correctly
- Template variables replaced
- Email sent successfully
- Delivery tracked
- Failed sends retried

## Known Test Limitations

### Payment Provider Tests

**Limitation:** Cannot test actual money movement in automated tests

**Mitigation:**
- Use provider sandbox environments
- Use test adapters that simulate provider responses
- Manual testing with small amounts in sandbox

### Email Delivery Tests

**Limitation:** Cannot verify actual email delivery in automated tests

**Mitigation:**
- Use email provider test mode
- Verify API call succeeds
- Manual verification of test emails

### PDF Rendering Tests

**Limitation:** Visual PDF quality requires manual review

**Mitigation:**
- Automated tests verify PDF generation succeeds
- Automated tests verify PDF contains required text
- Manual review of PDF layout and quality

### Cross-Browser Tests

**Limitation:** Automated tests may not cover all browsers

**Mitigation:**
- Test in Chrome, Firefox, Safari, Edge
- Use browser testing service (BrowserStack, etc.)
- Manual testing for critical flows

## Test Organization

### Directory Structure

```
tests/
├── unit/
│   ├── calculations.spec.js
│   ├── validation.spec.js
│   └── utils.spec.js
├── integration/
│   ├── auth.spec.js
│   ├── database.spec.js
│   └── api.spec.js
├── e2e/
│   ├── onboarding.spec.js
│   ├── quotations.spec.js
│   ├── invoices.spec.js
│   └── payments.spec.js
├── fixtures/
│   ├── users.json
│   ├── businesses.json
│   └── clients.json
└── helpers/
    ├── auth.js
    ├── database.js
    └── api.js
```

## Continuous Integration

### GitHub Actions (Example)

```yaml
name: Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm install
      - run: npm test
      - run: npm run test:coverage
```

## Test Reporting

### Coverage Reports

Generate coverage report:
```bash
npm test -- --coverage
```

Upload to coverage service (Codecov, Coveralls, etc.) if configured.

### Test Results

Test results output to console and JUnit XML format for CI integration.

## Manual Testing Checklist

Before releasing a feature:

- [ ] Feature works in development environment
- [ ] Feature works in staging environment
- [ ] Mobile layout tested on actual devices
- [ ] PDF quality reviewed manually
- [ ] Email delivery verified manually
- [ ] Payment flow tested in sandbox
- [ ] Accessibility tested with screen reader
- [ ] Performance acceptable
- [ ] Error states tested
- [ ] Security review completed

## Troubleshooting Tests

### Tests Failing Due to Database State

**Problem:** Tests fail because of dirty database state

**Solution:**
```bash
supabase db reset --db-url $TEST_DATABASE_URL
```

### Tests Failing Due to Environment Variables

**Problem:** Tests fail because environment variables not set

**Solution:**
```bash
cp .env.example .env.test
# Edit .env.test with test values
export NODE_ENV=test
npm test
```

### Flaky Tests

**Problem:** Tests pass sometimes, fail sometimes

**Solutions:**
- Add retries for network-dependent tests
- Use deterministic test data
- Fix race conditions
- Increase timeouts for slow operations

## Test Maintenance

### When to Update Tests

- When adding new features
- When fixing bugs
- When changing API contracts
- When modifying database schema
- When updating dependencies

### Test Debt

Track test debt in `docs/IMPLEMENTATION-STATUS.md`:
- Features without tests
- Tests with known issues
- Tests needing refactoring

---

**Status:** Testing strategy documented  
**Last updated:** October 20, 2026

---



<a id="document-18"></a>

# Document 18: `docs/UX-SPECIFICATION.md`

> Source path: [`docs/UX-SPECIFICATION.md`](../UX-SPECIFICATION.md)

---

# ZetuBIZ Screen-by-Screen UX Specification

**Status:** Proposed — requires product-owner and design review  
**Scope:** Product-wide navigation and Release 1 screens; later modules are mapped as planned areas, not implemented features.

## 1. UX outcomes

- Help a new business owner reach a useful workspace without answering irrelevant questions.
- Let users configure who the business is, what it does, how many people work there, and which workflows matter.
- Personalize navigation and dashboard recommendations from onboarding choices without hiding security controls.
- Make quotations, invoices, receipts, expenses, and customer follow-up fast on mobile and desktop.
- Clearly distinguish draft, save, finalize, send, publish, approve, and delete.
- Never show fabricated financial metrics or pretend an action succeeded when it did not.

## 2. Global application shell

### Desktop
- Left navigation with workspace switcher, pinned primary modules, grouped secondary modules, settings, help, and account menu.
- Main content area with page title, concise description where needed, breadcrumbs for deep flows, and one primary action.
- Top-level search for authorized records; results must be tenant- and permission-scoped.
- Notification center with unread state and preferences.
- Responsive layout should work at laptop widths without horizontal overflow.

### Mobile
- Compact top bar with workspace identity and contextual actions.
- Bottom navigation for the most frequently used destinations selected by the workspace configuration; a More menu contains secondary areas.
- Tables transform into readable row summaries or controlled horizontal scrolling, never tiny unreadable text.
- Sticky primary action only when it does not obscure content or keyboard input.
- Forms support one-handed use, large touch targets, clear validation, and preserving entered values after recoverable errors.

### Shared page states
Every data-driven page specifies:
- Loading/skeleton state.
- First-use empty state with a useful next action.
- Search/filter no-results state.
- Validation errors beside the relevant field and a summary for long forms.
- Permission-denied state without leaking record details.
- Network/server error with retry where safe.
- Success confirmation based on server response.
- Conflict/stale-data state where another teammate changed the record.
- Destructive or irreversible confirmation where applicable.

## 3. Onboarding: 10-step adaptive setup

Users must be able to save progress, go back, skip optional questions, and resume after signing in again. Do not create the final workspace or enable paid integrations until the relevant step is confirmed. Required fields must be clearly identified.

### Step 1 — Welcome and purpose
**Prompt:** “What would you like ZetuBIZ to help you manage first?”  
Choices: Customers and sales; quotations and invoices; expenses and payments; projects and tasks; products and stock; employees and HR; several areas / not sure.  
**Behavior:** Sets initial dashboard emphasis and onboarding hints. It does not grant permissions or permanently lock modules.

### Step 2 — Business type
**Prompt:** “Which best describes your business?”  
Choices: Freelancer/independent professional; agency or service company; retail/wholesale; consulting/professional services; contractor/project-based business; other.  
**Behavior:** Recommends relevant terminology, sample template styles, and initial module order. Do not create fake records.

### Step 3 — Business identity
**Fields:** Public business name (required), legal name (optional), business email, phone, website, country, and optional registration/tax identifiers.  
**Behavior:** Explain which details appear on documents. Validate email/phone formats without assuming every phone number is Kenyan.

### Step 4 — Location and financial defaults
**Fields:** Country, default currency, timezone, date format, fiscal year start, and default language. Default to Kenya, KES, Africa/Nairobi, DD/MM/YYYY, and English for a new Kenya-based workspace, but allow changes.  
**Behavior:** Show a clear summary of defaults. Currency/timezone changes must not silently rewrite historic records.

### Step 5 — Team size and working arrangement
**Prompt:** “Who will use this workspace?”  
Choices: Just me; 2–5 people; 6–15 people; 16–50 people; more than 50.  
**Follow-up:** “Would you like to invite teammates now?” Yes / later.  
**Behavior:** Team size is a planning hint, not a subscription entitlement or security role. If inviting now, collect name/email and proposed role, explain permissions, and send invitations only after confirmation. Pending invites can be revoked.

### Step 6 — What you sell or deliver
**Prompt:** “What do you usually sell or deliver?”  
Choices: Services/time; projects/milestones; products/stock; recurring services; mixed. Optional description and common line-item units.  
**Behavior:** Helps configure quotation/invoice fields and suggests modules. It must not automatically create real products or prices.

### Step 7 — Priority workflows
**Prompt:** “Which tasks should be easiest to manage every week?”  
Multi-select: Follow up customers; send quotations; invoice customers; track M-Pesa/online payments; record expenses; manage projects/tasks; track stock/suppliers; manage staff/leave; review reports.  
**Behavior:** Drives dashboard shortcuts and navigation ordering. Users can change these choices later.

### Step 8 — Current process and migration
**Prompt:** “Where is your business information today?”  
Choices: Paper; spreadsheets; another app; several tools; starting fresh.  
**Follow-up:** “Would you like help importing existing records?” Yes / no / decide later.  
**Behavior:** If import is selected, explain supported formats, preview, duplicate handling, and privacy. Do not upload or import until the user selects a file and confirms the preview.

### Step 9 — Branding and document preferences
**Fields:** Logo upload (optional), brand color selection, preferred document style, quotation/invoice numbering preference, default payment instructions, and document footer.  
**Template choices:** Modern Minimal; Professional Corporate; Clean Freelancer; Creative Studio; Software Development Proposal; Agency Services; Consulting Services; Product/Retail Sales; Milestone-Based Project; Simple Tax and Payment Summary.  
**Behavior:** Show real preview using clearly labeled placeholders, not fake saved customers or transactions. Allow template choice to be changed later. Validate image type/size and store private uploads securely.

### Step 10 — Review and create workspace
**Summary:** Business identity, location/currency/timezone, business type, team size, selected workflows, and document style.  
**Actions:** Back to edit; Create workspace.  
**Behavior:** Creating the workspace is a consequential action and requires an explicit click. Show terms/privacy notices and explain who will have access. On success, redirect to the real workspace preparation screen.

## 4. Workspace preparation screen

Show progress based on actual server-side setup state, not an animated timer or a hardcoded 100%.

Suggested steps:
1. Validate workspace details.
2. Create workspace record.
3. Add creator as owner.
4. Save preferences and enabled modules.
5. Save document branding/template preferences.
6. Create default numbering/settings records if required.
7. Configure onboarding and notification preferences.
8. Confirm access policies and initial membership.
9. Prepare the personalized dashboard.
10. Verify workspace is ready.

For each step, store a durable status such as pending, running, completed, retryable failure, or requires support. Make retries idempotent. If a step fails, explain what failed, preserve completed work, offer a safe retry, and provide a support reference. Never say “workspace ready” until the required steps succeed.

## 5. Dashboard personalization

Dashboard cards must be based on real authorized records and selected priorities. If there are no records, show onboarding actions, not fabricated totals.

Potential sections:
- Welcome with business name and a dismissible setup checklist.
- Quick actions: Add customer, Create quotation, Create invoice, Record expense.
- Sales: open quotations, acceptance status, follow-ups due.
- Finance: outstanding invoices, verified collections, expenses for a selected period.
- Work: active projects/tasks if enabled.
- Stock alerts if inventory is enabled and configured.
- Team: pending invitations and approvals if the user has permission.
- Recent activity with actor and timestamp.
- Reports shortcut with a clear date range.

Users may reorder or hide noncritical widgets. Required security notices, overdue setup warnings, and permission restrictions cannot be hidden in a way that misleads the user. Each metric must define its source, date range, currency, and calculation.

## 6. Release 1 screen inventory

### Authentication and account
- Sign up; email verification; sign in; password recovery; reset password; optional MFA; session/device management; profile; account deletion/export request.
- Errors must not reveal whether another person's email is registered beyond the auth provider's safe behavior.

### Workspace and team
- Workspace switcher; workspace creation; business profile; settings; branding; modules/preferences; member list; invite teammate; invitation acceptance; role/permission view; revoke invite; deactivate member; ownership transfer request.
- Show the effect of each role before invite confirmation.

### CRM
- Customer list with search, filters, pagination, import/export permissions.
- Create/edit customer form with type, name, contacts, billing details, tax fields, notes, archive state.
- Customer detail with profile, timeline, quotations, invoices, payments, balance, files, and tasks according to permissions.
- Lead list/pipeline, lead detail, assign owner, follow-up reminder, convert to customer, and record lost reason.

### Quotations
- List and filter by status/date/customer/owner.
- Create/edit quotation with customer, line items, quantity, unit, price, discount, tax treatment, validity, terms, notes, and template.
- Autosave/draft recovery with explicit save status.
- Preview before finalization; finalize/send confirmation.
- Send by email, download PDF, print, or create/revoke secure share link.
- Detail/activity history, revise, duplicate, expire, cancel, accept/decline result, convert to invoice/project.
- A sent quotation must preserve the exact sent version. Revisions create a new version.

### Invoices and receipts
- Invoice list with status, due date, customer, total, balance, and filters.
- Create from quotation or scratch; edit draft; preview; finalize; send.
- Invoice detail with payment history, allocations, activity, reminder actions, secure link, PDF, and receipt links.
- Record authorized manual payment with method, reference, amount, date, notes, and explicit confirmation.
- Payment initiation/status screen; verified webhook result; unmatched/uncertain payment review.
- Partial payments, overpayment, credit note, void/reversal flow, customer statement.
- Do not expose refund or payment-provider actions to users without the relevant permission.

### Document Studio
- Template gallery with category, preview, supported document types, and selection.
- Live preview with business branding and realistic placeholders labeled as preview data.
- Theme settings for typography, logo, color, spacing, header/footer, terms, payment instructions, and visibility of fields.
- Actions: preview, save as default, duplicate template, restore default, export/download preview where appropriate.
- Template edits must not change already-finalized document snapshots.

### Expenses and reports
- Expense list; create/edit draft; attach receipt; submit for approval; approve/reject; mark reimbursed where configured.
- Expense categories/settings; recurring expense schedule; budget view.
- Reports overview with date range and filters; income/expense summary; receivables aging; customer statement; export confirmation.
- Every report displays filters, date range, currency, generation time, and access restrictions.

### Notifications and help
- Notification inbox; mark read/unread; preferences; email delivery state where useful; help/support form; case status.
- Avoid exposing sensitive details in push/email previews.

## 7. Future module screen inventory

These screens remain planned until separately scoped and approved:
- Inventory: product catalogue, variants, stock movements, locations, transfers, purchase orders, receiving, suppliers, POS shifts and returns.
- Projects: project list/detail, task board/list, milestones, assignments, time entries, approval, project profitability.
- HR: employee directory, profile, documents, leave calendar, requests/approvals, attendance and restricted payroll.
- Website Studio: page list/editor, preview, publish state, domain setup, form submissions, media library.
- AI: assistant conversation, data/source references, action preview, confirmation, execution result, history and privacy controls.
- Platform admin: tenant list, plans/entitlements, incidents, support cases, audit events, usage and controlled support access.

Each future module needs a separate screen specification and role-based access review before implementation.

## 8. Quotation/invoice send and download interaction

Before sending:
1. Validate required customer/contact and document fields.
2. Render a final preview.
3. Let the sender choose email recipient, subject/message, attachment or secure link, and optional reminder.
4. Show what will be sent and from which business identity.
5. Require confirmation.
6. Create a delivery job and return a truthful queued/sent/failed status.
7. Record the document version, recipient, actor, timestamp, provider result, and retry status.

For download/print:
- Generate from the finalized snapshot where applicable.
- Show progress for slow PDF generation and retry safely.
- Never return another tenant's document based on a guessed identifier.
- Secure public links must be high-entropy, scoped, revocable, optionally expiring, and rate-limited.
- Log sensitive link creation/revocation and document sends without recording the secret token itself.

## 9. Accessibility and interaction rules

- WCAG 2.2 AA is the target.
- Every input has a programmatic label, visible focus, keyboard support, and understandable validation.
- Do not rely on color alone to communicate status.
- Confirm before irreversible or high-impact actions.
- Respect reduced-motion preferences.
- Preserve draft content after recoverable errors.
- Use plain language and explain unfamiliar financial terms.
- Provide empty states and onboarding hints that can be dismissed or revisited.
- Avoid surprise navigation, fake progress, fake activity, or disabled controls without an explanation.

## 10. UX acceptance checklist

- [ ] All Release 1 screens are mapped to roles and requirements.
- [ ] Every screen has loading, empty, error, success, and permission-denied states as applicable.
- [ ] Onboarding questions, conditional logic, back/skip/resume, and save behavior are reviewed.
- [ ] Workspace preparation reflects real progress and supports safe retry.
- [ ] Dashboard personalization uses real data and configurable priorities.
- [ ] Document template preview, PDF, email, secure link, and download flows are specified.
- [ ] Mobile and desktop navigation are specified.
- [ ] Accessibility and responsive review is complete.
- [ ] Each primary action maps to an API/domain workflow and acceptance test.
- [ ] Product owner and UX reviewer approve the spec before implementation.

---
