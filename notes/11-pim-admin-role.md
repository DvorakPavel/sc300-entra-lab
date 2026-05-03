# Task 11 – PIM for group-based admin access

**Date:**  
2026-04-18

## Goal

Use Privileged Identity Management (PIM) for a role-assignable group that carries a Microsoft Entra administrative role.

## Group used

GRP-LAB-PIM-TEST

## Role carried by the group

User Administrator

## What I did

- Created a dedicated role-assignable security group for PIM testing.
- Assigned the User Administrator role to the role-assignable group.
- Used PIM for Groups to make group membership eligible.
- Activated the group membership through My roles > Groups / Microsoft Entra roles view.
- Verified that privileged access was granted indirectly through the activated group membership.

## Result

The User Administrator role became active through group-based PIM activation, with time-limited access instead of permanent standing privilege.

## Lessons learned

- Role-assignable groups can carry Microsoft Entra roles.
- PIM for Groups can be used to control just-in-time access through group membership.
- Group-based activation provides indirect privileged access without assigning the role directly to the user.
``

## Evidence

Evidence for this task is stored in:

```text
evidence/task-11-pim-admin-role/
```
