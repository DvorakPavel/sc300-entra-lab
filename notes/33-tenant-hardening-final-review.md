# Task 33 – Tenant Hardening & Final Review

**Date:**
2026-06-11 — 2026-06-15

## Goal

Transition key Conditional Access policies from Report-only to Enforced mode, harden the tenant security posture, and investigate Identity Secure Score improvement opportunities.

## Identity Secure Score

- Score before hardening: 38.03%
- Score after hardening: 43.82%

### Score breakdown (6 active recommendations)

- Enable policy to block legacy authentication: 7.11/8
- Require multifactor authentication for administrative roles: 3.33/10
- Ensure all users can complete multifactor authentication: 0.67/9
- Protect all users with a user risk policy: 0/7
- Protect all users with a sign-in risk policy: 0/7
- Protect your tenant with Insider Risk condition in CA: 0/5

### Unachievable Score points

**Risk policies (0/14)** — Score evaluates risk-based policies through the deprecated Identity Protection blade, not through Conditional Access. Both sign-in risk and user risk policies are enforced via CA (the current Microsoft-recommended approach), but Score gives 0 points. The legacy IP blade is now read-only with a banner stating "will be retired on October 1, 2026." The "Policy enforcement" toggle is greyed out on Disabled — cannot be enabled. Known bug reported on Microsoft Q&A since 2022, acknowledged by Microsoft, never fixed.

**Admin MFA (3.33/10)** — Score counts all admin accounts. Break-glass accounts are intentionally excluded from MFA per Microsoft best practice for emergency access. Score penalizes this. The remaining 6.67 points are unachievable without violating emergency access design.

**MFA registration (0.67/9)** — Bulk phone assignment via Graph API (`New-MgUserAuthenticationPhoneMethod`) does not count as "completed registration." Score requires each user to interactively complete registration at mysignins.microsoft.com. Lab users never performed this step.

**Insider Risk (0/5)** — Requires Microsoft Purview with Adaptive Protection. Separate product, separate license. Not available on a developer tenant.

### Improvements applied

- SSPR expanded from the pilot group (GRP-LAB-HR) to All Users
- MFA phone method bulk-assigned to all users via `New-MgUserAuthenticationPhoneMethod`
- Admin account removed from Exclude on all CA policies
- Result: 38.03% → 43.82% (+5.79%)

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
- Changed to: Require multifactor authentication (classic checkbox)
- Note: "Require authentication strength: Multifactor authentication" (dropdown) did not work — only the classic "Require multifactor authentication" checkbox was accepted by the sign-in flow

## Admin account hardening

- Removed primary admin account from Exclude on all 8 enforced CA policies
- Only break-glass accounts (lab-bg-admin-01, lab-bg-admin-02) remain excluded
- Confirmed enforcement: sign-in correctly required MFA after exclusion removal (AADSTS50079)

## Final tenant security posture

- Total CA policies: 11
- Enforced: 8
- Report-only: 3 (intentionally — demo and lab-specific policies)
- Emergency access accounts are excluded from all policies
- Admin account covered by all CA policies 
- SSPR: All users
- Identity Secure Score: 43.82%

## What I did

- Recorded the Identity Secure Score before changes as a baseline (38.03%).
- Switched five Report-only Conditional Access policies to Enforced mode.
- Removed primary admin account from Exclude on all enforced policies — only BG accounts remain excluded.
- Encountered AADSTS50079 (MFA enrollment required) — confirmed CA enforcement works.
- Adjusted CA-Admin-Require-Phishing-Resistant-MFA from authentication strength to classic MFA due to lack of FIDO2 key and Bluetooth.
- Tested passkey registration in Microsoft Authenticator — fails on desktop without Bluetooth for cross-device authentication.
- Revoked sessions and confirmed successful sign-in with password + Authenticator push.
- Confirmed the final state of all 11 Conditional Access policies.
- Expanded SSPR from the pilot group to All Users via the Password Reset blade.
- Bulk-assigned MFA phone method to all users via PowerShell (`New-MgUserAuthenticationPhoneMethod`).
- Investigated Identity Secure Score — discovered Score checks deprecated legacy Identity Protection blade instead of CA policies.
- Navigated to legacy IP blade via direct Azure Portal URL, confirmed "Policy enforcement" toggle is greyed out (read-only).
- Documented all unachievable Score points with root cause analysis.
- Final Score after 48h refresh: 43.82%.

## Result

The tenant was hardened by transitioning five CA policies from Report-only to Enforced, expanding SSPR to all users, and bulk-assigning MFA phone methods. Identity Secure Score improved from 38.03% to 43.82%. Investigation revealed that ~33 additional points are unachievable due to platform limitations (deprecated legacy IP blade, break-glass account design, Purview license requirement).

## Lessons learned

- Report-only to Enforced transition follows the recommended production workflow: deploy in Report-only, monitor sign-in logs for impact, then enforce.
- Not all policies should be enforced — app-specific demo policies and lab-specific session controls are intentionally left in Report-only.
- Emergency access (break-glass) accounts must remain excluded from all CA policies to prevent lockout.
- Daily admin accounts should NOT be excluded from CA policies — BG accounts serve as the safety net.
- "Require authentication strength" and "Require multifactor authentication" in CA Grant controls are NOT interchangeable — the authentication strength framework can behave differently even when set to "Multifactor authentication" level.
- Phishing-resistant MFA (passkeys) via Microsoft Authenticator requires Bluetooth between the desktop and phone for cross-device authentication.
- Identity Secure Score evaluates risk-based policies through the deprecated legacy Identity Protection blade, not through Conditional Access. The legacy blade is read-only (retiring October 2026) — making those Score points permanently unachievable.
- Break-glass accounts without MFA is correct security design, but Score penalises it. Document the deliberate exception rather than "fixing" a gap that isn't one.
- Bulk phone assignment via Graph API does not satisfy Score's MFA registration check — users must complete interactive registration themselves.
- SSPR configured for a pilot group gives 0 Score points. Only "All users" counts.
- Always investigate what Score actually checks before chasing the number. A low score with a documented rationale demonstrates more expertise than a high score without context.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-33-tenant-hardening-final-review/
```
