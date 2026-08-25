# Project Overview

## Microsoft Entra Zero Trust Conditional Access

This project demonstrates the design, implementation, testing and controlled enforcement of Microsoft Entra Conditional Access policies in a home-lab environment.

The solution applies Zero Trust principles to protect fictional users, privileged administrators and Microsoft cloud resources.

## Business Scenario

A fictional organisation required stronger identity security without creating unnecessary disruption or risking administrator lockout.

The organisation needed to:

* Require multifactor authentication for selected users
* Protect access to Microsoft administration portals
* Block legacy authentication protocols
* Respond to risky sign-in activity
* Maintain emergency administrative access
* Test policies safely before enforcement
* Review actual sign-in results
* Produce clear technical evidence

## Zero Trust Principles

The project follows three core Zero Trust principles:

### Verify explicitly

Access decisions use identity, role, application, client type and sign-in risk signals.

### Use least-privilege access

Conditional Access testing was performed using a dedicated pilot user. A temporary administrative role used for validation was removed immediately after testing.

### Assume breach

Legacy authentication was blocked in policy design, risky sign-ins required additional verification and emergency access procedures were prepared before enforcement.

## Lab Environment

The project used:

* Microsoft Entra ID
* Microsoft Entra ID Premium P2
* Microsoft Entra Conditional Access
* Microsoft Entra Identity Protection risk signals
* Microsoft Authenticator
* Microsoft Entra sign-in logs
* Conditional Access What If
* Fictional cloud-only test identities

## Identity and Group Design

The following controlled identities and groups were prepared:

| Object                     | Purpose                                                      |
| -------------------------- | ------------------------------------------------------------ |
| Emergency Access Admin 01  | Cloud-only emergency administrator                           |
| Emergency Access Admin 02  | Secondary cloud-only emergency administrator                 |
| SG-CA-Emergency-Exclusions | Excludes emergency accounts from Conditional Access policies |
| CA Test User 01            | Dedicated Conditional Access pilot identity                  |
| SG-CA-Pilot-Users          | Limits policy enforcement to controlled test users           |

## Network Signal

A trusted named location called `NL-Trusted-Home-Lab` was created using a public IP range.

The IP address is intentionally excluded from this repository. The named location demonstrates how network signals can support Conditional Access decisions without exposing confidential environmental information.

## Conditional Access Policies

| Policy                              | Purpose                                                           | Final project state |
| ----------------------------------- | ----------------------------------------------------------------- | ------------------- |
| CA001-Require-MFA-For-Pilot-Users   | Requires MFA for the controlled pilot group                       | On                  |
| CA002-Block-Legacy-Authentication   | Blocks legacy authentication clients                              | Report-only         |
| CA003-Require-MFA-For-Admin-Portals | Protects privileged roles accessing administration resources      | Report-only         |
| CA004-Require-MFA-For-Risky-SignIns | Requires MFA and reauthentication for medium or high sign-in risk | Report-only         |

Only the pilot-user MFA policy was enabled after successful simulation. The remaining policies stayed in Report-only mode to demonstrate staged deployment and prevent unintended impact.

## Safe Deployment Process

The implementation followed this sequence:

1. Created two cloud-only emergency administrator accounts.
2. Added both accounts to a dedicated Conditional Access exclusion group.
3. Created a fictional pilot identity and security group.
4. Assigned the required Microsoft Entra ID P2 licence.
5. Created a trusted named location.
6. Created four Conditional Access policies.
7. Kept all new policies in Report-only mode initially.
8. Tested each policy using Conditional Access What If.
9. Corrected a missing target-resource configuration discovered during testing.
10. Temporarily assigned an administrative role for role-based policy validation.
11. Removed the temporary administrative role after testing.
12. Enabled only the pilot MFA policy.
13. Performed a successful real sign-in and MFA challenge.
14. Verified the enforced result in Microsoft Entra sign-in logs.

## Security Controls

The project demonstrates:

* Emergency access planning
* Conditional Access exclusions
* Group-based pilot deployment
* Multifactor authentication
* Administrator protection
* Legacy authentication blocking
* Risk-based access decisions
* Session reauthentication
* Named locations
* Report-only deployment
* What If simulation
* Sign-in log investigation
* Controlled enforcement
* Temporary privilege cleanup

## Security Notice

All identities used in this project are fictional.

The repository does not contain passwords, authentication QR codes, access tokens, recovery codes, telephone numbers, public IP addresses or confidential tenant information. Evidence was reviewed and sanitised before publication.
