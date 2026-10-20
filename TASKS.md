# Tasks

This document contains the prioritized documentation and implementation backlog for ZetuBIZ. **Documentation is the current work; implementation tasks below are blocked until the documentation approval gate is signed off.**

**Last updated:** October 20, 2026  
**Current milestone:** Documentation Gate — Product, UX, Architecture, Data, Security, Team and QA specifications

## Priority Levels

- **P0:** Critical - must complete for milestone
- **P1:** High - important for milestone
- **P2:** Medium - nice to have
- **P3:** Low - can defer

## Milestone D0 — Documentation and Product Readiness (CURRENT)

Implementation is paused. Complete and review the following documentation deliverables before starting app initialization, migrations, or feature code.

### P0 — Product scope and decisions
- [ ] Confirm target launch segment and validate with customer interviews
- [ ] Approve Release 1 scope and explicit exclusions
- [ ] Resolve onboarding and workspace setup questions
- [ ] Resolve subscription/entitlement assumptions
- [ ] Record product decisions in `docs/DECISIONS.md`

### P0 — UX and design specification
- [ ] Produce complete information architecture and role-aware navigation map
- [ ] Document each onboarding question, conditional logic, save/resume behavior, and completion state
- [ ] Document workspace preparation progress and failure/retry states
- [ ] Create screen-by-screen Release 1 UX specification
- [ ] Define responsive behavior and mobile navigation
- [ ] Define design tokens, typography, colors, components, interaction states, and accessibility rules
- [ ] Specify quotation/invoice templates, preview, download, print, email, and secure-link behavior

### P0 — Data, permissions, and architecture
- [ ] Correct and review the proposed database model against Supabase Auth
- [x] Draft conceptual data dictionary and relationship baseline
- [ ] Review data dictionary, produce/approve ERD, and reconcile against SQL examples
- [x] Draft module-level role/permission matrix
- [ ] Review and approve role/permission matrix
- [ ] Define tenant isolation and cross-tenant negative tests
- [ ] Resolve payment, PDF, email, background jobs, file storage, and deployment decisions in ADRs
- [ ] Define API contracts, provider callbacks, idempotency, retries, and error behavior
- [ ] Define audit events, retention, export, deletion, and backup/recovery

### P0 — Team ownership and coordination
- [x] Draft 15-member role-based assignment and ownership matrix (`docs/TEAM-ASSIGNMENT-MATRIX.md`)
- [ ] Map T01–T15 to actual members after confirming skills and availability
- [ ] Create or link task records for each D0 work package
- [ ] Confirm primary and backup reviewers for security-sensitive domains

### P0 — QA and operational readiness
- [x] Draft initial requirement-to-test traceability matrix
- [ ] Review, assign stable requirement IDs, and approve acceptance-test mapping
- [ ] Define unit, integration, end-to-end, security, accessibility, and performance checks
- [ ] Define staging/production separation and secret management
- [ ] Define monitoring, incident response, support, and rollback procedures
- [x] Draft role-based owner/reviewer allocation for all 15 team slots
- [ ] Assign actual named owners and reviewers to workstreams
- [ ] Reconcile README, master spec, architecture, database, status, tasks, and handover
- [ ] Hold documentation review and record product-owner approval

**Acceptance criteria for D0:** Every gate item is approved or explicitly deferred with a named owner and rationale; no contradictory instructions remain; implementation approval is recorded in the repository.

---

## Milestone 1: Brand and Design System (BLOCKED UNTIL D0 APPROVAL)

### P0 - Initialize Project
- [ ] Initialize package.json with project metadata
- [ ] Install TailwindCSS and dependencies
- [ ] Set up basic project structure (public/, workers/, docs/)
- [ ] Configure TailwindCSS
- [ ] Create .gitignore file
- [ ] Initialize Git repository

### P0 - Create ZetuBIZ Logo
- [ ] Design logo wordmark "ZetuBIZ"
- [ ] Design compact symbol for small sizes
- [ ] Create light background variant
- [ ] Create dark background variant
- [ ] Export as SVG
- [ ] Export as PNG for favicon
- [ ] Test logo at various sizes (16px, 32px, 64px, 128px)

### P0 - Define Color Palette
- [ ] Select primary color (teal or forest green)
- [ ] Define color scale (50-900)
- [ ] Define semantic colors (success, warning, error, info)
- [ ] Define neutral colors (gray scale)
- [ ] Ensure WCAG AA contrast ratios
- [ ] Document color tokens

### P0 - Define Typography
- [ ] Select primary font (Inter or DM Sans)
- [ ] Define type scale (display, headings, body, small, micro)
- [ ] Define font weights
- [ ] Define line heights
- [ ] Configure tabular numerals for financial data
- [ ] Test font rendering in PDFs

### P1 - Create Design Tokens
- [ ] Define spacing scale (4px base)
- [ ] Define border radius scale
- [ ] Define shadow scale
- [ ] Define border scale
- [ ] Define responsive breakpoints
- [ ] Define focus states
- [ ] Document all tokens

### P1 - Create Component Library
- [ ] Create button component (primary, secondary, ghost, danger)
- [ ] Create input component (text, email, password, number)
- [ ] Create select component
- [ ] Create card component
- [ ] Create table component
- [ ] Create status badge component
- [ ] Create modal component
- [ ] Create toast notification component
- [ ] Create loading skeleton component
- [ ] Create empty state component

### P1 - Create Layout Patterns
- [ ] Create responsive sidebar layout
- [ ] Create mobile navigation pattern
- [ ] Create page header pattern
- [ ] Create breadcrumb pattern
- [ ] Create form layout pattern
- [ ] Test responsive behavior (mobile, tablet, desktop)

### P2 - Document Design System
- [ ] Create design system documentation
- [ ] Include all design tokens
- [ ] Include component examples
- [ ] Include usage guidelines
- [ ] Include accessibility guidelines

**Acceptance Criteria for Milestone 1:**
- Brand assets created and documented
- Design tokens defined and documented
- Component library created with all basic components
- Responsive layouts tested
- Design system documented
- All components accessible

---

## Milestone 2: Authentication and Onboarding

### P0 - Set Up Supabase
- [ ] Create Supabase project
- [ ] Configure authentication
- [ ] Enable email provider
- [ ] Create database migrations
- [ ] Apply RLS policies
- [ ] Test authentication flow

### P0 - Implement Registration
- [ ] Create registration form UI
- [ ] Implement registration API endpoint
- [ ] Add email verification
- [ ] Add validation
- [ ] Add error handling
- [ ] Test registration flow

### P0 - Implement Login
- [ ] Create login form UI
- [ ] Implement login API endpoint
- [ ] Add session management
- [ ] Add error handling
- [ ] Test login flow

### P0 - Implement Password Recovery
- [ ] Create password recovery form
- [ ] Implement password reset API
- [ ] Add email sending
- [ ] Add token validation
- [ ] Test password recovery flow

### P0 - Implement Onboarding Screen 1 (Welcome)
- [ ] Create welcome screen UI
- [ ] Implement progress indicator
- [ ] Add navigation (Back, Continue)
- [ ] Save progress to database
- [ ] Test screen

### P0 - Implement Onboarding Screen 2 (Business Type)
- [ ] Create business type selection UI
- [ ] Implement adaptive consequences logic
- [ ] Save selection to database
- [ ] Test adaptive behavior

### P0 - Implement Onboarding Screen 3 (Business Details)
- [ ] Create business details form
- [ ] Add Kenya-specific defaults
- [ ] Implement validation
- [ ] Save to database
- [ ] Test form

### P0 - Implement Onboarding Screen 4 (Team Size)
- [ ] Create team size selection UI
- [ ] Implement adaptive behavior (solo vs team)
- [ ] Save to database
- [ ] Test adaptive behavior

### P0 - Implement Onboarding Screen 5 (Workflows)
- [ ] Create workflow selection UI
- [ ] Implement multiple selection
- [ ] Save to database
- [ ] Test selection

### P0 - Implement Onboarding Screen 6 (Referral)
- [ ] Create referral source selection UI
- [ ] Add optional text field for "Other"
- [ ] Save to database
- [ ] Test screen

### P0 - Implement Onboarding Screen 7 (Customization)
- [ ] Create customization form
- [ ] Add logo upload
- [ ] Add color picker (presets)
- [ ] Add live preview
- [ ] Save to database
- [ ] Test preview

### P0 - Implement Onboarding Screen 8 (Review)
- [ ] Create review screen UI
- [ ] Display summary of all choices
- [ ] Implement edit navigation
- [ ] Test review screen

### P0 - Implement Workspace Preparation
- [ ] Create preparation screen UI
- [ ] Implement real progress tracking
- [ ] Implement workspace creation API
- [ ] Add error handling and retry
- [ ] Test preparation flow

### P0 - Implement Workspace Ready Screen
- [ ] Create success screen UI
- [ ] Implement contextual next actions
- [ ] Test navigation to features

### P1 - Implement Dashboard Personalization
- [ ] Implement adaptive dashboard logic
- [ ] Create dashboard variants for each business type
- [ ] Test personalization

**Acceptance Criteria for Milestone 2:**
- Complete onboarding flow works end-to-end
- Workspace created correctly
- Dashboard personalized based on choices
- All onboarding screens tested
- Progress saved and recoverable
- Error handling implemented

---

## Milestone 3: Clients and Business Settings

### P0 - Implement Client CRUD
- [ ] Create client list UI
- [ ] Create client creation form
- [ ] Implement client API endpoints
- [ ] Add validation
- [ ] Add duplicate detection
- [ ] Test CRUD operations

### P0 - Implement Client Search and Filter
- [ ] Add search functionality
- [ ] Add filter by status
- [ ] Add pagination
- [ ] Test search and filter

### P0 - Implement Business Settings
- [ ] Create business settings UI
- [ ] Implement settings API
- [ ] Add validation
- [ ] Test settings changes

### P0 - Implement Changeable Preferences
- [ ] Allow changing business type
- [ ] Allow changing team size
- [ ] Allow changing preferred modules
- [ ] Test changes without data loss

### P1 - Implement Client Profile
- [ ] Create client profile UI
- [ ] Show related documents
- [ ] Show financial summary
- [ ] Test profile view

**Acceptance Criteria for Milestone 3:**
- Client CRUD works correctly
- Search and filter work
- Business settings configurable
- Preferences changeable without data loss
- Client profile shows related data

---

## Milestone 4: Document Studio and Templates

### P0 - Implement Document Rendering Engine
- [ ] Create document rendering service
- [ ] Implement template system
- [ ] Add PDF generation
- [ ] Test rendering

### P0 - Create Template Library
- [ ] Create 10 template designs
- [ ] Implement template selection UI
- [ ] Add template preview
- [ ] Test all templates

### P0 - Implement Branding Customization
- [ ] Add logo upload to templates
- [ ] Add color customization
- [ ] Add header/footer configuration
- [ ] Test customization

### P1 - Implement Template Defaults
- [ ] Allow setting default quotation template
- [ ] Allow setting default invoice template
- [ ] Save preferences to database
- [ ] Test defaults

**Acceptance Criteria for Milestone 4:**
- Document rendering engine works
- 10 templates available and tested
- Branding customization works
- Template preview matches PDF
- Defaults saved correctly

---

## Milestone 5: Quotation Workflow

### P0 - Implement Quotation Creation
- [ ] Create quotation creation UI
- [ ] Implement line item editor
- [ ] Add calculations (server-side)
- [ ] Add autosave
- [ ] Test creation flow

### P0 - Implement Quotation Preview
- [ ] Create preview UI
- [ ] Match preview to PDF
- [ ] Test preview accuracy

### P0 - Implement Quotation Finalization
- [ ] Implement safe numbering
- [ ] Add finalization API
- [ ] Create immutable snapshot
- [ ] Test finalization

### P0 - Implement PDF Generation
- [ ] Generate PDF from quotation
- [ ] Test PDF quality
- [ ] Test multi-page PDFs
- [ ] Test PDF download

### P0 - Implement Quotation Status Management
- [ ] Implement status transitions
- [ ] Add status validation
- [ ] Test status flow

### P1 - Implement Client Acceptance
- [ ] Create public quotation page
- [ ] Implement acceptance action
- [ ] Record acceptance event
- [ ] Test acceptance flow

### P1 - Implement Convert to Invoice
- [ ] Implement conversion API
- [ ] Preserve line items
- [ ] Test conversion

**Acceptance Criteria for Milestone 5:**
- Quotation creation works with accurate calculations
- Preview matches PDF
- Finalization creates immutable snapshot
- PDF generation works correctly
- Status management enforced
- Client acceptance recorded
- Conversion to invoice preserves data

---

## Milestone 6: Invoice Workflow

### P0 - Implement Invoice Creation
- [ ] Create invoice creation UI
- [ ] Implement line item editor
- [ ] Add calculations (server-side)
- [ ] Test creation flow

### P0 - Implement Invoice from Quotation
- [ ] Add conversion from quotation
- [ ] Preserve line items and prices
- [ ] Test conversion

### P0 - Implement Safe Numbering
- [ ] Implement concurrency-safe numbering
- [ ] Test concurrent invoice creation

### P0 - Implement Invoice Finalization
- [ ] Add finalization API
- [ ] Create immutable snapshot
- [ ] Test finalization

### P0 - Implement Partial Payments
- [ ] Add payment recording UI
- [ ] Implement payment allocation
- [ ] Calculate remaining balance
- [ ] Test partial payments

### P1 - Implement Credit Notes
- [ ] Create credit note UI
- [ ] Implement credit note API
- [ ] Adjust invoice balance
- [ ] Test credit notes

**Acceptance Criteria for Milestone 6:**
- Invoice creation works
- Conversion from quotation preserves data
- Numbering is concurrency-safe
- Finalization creates immutable snapshot
- Partial payments tracked correctly
- Credit notes adjust balances

---

## Milestone 7: Secure Sharing and Email

### P0 - Implement Secure Share Links
- [ ] Generate high-entropy tokens
- [ ] Create public document page
- [ ] Implement token validation
- [ ] Add token revocation
- [ ] Test sharing flow

### P0 - Implement Email Composition
- [ ] Create email composition UI
- [ ] Implement template variables
- [ ] Add preview
- [ ] Test composition

### P0 - Implement Email Sending
- [ ] Integrate email provider (Resend or SendGrid)
- [ ] Implement email sending API
- [ ] Add delivery tracking
- [ ] Test email sending

### P1 - Implement Reminders
- [ ] Create reminder templates
- [ ] Implement manual reminders
- [ ] Test reminder sending

**Acceptance Criteria for Milestone 7:**
- Secure links use high-entropy tokens
- Public page works correctly
- Email composition works
- Email sending works with provider
- Delivery tracking implemented
- Reminders can be sent

---

## Milestone 8: Payment Tracking and Integration

### P0 - Implement Payment Recording
- [ ] Create payment recording UI
- [ ] Implement payment API
- [ ] Add payment allocation
- [ ] Test recording

### P0 - Integrate Paystack
- [ ] Implement Paystack adapter
- [ ] Add payment initiation
- [ ] Add webhook verification
- [ ] Test in sandbox

### P0 - Implement Webhook Processing
- [ ] Create webhook handler
- [ ] Implement signature verification
- [ ] Add idempotency
- [ ] Test webhook processing

### P0 - Implement Idempotency
- [ ] Add idempotency keys
- [ ] Prevent duplicate charges
- [ ] Test idempotency

### P1 - Implement Payment History
- [ ] Create payment history UI
- [ ] Show payment allocations
- [ ] Test history view

**Acceptance Criteria for Milestone 8:**
- Payment recording works
- Paystack integration tested in sandbox
- Webhook verification works
- Idempotency prevents duplicates
- Payment history accurate

---

## Milestone 9: Expenses, Reports, and Teams

### P0 - Implement Expense Management
- [ ] Create expense CRUD UI
- [ ] Add file upload
- [ ] Implement expense API
- [ ] Test expense management

### P0 - Implement Financial Reports
- [ ] Create income/expense report
- [ ] Create receivables report
- [ ] Add date filters
- [ ] Test reports

### P0 - Implement Team Roles
- [ ] Create team management UI
- [ ] Implement role assignment
- [ ] Add permission enforcement
- [ ] Test roles

### P1 - Implement Team Invitations
- [ ] Create invitation flow
- [ ] Send invitation emails
- [ ] Test invitations

**Acceptance Criteria for Milestone 9:**
- Expense CRUD works
- File uploads work
- Reports accurate
- Team roles enforced
- Permissions work correctly

---

## Milestone 10: Platform Administration

### P0 - Create Separate Admin Console
- [ ] Create admin authentication
- [ ] Create admin UI
- [ ] Separate from business workspace
- [ ] Test separation

### P0 - Implement Tenant Management
- [ ] Create tenant list UI
- [ ] Add tenant actions (suspend, unsuspend)
- [ ] Implement audit logging
- [ ] Test tenant management

### P1 - Implement Audit Logs
- [ ] Create audit log viewer
- [ ] Add filtering
- [ ] Test audit viewing

**Acceptance Criteria for Milestone 10:**
- Admin console separate from business workspace
- Tenant management works
- Audit logs accessible
- Actions logged correctly

---

## Milestone 11: Security Review and Testing

### P0 - Security Review
- [ ] Review tenant isolation
- [ ] Review RLS policies
- [ ] Review secret management
- [ ] Review authorization
- [ ] Document findings

### P0 - Tenant Isolation Tests
- [ ] Write tenant isolation tests
- [ ] Test cross-tenant access prevention
- [ ] Test RLS policies
- [ ] Fix any issues

### P0 - Automated Tests
- [ ] Write unit tests for calculations
- [ ] Write integration tests for API
- [ ] Write E2E tests for critical flows
- [ ] Achieve coverage targets

### P1 - Mobile Layout Testing
- [ ] Test all screens on mobile
- [ ] Test responsive behavior
- [ ] Fix mobile issues

**Acceptance Criteria for Milestone 11:**
- Security review complete
- Tenant isolation verified
- All tests passing
- Coverage targets met
- Mobile layouts tested

---

## Milestone 12: Production Readiness

### P0 - Backup and Recovery
- [ ] Document backup procedures
- [ ] Test backup restoration
- [ ] Document recovery procedures

### P0 - Monitoring Setup
- [ ] Configure error tracking
- [ ] Configure performance monitoring
- [ ] Set up alerts
- [ ] Test monitoring

### P0 - Documentation Completion
- [ ] Update all documentation
- [ ] Verify documentation accuracy
- [ ] Complete API documentation

### P1 - Deployment Testing
- [ ] Deploy to staging
- [ ] Test all features in staging
- [ ] Deploy to production
- [ ] Verify production deployment

**Acceptance Criteria for Milestone 12:**
- Backup procedures documented and tested
- Monitoring configured
- Documentation complete and accurate
- Production deployment successful
- All features working in production

---

## Current Focus

**Active Milestone:** Milestone D0 - Documentation and Product Readiness

**Next Task:** Review the task handover template, assign named documentation owners, and continue closing the documentation gate. Application implementation remains blocked until explicit product-owner approval.

---

## Task Notes

- Tasks should be completed in order within each milestone
- Each milestone must meet acceptance criteria before proceeding
- Update IMPLEMENTATION-STATUS.md when completing tasks
- Update this file when adding or completing tasks
- Test each task before marking complete
