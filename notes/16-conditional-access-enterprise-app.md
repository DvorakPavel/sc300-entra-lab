# Task 16 – Require MFA for Enterprise Application with Conditional Access

**Date:**  
2026-04-27

## Goal
Create a Conditional Access policy to require multi-factor authentication for a specific Enterprise Application.

## Policy name
CA-Require-MFA-LAB-App-Registration-Test

## Scope
- Users/Groups: Primary admin account
- Resources: LAB-App-Registration-Test
- Conditions: None
- Grant control: Require multi-factor authentication
- Policy state: Report-only

## What I did
- Created a Conditional Access policy for a specific Enterprise Application.
- Scoped the policy to the primary admin account only.
- Targeted the Enterprise Application LAB-App-Registration-Test.
- Left additional conditions unconfigured.
- Set the grant control to require multi-factor authentication.
- Enabled the policy in Report-only mode.
- Triggered a new sign-in event for the application.
- Verified in sign-in logs that the policy was evaluated in Report-only mode.

## Result
A Conditional Access policy was created to require MFA for the LAB-App-Registration-Test Enterprise Application, and policy evaluation was confirmed in sign-in logs with a Report-only success result.

## Lessons learned
- Conditional Access policies for applications must target the Enterprise Application, not the App Registration object.
- Report-only mode is a safe way to validate application-targeted access controls.
- MFA requirements can be scoped to a single application without affecting the rest of the tenant.
- Sign-in logs are the correct place to confirm Conditional Access evaluation results.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-16-conditional-access-enterprise-app/
```
