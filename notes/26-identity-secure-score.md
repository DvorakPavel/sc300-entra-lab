# Task 26 – Identity Secure Score & Usage Insights

**Date:**
2026-06-06

## Goal

Review the Identity Secure Score to assess the tenant's security posture and explore Usage & insights for authentication method and application activity monitoring.

## Identity Secure Score

- Location: Identity → Monitoring & health → Identity Secure Score
- Score provides a percentage-based assessment of the tenant's identity security posture
- Includes comparison with similar tenants and industry benchmarks
- Lists improvement actions with current score, max score, and implementation guidance

## Improvement actions reviewed

- Identified completed actions resulting from previous lab tasks (MFA enforcement, legacy authentication block, Conditional Access policies)
- Reviewed outstanding recommendations for further security hardening
- Analysed the details of individual improvement actions, including implementation steps

## Usage & insights

### Authentication methods activity

- Displays registered authentication methods across users
- Shows registration and usage statistics
- Provides MFA registration coverage visibility

### Application activity

- Displays sign-in activity per enterprise application
- Helps identify unused applications for governance and cleanup

## What I did

- Reviewed the Identity Secure Score and overall security posture assessment.
- Examined the improvement actions list and identified completed and outstanding recommendations.
- Analysed a specific improvement action in detail, including its score contribution.
- Reviewed authentication methods usage and registration statistics.
- Reviewed application activity to identify active enterprise applications.

## Result

The Identity Secure Score was reviewed, and improvement actions were examined. Several completed actions correspond to Conditional Access policies and authentication configurations from previous tasks. Usage & insights confirmed MFA registration coverage and application activity across the tenant.

## Lessons learned

- Identity Secure Score aggregates security best practices into a single metric for executive-level reporting.
- Improvement actions provide specific guidance on what to configure and how much each action contributes to the overall score.
- Existing CA policies and authentication configurations contribute directly to the Secure Score.
- Usage & insights provides authentication method registration and usage statistics without requiring additional licensing.
- Application activity monitoring helps identify unused enterprise applications for access governance.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-26-identity-secure-score/
```
