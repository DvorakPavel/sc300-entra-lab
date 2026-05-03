# Task 12 – Access Reviews (Identity Governance)

**Date:**  
2026-04-19

## Goal

Use Microsoft Entra Access Reviews to periodically validate group membership and ensure that only required users retain access.

## Resource reviewed

- Resource type: Security group
- Group name: GRP-LAB-PIM-TEST

## Review configuration

- Review type: Resource review (Teams + Groups)
- Review scope: All users
- Reviewers: Users review their own access (self-review)
- Frequency: One-time
- Duration: 1 day
- Auto-apply results to resource: Enabled
- Action if reviewers don’t respond: Remove access
- Justification required: Enabled
- Email notifications: Enabled
- Reminders: Enabled
- Decision helpers:
  - No sign-in within 30 days: Enabled

## What I did

- Created a one-time access review for the GRP-LAB-PIM-TEST security group.
- Configured self-review so that group members must confirm whether they still require access.
- Enabled automatic application of review results to enforce governance decisions.
- Set the review to remove access automatically if no response is provided.
- Completed the access review through the My Access portal and approved continued membership with justification.

## Result

The access review was successfully completed.  
Group membership was reviewed and automatically enforced based on the review decision, confirming that access governance works as intended.

## Lessons learned

- Access Reviews help prevent stale or unnecessary access by enforcing periodic validation.
- Self-reviews are suitable for validating access to internal or low-risk groups.
- Automatic result application ensures governance actions are enforced without manual intervention.
- Combining Access Reviews with PIM strengthens the overall least-privilege model.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-12-access-reviews/
```
