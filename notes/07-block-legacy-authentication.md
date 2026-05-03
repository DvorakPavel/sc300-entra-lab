# Task 7 – Block legacy authentication with Conditional Access

**Date:**  
2026-04-15

## Goal
Create a Conditional Access policy to block legacy authentication across the tenant.

## Policy name
CA-Block-Legacy-Authentication

## Scope
- Users/Groups: All users
- Exclusions: bg-admin-01, bg-admin-02
- Resources: All resources
- Condition: Legacy authentication clients
- Grant control: Block access
- Initial state: Report-only
- Final state: On

## What I did
- Created a Conditional Access policy to block legacy authentication.
- Scoped the policy to all users.
- Excluded the two break-glass accounts.
- Targeted all resources.
- Configured the policy to apply only to legacy authentication clients.
- Set the grant control to block access.
- Started in Report-only mode to safely review impact before enforcement.
- Enabled the policy after validation.

## Result
A tenant-wide Conditional Access policy was created to block legacy authentication attempts while preserving emergency access exclusions.

## Lessons learned
- Legacy authentication does not support MFA and should be blocked rather than challenged.
- Broad Conditional Access policies must exclude emergency access accounts.
- Report-only mode is a safer first step before enforcing tenant-wide blocking controls.
- Break-glass accounts should be tested before creating any broad access policy.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-07-block-legacy-auth/
```
