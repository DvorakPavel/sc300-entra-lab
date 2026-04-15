# Task 2 – Create lab identities and groups

**Date:**  
2026-3-30

## Goal
Create a clean custom identity layer inside the sandbox for future SC-300 lab tasks.

## What I did
- Created 9 cloud-only lab users:
  - lab-admin
  - lab-bg-admin-01
  - lab-bg-admin-02
  - lab-user-01 to lab-user-06
- Created 4 security groups:
  - GRP-LAB-IT
  - GRP-LAB-HR
  - GRP-LAB-FIN
  - GRP-LAB-ADMINS as role-assignable group
- Added the initial group memberships:
  - IT: lab-admin, lab-user-01, lab-user-02
  - HR: lab-user-03, lab-user-04
  - FIN: lab-user-05, lab-user-06
  - ADMINS: lab-admin

## Result
A dedicated custom lab identity structure was created without modifying the existing sample data.

## Lessons learned
- Keeping sample data untouched helps preserve the sandbox baseline.
- Creating a separate LAB identity layer makes future lab tasks cleaner.
- Break-glass accounts should stay simple and separate from normal admin workflows.
