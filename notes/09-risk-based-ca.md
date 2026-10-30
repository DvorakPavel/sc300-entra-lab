# Task 9 – Risk-based Conditional Access

**Date:**
2026-04-15 (Phase 1) / 2026-10-30 (Phase 2)

## Goal

Implement risk-based Conditional Access using Microsoft Entra ID Protection, then migrate the tenant off the retired legacy risk policies and modernise the remediation controls for a passwordless administrator estate.

## Key concepts

### The three legacy ID Protection policies

ID Protection historically carried three standalone policies, all of which are being retired in favour of Conditional Access:

| Legacy policy | Purpose | Conditional Access replacement |
|---|---|---|
| Sign-in risk policy | React to a suspicious session | CA policy with a Sign-in risk condition |
| User risk policy | React to a compromised credential | CA policy with a User risk condition |
| MFA registration policy | Protect the act of registering security info | CA policy targeting the **Register security info** user action |

Legacy policies could not be scoped per application, combined with other conditions, or staged through Report-only mode. Moving them into Conditional Access puts risk alongside every other signal in a single evaluation model.

### Sign-in risk vs. user risk

- **Sign-in risk** scores the individual session — impossible travel, anonymous IP, unfamiliar sign-in properties. The credential may be fine; the session looks wrong.
- **User risk** scores the account — leaked credentials, risk detections accumulated over time. The session may look fine; the credential is suspect.

The two call for different responses: a suspicious session warrants re-proving identity, a suspect credential warrants replacing it.

### Remediation controls

| Grant control | Behaviour | Fits |
|---|---|---|
| Require password change | Forces a password reset to clear user risk | Password-based accounts |
| Require risk remediation | Lets ID Protection drive remediation regardless of sign-in method | Password and passwordless accounts alike |

`Require risk remediation` requires Entra ID P2 and can only be used on a policy assigned to **All resources** — it cannot be scoped to individual applications. Selecting it automatically adds **Require authentication strength** as a grant control and forces **Sign-in frequency: Every time** as a session control. It overrides `Require password change` rather than stacking with it. It only revokes refresh tokens, not access tokens, and it is not supported for external or guest users, because Entra ID cannot revoke their sessions.

## Phase 1 – Initial implementation (April 2026)

### Policy 1 — Sign-in risk

- Policy name: CA-Risk-SignIn-Require-MFA
- Users: All users
- Exclusions: Two break-glass accounts
- Resources: All resources
- Condition: Sign-in risk level Medium or High
- Grant control: Require multifactor authentication
- Policy state: Report-only, then On

### Policy 2 — User risk

- Policy name: CA-Risk-User-Force-Password-Change
- Users: All users
- Exclusions: two break-glass accounts
- Resources: All resources
- Condition: User risk level is High
- Grant control: Require password change
- Policy state: Report-only, then On

Both policies were built directly in Conditional Access rather than as legacy ID Protection policies.

## Phase 2 – Legacy retirement and modernisation (October 2026)

Microsoft retired legacy risk policy management in ID Protection: the user risk and sign-in risk settings can no longer be viewed or changed there, and the portal warns of a protection gap unless Conditional Access replacements exist. Because Phase 1 had been built in Conditional Access from the start, the retirement required no migration — but it did surface two gaps.

### Gap 1 — Remediation control mismatched the credential model

The user risk policy forced a password change. After Task 10 moved administrators to passkeys, those accounts sign in without ever entering a password, so a password reset is a poor remediation for them.

- Grant control changed to: Require risk remediation
- Require authentication strength added automatically, set to the built-in Multifactor authentication strength
- Sign-in frequency: Every time, applied automatically and non-configurable
- Guest and external users excluded, since the control cannot revoke their sessions
- Policy renamed from CA-Risk-User-Force-Password-Change to `CA-Risk-User-Require-Remediation`, so the name continues to describe what the policy enforces

### Gap 2 — No replacement for the MFA registration policy

Registering security info was unprotected. A new policy closes that:

- Policy name: CA-Protect-Security-Info-Registration
- Users: All users, excluding break-glass accounts and guests
- Target resources: User actions → Register security info
- Grant control: Require multifactor authentication
- Policy state: Report-only

## What I did

- Created a CA policy requiring MFA when sign-in risk is Medium or High.
- Created a CA policy acting on High user risk, excluding emergency access accounts from both.
- Ran both in Report-only mode, reviewed the evaluation results in the sign-in logs, and promoted them to On.
- Reviewed the ID Protection blade after legacy risk policy management was retired and confirmed both Conditional Access replacements were already in place and enforcing.
- Verified the sign-in risk condition still covered Medium as well as High, so no detections would be lost with the legacy policy gone.
- Replaced the user risk grant control with Require risk remediation and excluded guest and external users.
- Created a Conditional Access policy protecting the Register security info user action, replacing the retired MFA registration policy.

## Result

All three legacy ID Protection policies are now represented in Conditional Access. Risk signals are evaluated alongside every other condition, each policy can be staged through Report-only, and the user risk remediation path works for passwordless accounts as well as password-based ones.

## Lessons learned

- Risk-based Conditional Access is adaptive security: MFA is triggered by threat signals rather than applied statically to every sign-in.
- Sign-in risk and user risk answer different questions — a suspicious session versus a suspect credential — and warrant different remediations.
- Building risk policies in Conditional Access rather than in ID Protection turned a forced migration into a verification exercise. The portal still showed the generic "action required" banner, which is displayed regardless of whether replacements exist, so the banner is a prompt to check rather than evidence of a gap.
- When a legacy control is retired, the replacement must be checked for *scope* as well as existence. A Conditional Access policy covering only High sign-in risk would silently drop the Medium detections the legacy policy caught.
- A remediation control has to match the credential model. Forcing a password change on an account that authenticates with a passkey remediates nothing the attacker could use — the password was never the sign-in path. Require risk remediation exists because passwordless estates broke the old assumption.
- Require risk remediation carries hidden side effects: it silently adds an authentication strength requirement and pins sign-in frequency to Every time. Both are reasonable defaults, but an administrator who doesn't read the control ends up enforcing more than intended.
- Controls that depend on session revocation cannot work on identities the tenant doesn't own. Guest and external users need separate treatment, and excluding them is a design decision rather than an oversight.
- Protecting the Register security info action conflicts directly with any plan to remove a user's only authentication method. Requiring MFA to register security info, for a user who has no method left to satisfy it, is a closed loop — Temporary Access Pass or a trusted-network exclusion is what breaks it.
- Policy names drift from policy behaviour. Changing a grant control without renaming the policy leaves documentation that actively misleads the next administrator.
- Risk-based policies are among the highest contributors to Identity Secure Score.

## Evidence

```text
evidence/task-09-risk-based-ca/
├── task-09-01-ca-signin-risk-report-only-redacted.png
├── task-09-02-ca-user-risk-report-only-redacted.png
├── task-09-03-legacy-risk-policy-retired-redacted.png
├── task-09-04-ca-user-risk-require-remediation-redacted.png
└── task-09-05-ca-register-security-info-policy-redacted.png
```

Screenshots 01 and 02 are from Phase 1 and show both policies during their Report-only evaluation. Screenshots 03 to 05 document Phase 2: the retirement notice in ID Protection, the modernised user risk grant control, and the policy replacing the legacy MFA registration policy.
