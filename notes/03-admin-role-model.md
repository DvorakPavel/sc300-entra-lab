# Task 3 – Implement the administrative role model

**Date:**  
2026-03-31

## Goal
Implement a structured admin model with a daily admin account, a role-assignable admin group, and two emergency access accounts.

## What I did
- Verified that GRP-LAB-ADMINS was created as a role-assignable group.
- Assigned the User Administrator role to GRP-LAB-ADMINS.
- Confirmed that lab-admin is a member of GRP-LAB-ADMINS.
- Verified that lab-admin receives User Administrator privileges indirectly through group membership.
- Assigned the Global Administrator role to bg-admin-01 and bg-admin-02.

## Result
The lab now has a basic administrative model:
- bootstrap admin: initial tenant admin
- daily admin: lab-admin
- emergency admins: lab-bg-admin-01 and lab-bg-admin-02

## Lessons learned
- Role-assignable groups must be created correctly from the start and cannot be converted later.
- Indirect role assignment through groups is cleaner and more scalable than assigning roles to users one by one.
- Emergency access accounts should remain separate from day-to-day administration.
