# Task 10 – Authentication Strengths

**Date:**
2026-04-17

## Goal

Enforce phishing-resistant MFA for administrative accounts using Authentication Strengths in Conditional Access.

## Policy configuration

- Policy name: CA-Admin-Require-Phishing-Resistant-MFA
- Users: GRP-LAB-ADMINS
- Exclusions: Lab BG Admin 01, Lab BG Admin 02
- Resources: All resources
- Grant control: Require authentication strength → Phishing-resistant MFA
- Policy state: Report-only

## Authentication strength levels

- Multifactor authentication: Password + SMS, Password + Authenticator push
- Passwordless MFA: Microsoft Authenticator (passwordless), Windows Hello
- Phishing-resistant MFA: FIDO2 security keys, Windows Hello for Business, certificate-based auth

## What I did

- Reviewed the built-in authentication strength definitions in Entra.
- Enabled Passkey (FIDO2) authentication for administrative accounts.
- Created a CA policy requiring phishing-resistant MFA for the admin group.
- Excluded emergency access accounts.
- Set the policy to Report-only mode to evaluate impact.

## Result

Administrative sign-ins were evaluated against phishing-resistant MFA requirements without enforcement.

## Lessons learned

- Authentication Strengths enforce the quality of MFA, not just its presence — they define which methods are acceptable.
- Phishing-resistant MFA should target privileged accounts first; standard MFA is sufficient for regular users.
- The three built-in strength levels (MFA, Passwordless, Phishing-resistant) form a hierarchy — each is stricter than the previous.
- Report-only mode is essential when introducing stricter authentication controls to avoid locking out admins.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-10-authentication-strengths/
```
