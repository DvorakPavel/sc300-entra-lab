# Task 22 – Named Locations for Conditional Access

**Date:**  
2026-06-01

## Goal

Configure Named Locations in Microsoft Entra ID and use them as conditions in Conditional Access policies to implement location-based access control.

## Named Locations created

### LAB-Trusted-Office-Network

- Type: IP ranges
- Trusted: Yes
- Scope: Lab administrator's public IP address

### LAB-Blocked-Countries

- Type: Countries
- Location method: IP address (IPv4 and IPv6)
- Countries included: Selected countries where the organisation does not operate

## Conditional Access policy created

- Policy name: CA-Block-Untrusted-Countries
- Users: All users
- Exclusions: Lab BG Admin 01, Lab BG Admin 02
- Resources: All resources
- Condition: Locations — LAB-Blocked-Countries (included)
- Grant control: Block access
- Policy state: Report-only

## Existing policy updated

- Policy name: CA-Require-MFA-All-Users
- Change: Added location condition
- Locations excluded: LAB-Trusted-Office-Network
- Effect: MFA is not required when signing in from the trusted office network

## What I did

- Created an IP-based Named Location and marked it as a trusted location.
- Created a Country-based Named Location targeting countries outside the organisation's operating region.
- Verified both Named Locations in the Conditional Access Named Locations list.
- Created a Conditional Access policy to block access from untrusted countries.
- Excluded emergency access accounts from the blocking policy.
- Updated the existing tenant-wide MFA policy to exclude the trusted office network from MFA requirements.
- Reviewed the complete list of Conditional Access policies to verify the updated configuration.

## Result

Location-based access control was configured using Named Locations and Conditional Access. A trusted network location was defined and excluded from the MFA requirement. A blocking policy was created for access attempts from countries outside the organization's operating region.

## Lessons learned

- Named Locations define network locations that Conditional Access policies can reference as conditions.
- IP-based locations marked as trusted can be excluded from MFA requirements to improve user experience on corporate networks.
- Country-based locations enable geographic access control for blocking access from high-risk regions.
- The Include and Exclude conditions in Locations serve opposite purposes and must be configured correctly.
- Blocking policies must always exclude emergency access accounts.
- Trusted locations reduce false positives in Identity Protection risk detections.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-22-named-locations/
```
