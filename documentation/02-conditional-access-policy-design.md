# Conditional Access Policy Design

## Design Objectives

The Conditional Access design protects users and administrative resources while reducing the risk of accidental tenant lockout.

The implementation uses:

* Emergency access exclusions
* Security-group-based pilot deployment
* Report-only evaluation
* Microsoft-recommended templates
* Identity Protection risk signals
* Controlled enforcement
* Sign-in log validation

## Naming Standard

The following naming format was used:

```text
CA###-Purpose
```

Example:

```text
CA001-Require-MFA-For-Pilot-Users
```

Numbering makes policies easier to identify, document and troubleshoot.

## Policy Summary

| ID    | Policy                         | Identity scope             | Resource scope          | Control                              | State       |
| ----- | ------------------------------ | -------------------------- | ----------------------- | ------------------------------------ | ----------- |
| CA001 | Require MFA for pilot users    | SG-CA-Pilot-Users          | All resources           | MFA authentication strength          | On          |
| CA002 | Block legacy authentication    | All users                  | All resources           | Block access                         | Report-only |
| CA003 | Require MFA for admin portals  | Privileged directory roles | Microsoft admin portals | Require MFA                          | Report-only |
| CA004 | Require MFA for risky sign-ins | SG-CA-Pilot-Users          | All resources           | MFA and sign-in frequency every time | Report-only |

All policies exclude:

```text
SG-CA-Emergency-Exclusions
```

## CA001 — Require MFA for Pilot Users

### Purpose

Provides a controlled MFA deployment for a dedicated pilot group before wider organisational rollout.

### Configuration

| Setting                 | Value                           |
| ----------------------- | ------------------------------- |
| Included identities     | SG-CA-Pilot-Users               |
| Excluded identities     | SG-CA-Emergency-Exclusions      |
| Target resources        | All resources                   |
| Conditions              | None                            |
| Grant control           | Require authentication strength |
| Authentication strength | Multifactor authentication      |
| Final state             | On                              |

### Design Decision

The policy was initially configured in Report-only mode and tested with Conditional Access What If.

It was enabled only after the simulation showed the expected result. Because the policy targets only the pilot group, normal users and administrators were not affected by enforcement.

## CA002 — Block Legacy Authentication

### Purpose

Blocks authentication clients that use older protocols and cannot properly support modern MFA and Conditional Access controls.

### Configuration

| Setting             | Value                                                |
| ------------------- | ---------------------------------------------------- |
| Included identities | All users                                            |
| Excluded identities | SG-CA-Emergency-Exclusions                           |
| Target resources    | All resources                                        |
| Client apps         | Exchange ActiveSync clients and other legacy clients |
| Grant control       | Block access                                         |
| Final state         | Report-only                                          |

### Design Decision

The policy was kept in Report-only mode because legacy authentication can be used by older applications, devices or service accounts.

A real organisation should review report-only results and identify business dependencies before enforcement.

## CA003 — Require MFA for Admin Portals

### Purpose

Protects privileged Microsoft Entra roles when accessing Microsoft administration resources.

### Privileged Roles

The Microsoft template targets highly privileged built-in roles such as:

* Global Administrator
* Privileged Role Administrator
* Conditional Access Administrator
* Security Administrator
* User Administrator
* Authentication Administrator
* Privileged Authentication Administrator
* Application Administrator
* Cloud Application Administrator
* Exchange Administrator
* SharePoint Administrator

### Configuration

| Setting             | Value                               |
| ------------------- | ----------------------------------- |
| Included identities | Privileged built-in directory roles |
| Excluded identities | SG-CA-Emergency-Exclusions          |
| Target resources    | Microsoft administration portals    |
| Grant control       | Require multifactor authentication  |
| Final state         | Report-only                         |

### Validation Method

`CA Test User 01` was temporarily assigned the User Administrator role.

The What If tool was then used with the Azure Resource Manager application to verify that the policy applied to the privileged identity.

The role was removed immediately after testing.

## CA004 — Require MFA for Risky Sign-ins

### Purpose

Uses Microsoft Entra Identity Protection sign-in risk signals to challenge suspicious authentication attempts.

### Configuration

| Setting             | Value                              |
| ------------------- | ---------------------------------- |
| Included identities | SG-CA-Pilot-Users                  |
| Excluded identities | SG-CA-Emergency-Exclusions         |
| Target resources    | All resources                      |
| Sign-in risk        | Medium and High                    |
| Grant control       | Require multifactor authentication |
| Session control     | Sign-in frequency — Every time     |
| Final state         | Report-only                        |

### Troubleshooting Finding

The first What If simulation showed that this policy did not apply.

Policy inspection revealed:

```text
Included resources: No resources
```

The policy had valid users and risk conditions but no protected resource. The target-resource configuration was corrected to:

```text
All resources
```

After the correction, the policy successfully appeared under **Policies that will apply**.

## Emergency Access Exclusion

Two cloud-only emergency administrators were placed in:

```text
SG-CA-Emergency-Exclusions
```

This group is excluded from policies that block or restrict access.

A What If simulation using an emergency administrator confirmed that the Conditional Access policies did not apply to the account.

## Named Location

The following trusted location was created:

```text
NL-Trusted-Home-Lab
```

The location uses a public IP range marked as trusted. The actual IP address is excluded from all documentation and screenshots.

The location was prepared as a network signal for future location-based access controls. No location-blocking policy was enforced during this project.

## Policy Interaction

Conditional Access evaluates every applicable policy.

During the legacy authentication test:

* CA001 required multifactor authentication.
* CA002 required access to be blocked.

The block control takes precedence over a grant control. This demonstrated how multiple policies can affect one sign-in and why combined policy testing is necessary.

## Deployment Decision

Only `CA001` was enabled because:

* It targeted a controlled pilot group.
* What If testing succeeded.
* MFA registration was available.
* A real sign-in was successfully completed.
* Sign-in logs confirmed the policy result.

The remaining policies were intentionally kept in Report-only mode pending a longer monitoring period and business-impact review.
