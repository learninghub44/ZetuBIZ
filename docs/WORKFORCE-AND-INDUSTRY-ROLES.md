# ZetuBIZ Workforce, Specialist and Industry Role Catalogue

**Status:** Proposed product requirements for review; not implemented  
**Purpose:** Ensure ZetuBIZ considers a broad range of professions, small businesses, teams and specialist workflows rather than only software developers or office administrators.

## 1. Product principle

ZetuBIZ is a configurable business operations platform, not a job-title directory. Roles listed here describe customer segments, workspace responsibilities and possible workflow needs. They do not automatically grant application permissions. Access must always be configured through explicit, tenant-scoped permissions.

Do not promise a dedicated feature for every occupation in the first release. Validate demand, define workflows and sequence modules based on customer evidence, risk and delivery capacity.

## 2. Technology, infrastructure and security

Potential customer profiles:
- Network administrators and network engineers
- Systems administrators and IT support technicians
- Cloud administrators and infrastructure engineers
- Cybersecurity analysts, security consultants and privacy practitioners
- DevOps, site reliability and platform engineers
- Web developers, software engineers and QA testers
- Database administrators, data analysts and BI specialists
- Hardware, device, printer, CCTV and technical maintenance providers

Potential workflows: service enquiries, client/contact records, quotations, recurring support contracts, projects, time tracking, service-level agreements, inventory/assets, incident records, expenses, invoices, renewals and customer communication. Network configuration secrets, credentials and sensitive incident data must not be stored in ordinary notes; privileged technical records require dedicated access controls and audit logging.

## 3. Marketing, media and communications

Potential customer profiles:
- Social media managers and community managers
- Digital marketers and performance advertising specialists
- SEO specialists and content strategists
- Copywriters, editors and translators
- Graphic designers, brand designers and illustrators
- Photographers, videographers and video editors
- Influencer and creator managers
- Public relations and communications consultants
- Email marketing and marketing automation specialists

Potential workflows: campaign briefs, content calendars, approval steps, asset libraries, client approvals, campaign budgets, publishing schedules, lead capture, marketing attribution, invoices, retainers, deliverables and reports. Direct publishing to social networks is a separate integration requiring official API capability verification, user consent, token security and platform-policy review. Do not assume every social network permits every action.

## 4. Sales, commerce and customer relationships

Potential customer profiles:
- Sales representatives and sales managers
- Business development and partnership managers
- Account managers and customer-success teams
- Customer support agents and contact-centre operators
- Retailers, wholesalers, distributors and importers
- E-commerce sellers and marketplace operators
- Field sales teams and delivery coordinators
- Procurement officers, suppliers and purchasing teams

Potential workflows: lead pipelines, follow-ups, customer history, quotations, orders, invoices, stock, supplier records, returns, payment allocations, customer credit, service cases and performance reporting.

## 5. Professional and business services

Potential customer profiles:
- Freelancers and independent consultants
- Agencies and studios
- Accountants and bookkeepers
- Business, HR and operations consultants
- Lawyers and legal-service firms
- Architects, engineers and surveyors
- Training providers, tutors and education consultants
- Researchers and data-service providers
- Event planners and creative production teams

Potential workflows: client onboarding, proposals, engagement terms, milestones, time tracking, document approvals, expenses, billing, recurring retainers and project profitability. Regulated or legally sensitive services need domain review before specialised compliance claims are made.

## 6. Field services, trades and maintenance

Potential customer profiles:
- Electricians, plumbers and installers
- Contractors, builders and property maintenance teams
- Solar and renewable-energy installers
- Cleaning, pest-control and landscaping businesses
- Appliance, phone, computer and equipment repairers
- Automotive repair and service providers
- Logistics, transport and delivery operators
- Security-system and network-installation teams

Potential workflows: enquiries, site visits, bookings, work orders, quotations, job assignment, checklists, parts and stock usage, photos/documents, customer sign-off, scheduling, invoices and after-service reminders.

## 7. People, administration and business operations

Potential customer profiles:
- HR officers and recruiters
- Office and administrative managers
- Project and operations managers
- Payroll and benefits administrators
- Training and learning coordinators
- Facilities and fleet managers
- Compliance and quality coordinators

Potential workflows: employee records, leave, attendance, approvals, recruitment tracking, training, policies, assets, task assignments and reports. Payroll, tax, employment-law and regulated compliance workflows require current qualified review for each target jurisdiction.

## 8. Hospitality, wellness, property and local services

Potential customer profiles:
- Salons, barbers and beauty professionals
- Fitness trainers and wellness studios
- Clinics and healthcare-service businesses
- Restaurants, caterers and food businesses
- Hotels, guest houses and tour operators
- Property managers, landlords and real-estate agents
- Photographers, event venues and event service providers
- Repair shops and other appointment-based services

Potential workflows: bookings, schedules, service catalogues, deposits, customer reminders, invoices, inventory, staff rosters, feedback and repeat-customer management. Health and other sensitive personal data should not be collected unless a separately reviewed use case, lawful basis, security model and retention policy are approved.

## 9. Suggested workspace roles and permission boundaries

Use permission presets that can be customised per business:
- Owner
- Workspace administrator
- Finance / accountant
- Sales / account manager
- Marketing / content publisher
- Customer support
- Operations / project manager
- Inventory / purchasing
- HR (with separate salary permissions)
- IT / network administrator
- Security auditor
- Staff / contributor
- Read-only reviewer / external accountant
- Customer / client portal user
- Platform support administrator with controlled, audited access

A job title is not a permission. For example, a network administrator may manage infrastructure settings only if explicitly granted; a social media marketer may draft campaigns but not access payroll or bank/payment credentials. Separate content creation, approval, publishing, billing, exports and credential management where risk warrants it. Use least privilege, separation of duties, audit logs, and periodic access reviews.

## 10. Social media and marketing module requirements to evaluate

Do not assume all items belong in Release 1. Assess each during customer discovery:
- Multi-brand and multi-client workspaces
- Campaign goals, audiences, channels, budgets and dates
- Content calendar and draft/scheduled/published/failed states
- Post and asset approvals with comments and version history
- Image/video/document asset library with rights and consent metadata
- Campaign task assignment, deadlines and workload
- UTM links and lead-source tracking
- Analytics imports and clearly defined metric calculations
- Client approval links with expiry and revocation
- Campaign expense tracking, retainers, quotations and invoices
- Social account connection through supported official APIs only
- Secure token storage, scope review, revocation and token-expiry handling
- Publishing queue, retry rules and visible platform errors
- Moderation and community response tracking where API terms allow
- Consent, opt-out, privacy and retention settings for marketing contacts
- Clear distinction between measured results and estimates

## 11. Network and IT administration module requirements to evaluate

Potential needs:
- Client/site/device inventory and ownership
- Support tickets, incidents, priorities and escalation
- Maintenance schedules and recurring service contracts
- Site visits, work orders, time entries and service reports
- Asset warranty, licence and renewal reminders
- Approved network diagrams and documentation attachments
- Change records, approval history and rollback notes
- SLA targets, response/resolution tracking and customer updates
- Restricted access to sensitive infrastructure records
- Audit trails for viewing or changing sensitive records
- Secure secret-management integration rather than plain-text credential fields

This is not a substitute for dedicated monitoring, ticketing, network configuration or secret-management systems unless those integrations and controls are separately designed and validated.

## 12. Validation and release planning

Before adding an industry or specialist module:
1. Interview real users in that segment.
2. Record the current process, most expensive problem, existing tools and willingness to pay.
3. Map shared workflows to existing ZetuBIZ modules before proposing a separate subsystem.
4. Define roles, data sensitivity, permission boundaries and retention needs.
5. Document external integrations and verify official API/compliance requirements.
6. Add requirement IDs, acceptance criteria and negative security tests.
7. Estimate operational cost and support burden.
8. Record the release decision in `docs/DECISIONS.md`.

**Acceptance checklist**
- [ ] Roles represent both technical and non-technical customer segments.
- [ ] Role presets do not silently grant permissions.
- [ ] Marketing/social integrations are capability-verified before commitment.
- [ ] Network/IT workflows protect infrastructure details and secrets.
- [ ] Regulated domains have qualified review.
- [ ] Release 1 remains focused on validated, achievable customer needs.
