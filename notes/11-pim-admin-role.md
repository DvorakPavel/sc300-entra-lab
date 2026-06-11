# Task 11 – PIM for Group-based Admin Access

**Date:**
2026-04-18

## Goal

Use Privileged Identity Management (PIM) for a role-assignable group that carries a Microsoft Entra administrative role.

## PIM configuration

- Group: GRP-LAB-PIM-TEST
- Role carried by group: User Administrator
- PIM scope: Group membership (eligible assignment)
- Activation duration: Time-limited (default 8 hours)
- Approval required: No (lab environment)
- Justification required: Yes

## Assignment

- Eligible member: Lab admin account
- Assignment type: Eligible (not active — must be activated on demand)

## Activation flow

1. Navigate to My roles → Groups
2. Select GRP-LAB-PIM-TEST → Activate
3. Provide justification
4. Membership becomes active for the configured duration
5. User Administrator privileges are granted indirectly through group membership

## What I did

- Created a dedicated role-assignable security group (GRP-LAB-PIM-TEST).
- Assigned the User Administrator role to the group.
- Configured PIM for Groups to make group membership eligible (not permanent).
- Activated the group membership through the My roles portal.
- Verified that privileged access was granted indirectly through the activated group membership.

## Result

The User Administrator role became active through group-based PIM activation, providing time-limited access instead of permanent standing privilege.

## Lessons learned

- Role-assignable groups can carry Entra roles, enabling indirect privileged access through group membership.
- PIM for Groups provides just-in-time access — users activate membership only when needed.
- Eligible assignments are preferred over active assignments to reduce standing privilege exposure.
- Requiring justification creates an audit trail for every privileged access activation.
- Group-based PIM combines well with Access Reviews (Task 12) and Entitlement Management (Task 13) as part of a complete governance model.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-11-pim-admin-role/
```
