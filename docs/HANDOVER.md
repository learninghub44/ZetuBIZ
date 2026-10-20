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
