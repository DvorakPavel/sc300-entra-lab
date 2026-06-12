# SC-300 Entra ID Lab — Hands-on Identity & Access Management

33 hands-on tasks. Real M365 tenant. Real problems. Real fixes.

I built a full Microsoft Entra ID environment from scratch — tenant hardening, Conditional Access, Identity Protection, PIM, entitlement management, PowerShell bulk operations, and more — documenting every configuration, every troubleshooting session, and every lesson learned along the way.

Every task maps directly to an [SC-300 exam objective](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/sc-300). Every screenshot is real evidence from a live tenant. The notes don't just show *what* was configured — they show *what went wrong* and how it was fixed.

## Environment

| Component | Detail |
|---|---|
| Tenant type | Microsoft 365 E5 Developer |
| License | Entra ID P2 |
| Azure subscription | None (Entra-only lab) |
| Users | 6 lab users, 2 break-glass admins, 1 admin account, sample users |
| Tools used | Entra admin center, PowerShell (Microsoft Graph SDK), Graph Explorer |

## Lab Tasks & SC-300 Exam Mapping

### Domain 1 — Implement and Manage User Identities (20–25%)

| Task | Title | Skills covered |
|---|---|---|
| 01 | [Tenant Foundation](notes/01-tenant-foundation.md) | Tenant configuration, branding, user/group settings |
| 02 | [Lab Identities and Groups](notes/02-lab-identities-and-groups.md) | Create and manage users and groups |
| 03 | [Admin Role Model](notes/03-admin-role-model.md) | Built-in Entra roles, least privilege, break-glass accounts |
| 04 | [Authentication Foundation & SSPR](notes/04-authentication-foundation-and-sspr-pilot.md) | Self-service password reset, authentication methods |
| 05 | [End User Registration](notes/05-end-user-registration-and-first-auth-test.md) | User registration experience, MFA enrollment |
| 19 | [External Identities — B2B Guest Access](notes/19-external-identities-b2b-guest-access.md) | External collaboration, guest user lifecycle |
| 24 | [Administrative Units](notes/24-administrative-units.md) | Scoped administration, restricted management |
| 30 | [Custom Security Attributes](notes/30-custom-security-attributes.md) | Attribute sets, definitions, assignments |
| 31 | [Cross-tenant Access Settings](notes/31-cross-tenant-access-settings.md) | Inbound/outbound trust, MFA trust, device compliance |
| 32 | [Bulk Operations — PowerShell & Graph](notes/32-bulk-operations-powershell-graph.md) | Graph SDK, bulk user updates, role assignment reports, OData queries |

### Domain 2 — Implement Authentication and Access Management (25–30%)

| Task | Title | Skills covered |
|---|---|---|
| 06 | [First Conditional Access Baseline](notes/06-first-conditional-access-baseline.md) | CA policy design, pilot deployment |
| 07 | [Block Legacy Authentication](notes/07-block-legacy-authentication.md) | Legacy protocol blocking |
| 08 | [Broad MFA — Report-only](notes/08-broad-mfa-report-only.md) | Tenant-wide MFA, report-only monitoring |
| 09 | [Risk-based Conditional Access](notes/09-risk-based-ca.md) | Sign-in risk, user risk policies |
| 10 | [Authentication Strengths](notes/10-authentication-strengths.md) | Authentication strength policies, phishing-resistant MFA |
| 20 | [Identity Protection](notes/20-identity-protection.md) | Risk detection, remediation, risk-based policies |
| 21 | [Terms of Use](notes/21-terms-of-use.md) | ToU creation, CA integration |
| 22 | [Named Locations](notes/22-named-locations.md) | Trusted/untrusted locations, country-based blocking |
| 27 | [Password Protection](notes/27-password-protection.md) | Banned password lists, password policies |
| 28 | [Authentication Context](notes/28-authentication-context.md) | Authentication contexts, step-up authentication |
| 29 | [CA Session Management](notes/29-ca-session-management.md) | Sign-in frequency, persistent sessions, CAE |
| 33 | [Tenant Hardening & Final Review](notes/33-tenant-hardening-final-review.md) | Report-only to Enforced lifecycle, Secure Score optimization |

### Domain 3 — Plan and Implement Workload Identities (20–25%)

| Task | Title | Skills covered |
|---|---|---|
| 14 | [App Registration](notes/14-app-registration.md) | App registrations, redirect URIs, credentials |
| 15 | [Enterprise Applications](notes/15-enterprise-applications.md) | Enterprise app configuration, SSO |
| 16 | [Conditional Access — Enterprise App](notes/16-conditional-access-enterprise-app.md) | App-targeted CA policies |
| 17 | [App Roles & Group-based Access](notes/17-app-roles-and-group-based-enterprise-app-access.md) | App roles, group-based assignment |
| 18 | [User Consent & Admin Consent Workflow](notes/18-user-consent-and-admin-consent-workflow.md) | Consent settings, admin consent workflow |
| 23 | [Workload Identities](notes/23-workload-identities.md) | Service principals, managed identities |

### Domain 4 — Plan and Automate Identity Governance (20–25%)

| Task | Title | Skills covered |
|---|---|---|
| 11 | [PIM — Admin Role](notes/11-pim-admin-role.md) | PIM activation, eligible vs active assignments |
| 12 | [Access Reviews](notes/12-access-reviews.md) | Access review creation, monitoring, remediation |
| 13 | [Entitlement Management](notes/13-entitlement-management.md) | Catalogs, access packages, request policies |
| 25 | [Sign-in & Audit Logs](notes/25-sign-in-audit-logs.md) | Log analysis, filtering, troubleshooting |
| 26 | [Identity Secure Score](notes/26-identity-secure-score.md) | Score analysis, improvement actions |

## Final Tenant Security Posture

| Metric | Value |
|---|---|
| Conditional Access policies | 11 total (8 enforced, 3 report-only) |
| Break-glass accounts | 2 (excluded from all CA policies) |
| Admin account | Covered by all CA policies |
| MFA enforcement | All users and admins |
| Risk-based policies | Sign-in risk + User risk (enforced) |
| Legacy authentication | Blocked |
| Identity Secure Score | Baseline 38% → XX% |

## Repository Structure

```
sc300-entra-lab/
  notes/          # Configuration notes per task (what, why, how, lessons learned)
  evidence/       # Redacted screenshots organized by task
  README.md
```

## Key Takeaways

Things I learned the hard way — not from docs, but from doing:

- **"Require authentication strength" ≠ "Require MFA"** — These two CA Grant controls look equivalent, but behave differently. The authentication strength framework is stricter and can reject sign-ins that classic MFA would accept.
- **Phishing-resistant MFA needs hardware** — No FIDO2 key and no Bluetooth on desktop = no passkey authentication. Plan your MFA strategy around what your users actually have.
- **Global Admin can't do everything** — Custom Security Attributes require a dedicated role (Attribute Definition Administrator). Global Admin is not enough.
- **CAE is on by default** — There's no "enable" button. The Conditional Access option is for restricting it, not turning it on.
- **Report-only → Enforced is the real workflow** — Deploy in report-only, monitor sign-in logs for impact, then enforce. This lifecycle was applied across all 11 CA policies in this lab.

## Tools & Technologies

Microsoft Entra ID, Conditional Access, Identity Protection, PIM, Entitlement Management, Access Reviews, Custom Security Attributes, Administrative Units, Named Locations, Authentication Contexts, Authentication Strengths, App Registrations, Enterprise Applications, External Identities (B2B), PowerShell (Microsoft Graph SDK), Graph Explorer, Identity Secure Score

## Exam Reference

This lab is aligned with the [SC-300 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/sc-300) (measured as of April 27, 2026)

## Author

**Pavel Dvořák** — [LinkedIn](https://www.linkedin.com/in/pavel-dvorak88) | [GitHub](https://github.com/DvorakPavel)
