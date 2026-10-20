# ZetuBIZ

ZetuBIZ is a planned, customizable business operating platform for freelancers, agencies, service businesses, and SMEs in Kenya, with a roadmap for broader African and international use.

**Important project status:** ZetuBIZ is currently at the documentation/specification stage. The features described in this repository are requirements and plans until their implementation and tests are verified. See [Implementation Status](docs/IMPLEMENTATION-STATUS.md).

## Product direction

The long-term vision is one connected workspace for business operations, including:

- Workspace setup, members, roles, settings, and branding
- CRM, leads, customers, contacts, and follow-ups
- Quotations, proposals, contracts, invoices, receipts, and customer balances
- Payment collection, payment records, and reconciliation
- Expenses, supplier bills, and financial reporting
- Products, inventory, purchasing, and point-of-sale workflows
- Projects, tasks, milestones, and time tracking
- Employee records, leave, attendance, and payroll subject to compliance review
- Email, SMS/WhatsApp integrations, and notifications
- Website content management, public APIs, and webhooks
- Analytics, platform administration, and a permission-aware AI assistant
- Optional advanced capabilities such as a browser call centre

These are planned modules, not claims of existing functionality. The team will deliver them in stages based on customer validation, dependencies, security, and operational readiness. ZetuBIZ will maintain its own product identity, requirements, design, and implementation.

## Current documentation

Start with these documents before proposing implementation work:

- **[All-in-One Platform Requirements](docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md)** — expanded module catalogue, shared workflows, non-functional requirements, release sequence, acceptance criteria, and open decisions.
- **[Master Product Specification](docs/ZetuBIZ-MASTER-SPEC.md)** — existing product specification and initial workflows.
- **[Architecture](docs/ARCHITECTURE.md)** — planned system architecture and module boundaries.
- **[Database Schema](docs/DATABASE.md)** — planned data model, constraints, and tenant isolation.
- **[Implementation Status](docs/IMPLEMENTATION-STATUS.md)** — current verified implementation state.
- **[Tasks](TASKS.md)** — proposed implementation backlog; task completion must be evidence-based.
- **[Agent Instructions](AGENTS.md)** — repository rules for coding agents.
- **[Team Workflow](docs/TEAM-WORKFLOW.md)** — roles, task lifecycle, GitHub review rules, AI-tool independence, handovers, and the documentation approval gate.
- **[Task Handover Template](docs/TASK-HANDOVER-TEMPLATE.md)** — reusable, evidence-based handover format that works across people and AI tools.
- **[UX Specification](docs/UX-SPECIFICATION.md)** — onboarding, workspace setup, navigation, dashboards, document studio, and Release 1 screens.
- **[Permissions Matrix](docs/PERMISSIONS-MATRIX.md)** — proposed role presets, sensitive actions, tenant scope, and security tests.
- **[Data Dictionary](docs/DATA-DICTIONARY.md)** — shared business entities, relationships, and data integrity rules.
- **[Decision Log](docs/DECISIONS.md)** — proposed/accepted product and architecture decisions.
- **[Test Traceability](docs/TEST-TRACEABILITY.md)** — requirement-to-test mapping and release gates.
- **[Development Guide](docs/DEVELOPMENT.md)** — local development guidance.
- **[Environment Variables](docs/ENVIRONMENT.md)** — configuration and secrets reference.
- **[Testing Strategy](docs/TESTING.md)** — test strategy and acceptance gates.
- **[Deployment Guide](docs/DEPLOYMENT.md)** — deployment and operations guidance.

## Documentation-first workflow

1. Review the requirements and resolve open product decisions.
2. Validate the first target segment with real business owners.
3. Align the master specification, UX flows, architecture, data model, permissions, tests, and roadmap.
4. Approve the initial release scope and acceptance criteria.
5. Only then begin implementation.
6. Update implementation status only after work has been verified.

## Planned technology direction

The current proposal uses HTML, CSS, and JavaScript; Cloudflare Pages/Workers; Supabase PostgreSQL and Auth; Cloudflare R2; and a transactional email provider. These are proposed architecture choices and must be reviewed and confirmed in the architecture documentation before implementation.

## Security principles

- Tenant isolation must be enforced in the database and server-side authorization.
- Never expose service-role keys, payment credentials, or other secrets in client-side code.
- Financial records require traceable state transitions and safe correction workflows.
- Payment status must be based on verified provider events or an explicitly authorized manual process.
- AI-generated actions must respect the same permissions as ordinary user actions and require confirmation for consequential operations.
- No feature is production-ready without appropriate tests, documentation, monitoring, and operational procedures.

## Contributing

Contributors should read [AGENTS.md](AGENTS.md), the [All-in-One Platform Requirements](docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md), and the relevant domain documentation before proposing work. Do not treat a planned feature as implemented, introduce major architecture changes without review, or commit credentials.
