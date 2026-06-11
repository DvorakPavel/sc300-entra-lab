# Task 3 – Admin Role Model

**Date:**
2026-03-31

## Goal

Implement a structured admin model with a daily admin account, a role-assignable admin group, and two emergency access accounts.

## Role assignments

- GRP-LAB-ADMINS → User Administrator (indirect assignment via group)
- lab-admin → User Administrator (inherited through GRP-LAB-ADMINS membership)
- lab-bg-admin-01 → Global Administrator (direct permanent assignment)
- lab-bg-admin-02 → Global Administrator (direct permanent assignment)

## Admin model

- Bootstrap admin: Initial tenant admin (sandbox account)
- Daily admin: lab-admin (least-privilege via User Administrator role)
- Emergency access: lab-bg-admin-01, lab-bg-admin-02 (Global Administrator, no MFA, no CA)

## What I did

- Verified that GRP-LAB-ADMINS was created as a role-assignable group.
- Assigned the User Administrator role to GRP-LAB-ADMINS.
- Confirmed that lab-admin receives User Administrator privileges indirectly through group membership.
- Assigned the Global Administrator role directly to both break-glass accounts.
- Verified role inheritance by checking effective permissions of lab-admin.

## Result

The lab has a tiered administrative model: daily operations use the least-privilege User Administrator role via group membership, while emergency access accounts hold Global Administrator as a safety net.

## Lessons learned

- Role-assignable groups must be created correctly from the start — they cannot be converted later.
- Indirect role assignment through groups is cleaner and more scalable than per-user assignments.
- Emergency access accounts should have permanent Global Admin, no MFA, and no Conditional Access — their purpose is to survive any lockout scenario.
- The daily admin account should use the minimum role required, not Global Administrator.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-03-admin-role-model/
```
