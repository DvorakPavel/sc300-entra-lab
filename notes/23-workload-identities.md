# Task 23 – Workload Identities: Certificates & Application Permissions

**Date:**
2026-06-05

## Goal

Add a certificate credential to an existing App Registration, configure an Application permission with admin consent, and review workload identity risk monitoring.

## Certificate credential

- App: LAB-App-Registration-Test
- Certificate subject: CN=LAB-App-Registration-Test
- Type: Self-signed (RSA 2048-bit, SHA256)
- Validity: 6 months
- Exported as: .cer (public key only)
- Uploaded to: Certificates & secrets → Certificates tab

## Application permission added

- API: Microsoft Graph
- Permission: User.Read.All
- Type: Application
- Admin consent: Granted

## Permissions overview

- User. Read — Delegated — Granted (admin consent)
- Directory.Read.All — Delegated — Not granted
- User.Read.All — Application — Granted (admin consent)

## Risky workload identities

- Location: Protection → Identity Protection → Risky workload identities
- Result: No risky workload identities detected

## Note on Managed Identity

- Managed Identity cannot be created in the dev tenant because no Azure resources are provisioned
- Two types: System-assigned (tied to a single resource) and User-assigned (shared across resources)
- Credentials are managed and rotated automatically by the Azure platform

## What I did

- Generated a self-signed certificate in PowerShell and exported the public key as a .cer file.
- Uploaded the certificate to LAB-App-Registration-Test.
- Added the User.Read.All Application permissions from Microsoft Graph.
- Granted admin consent for the Application permission.
- Reviewed the API permissions list with both Delegated and Application permission types.
- Checked the Risky workload identities report in Identity Protection.

## Result

The app registration was secured with a certificate credential and configured with an Application permission for daemon-style access. The Risky workload identities report was reviewed with no detections in the dev tenant.

## Lessons learned

- Certificates are more secure than client secrets — the private key never leaves the client machine.
- Entra ID only stores the public key portion of the certificate.
- Application permissions operate without user context and always require admin consent.
- Identity Protection monitors workload identities for anomalous behaviour through the Risky workload identities report.
- Managed Identity eliminates manual credential management for Azure resources but requires provisioned Azure resources.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-23-workload-identities/
```
