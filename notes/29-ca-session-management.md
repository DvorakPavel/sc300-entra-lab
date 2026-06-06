# Task 29 – Conditional Access: Session Management

**Date:**
2026-06-06

## Goal

Configure Conditional Access session controls for sign-in frequency and persistent browser session management, and review Continuous Access Evaluation (CAE).

## Conditional Access policy

- Policy name: CA-Session-Management
- Users: All users
- Exclusions: Lab BG Admin 01, Lab BG Admin 02
- Target resources: All cloud apps
- Session controls: Sign-in frequency — Every time, Persistent browser session — Never persistent
- Policy state: Report-only

## Continuous Access Evaluation

- Status: Enabled by default (no separate configuration blade)
- Configurable per CA policy via "Customize continuous access evaluation" session control option — left at default (enabled)
- Provides near real-time token revocation for critical events (password change, account disable, location change)
- Supported by Microsoft 365 services natively

## What I did

- Created a Conditional Access policy with session controls for sign-in frequency and persistent browser session.
- Configured sign-in frequency to require authentication on every access.
- Set the persistent browser session never to persist to prevent session survival after browser close.
- Excluded emergency access accounts from the policy.
- Confirmed Continuous Access Evaluation is enabled by default and available as a per-policy session control option.

## Result

A Conditional Access policy was created with session controls governing sign-in frequency and browser session persistence. CAE was confirmed as enabled, providing near real-time token revocation alongside the session controls.

## Lessons learned

- Sign-in frequency controls how often a user must re-authenticate. "Every time" eliminates SSO for targeted users.
- Persistent browser session controls whether the session survives a browser close. "Never persistent" forces re-authentication after browser restart.
- Session controls complement grant controls — grant controls decide whether to allow access, session controls define the conditions during the session.
- Continuous Access Evaluation is enabled by default and does not require a separate configuration blade — it is configurable per CA policy as a session control option.
- The "Customize continuous access evaluation" option in session controls is used to restrict or disable CAE, not to enable it.
- CAE and session controls address different aspects — CAE handles event-driven revocation, session controls handle time-based re-authentication.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-29-ca-session-management/
```
