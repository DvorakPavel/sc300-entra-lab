# Task 10 – Authentication Strengths (Phishing-resistant MFA)

**Date:**  
2026-04-17

## Goal
Enforce phishing-resistant MFA for administrative accounts using Authentication Strengths.

## Policy name
CA-Admin-Require-Phishing-Resistant-MFA

## Scope
- Users: GRP-LAB-ADMINS
- Exclusions: Lab BG Admin 01, Lab BG Admin 02
- Resources: All resources
- Grant control: Require authentication strength (Phishing-resistant MFA)
- Policy state: Report-only

## What I did
- Enabled Passkey (FIDO2) authentication for administrative accounts.
- Created a Conditional Access policy requiring phishing-resistant MFA.
- Scoped the policy to admin users only.
- Excluded emergency access accounts.
- Set the policy to Report-only mode to evaluate impact safely.

## Result
Administrative sign-ins were evaluated against phishing-resistant MFA requirements without enforcement.

## Lessons learned
- Authentication Strengths enforce the quality of MFA, not just MFA presence.
- Phishing-resistant MFA should be reserved for privileged accounts.
- Report-only mode is essential when introducing stricter authentication controls.
``

## Evidence

Evidence for this task is stored in:

```text
evidence/task-10-authentication-strengths/
```
