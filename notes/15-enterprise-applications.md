# Task 15 – Enterprise Applications & Consent

**Date:**  
2026-04-26

## Goal

Understand how an application registered in Microsoft Entra ID is represented as an
Enterprise Application (service principal) and how access, consent, and runtime usage
are managed at the tenant level.

---

## Enterprise Application

- Name: LAB-App-Registration-Test
- Origin: Automatically created from App Registration
- Activation status: Activated

The enterprise application represents the service principal instance of the app in the
tenant and is the primary object used for access control, consent, Conditional Access,
and monitoring.

---

## Assignment Configuration

- Assignment required: Yes

Requiring assignment ensures that only explicitly assigned users can access the
application, enforcing the principle of least privilege.

---

## User Assignment

- Assigned user: Primary admin account
- Assigned role: Default access

User access is controlled at the Enterprise Application level and is independent from
the App Registration configuration.

---

## Permissions and Consent

- API: Microsoft Graph
- Permission type: Delegated
- Permission: User.Read
- Consent type: Admin consent
- Consent status: Granted

Admin consent was granted to allow delegated Microsoft Graph access without requiring
individual user consent prompts.

---

## Runtime Validation

- Log source: Enterprise Application sign-in logs
- Sign-in type: User sign-ins (interactive)
- Observed results:
  - Successful sign-in
  - Interrupted sign-in (expected during authentication/consent flow)
- Conditional Access: Not applied

Sign-in logs confirm that the Enterprise Application is actively used and that delegated
authentication flows function correctly.

---

## Result

- The Enterprise Application is active and properly configured.
- Assignment-based access control is enforced.
- Delegated permissions are approved via admin consent.
- Application usage is visible and auditable in sign-in logs.
- The distinction between App Registration and Enterprise Application was validated.

---

## Lessons Learned

- App Registrations define application identity, not access.
- Enterprise Applications govern tenant-specific access and enforcement.
- Assignment requirement is a critical security control.
- Admin consent centralizes delegated permission management.
- Sign-in logs are the authoritative source for application runtime troubleshooting.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-15-enterprise-applications/
```
