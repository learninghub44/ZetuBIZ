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
