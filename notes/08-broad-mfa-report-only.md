# Task 8 – Broad MFA rollout (Report-only)

**Date:**  
2026-04-15

## Goal

Prepare a tenant-wide MFA policy using Conditional Access in Report-only mode.

## Policy name

CA-Require-MFA-All-Users

## Scope

- Users: All users
- Exclusions: Lab BG Admin 01, Lab BG Admin 02, primary admin account
- Resources: All resources
- Grant control: Require multifactor authentication
- Policy state: Report-only

## What I did

- Created a tenant-wide Conditional Access policy to require MFA.
- Excluded emergency access (break-glass) accounts and the primary admin account as a safety measure.
- Scoped the policy to all cloud resources.
- Set the policy to Report-only mode to safely evaluate impact.
- Reviewed sign-in logs to validate Report-only results.

## Result

A tenant-wide MFA policy was successfully evaluated without enforcing MFA or risking tenant lockout.

## Lessons learned

- Report-only mode is critical before enforcing broad Conditional Access policies.
- Emergency access accounts must always be excluded from tenant-wide MFA policies.
- Sign-in logs provide clear insight into the future impact of MFA enforcement.
