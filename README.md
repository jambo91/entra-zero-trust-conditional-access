# Microsoft Entra Zero Trust Conditional Access

## Project Status

🟢 **Completed** — Conditional Access design, simulation, controlled enforcement, troubleshooting and evidence collection completed.

## Project Overview

This home-lab project demonstrates an enterprise-style Microsoft Entra Conditional Access implementation using Zero Trust principles.

The solution protects fictional users and privileged administrators through multifactor authentication, legacy authentication blocking, risk-based access controls, emergency access exclusions and staged policy deployment.

The project goes beyond policy creation by demonstrating safe rollout, What If simulation, real sign-in testing, sign-in log analysis and documented troubleshooting.

## Business Scenario

A fictional organisation required stronger identity security while avoiding user disruption and administrator lockout.

The organisation needed to:

* Require MFA for controlled pilot users
* Protect privileged access to Microsoft administration portals
* Block legacy authentication protocols
* Respond to medium and high-risk sign-ins
* Preserve emergency administrative access
* Test security controls before enforcement
* Investigate policy results through sign-in logs
* Maintain clear implementation evidence

## Zero Trust Principles

The implementation follows three Zero Trust principles:

* **Verify explicitly** — access decisions use identity, role, application, client type and sign-in-risk signals.
* **Use least privilege** — policy testing used a controlled pilot account, and temporary administrative access was removed after validation.
* **Assume breach** — legacy authentication was restricted, risky sign-ins required additional verification and emergency access was prepared before enforcement.

## Solution Components

| Component                               | Purpose                                                 |
| --------------------------------------- | ------------------------------------------------------- |
| Two cloud-only emergency administrators | Provide redundant recovery access                       |
| SG-CA-Emergency-Exclusions              | Excludes emergency accounts from restrictive policies   |
| CA Test User 01                         | Dedicated fictional pilot identity                      |
| SG-CA-Pilot-Users                       | Controls the enforcement scope                          |
| NL-Trusted-Home-Lab                     | Demonstrates a trusted network signal                   |
| Microsoft Entra ID P2                   | Provides Conditional Access and risk-based capabilities |
| What If                                 | Simulates policy outcomes before enforcement            |
| Sign-in logs                            | Confirms real authentication and policy results         |

## Conditional Access Policies

| Policy                              | Security control                                                   | Project state   |
| ----------------------------------- | ------------------------------------------------------------------ | --------------- |
| CA001-Require-MFA-For-Pilot-Users   | Requires MFA authentication strength for the pilot group           | **On**          |
| CA002-Block-Legacy-Authentication   | Blocks legacy authentication clients                               | **Report-only** |
| CA003-Require-MFA-For-Admin-Portals | Requires MFA for privileged administrator access                   | **Report-only** |
| CA004-Require-MFA-For-Risky-SignIns | Requires MFA and reauthentication for medium or high-risk sign-ins | **Report-only** |

Only the pilot MFA policy was enabled after successful simulation. The remaining policies were intentionally left in Report-only mode pending longer monitoring and business-impact assessment.

## Safe Deployment Workflow

1. Created two cloud-only emergency administrator accounts.
2. Created a dedicated emergency access exclusion group.
3. Created a fictional pilot user and security group.
4. Assigned the required Microsoft Entra ID P2 licence.
5. Created a trusted named location without exposing its IP address.
6. Created four Conditional Access policies.
7. Placed all new policies in Report-only mode.
8. Simulated user, application, role, client and risk scenarios.
9. Identified and corrected a missing target-resource configuration.
10. Temporarily assigned an administrative role for role-based testing.
11. Removed the temporary role after validation.
12. Enabled only the pilot MFA policy.
13. Completed a real interactive MFA sign-in.
14. Verified the successful policy result in Microsoft Entra sign-in logs.

## Validation Results

| Test                                           | Expected outcome                   | Result                   |
| ---------------------------------------------- | ---------------------------------- | ------------------------ |
| Pilot user accesses Office 365                 | Require MFA                        | Passed                   |
| Pilot user uses a legacy client                | Block access                       | Passed                   |
| Privileged test user accesses Azure management | Require MFA                        | Passed                   |
| Pilot user has a high-risk sign-in             | Require MFA and reauthentication   | Passed after remediation |
| Emergency administrator is evaluated           | Policies excluded                  | Passed                   |
| Pilot user performs real sign-in               | MFA succeeds and access is granted | Passed                   |

## Key Troubleshooting Finding

The first high-risk sign-in simulation did not apply `CA004`.

Investigation showed:

```text
Included resources: No resources
```

The identity scope and risk conditions were correct, but no applications were protected. The target-resource scope was corrected to **All resources**, and the repeated simulation succeeded.

This demonstrated that an effective Conditional Access policy requires:

1. Identity scope
2. Resource scope
3. Conditions
4. Access or session controls

## Documentation

* [Project overview](documentation/01-project-overview.md)
* [Conditional Access policy design](documentation/02-conditional-access-policy-design.md)
* [Testing and validation](documentation/03-testing-and-validation.md)
* [Troubleshooting and lessons learned](documentation/04-troubleshooting-and-lessons-learned.md)

## Evidence

### Pilot MFA

* [Pilot-user MFA test conditions](screenshots/01a-pilot-user-mfa-test-conditions.jpeg)
* [Pilot-user MFA result](screenshots/01b-pilot-user-mfa-result.jpeg)

### Legacy Authentication

* [Legacy authentication test conditions](screenshots/02a-legacy-auth-test-conditions.jpeg)
* [Legacy authentication block result](screenshots/02b-legacy-auth-block-result.jpeg)

### Administrator Protection

* [Administrator portal test conditions](screenshots/03a-admin-portal-mfa-test-conditions.jpeg)
* [Administrator portal MFA result](screenshots/03b-admin-portal-mfa-result.jpeg)

### Risk-Based Protection

* [High-risk sign-in test conditions](screenshots/04a-high-risk-signin-test-conditions.jpeg)
* [High-risk sign-in MFA result](screenshots/04b-high-risk-signin-mfa-result.jpeg)

### Controlled Enforcement

* [Enforced MFA sign-in result](screenshots/05a-enforced-mfa-signin-result.jpeg)
* [MFA authentication details](screenshots/05b-mfa-authentication-details.jpeg)

### Emergency Access and Network Signals

* [Emergency access policy exclusion](screenshots/06-emergency-access-policy-exclusion.jpeg)
* [Emergency access group members](screenshots/07-emergency-access-group-members.jpeg)
* [Trusted named location](screenshots/08-trusted-named-location.jpeg)

## Skills Demonstrated

* Microsoft Entra Conditional Access
* Zero Trust access design
* Microsoft Entra multifactor authentication
* Microsoft Entra Identity Protection
* Risk-based Conditional Access
* Emergency access planning
* Legacy authentication blocking
* Named locations
* Authentication strengths
* Report-only policy deployment
* What If simulation
* Sign-in log investigation
* Policy troubleshooting
* Least-privilege testing
* Controlled security rollout
* Technical documentation
* Evidence sanitisation

## Production Recommendations

Before wider deployment, an organisation should:

* Monitor Report-only outcomes over an agreed period
* Identify legacy applications and service accounts
* Communicate MFA requirements to users
* Confirm emergency accounts are operational
* Use phishing-resistant authentication for privileged administrators
* Alert on emergency-account activity
* Deploy policies through staged pilot groups
* Review exclusions regularly
* Follow formal change-management and rollback procedures

## Security Notice

All identities used in this project are fictional.

This repository contains no passwords, authentication QR codes, access tokens, recovery codes, telephone numbers, public IP addresses or confidential tenant information. All evidence was reviewed and sanitised before publication.

