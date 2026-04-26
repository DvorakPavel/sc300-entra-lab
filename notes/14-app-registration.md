# Task 14 – App Registration (Workload Identities)

**Date:**  
2026-04-26

## Goal

Create an application identity using Microsoft Entra App Registration and understand
the fundamentals of workload identities, including application identifiers,
credentials, and API permissions.

---

## App Registration Overview

- Application name: LAB-App-Registration-Test
- Account type: Single tenant (organizational directory only)
- Redirect URI: Not configured
- Status: Enabled

The application is configured as a single-tenant workload identity intended for
internal use and testing scenarios.

---

## Application Identity

The following identifiers were generated during registration:

- Application (Client) ID  
  Used by the application to identify itself when requesting tokens.

- Directory (Tenant) ID  
  Identifies the Microsoft Entra tenant where the application is registered.

- Object ID  
  Internal identifier used by Entra ID to reference the application object.

The client ID is used by external systems, while the object ID is used only within
Microsoft Entra ID.

---

## Application Credentials

- Credential type: Client secret
- Description: LAB-App-Secret
- Validity period: 6 months

The client secret represents an application credential used for service-to-service
authentication. It must be stored securely and is not recoverable after creation.

---

## API Permissions

- API: Microsoft Graph
- Permission type: Delegated
- Permission added: User.Read
- Admin consent: Not granted

Delegated permissions allow the application to access resources on behalf of a signed-
in user. Admin consent was intentionally not granted in this task and will be
addressed in a subsequent task.

---

## Result

- A workload identity was created using App Registration.
- The application has a unique client ID and tenant association.
- A client secret was generated for authentication.
- Delegated Microsoft Graph permissions were configured.
- The application is prepared for further integration and consent configuration.

---

## Lessons Learned

- App registrations define application identities, not user access.
- Client ID and Object ID serve different purposes and must not be confused.
- Client secrets represent credentials and must be protected.
- Delegated permissions require user context and optional admin consent.
- App registrations are the foundation for Enterprise Applications and consent flows.
