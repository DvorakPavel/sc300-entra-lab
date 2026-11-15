# Task 23 – Workload Identities & Azure Resource Access

**Date:**
2026-06-05 (Phase 1) / 2026-11-15 (Phase 2)

## Goal

Secure an application identity in Microsoft Entra ID with a certificate credential and application permissions, then extend the same theme into Azure: a managed identity that runs code and reads a secret without any stored credential, governed by Azure RBAC.

## Key concepts

### What counts as a workload identity

A workload identity is an identity used by software rather than a person. Microsoft Entra ID recognises three forms:

| Form | Credential | Lives in |
|---|---|---|
| Application / service principal | client secret or certificate, managed by you | Entra ID |
| Managed identity | none visible — issued and rotated by the platform | Entra ID, tied to an Azure resource |

The distinction matters because credential management is the weak point of the first form and is eliminated entirely in the second.

### Azure roles vs. Entra roles

| | Microsoft Entra roles | Azure roles (RBAC) |
|---|---|---|
| Govern | directory objects — users, groups, applications | Azure resources — subscriptions, resource groups, services |
| Examples | Global Administrator, User Administrator | Owner, Contributor, Reader, Key Vault Secrets User |
| Scope | the tenant | management group, subscription, resource group, resource |

Holding a Global Administrator role in the tenant grants nothing in Azure by default, and owning an Azure subscription does not make anyone a directory administrator. The two systems are deliberately separate.

### Control plane vs. data plane

Azure RBAC separates managing a resource from using its contents. An Owner on a subscription can delete a key vault but cannot read a secret inside it; that requires a data-plane role such as Key Vault Secrets Officer or Key Vault Secrets User. Treating Owner as "access to everything" is the most common misconception when moving a vault to the RBAC permission model.

### Billing ownership vs. directory trust

A subscription trusts exactly one Microsoft Entra tenant, and that trust decides which identities can be given Azure roles. Billing ownership is a separate relationship, held by the account that signed up. Changing the directory is a service-level operation and does not move billing, so the account that pays and the account that administers can be — and often are — in different tenants.

## Phase 1 – Entra workload identity (June 2026)

### Certificate credential

- App: LAB-App-Registration-Test
- Certificate subject: CN=LAB-App-Registration-Test
- Type: self-signed, RSA 2048-bit, SHA256
- Validity: 6 months
- Exported as `.cer` (public key only), uploaded to Certificates & secrets

### Application permission

- API: Microsoft Graph
- Permission: User.Read.All
- Type: Application
- Admin consent: granted

### Permissions overview

| Permission | Type | Status |
|---|---|---|
| User.Read | Delegated | Granted |
| Directory.Read.All | Delegated | Granted |
| User.Read.All | Application | Granted |

### Risky workload identities

Reviewed under Identity Protection — no detections in the lab tenant. Risk detection for workload identities requires the Workload Identities Premium add-on, which is outside the P1/P2 licensing used here, so coverage is limited by design rather than by configuration.

Managed identity could not be demonstrated at this stage because the tenant had no Azure subscription and therefore no Azure resources to attach one to.

## Phase 2 – Azure resource access (November 2026)

### Getting a subscription into the lab tenant

Closing the Phase 1 gap first required an Azure subscription inside the lab tenant, which turned out to be the hardest part of the task.

**A Microsoft 365 Developer Program tenant cannot host an Azure subscription.** Signing up from a developer-tenant account returns *"Your current account type is not supported"*, and the in-portal offers page marks both Free Trial and Pay-As-You-Go as *not eligible*. This is a property of the account type, not a misconfiguration.

The workaround is to create the subscription under a separate account and transfer it:

1. A Pay-As-You-Go subscription was created under a personal Microsoft account. The Azure free account was unavailable because that offer is limited to one per customer and had been used previously.
2. The personal account was invited into the lab tenant as a B2B guest. A directory transfer requires the owning account to exist in both the source and target directories.
3. The transfer was initiated from the source directory and failed on acceptance with *"Your current settings don't allow transferring this subscription."*
4. The cause was a tenant policy. **Since 1 May 2026, Microsoft defaults both subscription transfer policies to "Allow no users (except elevated Global Admins)"**, blocking subscriptions from entering or leaving a directory unless explicitly permitted. The stated reason is that an authorised user — including a guest — could otherwise create a subscription elsewhere using their own billing permissions and move it into the organisation's tenant.
5. Editing those policies requires more than the Global Administrator role: the administrator must first **elevate access** under Entra ID → Properties → *Access management for Azure resources*, which temporarily grants User Access Administrator over everything in the tenant.
6. Rather than switching the policy to *Allow all users*, the personal account was added to the **Allowed Users** exemption list, opening the path for one identity instead of all of them.
7. The transfer was then accepted and completed.

After the move, all Azure role assignments from the source directory were gone, so the lab administrator account was granted Owner on the subscription while elevation was still active. Both the elevation and the policy exemption were reverted immediately afterwards.

The result is a deliberate split: the personal account remains the billing owner and the only identity that can change the payment method or cancel the subscription, while the lab administrator holds Owner and manages everything inside it using identities from the lab tenant.

### Managed identity

- Name: `mi-sc300-lab`, user-assigned
- Resource group: `rg-sc300-lab`
- Attached to an Azure Automation Account

User-assigned was chosen over system-assigned so the identity exists as a resource in its own right, can be attached to several resources, and survives the deletion of any one of them.

### Key Vault

- Permission model: **Azure role-based access control**, not the legacy vault access policy model
- Secret: `lab-test-secret`
- Roles: lab administrator as **Key Vault Secrets Officer**, `mi-sc300-lab` as **Key Vault Secrets User**

The identity was deliberately given the read-only role. A workload that only consumes a secret has no reason to be able to overwrite or delete it.

### Proof of access

A PowerShell runbook in Azure Automation authenticated with `Connect-AzAccount -Identity`, retrieved the secret, and returned it in the job output. The runbook contains no password, certificate, connection string or client secret of any kind.

### Azure RBAC for directory groups

- `GRP-LAB-IT` assigned **Reader** on `rg-sc300-lab` as an **eligible time-bound** assignment rather than active permanent, so members activate the role when they need it and the assignment expires on its own
- Custom role `LAB-KeyVault-Reader-NoDelete` defined from scratch with `Microsoft.KeyVault/vaults/read` and `Microsoft.KeyVault/vaults/secrets/read`, scoped to the resource group
- Effective permissions for a lab user verified with **Check access** and `Get-AzRoleAssignment`

## What I did

- Generated a self-signed certificate in PowerShell and uploaded the public key to the app registration.
- Added the User.Read.All Application permission and granted admin consent.
- Reviewed the Risky workload identities report.
- Established that a developer tenant cannot host an Azure subscription, and created one under a personal account instead.
- Invited the personal account into the lab tenant as a guest so it existed in both directories.
- Diagnosed the failed transfer as a tenant subscription policy, elevated access to edit it, and added a single-account exemption rather than opening the policy to everyone.
- Completed the transfer, restored administrative access to the subscription, and reverted both the elevation and the exemption.
- Created a user-assigned managed identity and a Key Vault using the RBAC permission model.
- Assigned data-plane roles separately from the subscription-level Owner role, giving the identity read-only access.
- Ran an Automation runbook that authenticated as the managed identity and retrieved the secret with no stored credential.
- Assigned an Azure role to a lab security group and built a custom role limited to specific Key Vault actions.
- Analysed effective permissions for a lab user through Check access and PowerShell.

## Result

Workload identity is now demonstrated in both of its forms. The application registration authenticates with a certificate the lab manages; the managed identity authenticates with credentials it never sees. Access to Azure resources is governed by RBAC using groups from the lab tenant, and the subscription itself sits in that tenant while being billed to a separate account.

## Lessons learned

- Certificates are stronger than client secrets because the private key never leaves the client. Entra ID stores only the public portion.
- Application permissions operate without a signed-in user and always require admin consent, which is what makes them suitable for daemons and dangerous when over-granted.
- A managed identity removes the credential rather than protecting it. There is no secret to rotate, leak, expire or commit to a repository — the strongest version of credential hygiene is not having a credential.
- Entra roles and Azure roles are separate systems. Global Administrator grants nothing in Azure by default, which is why the elevation switch exists at all.
- Owner on a subscription does not grant access to data inside a key vault. The control plane manages the resource, the data plane manages its contents, and conflating them produces an RBAC error at exactly the moment the vault is first used.
- Role assignments take up to a minute to propagate. A permission error immediately after granting a role is usually impatience rather than misconfiguration.
- A Microsoft 365 Developer Program tenant cannot hold an Azure subscription, and the portal does not explain "account type is not supported". The only route is to create the subscription elsewhere and transfer the directory.
- Since May 2026, subscription transfer into or out of a tenant is blocked by default. The policy exists because a guest with billing permissions elsewhere could otherwise introduce a subscription into the organisation's directory — an attack path that is easy to overlook when thinking of guests as read-only.
- Editing those policies requires an elevated Global Administrator, not merely a Global Administrator. Elevation grants User Access Administrator across the entire tenant and should be switched off the moment the task that needed it is finished.
- Where a policy supports an exemption list, exempting one account is better than relaxing the policy for everyone. The weaker setting tends to outlive the reason it was weakened.
- Transferring a subscription removes every Azure role assignment, including the one belonging to the administrator performing the move. Planning the route back in before starting is the difference between a migration and a lockout.
- Billing ownership and directory trust are independent. The account that pays cannot necessarily administer the resources, and the administrator cannot change the payment method or cancel the subscription. That separation is a feature, and it mirrors how finance and IT are split in most organisations.
- Azure RBAC reads group membership directly from Microsoft Entra ID. Assigning a role to a directory group is the point where identity governance stops being an Entra-only concern and starts controlling infrastructure.
- A custom role is defined by what it permits, not by what it forbids. Starting from scratch and adding two actions produces a far more predictable result than starting from a built-in role and subtracting.
- The custom role landed both Key Vault permissions in `actions` rather than `dataActions`, which means it grants the ability to list secrets and read their metadata but not to read their values. Reading a value needs `Microsoft.KeyVault/vaults/secrets/getSecret/action` in `dataActions`. The role name promised more than the definition delivered — the same control-plane and data-plane split that stops a subscription Owner from reading a secret, met from the other direction. Reading the generated JSON is what exposes it; the portal's permission picker does not make the distinction obvious.
- An Azure role can be assigned as eligible and time-bound rather than active and permanent, which is Privileged Identity Management applied to Azure resources rather than to directory roles. Standing access to a resource group is the kind of permission that outlives the reason it was granted, so making it something a user activates when needed costs nothing and removes a permanent privilege from the tenant.

## Evidence

```text
evidence/task-23-workload-identities/
├── task-23-01-powershell-certificate-created-redacted.png
├── task-23-02-certificate-uploaded-redacted.png
├── task-23-03-application-permission-added-redacted.png
├── task-23-04-admin-consent-granted-redacted.png
├── task-23-05-azure-signup-not-supported-dev-tenant-redacted.png
├── task-23-06-subscription-transfer-policy-blocked-redacted.png
├── task-23-07-subscription-in-lab-tenant-redacted.png
├── task-23-08-managed-identity-created-redacted.png
├── task-23-09-keyvault-rbac-permission-model-redacted.png
├── task-23-10-keyvault-role-assignments-redacted.png
├── task-23-11-runbook-output-success-redacted.png
├── task-23-12-azure-role-assignment-group-redacted.png
├── task-23-13-custom-role-json-redacted.png
└── task-23-14-check-access-analysis-redacted.png
```

Screenshots 01 to 04 cover Phase 1. Screenshots 05 to 07 document why a developer tenant cannot hold a subscription and how the transfer policy blocked and then permitted the move. Screenshots 08 to 14 cover the Azure resource access work.
