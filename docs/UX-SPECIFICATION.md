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
