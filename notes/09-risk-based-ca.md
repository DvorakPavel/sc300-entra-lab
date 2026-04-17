# Task 9 – Risk-based Conditional Access

**Date:**  
2026-04-15

## Goal
Implement risk-based Conditional Access policies using Microsoft Entra Identity Protection.

## Policies created
- CA-Risk-SignIn-Require-MFA
- CA-Risk-User-Force-Password-Change

## Scope
- Users: All users
- Exclusions: Lab BG Admin 01, Lab BG Admin 02, primary admin account
- Resources: All resources
- Policy state: Report-only

## What I did
- Created a Conditional Access policy to require MFA when sign-in risk is Medium or High.
- Created a Conditional Access policy to force password change when user risk is High.
- Excluded emergency access accounts from both policies.
- Configured both policies in Report-only mode to safely evaluate impact.

## Result
Risk-based Conditional Access policies were successfully configured and validated without enforcing remediation actions.

## Lessons learned
- Risk-based Conditional Access enables adaptive security instead of static MFA.
- Sign-in risk and user risk address different threat scenarios.
- Report-only mode is essential when introducing risk-based policies.
``
