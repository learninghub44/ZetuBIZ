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

## 10. Handover template

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

## 11. Definition of done

A task is done only when:
- Scope and acceptance criteria are met.
- An independent reviewer approved it.
- Relevant documentation, status, and links are updated.
- Tests/checks were run where applicable and results are truthful.
- Security, privacy, accessibility, and tenant impact were considered.
- No untracked temporary workaround or unresolved critical blocker remains.
- The next contributor can continue without guessing.

## 12. Immediate team actions

1. Assign named owners to the workstreams and documents.
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
