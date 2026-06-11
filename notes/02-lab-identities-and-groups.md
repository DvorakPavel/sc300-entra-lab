# Task 2 – Lab Identities and Groups

**Date:**
2026-03-30

## Goal

Create a clean custom identity layer inside the sandbox for future SC-300 lab tasks.

## Users created

- lab-admin: Daily admin account
- lab-bg-admin-01: Emergency access (break-glass) account #1
- lab-bg-admin-02: Emergency access (break-glass) account #2
- lab-user-01 through lab-user-06: Standard lab users

All accounts are cloud-only identities with Usage Location set and temporary passwords assigned.

## Groups created

- GRP-LAB-IT: Security group (lab-admin, lab-user-01, lab-user-02)
- GRP-LAB-HR: Security group (lab-user-03, lab-user-04)
- GRP-LAB-FIN: Security group (lab-user-05, lab-user-06)
- GRP-LAB-ADMINS: Role-assignable security group (lab-admin)

## What I did

- Created 9 cloud-only lab users with consistent naming convention (lab-* prefix).
- Created 4 security groups representing departmental and administrative structure.
- Made GRP-LAB-ADMINS a role-assignable group at creation time.
- Assigned group memberships based on department.
- Left existing sample tenant users untouched.

## Result

A dedicated custom lab identity structure was created without modifying the existing sample data.

## Lessons learned

- Keeping sample data untouched preserves the sandbox baseline for troubleshooting.
- A role-assignable group must be marked as such at creation — it cannot be converted later.
- Break-glass accounts should remain simple with no group memberships or conditional policies.
- Consistent naming conventions (lab-*, GRP-LAB-*) make policies and assignments cleaner.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-02-lab-identities-and-groups/
```
