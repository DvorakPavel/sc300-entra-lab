# Task 27 – Password Protection

**Date:**
2026-06-06

## Goal

Configure Microsoft Entra Password Protection with a custom banned password list and review smart lockout settings.

## Custom banned password list

- Enforce custom list: Yes
- Words added: organisation-specific terms to prevent predictable passwords
- Entra ID automatically detects common variations and substitutions

## Smart lockout settings

- Lockout threshold: default
- Lockout duration: default
- Mode: Enforced

## What I did

- Opened the Password protection settings in the Authentication methods.
- Enabled the custom banned password list and added organisation-specific terms.
- Reviewed the smart lockout threshold and duration settings.
- Saved the updated password protection configuration.

## Result

Password protection was configured with a custom banned password list to prevent users from choosing predictable passwords based on organisation-specific terms. Smart lockout settings were reviewed and documented.

## Lessons learned

- The global banned password list is always active and maintained by Microsoft.
- The custom-banned password list supplements the global list with organisation-specific terms.
- Entra ID evaluates password variations automatically — adding a base term blocks common substitutions and patterns.
- Smart lockout protects against brute-force attacks by temporarily locking accounts after repeated failures.
- Cloud-based password protection applies to all cloud authentication. On-premises protection requires an additional agent deployed on domain controllers.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-27-password-protection/
```
