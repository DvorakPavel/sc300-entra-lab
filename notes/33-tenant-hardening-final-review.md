# Task 33 – Tenant Hardening & Final Review

**Date:**
2026-06-11

## Goal

Transition key Conditional Access policies from Report-only to Enforced mode to harden the tenant security posture and improve the Identity Secure Score.

## Identity Secure Score

- Score before hardening: ~38%
- Target: 70%+
- Score after hardening: pending refresh (up to 48 hours)

## Conditional Access policies switched to Enforced

- CA-Risk-SignIn-Require-MFA: Report-only → On
- CA-Risk-User-Force-Password-Change: Report-only → On
- CA-Admin-Require-Phishing-Resistant-MFA: Report-only → On
- CA-Require-MFA-All-Users: Report-only → On
- CA-Block-Untrusted-Countries: Report-only → On

## Conditional Access policies kept in Report-only

- CA-Require-MFA-LAB-App-Registration-Test: app-specific demo policy
- CA-Require-StepUp-Sensitive-Access: authentication context demo policy
- CA-Session-Management: session controls demo policy

## Previously enforced policies (unchanged)

- CA-Block-Legacy-Authentication: On
- CA-Pilot-HR-Require-MFA: On
- CA-Require-Terms-of-Use: On

## CA-Admin-Require-Phishing-Resistant-MFA adjustment

- Original Grant control: Require authentication strength → Phishing-resistant MFA
- Issue: No FIDO2 USB key, no biometrics on desktop, no Bluetooth for cross-device passkey
- Changed to: Require multifactor authentication
- Note: "Require authentication strength: Multifactor authentication" (dropdown) did not work — only the classic "Require multifactor authentication", the sign-in flow accepted checkbox

## Admin account hardening

- Removed primary admin account from Exclude on all 8 enforced CA policies
- Only break-glass accounts (lab-bg-admin-01, lab-bg-admin-02) remain excluded
- Confirmed enforcement: sign-in correctly required MFA after exclusion removal (AADSTS50079)

## Final tenant security posture

- Total CA policies: 11
- Enforced: 8
- Report-only: 3 (intentionally — demo and lab-specific policies)
- Emergency access accounts are excluded from all policies
- Admin account covered by all CA policies (no longer excluded)

## What I did

- Recorded the Identity Secure Score before changes as a baseline (~38.03%).
- Switched five Report-only Conditional Access policies to Enforced mode.
- Removed primary admin account from Exclude on all enforced policies — only BG accounts remain excluded.
- Encountered AADSTS50079 (MFA enrollment required) — confirmed CA enforcement works.
- Adjusted CA-Admin-Require-Phishing-Resistant-MFA from authentication strength to classic MFA due to lack of FIDO2 key and Bluetooth.
- Tested passkey registration in Microsoft Authenticator — fails on desktop without Bluetooth for cross-device authentication.
- Revoked sessions and confirmed successful sign-in with password + Authenticator push.
- Confirmed the final state of all 11 Conditional Access policies.

## Result

The tenant security posture was hardened by transitioning five Conditional Access policies from Report-only to Enforced mode. This follows the production deployment pattern of monitoring in Report-only mode before enforcement. The final configuration includes 8 enforced and 3 intentionally Report-only policies.

## Lessons learned

- Report-only to Enforced transition follows the recommended production workflow: deploy in Report-only, monitor sign-in logs for impact, then enforce.
- Risk-based policies (sign-in risk and user risk) are among the highest contributors to the Identity Secure Score.
- Not all policies should be enforced — app-specific demo policies and lab-specific session controls are intentionally left in Report-only.
- Emergency access (break-glass) accounts must remain excluded from all Conditional Access policies to prevent lockout.
- Daily admin accounts should NOT be excluded from CA policies — BG accounts serve as the safety net.
- Identity Secure Score updates are not immediate — changes can take up to 48 hours to reflect in the score.
- "Require authentication strength" and "Require multifactor authentication" in CA Grant controls are NOT interchangeable — the authentication strength framework can behave differently even when set to "Multifactor authentication" level.
- Phishing-resistant MFA (passkeys) via Microsoft Authenticator requires Bluetooth between desktop and phone for cross-device authentication. Without Bluetooth, a physical FIDO2 USB key or Windows Hello for Business is needed.
- The hardening process demonstrates the full lifecycle: design → implement (Report-only) → validate → enforce.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-33-tenant-hardening-final-review/
```
