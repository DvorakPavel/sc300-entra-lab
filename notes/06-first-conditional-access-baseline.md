# Task 6 – First Conditional Access baseline

**Date:**  
2026-04-02

## Goal
Create the first Conditional Access pilot policy to require MFA for a selected lab group.

## Policy name
CA-Pilot-HR-Require-MFA

## Scope
- Users/Groups: GRP-LAB-HR
- Cloud apps/resources: All resources
- Grant control: Require multifactor authentication
- Initial state: Report-only
- Final state: On

## What I did
- Created a new Conditional Access policy for the HR pilot group.
- Targeted the policy to all resources.
- Configured the grant control to require MFA.
- Started in Report-only mode to safely validate the impact.
- Tested a sign-in with a pilot user and reviewed the Conditional Access result in the sign-in log.
- After successful validation, changed the policy state from Report-only to On.

## Result
The first Conditional Access baseline policy was created, validated in Report-only mode, and then enabled for enforcement without risking tenant lockout.

## Lessons learned
- Report-only mode is a safe way to validate Conditional Access before enforcement.
- A pilot group is better than targeting all users for the first CA test.
- Break-glass exclusions become critical when policies are broadened beyond a small pilot group.
- Enabling the policy only after validation provides a safer rollout approach.
