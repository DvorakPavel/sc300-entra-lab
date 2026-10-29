# Task 10 – Authentication Strengths & Passkeys

**Date:**
2026-04-17 (Phase 1) / 2026-10-29 (Phase 2)

## Goal

Enforce phishing-resistant MFA for administrative accounts using Authentication Strengths in Conditional Access, then narrow the accepted credential set to passkeys held in Microsoft Authenticator by applying AAGUID restrictions to both registration and sign-in.

## Key concepts

### Authentication strength levels

| Level | Accepted methods |
|---|---|
| Multifactor authentication | Password + SMS, Password + Authenticator push |
| Passwordless MFA | Microsoft Authenticator (passwordless), Windows Hello |
| Phishing-resistant MFA | Passkeys (FIDO2), Windows Hello for Business, certificate-based auth |

Authentication strengths control the *quality* of MFA, not just its presence. A custom strength narrows this further by restricting which AAGUIDs satisfy the policy.

### Device-bound vs. synced passkeys

| Type | Private key location | Attestation | Examples |
|---|---|---|---|
| Device-bound | Created and stored on one physical device, never leaves it | Supported | Microsoft Authenticator (iOS/Android), FIDO2 security key |
| Synced | Encrypted locally, synced to the provider's cloud | Not supported | Apple Passwords (iCloud Keychain), Google Password Manager |

Microsoft's guidance is device-bound passkeys for administrators and privileged users, and synced passkeys for standard users.

### Two layers of control

Restricting an organisation to a single authenticator model requires two separate configurations, and conflating them is a common mistake:

| Layer | Where | Controls |
|---|---|---|
| Registration | Authentication methods policy → Passkey (FIDO2) → passkey profile with key restrictions | What a user may *enrol* |
| Sign-in | Conditional Access → custom authentication strength with AAGUID | What a user may *authenticate with* |

An AAGUID on the authentication strength alone does not prevent a user from registering an arbitrary security key — it only prevents that key from satisfying the policy. Blocking enrolment requires the passkey profile.

### Microsoft Authenticator AAGUIDs

| Platform | AAGUID |
|---|---|
| Authenticator for iOS | `90a3ccdf-635c-4729-a248-9b709135078f` |
| Authenticator for Android | `de1e552d-db1d-4423-a619-566b625cdc84` |

AAGUIDs are public vendor identifiers, identical across every installation of a given app or security key model, so they are not sensitive data.

## Phase 1 – Initial implementation (April 2026)

The first implementation applied a built-in authentication strength to the administrator policy and validated it in Report-only mode before enforcement.

- Policy name: CA-Admin-Require-Phishing-Resistant-MFA
- Include: four directory roles (Global Administrator, Helpdesk Administrator, Intune Administrator, User Administrator) plus the lab administrator group
- Exclude: two break-glass administrator accounts
- Target resources: All resources
- Grant control: Require authentication strength → Phishing-resistant MFA (built-in)
- Policy state: Report-only, then On

At this stage, no passkey credential existed in the tenant. The workstation used for administration has no biometric sensor and no attached FIDO2 security key, so there was no way to register or exercise a phishing-resistant credential, and the control could not be validated end to end.

## Phase 2 – Hardening (October 2026)

Once a Bluetooth adapter made cross-device registration possible, the control was completed and narrowed.

### Passkey profile

- Profile name: `PASSKEY-Authenticator-Only`
- Passkey types: Device-bound
- Enforce attestation: No
- Enforce key restrictions: Yes, behaviour Allow
- Allowed AAGUIDs: Authenticator for iOS and Authenticator for Android
- Target: lab administrator group, replacing the Default passkey profile

### Authentication strength

- Name: `AS-Passkey-Authenticator-Only`
- Method: Passkeys (FIDO2)
- Advanced options: AAGUIDs for Authenticator on iOS and Android

### Conditional Access policy

- Grant control changed to: Require authentication strength → `AS-Passkey-Authenticator-Only`
- Policy state: Report-only for validation, then On

## What I did

- Reviewed the built-in authentication strength definitions in Entra.
- Registered a device-bound passkey in Microsoft Authenticator through Security info using the cross-device flow: a QR code displayed on the workstation, scanned by the phone, with a Bluetooth proximity check confirming physical presence.
- Verified the credential in Security info and confirmed its AAGUID matched Microsoft Authenticator for iOS.
- Opted in to passkey profiles, which migrated the existing global passkey settings into a Default passkey profile.
- Created a profile restricted to device-bound passkeys with key restrictions allowing only the Authenticator AAGUIDs for iOS and Android.
- Replaced the Default passkey profile on the administrator group with the restrictive profile, rather than adding it alongside.
- Created a custom authentication strength carrying the same two AAGUIDs.
- Audited the existing grant control before changing it and found the policy was granting on plain "Require multifactor authentication" despite its name.
- Replaced the grant control with the custom authentication strength and returned the policy to Report-only.
- Signed in and confirmed in the sign-in logs that the policy evaluated as Report-only: Success, with Grant Controls reported as Satisfied against the custom strength.
- Promoted the policy to On and re-tested, confirming enforcement.

## Result

Administrative accounts authenticate with a device-bound passkey held in Microsoft Authenticator. Sign-in is fully passwordless — no password is entered at any point. Conditional Access enforces the custom authentication strength for all resources, and the AAGUID allowlist on both the passkey profile and the authentication strength keeps the credential inventory for privileged accounts to a single known authenticator model on either mobile platform.

## Lessons learned

- Authentication Strengths enforce the quality of MFA, not just its presence. The three built-in levels form a hierarchy, each stricter than the previous.
- A grant control named after a security goal does not guarantee it enforces one. The policy carried "Phishing-Resistant" in its name while granting on plain "Require multifactor authentication", which any registered method would satisfy. Policy names are documentation, not enforcement, and the two drift apart over time — auditing the control itself is the only reliable check.
- "Require authentication strength" and "Require multifactor authentication" are mutually exclusive in the same grant control. Switching between them means clearing one before selecting the other.
- Exclusions always take precedence over inclusions. Break-glass accounts holding Global Administrator match the policy's directory-role inclusion, and the exclusion alone keeps them usable. Without it, the policy guarding against lockout would itself cause one. An exclusion that has never been tested is an assumption, not a safeguard.
- Targeting directory roles rather than only a group is the stronger pattern for administrator policies, because it covers anyone who later acquires the role, including activations through PIM.
- Passkey profiles are evaluated independently: a user may register a passkey if they satisfy *any* profile scoped to them. Adding a restrictive profile alongside the permissive Default profile therefore changes nothing — the restrictive profile has to replace it.
- The same OR logic applies to profiles Entra assigns automatically. Tenants where SMS or voice authentication is enabled are in scope for passkey auto-enablement, which assigns a system-managed profile permitting all passkey types without restriction. A correctly configured allowlist can be silently bypassed by a profile the administrator never created.
- Restricting an organisation to one authenticator needs both a passkey profile and an authentication strength. The profile governs enrolment, the strength governs sign-in, and configuring only one leaves a gap.
- Each Authenticator platform has its own AAGUID. Allowing only the iOS value silently blocks every Android user, so both belong in the allowlist unless the platform split is deliberate.
- Removing an AAGUID from an allowlist is a breaking change — users who already registered with that provider lose the ability to sign in with it, not merely the ability to register another.
- Signing in with the passkey was fully passwordless: the passkey replaced the password rather than acting as a second factor alongside it. That is the practical difference between multifactor and phishing-resistant passwordless authentication — a secret that is never typed cannot be phished or replayed.
- The cross-device WebAuthn QR code can be scanned either with the device camera or with the camera built into Authenticator. Both paths were tested on iOS, and both completed the sign-in. Microsoft documents the in-app camera as the fallback for environments such as an Android Work Profile where the system camera app isn't deployed.
- Configured is not the same as verified. The Android AAGUID was added so the control covers both platforms, but no Android credential was registered, so that path remains documented rather than demonstrated.
- Attestation and cross-device registration conflict. With attestation enforced, iOS users can only register inside the Authenticator app itself, which removes the QR-code path that a workstation without biometrics or a security key depends on.
- With attestation disabled, Entra cannot verify any attribute of a passkey, including whether it is device-bound or synced. The admin centre states this explicitly when saving such a profile, and the AAGUID allowlist should then be read as a policy guide rather than a hard security control.
- A policy requiring phishing-resistant authentication for All resources can trap users in a registration loop, because the Authenticator app is itself a cloud application covered by the policy. Splitting the policy by device platform, or scoping it with application filters and issuing a Temporary Access Pass, is the documented way out.
- The successful sign-in came from a device recorded as Unknown and unregistered. The passkey satisfied the requirement on its own, independent of device compliance — credential strength and device trust are separate controls.
- Passkeys in Microsoft Authenticator are device-bound, cannot be synced or restored to a new device, and are limited to one per account. Recovery planning matters more than with synced credentials.
- Passkey registration is not supported for guest users, including B2B collaboration accounts in the resource tenant.
- Users must have completed MFA within the previous five minutes before they can register a passkey.
- Opting in to passkey profiles is irreversible. The tenant supports up to ten profiles, including the Default profile, which cannot be deleted or renamed.
- Report-only mode is where a change like this earns confidence. Validating the custom strength before enforcement turned a potential lockout into a log entry.

## Evidence

```text
evidence/task-10-authentication-strengths/
├── task-10-01-passkey-profile-aaguid-redacted.png
├── task-10-02-passkey-profile-target-admins-redacted.png
├── task-10-03-custom-auth-strength-redacted.png
├── task-10-04-ca-grant-control-redacted.png
├── task-10-05-security-info-passkey-aaguid-redacted.png
├── task-10-06-signin-ca-policy-success-redacted.png
├── task-10-07-signin-ca-policy-on-success-redacted.png
└── task-10-08-signin-report-only-phase1-redacted.png
```

Screenshots 06 and 07 document the Phase 2 rollout in sequence: the policy evaluating as Report-only: Success during validation, then as Enabled: Success once enforced. Screenshot 08 is retained from Phase 1 as the only surviving record of the original report-only evaluation, since sign-in logs are kept for 30 days.
