# Task 33 – Tenant Hardening & Final Review

**Date:**
2026-06-11 — 2026-06-15

## Goal

Transition key Conditional Access policies from Report-only to Enforced mode, harden the tenant security posture, and investigate Identity Secure Score.

## Conditional Access — final state

| Policy | Status | Note |
|---|---|---|
| CA-Block-Legacy-Authentication | Enforced | unchanged |
| CA-Pilot-HR-Require-MFA | Enforced | unchanged |
| CA-Require-Terms-of-Use | Enforced | unchanged |
| CA-Risk-SignIn-Require-MFA | Enforced | switched from Report-only |
| CA-Risk-User-Force-Password-Change | Enforced | switched from Report-only |
| CA-Admin-Require-Phishing-Resistant-MFA | Enforced | switched, adjusted Grant — see below |
| CA-Require-MFA-All-Users | Enforced | switched from Report-only |
| CA-Block-Untrusted-Countries | Enforced | switched from Report-only |
| CA-Require-MFA-LAB-App-Registration-Test | Report-only | app-specific demo |
| CA-Require-StepUp-Sensitive-Access | Report-only | authentication context demo |
| CA-Session-Management | Report-only | session controls demo |

**CA-Admin adjustment:** Original Grant was "Require authentication strength → Phishing-resistant MFA." Without FIDO2 key or Bluetooth for cross-device passkey, changed to classic "Require multifactor authentication" checkbox. The dropdown "Require authentication strength: Multifactor authentication" did not work — only the classic checkbox was accepted by the sign-in flow.

## Admin account hardening

- Removed primary admin account from Exclude on all 8 enforced CA policies
- Only break-glass accounts (lab-bg-admin-01, lab-bg-admin-02) remain excluded
- Confirmed enforcement via AADSTS50079 after exclusion removal

## Identity Secure Score

- Before: 38.03%
- After: 43.82%

### What improved

- SSPR expanded from the pilot group to All Users
- MFA phone method bulk-assigned to all users via `New-MgUserAuthenticationPhoneMethod`
- Admin account removed from CA exclusions → admin MFA moved from 0/10 to 3.33/10

### What didn't improve and why

- **Risk policies (0/7 + 0/7)** — Score checks the deprecated Identity Protection blade, not CA policies. Both risk policies are enforced via CA, but Score gives 0 points. Legacy IP blade is read-only (retiring October 2026), "Policy enforcement" toggle is greyed out.
- **Admin MFA (3.33/10)** — Break-glass accounts excluded from MFA per Microsoft best practice. Score penalises every admin without MFA. The remaining 6.67 points are unachievable without violating emergency access design.
- **MFA registration (0.67/9)** — Bulk phone assignment via Graph API doesn't count as completed registration. Users must interactively register at mysignins.microsoft.com.
- **Insider Risk (0/5)** — Requires Microsoft Purview with Adaptive Protection. Not available on a developer tenant.

## What I did

- Recorded Score baseline (38.03%).
- Switched five CA policies from Report-only to Enforced.
- Adjusted CA-Admin Grant from phishing-resistant to classic MFA (no FIDO2/Bluetooth).
- Removed admin account from all CA exclusions, confirmed enforcement (AADSTS50079).
- Expanded SSPR to All Users, bulk-assigned MFA phone method via Graph API.
- Investigated Score — discovered legacy IP blade bug, confirmed toggle is greyed out.
- Final Score after refresh: 43.82%.

## Result

Tenant hardened with 8 enforced and 3 intentionally Report-only CA policies. Score improved from 38.03% to 43.82%. Remaining ~33 unachievable points documented with root cause (deprecated legacy blade, break-glass design, Purview license).

## Lessons learned

- "Require authentication strength" and "Require multifactor authentication" in CA Grant controls are NOT interchangeable — even at the same MFA level, they behave differently.
- Phishing-resistant MFA via Authenticator requires Bluetooth for cross-device authentication. Without it, you need a physical FIDO2 key or Windows Hello.
- Identity Secure Score checks deprecated legacy Identity Protection policies, not CA-based risk policies. Legacy blade is read-only — those points are permanently unachievable.
- Break-glass accounts without MFA are a correct design, but Score penalises it. Document the exception rather than "fix" it.
- Bulk Graph API phone assignment ≠ completed MFA registration. SSPR on pilot group = 0 Score points, must be All Users.
- A low Score with a documented rationale shows more expertise than a high Score without context.

## Evidence

```text
evidence/task-33-tenant-hardening-final-review/
```
