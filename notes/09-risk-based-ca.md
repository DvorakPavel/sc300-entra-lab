# Task 9 – Risk-based Conditional Access

**Date:**
2026-04-15

## Goal

Implement risk-based Conditional Access policies using Microsoft Entra Identity Protection.

## Policy 1 — Sign-in risk

- Policy name: CA-Risk-SignIn-Require-MFA
- Users: All users
- Exclusions: Lab BG Admin 01, Lab BG Admin 02, primary admin account
- Resources: All resources
- Condition: Sign-in risk level Medium or High
- Grant control: Require multifactor authentication
- Policy state: Report-only

## Policy 2 — User risk

- Policy name: CA-Risk-User-Force-Password-Change
- Users: All users
- Exclusions: Lab BG Admin 01, Lab BG Admin 02, primary admin account
- Resources: All resources
- Condition: User risk level High
- Grant control: Require password change
- Policy state: Report-only

## What I did

- Created a CA policy to require MFA when sign-in risk is Medium or High.
- Created a CA policy to force password change when user risk is High.
- Excluded emergency access accounts from both policies.
- Configured both policies in Report-only mode to safely evaluate impact.
- Reviewed sign-in logs for report-only evaluation results.

## Result

Risk-based Conditional Access policies were successfully configured and validated without enforcing remediation actions.

## Lessons learned

- Risk-based CA enables adaptive security — MFA is triggered by threat signals, not applied statically to every sign-in.
- Sign-in risk and user risk address different scenarios: sign-in risk covers suspicious sessions, user risk covers compromised credentials.
- Report-only mode is essential when introducing risk-based policies to avoid unintended lockouts.
- These policies are among the highest contributors to Identity Secure Score.

## Evidence

Evidence for this task is stored in:

``text
evidence/task-09-risk-based-ca/
``
