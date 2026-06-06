# Task 28 – Authentication Context & Conditional Access

**Date:**
2026-06-06

## Goal

Create an Authentication Context and link it to a Conditional Access policy to enable step-up authentication for access to sensitive resources.

## Authentication Context

- Name: Require step-up for sensitive access
- ID: C1
- Published to apps: Yes

## Conditional Access policy

- Policy name: CA-Require-StepUp-Sensitive-Access
- Users: All users
- Exclusions: Lab BG Admin 01, Lab BG Admin 02
- Target resources: Authentication context — Require step-up for sensitive access (C1)
- Grant control: Require multifactor authentication
- Policy state: Report-only

## What I did

- Created an Authentication Context for sensitive resource access.
- Published the Authentication Context to apps.
- Created a Conditional Access policy targeting the Authentication Context.
- Excluded emergency access accounts from the policy.
- Verified the policy in the Conditional Access policies list.

## Result

An Authentication Context was created and linked to a Conditional Access policy. The policy evaluates only when a user accesses a resource tagged with the Authentication Context, enabling step-up authentication without affecting regular sign-ins.

## Lessons learned

- Authentication Context defines a tag that triggers Conditional Access evaluation mid-session rather than at sign-in.
- The CA policy must target the Authentication Context in the Target resources section, not Cloud apps.
- Authentication Context requires integration with sensitivity labels or SharePoint site classification to be fully operational.
- Step-up authentication provides an additional layer of access control for sensitive resources without increasing friction for routine access.
- Emergency access accounts must be excluded from step-up policies.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-28-authentication-context/
```
