# ZetuBIZ Role and Permission Matrix

**Status:** Proposed authorization baseline; must be reviewed before implementation.  
**Security rule:** This matrix is a product-level guide. API authorization and database RLS must independently enforce the approved permissions.

## 1. Permission legend

- **Full:** May perform the action within the authorized workspace.
- **Scoped:** May perform only assigned/owned records or specific granted actions.
- **View:** Read-only access to permitted records.
- **Request:** May submit a request that requires another role's approval.
- **No:** No access by default.
- **Configurable:** Must be explicitly granted; never assume it from a broad role.

## 2. Proposed role presets

| Capability | Owner | Admin | Finance | Sales | Manager/Ops | HR | Staff | Read-only/Auditor |
|---|---|---|---|---|---|---|---|---|
| View general workspace profile | Full | Full | View | View | View | View | View | View |
| Edit business settings/branding | Full | Full | No | No | Configurable | No | No | No |
| Manage workspace members | Full | Scoped | No | No | No | No | No | No |
| Transfer ownership/delete workspace | Full | No | No | No | No | No | No | No |
| View all customers | Full | Full | View | Full | Scoped | No | Scoped | View |
| Edit customers/leads | Full | Full | Scoped | Full | Scoped | No | Scoped | No |
| View quotations | Full | Full | View | Full | Scoped | No | Scoped | View |
| Create/edit draft quotations | Full | Full | No | Full | Scoped | No | Scoped | No |
| Finalize/send quotations | Full | Full | No | Configurable | Configurable | No | No by default | No |
| View invoices/payment status | Full | Full | Full | Scoped | Scoped | No | Scoped | View |
| Create/edit draft invoices | Full | Full | Full | Configurable | Configurable | No | No by default | No |
| Finalize/void invoices | Full | Configurable | Configurable | No by default | No by default | No | No | No |
| Record/reconcile payments | Full | Configurable | Full | No by default | No by default | No | No | View |
| Initiate refunds/provider actions | Full | Configurable | Configurable | No | No | No | No | No |
| Create expenses | Full | Full | Full | Scoped | Full | No | Request | View |
| Approve expenses | Full | Configurable | Configurable | No by default | Configurable | No | No | View |
| View financial reports | Full | Full | Full | Scoped | Scoped | No | No | View |
| Export financial data | Full | Configurable | Configurable | No by default | Configurable | No | No | Configurable |
| Manage products/stock | Full | Full | Scoped | Scoped | Full | No | Scoped | View |
| Manage projects/tasks | Full | Full | Scoped | Scoped | Full | No | Scoped | View |
| View employee directory | Full | Configurable | No | No | Scoped | Full | Self only | Configurable |
| View salary/payroll details | Full | No by default | Configurable | No | No | Configurable | Self payslip only | No |
| Run/approve payroll | Full | No by default | Configurable | No | No | Configurable | No | No |
| Manage integrations/secrets | Full | Configurable | Configurable | No | No | No | No | No |
| Configure website/public API | Full | Full | No | Configurable | Configurable | No | No | No |
| Use AI assistant | Scoped to own permissions | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped read-only |
| Access platform admin console | No by default | No | No | No | No | No | No | No |

The platform support administrator is a separate platform-level role, not a workspace role. Access to tenant content is denied by default; temporary break-glass access requires a reason, approval where feasible, time limit, narrow scope, and audit log.

## 3. Sensitive action controls

Require explicit permission checks and audit events for:
- Inviting, removing, deactivating, or changing a member's role.
- Ownership transfer and workspace deletion.
- Exporting customer, financial, HR, or payroll data.
- Finalizing, voiding, crediting, or reversing financial documents.
- Recording manual payments or initiating refunds.
- Changing payment-provider credentials or webhook settings.
- Changing tax, currency, numbering, or financial-period configuration.
- Viewing/editing payroll, employee documents, or sensitive personal data.
- Publishing websites or changing public API keys/webhooks.
- Granting temporary support access or using platform-admin tools.

For high-risk financial workflows, separate creator and approver where feasible and configurable. The owner role must not bypass audit logging.

## 4. Tenant and record-scope rules

1. A user may belong to several workspaces, each with separate membership and permissions.
2. Every request must resolve the active workspace from an authorized server-side membership context.
3. Never trust a submitted `business_id`, record ID, role, or owner ID without checking authorization.
4. Public document links are a separate restricted access path, scoped to one document and limited actions.
5. A manager's access to a project does not imply access to all customer finances or employee salary records.
6. HR access does not automatically include payroll, medical, disciplinary, or all employee attachments.
7. Finance access does not automatically include workspace member administration or secrets.
8. Read-only access does not automatically include export, download of sensitive attachments, or bulk search.
9. AI retrieval and tool execution must apply the same record-level permissions as normal UI/API requests.
10. Platform administrators and support tools must be isolated from tenant workspace roles.

## 5. Implementation requirements

- Convert approved capabilities into explicit permission keys, not scattered role-name conditionals.
- Define permission inheritance and deny precedence.
- Enforce access in server-side route/service layer and database RLS.
- Use safe database functions for operations that require atomic state changes.
- Add negative tests for cross-tenant access, guessed IDs, revoked memberships, expired invitations, disabled users, and public-link misuse.
- Test every sensitive action for each role, including direct API calls that bypass the UI.
- Audit actor, workspace, action, target, outcome, timestamp, and minimal metadata.
- Never log secrets, access tokens, or unnecessary personal data.
- Review permission changes as security-sensitive schema/API changes.

## 6. Required permission test cases

- [ ] User A cannot read or update User B's workspace records by changing an ID.
- [ ] A removed member loses access promptly, including active sessions as policy requires.
- [ ] A user with read-only access cannot mutate records through direct API calls.
- [ ] A sales user cannot view restricted payroll data or provider secrets.
- [ ] An HR user cannot run payroll unless explicitly granted that permission.
- [ ] A finance user cannot manage workspace roles unless separately granted.
- [ ] A public share token grants access only to its exact document and allowed actions.
- [ ] Revoked or expired public tokens stop working.
- [ ] An AI action cannot read or modify records outside the caller's permissions.
- [ ] Platform support access is denied by default and any approved elevation is time-limited and audited.
- [ ] Exports and bulk operations enforce the same permissions as record-by-record access.
- [ ] Branch/location restrictions are enforced if multi-branch support is enabled.

## 7. Approval

This matrix is a proposal. The product owner, domain leads, and security reviewer must approve it before migrations or authorization code are written. Record changes in `docs/DECISIONS.md` and link them to test cases.
