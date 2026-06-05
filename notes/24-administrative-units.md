# Task 24 – Administrative Units (Scoped Administration)

**Date:**
2026-06-05

## Goal

Create an Administrative Unit for scoped administration and assign a User Administrator role limited to the AU scope.

## Administrative Unit

- Name: AU-LAB-IT
- Description: Administrative unit for IT department lab users
- Restricted management: No
- Membership type: Assigned

## Members

- lab-admin
- lab-user-01
- lab-user-02

## Scoped role assignment

- Role: User Administrator
- Scope: AU-LAB-IT
- Assigned to: lab-user-02
- Assignment type: Active
- Validity: 6 months

## Scoped access verification

- Tested as: lab-user-02
- Password reset for lab-user-01 (in the AU): Succeeded
- Password reset for lab-user-03 (outside AU): Blocked — "incorrect level of administrative privilege"

## What I did

- Created an Administrative Unit for the IT department lab users.
- Added three IT group members to the AU using assigned membership.
- Assigned the User Administrator role scoped to AU-LAB-IT to lab-user-02.
- Signed in as lab-user-02 and successfully reset the password for lab-user-01 (within AU).
- Attempted to reset the password for lab-user-03 (outside AU) and confirmed it was blocked.

## Result

Scoped administration was configured using an Administrative Unit. The User Administrator role assigned at the AU scope limits management capabilities to users within the AU only. Verification confirmed that the scoped admin cannot manage users outside the AU.

## Lessons learned

- Administrative Units enable scoped delegation of admin roles without granting tenant-wide access.
- AU membership is independent from security group membership.
- Scoped role assignments are visible in the AU's Roles and administrators section, not in the tenant-wide role assignments view.
- Restricted management AUs prevent even tenant-wide admins from managing AU members — useful for sensitive populations.
- Assigned membership provides explicit control over the AU population; dynamic membership automates it based on user attributes.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-24-administrative-units/
```
