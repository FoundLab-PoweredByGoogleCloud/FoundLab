---
title: "GitHub Enterprise Cloud — SOC 3 (Security) Report"
doc_type: "third_party_assurance"
publisher: "GitHub, Inc."
service: "GitHub Enterprise Cloud"
assurance: "SOC 3® — Trust Services Criteria (Security)"
auditor: "Coalfire"
report_date: "2025-06-02"
period_start: "2024-10-01"
period_end: "2025-03-31"
method: "Carve-out (Subservice orgs excluded)"
scope_note: "Security (TSC) only"
repo_owner: "FoundLab-PoweredByGoogleCloud"
repo: "Foundlab-GooglePartner"
status: "ACTIVE_REFERENCE"
classification: "Partner Docs / Due Diligence"
foundlab_owner: "Office of the CTO"
last_reviewed: "2025-12-31"
---


# GitHub Enterprise Cloud — SOC 3® (Security)

**Report Date:** 2025-06-02  
**Period Covered:** 2024-10-01 → 2025-03-31  
**Auditor:** Coalfire (Independent Service Auditor)  
**Assurance:** SOC 3® (Trust Services Criteria — Security)  
**Method:** **Carve-out** (subservice organizations excluded)

> This document is a **FoundLab-hardened** representation of the SOC 3 report for partner due diligence.
> It focuses on: **(1) scope, (2) carve-outs, (3) required complementary controls (CUECs), (4) what FoundLab must do**.

---

## Table of Contents

- [1. Executive Summary](#1-executive-summary)
- [2. Auditor Opinion (What it actually says)](#2-auditor-opinion-what-it-actually-says)
- [3. Scope & System Boundaries (Attachment A)](#3-scope--system-boundaries-attachment-a)
- [4. Carve-outs & Dependencies](#4-carve-outs--dependencies)
  - [4.1 Subservice Organizations](#41-subservice-organizations)
  - [4.2 Complementary User Entity Controls (CUECs)](#42-complementary-user-entity-controls-cuecs)
- [5. FoundLab Inheritance Model](#5-foundlab-inheritance-model)
  - [5.1 Control Mapping: GitHub → FoundLab](#51-control-mapping-github--foundlab)
  - [5.2 Evidence Pack (What we must produce)](#52-evidence-pack-what-we-must-produce)
- [6. Operational Checklist (Build + Run)](#6-operational-checklist-build--run)
- [7. Risk Register (If auditors get hostile)](#7-risk-register-if-auditors-get-hostile)
- [8. Excerpt Policy (Copyright-safe)](#8-excerpt-policy-copyright-safe)
- [Appendix: Quick Facts](#appendix-quick-facts)

---

## 1. Executive Summary

GitHub’s SOC 3 report provides reasonable assurance that **security controls** for **GitHub Enterprise Cloud** were effective throughout the stated period **under explicit assumptions**:

1) **Customer implements CUECs** (Complementary User Entity Controls)  
2) **Subservice organizations implement their complementary controls** (because they are **excluded** from scope)

**Takeaway for FoundLab:**  
FoundLab can rely on GitHub Enterprise Cloud as a third-party component with security assurance, but **MUST implement and evidence CUECs** to avoid being flagged as the weak link during customer audits.

---

## 2. Auditor Opinion (What it actually says)

The independent auditor’s opinion states that GitHub’s controls were effective throughout the period to meet applicable **Security** Trust Services Criteria **IF**:

- complementary controls at **subservice organizations** operated effectively, and
- complementary controls at **user entities (customers)** operated effectively.

This is the critical compliance detail: **SOC 3 is not end-to-end assurance** when using carve-outs and CUECs.

---

## 3. Scope & System Boundaries (Attachment A)

### In-scope Service
**GitHub Enterprise Cloud**, including (as described in the report boundaries):
- core collaboration features (repos, PRs, issues, discussions, wikis, projects, docs, etc.)
- GitHub Actions
- GitHub Advanced Security (if enabled)
- GitHub Copilot (if enabled)
- optional Data Residency offering (if enabled)

### Included Boundary Dimensions
The report describes the system boundary across:
- Infrastructure
- Software
- People
- Procedures / policies
- Data

### Notable Boundary Characteristics
- GitHub uses a combination of third-party infrastructure hosting and GitHub-managed colocation sites.
- Governance references: internal policies, standards, operational procedures, security program structure.

---

## 4. Carve-outs & Dependencies

### 4.1 Subservice Organizations

GitHub uses subservice organizations for:
- colocation data center services
- infrastructure hosting

**These subservice organizations are excluded** from the examination scope (carve-out method). GitHub expects these providers to operate certain control categories (primarily physical and environmental controls, among others).

#### FoundLab Implication
FoundLab MUST treat this as a dependency that requires:
- vendor evidence review (SOC 2/ISO where applicable)
- a risk register entry
- compensating controls where necessary

---

### 4.2 Complementary User Entity Controls (CUECs)

The report states that GitHub’s control design assumes that **customers implement certain controls**. The auditor explicitly did not test customer controls.

#### FoundLab Required CUECs (Minimum Baseline)

> These are **non-negotiable** for enterprise due diligence, especially Tier-1 banks.

##### Identity & Access
- Enforce SSO/SAML for the GitHub org/enterprise
- Enforce MFA
- Minimize organization owners (break-glass only)
- Least privilege for teams and repos
- Quarterly access reviews (or more frequent in high-risk programs)

##### Repository Security
- Branch protection rules (no force push)
- Required reviews / approvals
- Required status checks (CI)
- Signed commits/tags (where applicable)
- Secret scanning enabled (and push protection if available)

##### Audit & Monitoring
- Audit log retention configured
- Alerting on sensitive administrative actions
- Incident workflow referencing GitHub telemetry / audit events

##### CI/CD (GitHub Actions)
- Restrict `GITHUB_TOKEN` permissions (least scope)
- Pin actions by SHA (avoid mutable tags)
- Use protected environments for deploy
- Harden self-hosted runners (if used) or explicitly avoid them

##### Copilot (If Enabled)
- Admin policy governance enabled
- Exclusion of sensitive files/content configured
- Confirm non-retention claims per product terms are acceptable to customer policy
- Validate that customer policy accepts AI tooling in SDLC

---

## 5. FoundLab Inheritance Model

### 5.1 Control Mapping: GitHub → FoundLab

This section answers the audit question:
> “What controls do you inherit from GitHub, and what do you own?”

| Control Domain | Inherited from GitHub | Owned by FoundLab | Evidence Required |
|---|---:|---:|---|
| Platform security controls (service-side) | ✅ | ❌ | SOC 3 reference + vendor artifacts |
| IAM enforcement for FoundLab org | ❌ | ✅ | SSO/MFA config exports, access review logs |
| Repo hardening (branch protection, CODEOWNERS) | ❌ | ✅ | Policy screenshots/export + rulesets |
| CI/CD hardening (Actions policies) | ❌ | ✅ | Workflow policies, pinned SHAs, env protections |
| Secret scanning posture | Partially (feature availability) | ✅ (enable/config) | Feature enablement proof + alerts config |
| Incident response for GitHub events | ❌ | ✅ | Runbook + ticket evidence (tabletop OK) |
| Subservice org physical controls | Assumed | ✅ (risk mgmt) | Vendor review checklist, risk register |

---

### 5.2 Evidence Pack (What we must produce)

Create/maintain: `evidence/github/` (private repo or internal vault)

Minimum artifacts:
- SSO/SAML enforcement proof
- MFA enforcement proof
- Org owners list + break-glass policy
- Quarterly access review record (date, reviewer, outcomes)
- Ruleset export (branch protection, required checks, review rules)
- Actions security posture:
  - token permissions default
  - allowed actions policy
  - pinned actions evidence
- Secret scanning + push protection status
- Audit logging retention and review SOP
- Incident runbook mapping GitHub events → FoundLab response actions
- Vendor dependency review notes (subservice org carve-out awareness)

---

## 6. Operational Checklist (Build + Run)

### Build-time (MUST)
- [ ] Enforce SSO/SAML at enterprise/org level
- [ ] Enforce MFA
- [ ] Configure org rulesets (branch protection + required checks)
- [ ] Enable secret scanning (and push protection if licensed)
- [ ] Configure Actions policies (least privilege + pinning)
- [ ] Configure audit log retention and review cadence
- [ ] Create incident runbook for GitHub compromise scenarios

### Run-time (MUST)
- [ ] Quarterly access reviews completed
- [ ] Monitor audit events for high-risk actions (owners added, tokens created, settings changed)
- [ ] Rotate credentials and review tokens periodically
- [ ] Validate Actions workflows are pinned and reviewed
- [ ] Execute tabletop for “GitHub org compromised” scenario at least annually

---

## 7. Risk Register (If auditors get hostile)

| Risk | Likelihood | Impact | Mitigation | Residual |
|---|---:|---:|---|---|
| SOC 3 is carve-out; subservice org controls not examined | Medium | Medium | Maintain vendor evidence + document dependency | Low |
| CUECs not implemented → audit fail at customer | Medium | High | Implement baseline CUECs + evidence pack | Low |
| Actions supply-chain compromise (unpinned actions) | Medium | High | SHA pinning + allowed actions policy | Low |
| Excess org owners / weak break-glass | Low | High | Minimize owners + strict process | Low |
| Copilot policy conflicts with customer requirements | Medium | Medium | Default off; explicit opt-in with policy docs | Medium |

---

## 8. Excerpt Policy (Copyright-safe)

This file is a **structured summary** and control interpretation for FoundLab due diligence.

If short excerpts are required for internal evidence:
- Limit to small snippets only (≤ 25 words each)
- Always attribute source to the original report file
- Avoid reproducing entire sections verbatim

---

## Appendix: Quick Facts

- **Report:** SOC 3® (Security)
- **Service:** GitHub Enterprise Cloud
- **Period:** 2024-10-01 → 2025-03-31
- **Report date:** 2025-06-02
- **Auditor:** Coalfire
- **Key audit caveat:** effectiveness asserted **conditional on** CUECs + subservice controls
