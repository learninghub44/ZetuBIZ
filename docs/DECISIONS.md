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
