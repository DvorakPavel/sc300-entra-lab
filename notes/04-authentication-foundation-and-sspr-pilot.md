# Task 4 – Authentication Foundation & SSPR Pilot

**Date:**
2026-04-02

## Goal

Configure a modern authentication pilot and enable self-service password reset for a selected lab group.

## Authentication methods enabled

- Microsoft Authenticator: Enabled for GRP-LAB-HR
- SMS: Enabled for GRP-LAB-HR
- Temporary Access Pass (TAP): Enabled for GRP-LAB-HR

Configuration source: Authentication methods policy (modern, recommended over legacy per-user MFA)

## SSPR configuration

- Scope: GRP-LAB-HR (pilot group)
- Methods required to reset: 2
- Methods available: Mobile app notification, mobile app code, mobile phone (SMS)
- Registration: Required at next sign-in
- Re-confirmation interval: 180 days

## What I did

- Configured the Authentication methods policy as the modern baseline.
- Enabled Microsoft Authenticator, SMS, and TAP for the pilot group.
- Enabled SSPR for GRP-LAB-HR with two-method requirement.
- Verified that combined registration is active (single registration for both MFA and SSPR).

## Result

A modern authentication and password reset pilot was prepared for non-admin lab users in the HR group.

## Lessons learned

- Authentication Methods policy is the recommended place to manage methods — legacy per-user MFA settings should be avoided.
- SSPR should be rolled out to a selected pilot group before tenant-wide enablement.
- Combined registration simplifies the end-user experience by unifying MFA and SSPR enrollment.
- TAP is useful for initial onboarding when a user has no registered methods yet.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-04-auth-foundation-sspr/
```
