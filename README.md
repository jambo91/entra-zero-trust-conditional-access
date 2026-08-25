# Microsoft Entra Zero Trust Conditional Access

## Project Status

🟡 Project in progress — environment preparation and security control implementation underway.

## Project Overview

This home-lab project demonstrates the design, implementation and testing of enterprise-style Microsoft Entra Conditional Access policies using Zero Trust principles.

The project uses fictional users and controlled test scenarios to demonstrate how an organisation can protect identities, applications and administrative access while reducing the risk of accidental account lockout.

## Business Scenario

A fictional organisation requires stronger identity security for its users and administrators.

The organisation wants to:

* Require multifactor authentication for administrators
* Protect access from untrusted locations
* Block legacy authentication
* Respond to risky users and risky sign-ins
* Use stronger authentication for sensitive access
* Protect emergency administrator accounts
* Test policies safely before enforcement
* Monitor policy results through sign-in logs

## Planned Security Controls

The project will include:

1. Emergency access account preparation
2. Named location configuration
3. MFA protection for administrative roles
4. Legacy authentication blocking
5. Location-based access controls
6. User-risk and sign-in-risk policies
7. Authentication strength requirements
8. Report-only policy deployment
9. What If testing
10. Sign-in log monitoring and troubleshooting

## Zero Trust Principles

This project follows three Zero Trust principles:

* Verify explicitly
* Use least-privilege access
* Assume breach

## Safe Deployment Approach

Conditional Access policies will first be configured in **Report-only** mode.

Each policy will be validated using:

* Microsoft Entra What If
* Controlled test accounts
* Sign-in logs
* Conditional Access policy results
* Emergency access account exclusions

Policies will only be enabled after successful testing.

## Environment

* Microsoft Entra ID
* Microsoft Entra ID Premium P2
* Microsoft Entra admin centre
* Fictional test identities
* Home-lab environment

## Documentation

Detailed implementation and testing documentation will be added as the project progresses.

## Security Notice

This repository will not contain passwords, access tokens, recovery codes or confidential tenant information. Screenshots will be reviewed and sanitised before publication.

