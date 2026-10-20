# AGENTS.md - Instructions for Coding Agents

This document contains permanent instructions for all coding agents working on ZetuBIZ.

## Before Starting Work

**Required Reading:**
1. Read this file (`AGENTS.md`)
2. Read the master specification (`docs/ZetuBIZ-MASTER-SPEC.md`)
3. Read the architecture document (`docs/ARCHITECTURE.md`)
4. Read the implementation status (`docs/IMPLEMENTATION-STATUS.md`)
5. Read the current tasks (`TASKS.md`)
6. Read the latest handover notes (`docs/HANDOVER.md`)
7. Read the expanded platform requirements (`docs/ALL-IN-ONE-PLATFORM-REQUIREMENTS.md`)
8. Read the team workflow (`docs/TEAM-WORKFLOW.md`)

**Documentation-first rule:** The project owner has directed that documentation must be reviewed and completed before implementation begins. Do not initialize the app, create migrations, configure production services, or start feature coding until the documentation gate in `docs/TEAM-WORKFLOW.md` is explicitly approved.

**Required Actions:**
- Inspect the actual code and Git status
- Do not trust documentation blindly - verify against reality
- Understand what is actually implemented vs. what is planned
- Identify the exact next task from TASKS.md

## Architecture Rules

### Technology Stack
- **Frontend:** HTML5, CSS3, JavaScript (ES6+)
- **Styling:** TailwindCSS
- **Backend:** Cloudflare Workers (serverless)
- **Hosting:** Cloudflare Pages
- **Storage:** Cloudflare R2
- **Database:** Supabase PostgreSQL
- **Authentication:** Supabase Auth
- **Email:** Resend or SendGrid
- **PDF:** Puppeteer or similar

### Strict Requirements
- Follow the existing stack - do not introduce new frameworks without explicit approval
- Use vanilla JavaScript unless there's a documented reason for a framework
- All financial calculations must be server-side with decimal-safe arithmetic
- Never use floating-point math for money
- All authorization must be server-side - never trust client-side claims

## Security Requirements

### Tenant Isolation
- Every database table must include `business_id` foreign key
- Supabase Row Level Security (RLS) policies must enforce tenant isolation
- Server-side authorization must validate business membership on every operation
- Review RLS policies for every relevant database change
- Test tenant isolation before claiming a feature works

### Secret Management
- Never commit secrets to Git
- Never include secrets in documentation
- Never log secrets or credentials
- Use environment variables via Cloudflare Workers secrets
- Encrypt payment credentials at rest
- Redact secrets from error messages and audit logs

### Authentication
- Use Supabase Auth for authentication
- Never weaken authentication to make tests pass
- Session tokens must be validated server-side
- Implement proper session expiry handling

## Coding Conventions

### File Organization
- Keep frontend in `public/` or `src/` (to be determined during implementation)
- Keep Cloudflare Workers in `workers/` or `functions/`
- Keep database migrations in `supabase/migrations/`
- Keep documentation in `docs/`

### Code Style
- Use consistent indentation (2 or 4 spaces - choose one and stick to it)
- Use meaningful variable and function names
- Add comments for complex logic
- Keep functions focused and reasonably sized
- Avoid deeply nested code

### Database
- All schema changes must be in versioned migrations
- Never modify production schema directly
- Test migrations on staging before production
- Use transactions for multi-table changes
- Add indexes for foreign keys and frequently queried columns

## Testing Requirements

### What to Test
- All financial calculations (unit tests)
- All status transitions (unit tests)
- Tenant isolation (integration tests)
- Authentication and authorization (integration tests)
- Payment provider integrations (integration tests with test adapters)
- Critical user workflows (end-to-end tests)

### Test Data
- Do not use live payments in automated tests
- Use provider sandbox environments where available
- Use test fixtures that are clearly marked as test data
- Never use real customer data in tests

### Test Execution
- Run tests before committing
- Report actual test results - do not claim tests pass without running them
- Update test coverage when adding new features
- Fix failing tests before proceeding

## Implementation Guidelines

### One Milestone at a Time
- Implement one milestone from the roadmap completely
- Do not jump between milestones
- Get acceptance criteria met before moving on
- Update IMPLEMENTATION-STATUS.md when functionality changes

### Real Data Only
- Never use fake or hardcoded financial figures
- Never show sample clients or fabricated revenue
- Use real persisted data for all features
- Empty states should show helpful next actions, not fake data

### No Unnecessary Rewrites
- Do not rewrite working code without a documented reason
- Do not duplicate systems
- Do not introduce unnecessary dependencies
- Follow existing patterns unless there's a clear improvement

### Document Your Changes
- Update relevant documentation when implementing features
- Update IMPLEMENTATION-STATUS.md
- Update TASKS.md when completing tasks
- Add comments for complex logic
- Update API documentation if adding endpoints

## Approval Required

Ask for approval before:
- Destructive database operations (DROP, ALTER COLUMN with data loss)
- Significant architecture changes
- Production transactions
- Actions that could lose data
- Introducing new major dependencies
- Changing the authentication system
- Modifying RLS policies in production

## Common Pitfalls to Avoid

### Don't Do This
- Trust client-side payment confirmation
- Expose secrets in frontend code
- Skip tenant isolation checks
- Use floating-point math for money
- Claim a feature works without testing
- Modify production schema without migrations
- Commit secrets to Git
- Weaken security for convenience
- Create fake data for dashboards
- Skip error handling

### Do This Instead
- Verify payments via webhooks or server-side API calls
- Keep secrets server-side
- Enforce tenant isolation at database and application level
- Use decimal or integer arithmetic for money
- Test features end-to-end
- Use versioned migrations
- Use environment variables
- Implement proper security from the start
- Use real data or helpful empty states
- Handle errors gracefully

## Communication

### When to Ask for Help
- If requirements are unclear
- If you encounter a blocker
- If you need to make an architectural decision
- If you discover a security issue
- If tests are failing and you can't determine why

### How to Report Progress
- Update IMPLEMENTATION-STATUS.md
- Update TASKS.md
- Report actual test results
- Note any deviations from the specification
- Document any workarounds or temporary solutions

## Handover Protocol

When handing over to another agent:
1. Update IMPLEMENTATION-STATUS.md to reflect current state
2. Update TASKS.md with remaining work
3. Create or update docs/HANDOVER.md with:
   - What is implemented and verified
   - What is partially implemented
   - What is still missing
   - Current architecture
   - Known bugs and risks
   - Exact next task with acceptance criteria
4. Ensure all documentation is consistent
5. Verify no secrets are committed
6. Run tests and report results

## Quality Checklist

Before considering a task complete:
- [ ] Code follows existing conventions
- [ ] Tests pass
- [ ] Tenant isolation verified
- [ ] No secrets in code
- [ ] Documentation updated
- [ ] Error handling implemented
- [ ] Accessibility considered
- [ ] Mobile layout tested (if UI)
- [ ] Security reviewed (if sensitive)

## Emergency Procedures

If you accidentally:
- **Commit secrets:** Immediately remove from Git history, rotate the secrets
- **Break production:** Rollback using documented procedures, investigate cause
- **Expose customer data:** Immediately revoke access, notify stakeholders, investigate scope
- **Delete important data:** Check backups, assess impact, notify stakeholders

## Contact and Support

For questions about:
- **Product requirements:** Reference docs/ZetuBIZ-MASTER-SPEC.md
- **Architecture:** Reference docs/ARCHITECTURE.md
- **Database:** Reference docs/DATABASE.md
- **Environment setup:** Reference docs/DEVELOPMENT.md
- **Deployment:** Reference docs/DEPLOYMENT.md
- **Current status:** Reference docs/IMPLEMENTATION-STATUS.md and docs/HANDOVER.md

---

**Last updated:** October 20, 2026
**Documentation-first directive:** Do not begin implementation until the product owner approves the documentation gate.
**Maintained by:** Product team
