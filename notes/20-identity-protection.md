# Task 20 – Identity Protection: Risk Investigation, Graph API, and MFA Registration Policy

**Date:**  
2026-05-30

## Goal

Explore the Microsoft Entra Identity Protection dashboard, investigate risk reports, use Microsoft Graph API to confirm and dismiss user risk, and configure the MFA registration policy.

## Identity Protection reports reviewed

- Risky users
- Risky sign-ins
- Risk detections
- Risky workload identities

## Risk investigation

- User tested: Lab User 05
- Confirm compromised: performed via Microsoft Graph Explorer (POST to confirmCompromised endpoint)
- Permission required: IdentityRiskyUser.ReadWrite.All
- Result: user appeared in Risky users with Risk level High and Risk state Confirmed compromised
- Dismiss risk: performed via Microsoft Graph Explorer (POST to dismiss endpoint)
- Result: Risk state changed to Dismissed

## MFA Registration policy

- Users included: GRP-LAB-HR
- Users excluded: Lab BG Admin 01, Lab BG Admin 02
- Policy state: Enabled

## Related Conditional Access policies (from Task 09)

- CA-Risk-SignIn-Require-MFA (Report-only)
- CA-Risk-User-Force-Password-Change (Report-only)

## What I did

- Opened the Identity Protection Overview dashboard and reviewed risk summaries.
- Reviewed the Risky users, Risky sign-ins, and Risk detections reports.
- Used Microsoft Graph Explorer to confirm a lab user as compromised via the Identity Protection API.
- Verified in the Entra portal that the user appeared in Risky users with High risk level.
- Dismissed the user risk via Microsoft Graph Explorer after simulated investigation.
- Configured the MFA Registration policy for the HR pilot group with break-glass exclusions.
- Verified that the existing risk-based Conditional Access policies from Task 09 remain active.

## Result

Identity Protection investigation and remediation workflows were validated using Microsoft Graph API for both confirming compromise and dismissing risk. The MFA Registration policy was configured to enforce MFA enrollment for pilot users independently from Conditional Access.

## Lessons learned

- Identity Protection provides centralised risk visibility through Risky users, Risky sign-ins, and Risk detections reports.
- The Risky users blade only displays users with active risk detections — users with no risk do not appear in the list.
- Microsoft Graph API can be used to manage Identity Protection programmatically when portal options are limited.
- The MFA Registration policy is a standalone Identity Protection feature, separate from Conditional Access.
- Identity Protection detects risk, while Conditional Access policies respond to it — both must be configured together.
- Break-glass accounts must always be excluded from MFA registration policies.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-20-identity-protection/
```
