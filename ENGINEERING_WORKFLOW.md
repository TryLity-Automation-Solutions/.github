# TryLity Engineering Workflow

This document is the reference guide for how TryLity-Automation-Solutions runs engineering work on GitHub: issue tracking, the central project board, labels, pull requests, and the QA lifecycle. It reflects the actual configuration in this organization as of the date below — nothing here describes a process that has not been set up.

_Last updated: 2026-09-22_

## 1. Overview & purpose

TryLity runs all engineering work — bugs, features, tasks, tech debt, improvements, and incidents — natively on GitHub, using GitHub Issues, native Issue Types and Fields, and a single org-level Project (**Engineering Hub**) as the source of truth. This replaces the need for a separate tool such as Jira, Linear, or Trello. Everything described below uses GitHub's built-in features; no GitHub Actions workflows or third-party automation were introduced.

## 2. Repository inventory

The organization currently has 12 repositories. Eleven are active product/engineering repositories wired into the Engineering Hub project and given the shared issue/PR configuration described in this document; the twelfth, `.github`, is this org-wide reference repository itself (see section 14).

| Repository | Default branch |
|---|---|
| trylityBackend-v2 | main |
| trylity-backend-services | main |
| trylity-super-admin | master |
| trylity-workers-2 | main |
| worker-payment | main |
| worker-send-message | main |
| worker-broadcast | main |
| worker-media | main |
| wa_media | main |
| trylity-android | main |
| nexa | main |

## 3. The central project: Engineering Hub

**Engineering Hub** (org Project #6) is the single board all engineering issues and pull requests flow through, across all 11 repositories. It is private to the organization.

URL: `https://github.com/orgs/TryLity-Automation-Solutions/projects/6`

## 4. Issue Types

Six org-wide native Issue Types are available on every repository:

- Task
- Bug
- Feature
- Tech Debt
- Improvement
- Incident

## 5. Custom fields

The following native Issue Fields / Project fields drive triage, filtering, and reporting:

- **Status** — the lifecycle stage (see section 13)
- **Priority** — P0-Critical, P1-High, P2-Medium, P3-Low
- **Severity** — Critical, High, Medium, Low
- **Module** — the affected part of the system (Authentication, Broadcast, WhatsApp, Ecommerce, Catalog, Inventory, Payments, Worker, API, Frontend, Admin, Infrastructure, Database, Other)
- **Environment** — Production, Staging, Development, Local, Other
- **Target Release** — free-text release/version identifier
- **Sprint** — the sprint an item is scheduled into

New items default to **Status: Backlog**, **Priority: P2-Medium**, **Type: Task** unless set otherwise at creation.

## 6. Labels

Every repository carries a consistent label set, including (non-exhaustive): `bug`, `needs-info`, and `blocked` (blocked by another issue, dependency, or external factor). Labels are kept consistent by name and color across all 11 repositories.

## 7. Project views

Engineering Hub has 8 saved views:

1. **All Work** — the full board, grouped by Status
2. **Bugs** — issues of Type = Bug
3. **QA Queue** — items in the QA Testing status
4. **Critical Bugs** — Bug-type issues at P0/P1 priority or Critical severity
5. **My Work** — items assigned to the current viewer
6. **By Repository** — grouped by source repository
7. **Current Sprint** — items in the active Sprint
8. **Release** — grouped/filtered by Target Release

## 8. Reporting a bug

Every repository has a **"🐛 Bug Report"** issue form (`.github/ISSUE_TEMPLATE/bug.yml`) reachable from that repo's "New issue" page. Submitting it automatically:

- Sets the native Issue Type to **Bug**
- Applies the `bug` label
- Asks for description, steps to reproduce, expected vs. actual behavior, environment, severity, module, target release, and logs/context

Reporters are asked to search existing issues first to avoid duplicates.

## 9. Branching & commits (recommended convention)

GitHub does not enforce branch names, so this is a recommended convention for consistency rather than a configured restriction:

- Branch names: `type/short-description` (e.g. `fix/broadcast-quota-rollback`, `feat/catalog-bulk-import`)
- Reference the issue number in the branch name or first commit where practical (e.g. `fix/158-broadcast-quota`)
- Keep commits scoped and descriptive; squash-merge is recommended for a clean history

## 10. Pull requests & code review

Every repository has a shared PR template (`.github/PULL_REQUEST_TEMPLATE.md`) that prompts for: description, related issue (with a `Closes #` line so merging auto-closes the linked issue), type of change, testing notes, a review checklist, screenshots, and additional context.

Link a PR to its issue (e.g. `Closes #123`) so the native automation in section 12 can move the issue through Status automatically.

## 11. Code ownership (CODEOWNERS)

No `CODEOWNERS` file has been created in any repository. This was a deliberate decision rather than an oversight: per the standing rule never to invent GitHub usernames or ownership data, a CODEOWNERS file was only going to be created if real, verifiable ownership data existed to base it on.

What was actually verified (all real, from the org's People and Teams pages, not invented):

- **5 org members**: abhay9061, Gaurikr162, Shubhamsinghal1998, skcodar, yourresult (Owner)
- **2 teams**: `core-dev-team` (skcodar, Shubhamsinghal1998, Gaurikr162 — broad Write/Maintain/Read access across trylity-backend-services, trylity-super-admin, trylity-workers-2, trylityBackend-v2, worker-broadcast, worker-payment, worker-send-message) and `permission-to-deploy` (1 member, deployment permission only, not a code-ownership grouping)
- **7 outside collaborators**, each with direct per-repo access:
  - Bhavna2918 → trylityBackend-v2, trylity-super-admin, worker-send-message
  - Deeprajojha1 → trylityBackend-v2, worker-payment
  - ErSohrab → trylityBackend-v2
  - kajalchandra → trylityBackend-v2
  - nallaperumal007 → trylity-android
  - priyanshudas006 → trylityBackend-v2, trylity-workers-2, worker-send-message
  - rishavkumar003 → nexa

This is repository **access** data, not per-path **ownership** data — several people often share write access to the same repository, and access alone doesn't say who should be the required reviewer for a given path or module. Turning this into a CODEOWNERS file would mean guessing ownership intent rather than reading it off real data, which is exactly what the standing rule was meant to avoid. **This is left as a manual action** — see section 14 — with the real access table above provided so it can be turned into a CODEOWNERS file quickly once ownership per module/path is decided by the team.

## 12. Native project automation

Engineering Hub uses GitHub's built-in Project workflows exclusively (no GitHub Actions). 7 of 8 default workflows are enabled:

| Workflow | Trigger | Effect |
|---|---|---|
| Item added to project | Issue/PR added | Status → Backlog |
| Auto-add to project | New/updated item matches filter | Adds item to the project (see limitation below) |
| Auto-add sub-issues to project | Item gains sub-issues | Sub-issues added to the project |
| Pull request linked to issue | PR linked to an issue | Status → In Progress |
| Pull request merged | PR merged | Status → Done |
| Item closed | Issue/PR closed | Status → Done |
| Auto-close issue | Status set to Done | Closes the issue |
| Auto-archive items | — | **Off** (left disabled so items stay visible on the board) |

**Known limitation:** the org's current GitHub plan allows only **one** "Auto-add to project" workflow, and it can only be scoped to a single repository (currently `trylityBackend-v2`). The other 10 repositories do not automatically add new issues/PRs to Engineering Hub — items there need to be added manually (drag into the project, or "Add item" from the project toolbar) until the plan is upgraded. This is a plan limit, not a configuration gap.

GitHub's native workflows also have no trigger for "assignee added" or a distinct "PR opened → Code Review" transition. In practice: move an item to **Assigned** manually when someone is assigned, and to **Code Review** manually when a PR is opened for review (or is marked ready for review) — the **In Progress** and **Done** transitions above still happen automatically.

## 13. Status lifecycle (QA workflow)

The **Status** field defines the full lifecycle, left to right:

**Backlog → Triage → Assigned → In Progress → Code Review → QA Testing → Reopened → Done**

Typical flow for a bug: reported via the Bug Report form (→ Backlog) → triaged and prioritized (→ Triage) → an engineer is assigned (→ Assigned) → work begins (→ In Progress, automatic once a PR is linked) → a PR is opened for review (→ Code Review, manual) → the PR merges (→ Done, automatic) or the fix fails QA (→ Reopened, manual) and the cycle repeats from Assigned.

## 14. Known limitations & manual actions required

- **Auto-add to project** only covers `trylityBackend-v2` (GitHub plan limit — see section 12). Issues/PRs in the other 10 repositories must be added to Engineering Hub manually until the plan is upgraded.
- **CODEOWNERS** has not been created in any repository — see section 11 for the real access data gathered and why it wasn't turned into an ownership file without a human decision on actual per-module ownership.
- **Org-wide issue/PR template propagation**: GitHub only propagates a `.github` repository's issue/PR templates to other repositories when that `.github` repository is **public**. Since this organization's repositories are private, the Bug Report form and PR template were instead deployed individually to all 11 repositories directly (see sections 8 and 10) rather than making `.github` public. This repository (`.github`) is kept private and now serves as the org's central reference documentation (this file) plus a reference copy of the bug form for any future new repository to copy from — it does not auto-apply anything.
- **Billing**: the organization's GitHub billing shows "overdue for payment" — this is a billing/account matter for the org owner to resolve directly with GitHub and was intentionally left untouched by this configuration work.
- The organization currently has only one Owner (yourresult) — GitHub itself recommends at least two owners for redundancy.
