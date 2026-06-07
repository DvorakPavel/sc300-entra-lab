# Task 31 – Cross-tenant Access Settings

**Date:**
2026-06-07

## Goal

Configure cross-tenant access default settings for inbound trust and review outbound access defaults to manage B2B collaboration security.

## Inbound default trust settings

- Trust multifactor authentication from Microsoft Entra tenants: Yes
- Trust compliant devices: Yes
- Trust Microsoft Entra hybrid joined devices: Yes

## Outbound defaults

- B2B collaboration: Allowed (default)
- B2B direct connect: Blocked (default)

## What I did

- Reviewed the cross-tenant access settings overview and existing default configuration.
- Configured inbound default trust settings for MFA, compliant devices, and hybrid joined devices.
- Reviewed outbound default settings for B2B collaboration and B2B direct connect.
- Verified the overall cross-tenant access configuration after changes.

## Result

Cross-tenant access default trust settings were configured to accept MFA claims, device compliance, and hybrid join status from external Microsoft Entra tenants. This reduces authentication friction for B2B guests while maintaining security through trust federation. Outbound defaults were reviewed and confirmed at their default state.

## Lessons learned

- Cross-tenant access settings control how B2B collaboration works between tenants — inbound governs what external users can access, outbound governs where internal users can go.
- Trust settings eliminate redundant MFA prompts for B2B guests — without MFA trust, guests must complete MFA again in the resource tenant even if they already authenticated in their home tenant.
- Device compliance trust extends Conditional Access device-based controls to B2B scenarios by recognising the guest's home tenant device state.
- Default settings apply to all external organisations without explicit per-organisation configuration. Per-organisation settings override defaults for specific tenants.
- B2B collaboration (guest accounts) is allowed by default. B2B direct connect (Teams shared channels) is blocked by default and requires explicit per-organisation configuration.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-31-cross-tenant-access-settings/
```
