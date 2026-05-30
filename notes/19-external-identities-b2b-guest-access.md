# Task 19 – External Identities: B2B Guest Access

**Date:**  
2026-05-24

## Goal

Configure external collaboration settings, invite a B2B guest user, assign the guest to a lab group, and validate the invitation redemption flow.

## External collaboration settings

- Guest user access: Limited access to directory object properties and memberships
- Guest invite restrictions: Member users and specific admin roles can invite
- Self-service sign-up via user flows: Disabled
- Collaboration restrictions: Allow invitations to any domain

## Cross-tenant access settings

- Inbound access (default): All users and applications allowed
- Outbound access (default): All users and applications allowed
- No organisation-specific overrides configured

## Guest user

- Display name: LAB Guest User
- Email: External personal email address
- User type: Guest
- Source: Invited user
- Group membership: GRP-LAB-HR

## What I did

- Reviewed and documented the tenant-wide external collaboration settings.
- Reviewed the default cross-tenant access settings for inbound and outbound B2B collaboration.
- Invited an external user using the B2B guest invitation flow.
- Verified that the guest user appeared in the directory with User type Guest.
- Assigned the guest user to the GRP-LAB-HR security group.
- Accepted the invitation from the guest user's external email account.
- Confirmed the invitation redemption in audit logs.

## Result

A B2B guest user was successfully invited, added to a lab group, and the full invitation redemption flow was validated. External collaboration settings and cross-tenant access defaults were reviewed and documented.

## Lessons learned

- Guest users have User type Guest, which limits their default directory permissions compared to Member users.
- B2B invitations create a federated identity - the guest authenticates using their home identity provider.
- Cross-tenant access settings control which organisations can collaborate through B2B at the tenant level.
- Guest invite restrictions determine who in the organisation is allowed to send B2B invitations.
- Guest users can be added to security groups and are subject to Conditional Access policies.
- The invitation must be redeemed (accepted) before the guest can access tenant resources.
- Audit logs record the invitation event for compliance and governance tracking.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-19-external-identities-b2b-guest-access/
```
