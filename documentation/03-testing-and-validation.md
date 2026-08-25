# Testing and Validation

## Testing Approach

Conditional Access policies were validated using two methods:

1. Conditional Access What If simulations
2. A controlled real sign-in using the pilot account

The What If tool was used before enforcement to confirm identity scope, resource scope, client conditions, risk conditions, grant controls and exclusions.

## Test Summary

| Test | Scenario                                              | Expected result                      | Actual result                         |
| ---- | ----------------------------------------------------- | ------------------------------------ | ------------------------------------- |
| 1    | Pilot user accesses Office 365                        | Require MFA                          | Passed                                |
| 2    | Pilot user uses a legacy authentication client        | Block access                         | Passed                                |
| 3    | Temporarily privileged user accesses Azure management | Require MFA                          | Passed                                |
| 4    | Pilot user has a high-risk sign-in                    | Require MFA and reauthentication     | Passed after configuration correction |
| 5    | Emergency administrator accesses Azure management     | Conditional Access policies excluded | Passed                                |
| 6    | Pilot user performs a real interactive sign-in        | MFA required and sign-in succeeds    | Passed                                |

## Test 1 — Pilot User MFA

### Scenario

`CA Test User 01` was evaluated while accessing Office 365 from Windows using a browser.

### Expected Result

```text
CA001-Require-MFA-For-Pilot-Users
```

The policy should apply and require multifactor authentication.

### Result

The What If evaluation showed `CA001` under **Policies that will apply**.

### Evidence

* [Pilot MFA test conditions](../screenshots/01a-pilot-user-mfa-test-conditions.jpeg)
* [Pilot MFA policy result](../screenshots/01b-pilot-user-mfa-result.jpeg)

## Test 2 — Legacy Authentication

### Scenario

The pilot user was evaluated against Office 365 Exchange Online using the **Other clients** legacy-client option.

### Expected Result

```text
CA002-Block-Legacy-Authentication
```

The policy should apply with the **Block access** grant control.

### Result

The evaluation showed:

* CA001 requiring MFA
* CA002 blocking access

This confirmed that both applicable policies were evaluated and that the blocking policy would take precedence.

### Evidence

* [Legacy authentication test conditions](../screenshots/02a-legacy-auth-test-conditions.jpeg)
* [Legacy authentication block result](../screenshots/02b-legacy-auth-block-result.jpeg)

## Test 3 — Administrator Portal Protection

### Scenario

`CA Test User 01` was temporarily assigned the User Administrator role.

The What If simulation evaluated access to Azure Resource Manager using the official Microsoft Azure Management application identifier.

### Expected Result

```text
CA003-Require-MFA-For-Admin-Portals
```

The policy should apply and require multifactor authentication.

### Result

The policy appeared under **Policies that will apply**.

The temporary User Administrator role was removed immediately after the evidence was captured.

### Evidence

* [Administrator portal test conditions](../screenshots/03a-admin-portal-mfa-test-conditions.jpeg)
* [Administrator portal MFA result](../screenshots/03b-admin-portal-mfa-result.jpeg)

## Test 4 — High-Risk Sign-in

### Scenario

The pilot user was evaluated with:

```text
Sign-in risk: High
```

### Initial Result

`CA004` did not apply.

Policy inspection revealed that no target resources were configured.

### Remediation

The policy was updated to include:

```text
All resources
```

### Final Result

The repeated What If simulation showed:

* CA001 requiring MFA
* CA004 requiring MFA
* CA004 requiring a new sign-in every time

### Evidence

* [High-risk sign-in test conditions](../screenshots/04a-high-risk-signin-test-conditions.jpeg)
* [High-risk sign-in MFA result](../screenshots/04b-high-risk-signin-mfa-result.jpeg)

## Test 5 — Emergency Access Exclusion

### Scenario

An emergency Global Administrator account was evaluated against Azure Resource Manager.

### Expected Result

The account should be excluded from the Conditional Access policies to preserve emergency tenant access.

### Result

All four project policies appeared under **Policies that will not apply** for the emergency identity.

### Evidence

* [Emergency access policy exclusion](../screenshots/06-emergency-access-policy-exclusion.jpeg)
* [Emergency access group members](../screenshots/07-emergency-access-group-members.jpeg)

## Test 6 — Controlled Enforcement

### Scenario

After successful What If validation, `CA001` was changed from Report-only to On.

The pilot user signed in interactively and completed multifactor authentication.

### Expected Result

* MFA challenge required
* Access granted after successful MFA
* Conditional Access result recorded as Success

### Result

The user completed sign-in successfully. Microsoft Entra sign-in logs showed `CA001` with a successful result.

### Evidence

* [Enforced MFA sign-in result](../screenshots/05a-enforced-mfa-signin-result.jpeg)
* [MFA authentication details](../screenshots/05b-mfa-authentication-details.jpeg)

## Supporting Configuration Evidence

* [Trusted named location](../screenshots/08-trusted-named-location.jpeg)

## Final Validation Status

| Control                              | Status    |
| ------------------------------------ | --------- |
| Emergency access exclusion           | Validated |
| Pilot MFA simulation                 | Validated |
| Legacy authentication blocking       | Validated |
| Administrator portal MFA             | Validated |
| Risk-based MFA                       | Validated |
| Real enforced MFA sign-in            | Validated |
| Temporary administrator role removal | Completed |
| Sensitive evidence review            | Completed |

## Conclusion

The project demonstrated that Conditional Access policies must be tested as a complete policy set rather than individually assumed to work.

The combination of Report-only mode, What If simulations, emergency exclusions, pilot deployment and sign-in log review allowed the controls to be validated without affecting normal users or causing administrator lockout.
