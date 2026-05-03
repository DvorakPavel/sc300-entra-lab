# Task 18 – User Consent Settings and Admin Consent Workflow

**Date:**  
2026-05-03

## Goal

Configure tenant-level user consent settings and enable the admin consent workflow for applications that require administrative approval.

## Tenant consent configuration

- User consent for applications: Do not allow user consent
- Admin consent workflow: Enabled
- Reviewers: GRP-LAB-IT and lab administrator account
- Email notifications: Enabled
- Request expiration: 30 days

## Application

- App Registration: LAB-App-Registration-Test
- Enterprise Application: LAB-App-Registration-Test
- API: Microsoft Graph
- Permission added: Directory.Read.All
- Permission type: Delegated
- Admin consent required: Yes
- Admin consent status: Not granted during initial validation

## What I did

- Opened tenant-wide user consent settings.
- Restricted standard users from granting consent to applications.
- Enabled the admin consent workflow.
- Configured GRP-LAB-IT and a lab administrator account as reviewers for admin consent requests.
- Added a Microsoft Graph delegated permission requiring admin consent to the existing lab application.
- Verified that the Directory.Read.All delegated permission required administrative approval.
- Confirmed that admin consent for Directory.Read.All was not granted during initial validation.

## Result

User consent is now restricted at the tenant level.

The admin consent workflow is enabled and configured with lab reviewers. Applications that require administrative approval can now be handled through a controlled review process instead of being approved directly by standard users.

The LAB-App-Registration-Test application was configured with the Microsoft Graph Directory.Read.All delegated permission. This permission requires admin consent and remained ungranted during validation.

The end-user admin consent request flow was not fully tested because the lab application does not currently have a working application sign-in experience or redirect flow. Further validation would require a functional OAuth/OIDC sign-in flow or a test redirect URI.

## Lessons learned

- User consent settings control whether standard users can grant application permissions.
- Restricting user consent reduces the risk of unauthorized or risky application access.
- Admin consent workflow provides a controlled process for reviewing application permission requests.
- Reviewers can be assigned to review requests, but approval still requires appropriate administrative permissions.
- API permissions should be reviewed carefully before granting tenant-wide admin consent.
- Microsoft Graph permissions such as Directory.Read.All should be treated as sensitive.
- Requested API permissions are configured on the App Registration.
- Granted permissions can be reviewed from the related Enterprise Application.
- A full end-user consent request validation requires a working OAuth/OIDC application flow.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-18-user-consent-and-admin-consent-workflow/
```
