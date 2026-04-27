# Task 16 – Conditional Access for Enterprise Application

**Date:**  
2026-04-27

## Goal

Create a Conditional Access policy targeted to a specific Enterprise Application and
require multi-factor authentication for that application only.

---

## Conditional Access Policy

- Policy name: CA-Require-MFA-LAB-App-Registration-Test
- Target users: Primary admin account
- Target resource: LAB-App-Registration-Test
- Conditions: None
- Grant control: Require multi-factor authentication
- Policy mode: Report-only

The policy was designed to apply MFA only when accessing the selected Enterprise
Application, without affecting other applications or users in the tenant.

---

## Target Scope

The Conditional Access policy was scoped to:

- Resource type: Resources (formerly cloud apps)
- Include: Select resources
- Selected resource: LAB-App-Registration-Test

This confirms that the policy targets the Enterprise Application instance rather than
the App Registration object.

---

## Policy Evaluation

A new sign-in event was generated for the application after the policy was created.

The sign-in logs for the Enterprise Application show that the Conditional Access policy:

- Was evaluated in report-only mode
- Applied the MFA grant control
- Returned the result: Report-only: Success

This confirms that the policy was correctly targeted and successfully evaluated during
the sign-in flow.

---

## Result

- The Conditional Access policy was created successfully.
- The policy targets the correct Enterprise Application.
- MFA is configured as the required grant control.
- Report-only mode was used to safely validate the policy behavior.
- The policy evaluation is visible in Enterprise Application sign-in logs.

---

## Lessons Learned

- Conditional Access for applications is applied to the Enterprise Application
  (service principal), not to the App Registration object.
- Report-only mode is the safest way to validate application-targeted CA policies.
- Targeting a specific Enterprise Application allows precise access control.
- MFA requirements can be scoped to individual applications without broader tenant impact.
- Sign-in logs are the correct place to validate Conditional Access evaluation results.
