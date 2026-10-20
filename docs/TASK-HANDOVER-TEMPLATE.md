# ZetuBIZ Task Handover Template

**Purpose:** Make every task transferable between people, AI models, coding assistants, and development environments without relying on private conversation history.

Copy this template into the relevant issue, pull request, or task record. Delete instructional notes before finalizing where appropriate.

---

## 1. Task identity

- **Task ID / issue:**
- **Title:**
- **Task owner:**
- **Reviewer(s):**
- **Status:** Backlog / Ready for documentation / In progress / Needs decision / In review / Changes requested / Approved / Blocked / Done / Deferred
- **Priority:**
- **Related requirements and documents:**
- **Current branch:**
- **Latest commit SHA or PR:**

## 2. Goal and scope

**Problem to solve:**  
Describe the user or business problem, not just the proposed technical change.

**Expected outcome:**  
Describe what should be true when the task is complete.

**In scope:**
- 

**Out of scope:**
- 

**Assumptions:**
- Mark each assumption as unverified until evidence confirms it.

## 3. Acceptance criteria

- [ ] 
- [ ] 
- [ ] 

Criteria must be observable and testable. Link each criterion to requirement IDs where available.

## 4. Context read

List the authoritative files, sections, issues, or decisions reviewed. Do not assume the next contributor has the previous conversation.

- AGENTS.md:
- docs/TEAM-WORKFLOW.md:
- Task-specific specifications:
- Relevant decision records:
- Relevant database/API contracts:
- Related issues or PRs:

## 5. Work completed

For each change, state what changed, why, and link the exact file, commit, or PR.

| File or resource | Change made | Reason / requirement |
|---|---|---|
|  |  |  |

Do not describe proposed work as completed work.

## 6. Verification and evidence

- **Checks/tests run:**
- **Exact command or procedure:**
- **Result:** Pass / Fail / Not run
- **Evidence or logs:**
- **Manual checks performed:**
- **Checks not performed and why:**

Only report results actually observed. If no test was run, say so explicitly.

## 7. Decisions and unresolved questions

**Decisions made:**  
Include a link to the approved decision record. A conversation message alone is not a permanent decision record.

**Open questions:**
- Question:
- Impact if unresolved:
- Person or role needed to decide:

**Conflicts with existing documentation:**  
List any contradictions discovered. Do not silently choose between conflicting specifications.

## 8. Risks, defects, and dependencies

- **Known defects:**
- **Security/privacy/tenant-isolation impact:**
- **Data migration or compatibility impact:**
- **External dependencies or provider assumptions:**
- **Blocked by:**
- **Rollback or recovery considerations:**

Never include passwords, API keys, tokens, production secrets, or unnecessary real customer data.

## 9. Next action

- **Exact next task:**
- **Next owner:**
- **Acceptance criteria for next task:**
- **Dependencies that must be resolved first:**
- **Suggested starting files or sections:**

The next action must be specific enough for a new contributor to start without asking the previous contributor to reconstruct their context.

## 10. AI-tool portability checklist

- [ ] Relevant findings are recorded in the repository or task record.
- [ ] No critical requirement exists only in an AI conversation or hidden prompt.
- [ ] No provider-specific feature is required unless explicitly approved.
- [ ] Generated suggestions were checked against the approved specifications.
- [ ] Secrets and unnecessary personal/customer data were not supplied to an AI tool or committed.
- [ ] A human reviewer is identified where review is required.
- [ ] The current documentation approval gate has been respected.

## 11. Final handover summary

**Current state:**  
Summarize the verified state in 2–5 sentences.

**What the next contributor should do first:**  
Give one concrete first action.

**Prepared by:**  
**Date:**  
**Reviewer / receiver acknowledgement:**

---

## Example of a good status update

> Completed the draft customer lifecycle requirements and linked them to the UX specification. Reviewed the existing master specification and recorded two unresolved questions in the decision log. No application code was changed. The document is awaiting product-owner review. Next action: the product owner decides the customer-merge policy before the data model is approved.

## Completion rule

A handover is not complete if it only says “done,” “continue,” or “the AI knows.” The repository/task record must contain enough context, evidence, and next steps for another authorized person or AI tool to continue independently.
