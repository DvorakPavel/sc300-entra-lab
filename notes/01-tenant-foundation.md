# Task 1 – Tenant Foundation

**Date:**
2026-03-30

## Goal

Verify access to the Microsoft 365 Developer sandbox and confirm the tenant foundation for the SC-300 lab.

## Tenant configuration

- Tenant type: Microsoft 365 E5 Developer Sandbox
- Primary domain: *.onmicrosoft.com (redacted)
- Licenses: Microsoft 365 E5, Entra ID P2
- Azure subscription: None (Entra-only lab)
- Default user settings: Verified in Entra admin center

## Admin access verified

- Microsoft 365 admin center: Confirmed
- Microsoft Entra admin center: Confirmed
- Signed in with sandbox admin account (Global Administrator)

## What I did

- Opened the subscription from the Microsoft 365 Developer dashboard.
- Signed in with the sandbox admin account.
- Verified access to the Microsoft 365 admin center and Microsoft Entra admin center.
- Confirmed tenant name, primary domain, and tenant ID.
- Reviewed default tenant settings and license assignments.

## Result

The sandbox tenant was confirmed as active and ready for SC-300 lab tasks.

## Lessons learned

- The sandbox uses a separate cloud-only admin identity — not linked to a personal Microsoft account.
- Confirming tenant identity early helps avoid working in the wrong directory.
- M365 Developer tenant includes Entra ID P2, which covers Identity Protection, PIM, and Access Reviews without a separate Azure subscription.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-01-tenant-foundation/
```
