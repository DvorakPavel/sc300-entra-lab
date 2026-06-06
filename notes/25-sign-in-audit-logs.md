# Task 25 – Sign-in Logs & Audit Logs

**Date:**
2026-06-06

## Goal

Explore sign-in logs, audit logs, and provisioning logs in Microsoft Entra ID for identity monitoring and troubleshooting.

## Sign-in log types reviewed

- Interactive user sign-ins
- Non-interactive user sign-ins
- Service principal sign-ins
- Managed identity sign-ins

## Sign-in log detail structure

- Basic info: user, application, resource, IP address, location, status, error code
- Conditional Access: evaluated policies, result, grant controls, session controls
- Authentication Details: method, step results

## Sign-in log filters applied

- Status: Failure
- User: specific lab user
- Conditional Access: Success / Failure

## Audit log analysis

- Date range: Last 24 hours
- Activity types reviewed: administrative unit changes, role assignments, policy updates
- Detail structure: activity, date, initiated by (actor), target(s), modified properties

## Provisioning logs

- Status: No provisioning events in the dev tenant
- Purpose: tracks automatic user provisioning via SCIM and HR inbound connectors

## What I did

- Reviewed interactive sign-in logs and analysed a sign-in record in detail.
- Examined the Conditional Access tab to see which policies were evaluated and their results.
- Filtered sign-in logs by status, user, and Conditional Access result.
- Reviewed non-interactive and service principal sign-in log types.
- Reviewed audit logs and filtered for recent configuration changes.
- Analysed an audit log record to identify the actor, activity, and target.
- Checked provisioning logs for automatic provisioning events.

## Result

Sign-in logs, audit logs, and provisioning logs were explored and analysed. Sign-in log filtering was validated for troubleshooting scenarios. Audit logs confirmed visibility into tenant configuration changes, including recent Administrative Unit operations from Task 24.

## Lessons learned

- Sign-in logs are the primary tool for troubleshooting authentication issues.
- The Conditional Access tab in the sign-in log details shows which policies were evaluated and why access was granted or denied.
- Non-interactive sign-ins are the most frequent type and represent background token refresh operations.
- Service principal sign-ins track application-level authentication separately from user sign-ins.
- Audit logs record configuration changes with full actor and target attribution.
- Provisioning logs track automatic user lifecycle operations and are empty when no provisioning connectors are configured.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-25-sign-in-audit-logs/
```
