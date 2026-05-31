# Task 21 – Terms of Use

**Date:**  
2026-05-31

## Goal

Create a Terms of Use policy, integrate it with Conditional Access, and validate the acceptance flow.

## Note on Lifecycle Workflows

Lifecycle Workflows were explored, but require a Microsoft Entra ID Governance license that is not included in the Microsoft 365 Developer tenant. The concept of joiner, mover, and leaver automation was reviewed.

## Terms of Use configuration

- Name: LAB-Acceptable-Use-Policy
- Display name: LAB Acceptable Use Policy
- Document: PDF with acceptable use terms
- Require users to expand the terms: Yes
- Require consent on every device: No
- Expire consents: No

## Conditional Access policy

- Policy name: CA-Require-Terms-of-Use
- Users: GRP-LAB-HR
- Resources: All resources
- Grant control: LAB-Acceptable-Use-Policy (Terms of Use)
- Policy state: Report-only

## What I did

- Created a PDF document with acceptable use policy terms.
- Configured a Terms of Use in Microsoft Entra Identity Governance.
- Created a Conditional Access policy that requires acceptance of the Terms of Use.
- Scoped the policy to the HR pilot group.
- Reviewed the Terms of Use acceptance report.

## Result

A Terms of Use policy was created and integrated with Conditional Access. Users in the HR pilot group are required to accept the terms before accessing resources. The acceptance report provides visibility into compliance status.

## Lessons learned

- Terms of Use integrate with Conditional Access as a grant control alongside MFA.
- PDF documents must be uploaded when creating Terms of Use.
- The "require users to expand" setting ensures users open and read the document before accepting.
- Terms of Use acceptance can be tracked through built-in acceptance reports.
- Expired consents can be configured to require periodic re-acceptance for compliance.
- Terms of Use are part of Identity Governance and support regulatory compliance requirements.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-21-terms-of-use/
```
