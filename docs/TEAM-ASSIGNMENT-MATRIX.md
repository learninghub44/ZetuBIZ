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

The roster deliberately includes technical, infrastructure, security, marketing, sales, support, and operational capabilities. One person may cover adjacent duties only where they have the skills and capacity; sensitive reviews must remain independent.

| Slot | Primary role | Accountable area | Initial deliverables | Primary reviewer |
|---|---|---|---|---|
| T01 | Product owner / product lead | Product direction, priorities, final approvals | Target segment, success measures, Release 1 scope, decision ownership, documentation gate approval | Independent reviewer for clarity; product owner retains final approval |
| T02 | Product analyst / customer researcher | Customer needs and requirements | Interview plan, personas, requirement IDs, workflow research, measurable outcomes | T01 |
| T03 | UX/UI and accessibility designer | User journeys, interface and design system | Sitemap, onboarding flows, prototypes/specifications, responsive and accessibility rules | T02 + T14 |
| T04 | Frontend / web experience lead | Web interface requirements and future implementation planning | Frontend boundaries, responsive behaviour, component contracts, browser support | T03 + T05 |
| T05 | Backend / database architect | APIs, data model, application architecture | Architecture, module boundaries, ERD/data contracts, migration principles | T07 + T06 |
| T06 | Network and cloud administrator | DNS, domains, networking, hosting, environments and connectivity | Network/deployment topology, DNS/TLS, access controls, environment separation, recovery runbook | T05 + T07 |
| T07 | Cybersecurity and privacy lead | Threat model, identity, tenant boundaries and data protection | Threat model, authorization rules, privacy/retention requirements, security review checklist | T05; independent review for high-risk changes |
| T08 | API, integrations and payments lead | External providers and integration behaviour | API contracts, payment provider capability checks, webhooks, idempotency, retries and failure states | T05 + T07 |
| T09 | Finance and business-process specialist | Quotations, invoices, expenses, reconciliation and reporting rules | Financial workflows, calculations, auditability, reconciliation exceptions and compliance questions | T02 + T08 + T14 |
| T10 | Social media manager / community lead | Social channels, publishing and community engagement | Channel strategy, content calendar, publishing approvals, moderation and engagement metrics | T01 + T11 |
| T11 | Digital marketer / content and SEO specialist | Brand messaging, website content, campaigns and discovery | Content plan, SEO requirements, campaign measurement, landing-page copy and analytics requirements | T01 + T10 |
| T12 | Sales, partnerships and growth lead | Customer acquisition and commercial partnerships | Lead pipeline, outreach process, partnership targets, CRM requirements and sales feedback | T01 + T02 |
| T13 | Customer support and customer-success lead | Onboarding, help, feedback and retention | Support workflow, help content, escalation rules, feedback loop and service measures | T01 + T15 |
| T14 | QA, accessibility and release-quality lead | Testability and acceptance evidence | Requirement-to-test mapping, regression plan, security-negative tests, accessibility and release criteria | T07 + relevant domain owners |
| T15 | Operations, documentation and delivery coordinator | Cross-team coordination, handovers and operational readiness | Documentation consistency, task tracking, support/incident process, meeting notes and handovers | T01 + relevant domain reviewer |

**Specialist coverage note:** This 15-person allocation is a starting team structure, not a claim that every specialist skill is already present. If ZetuBIZ later needs dedicated HR/payroll compliance, inventory/POS, legal/tax, data analytics, paid advertising, video/design production, or advanced network engineering expertise, assign a qualified contributor or external reviewer before approving that domain. Do not treat general experience as a substitute for regulated professional advice.

**Capacity note:** Confirm each person's actual skills, availability, preferred work, and backup coverage before assigning by name. T01 owns final product decisions but must not self-review their own technical or product work.

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
