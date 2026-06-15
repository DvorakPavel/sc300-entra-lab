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
- After: 63.07%

### What improved

| Recommendation | Before | After | Change |
|---|---|---|---|
| User risk policy | 0/7 | 6.48/7 | CA risk policies finally recognized after 4+ days |
| Sign-in risk policy | 0/7 | 6.22/7 | Same — severe delay, not the "permanent bug" initially assumed |
| Legacy authentication | 7.11/8 | 7.41/8 | Minor improvement |
| Admin MFA | 0/10 | 3.33/10 | Admin removed from CA exclusions |
| MFA registration | 0.67/9 | 1.33/9 | Slight increase |
| SSPR | 0 | scored | Expanded from pilot group to All Users |

### What still didn't improve and why

- **Admin MFA (3.33/10)** — Break-glass accounts excluded from MFA per Microsoft best practice. Score penalises every admin without MFA. The remaining points are unachievable without violating the emergency access design.
- **MFA registration (1.33/9)** — Bulk phone assignment via Graph API doesn't fully count as completed registration. Users must interactively register at mysignins.microsoft.com.
- **Insider Risk (0/5)** — Requires Microsoft Purview with Adaptive Protection. Not available on a developer tenant.

## What I did

- Recorded Score baseline (38.03%).
- Switched five CA policies from Report-only to Enforced.
- Adjusted CA-Admin Grant from phishing-resistant to classic MFA (no FIDO2/Bluetooth).
- Removed admin account from all CA exclusions, confirmed enforcement (AADSTS50079).
- Expanded SSPR to All Users, bulk-assigned MFA phone method via Graph API.
- Investigated Score — initially found 0 points for risk policies, documented as a legacy IP blade bug.
- After 4+ days, Score finally recognised CA-based risk policies — jumped from 43.82% to 63.07%.
- MFA registration also slightly increased (0.67 → 1.33) without additional user action.

## Result

Tenant hardened with 8 enforced and 3 intentionally Report-only CA policies. Score improved from 38.03% to 63.07%. Risk policies (0/14 → 12.7/14) were eventually recognised after a multi-day sync delay. Remaining gaps documented: break-glass MFA conflict (3.33/10), incomplete interactive MFA registration (1.33/9), Purview license requirement (0/5).

## Lessons learned

- "Require authentication strength" and "Require multifactor authentication" in CA Grant controls are NOT interchangeable — even at the same MFA level, they behave differently.
- Phishing-resistant MFA via Authenticator requires Bluetooth for cross-device authentication. Without it, you need a physical FIDO2 key or Windows Hello.
- Identity Secure Score initially showed 0 points for CA-based risk policies (known issue since 2022). After 4+ days, it synced and recognised them. The documented 24-hour refresh cycle is misleading — real sync can take much longer. Don't assume it's a permanent bug.
- Break-glass accounts without MFA are a correct design, but Score penalises it. Document the exception rather than "fix" it.

## Evidence

```text
evidence/task-33-tenant-hardening-final-review/
```
