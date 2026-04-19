# Task 13 – Entitlement Management (Access Packages)

**Date:**  
2026-04-19

## Goal

Implement self-service access governance using Microsoft Entra Entitlement Management.
The goal was to provide temporary, approval-based access to a security group without
using permanent or direct group assignments.

---

## Catalog

- Catalog name: LAB – PIM Test Access
- Enabled: Yes
- Catalog owners:
  - Primary admin account
  - Lab User 03

Catalog owners are used as sponsors for access package approvals, which represents
the recommended and reliable approval model in Entitlement Management.

---

## Access Package

- Access package name: LAB – PIM Test Access
- Description: Access package for granting temporary access to the PIM test group
- Catalog: LAB – PIM Test Access
- Status: Enabled

---

## Resources

The access package includes the following resource:

- Resource type: Security group
- Group name: GRP-LAB-PIM-TEST
- Role granted: Member
- Resource subtype: Group assignable to roles

Group membership is granted exclusively through the access package lifecycle.

---

## Request Policy

- Policy name: Default request policy
- Who can get access: All members (excluding guests)
- Who can request access:
  - Self
  - Admin
- Approval required: Yes
- Approval stages: 1
- First approver: Sponsors (catalog owners)
- Fallback approver: Lab User 03
- Requestor justification required: Yes
- Approver justification required: Yes
- Decision window: 14 days
- Assignment expiration: 7 days
- Access reviews in lifecycle: Disabled

The approval flow is based on catalog sponsors with a fallback approver to ensure
a resilient and auditable governance process.

---

## Request and Approval Flow

1. The primary admin account requested access through the My Access portal.
2. The request entered a pending approval state.
3. A catalog owner reviewed and approved the request in My Access.
4. The assignment was successfully delivered after approval.

---

## Result

- The access package request was approved.
- Temporary group membership was granted to the requester.
- The assignment includes a fixed expiration period of seven days.
- The access package appears as Active in My Access.
- Access is fully governed, time-bound, and auditable.

---

## Lessons Learned

- Entitlement Management enables scalable self-service access governance.
- Using catalog owners (sponsors) is the most reliable approval model.
- Access packages prevent permanent group membership and enforce least privilege.
- Approval, assignment, and expiration are handled automatically.
- Access packages integrate cleanly with PIM and Access Reviews as part of a complete
  identity governance solution.
