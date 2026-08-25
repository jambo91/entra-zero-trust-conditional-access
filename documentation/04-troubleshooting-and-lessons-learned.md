# Troubleshooting and Lessons Learned

## Overview

This project included several configuration and testing challenges that required investigation rather than assumption.

Documenting these issues demonstrates the troubleshooting process used to identify root causes, apply corrections and verify the final result.

## Issue 1 — Directory Roles Were Not Available in the Custom Policy Builder

### Symptom

While creating the administrator MFA policy, the user assignment interface displayed only:

* None
* All users
* Select users and groups

The expected **Directory roles** option was not available.

### Resolution

A Microsoft Conditional Access template was used:

```text
Require multifactor authentication for admins accessing Microsoft admin portals
```

The template automatically targeted the supported privileged built-in roles.

The policy was renamed to:

```text
CA003-Require-MFA-For-Admin-Portals
```

The emergency access exclusion group was then added, and the policy remained in Report-only mode.

### Lesson Learned

Portal experiences can differ between tenants and interface versions. Microsoft templates provide a consistent method for implementing recommended policy designs when manual options are unavailable.

## Issue 2 — Azure Management Resource Was Difficult to Locate

### Symptom

The What If target-resource picker did not return a result when searching for:

```text
Microsoft Azure Management
```

Searching for similar names returned unrelated enterprise applications.

### Resolution

The official Microsoft Azure Management application ID was used:

```text
797f4846-ba00-4fd7-ba43-dac1f8f63013
```

The search returned:

```text
Azure Resource Manager
```

This resource was selected for the administrator-policy simulation.

### Lesson Learned

Enterprise application display names can vary. A verified application ID provides a more reliable method of identifying the intended Microsoft resource.

## Issue 3 — Administrator Policy Did Not Initially Apply

### Symptom

`CA003` did not apply during the first What If simulation.

### Root Cause

The pilot user did not have an administrative directory role. Conditional Access evaluates the user’s existing directory-role assignments; the What If tool does not provide a separate role selector.

### Resolution

The User Administrator role was temporarily assigned to `CA Test User 01`.

After role propagation, the simulation was repeated and `CA003` applied successfully.

The temporary role was removed immediately after testing.

### Lesson Learned

Role-targeted Conditional Access policies evaluate real active role assignments. Temporary privilege used for testing must be controlled, time-limited and removed after validation.

## Issue 4 — Risk-Based Policy Did Not Apply

### Symptom

A high-risk sign-in simulation showed only `CA001`. The expected risk-based policy, `CA004`, was missing.

### Investigation

The policy details showed:

```text
Included resources: No resources
```

The identity scope, sign-in-risk condition and access controls were present, but no application or resource was protected.

### Resolution

The target-resource configuration was updated to:

```text
All resources
```

The same high-risk simulation was repeated.

### Final Result

`CA004` applied successfully with:

* Require multifactor authentication
* Sign-in frequency set to every time

### Lesson Learned

A Conditional Access policy requires all three main components:

1. Identity scope
2. Resource scope
3. Access control

A policy can appear correctly configured at first glance while still being ineffective because one component is missing.

## Issue 5 — Multiple Policies Applied to One Sign-in

### Observation

During the legacy authentication simulation:

* CA001 required MFA.
* CA002 blocked access.

### Explanation

Conditional Access evaluates all applicable policies. Controls are combined, and a block decision takes precedence over grant requirements such as MFA.

### Lesson Learned

Policies should be tested together as a complete set. Testing only one policy at a time can miss overlapping or conflicting results.

## Issue 6 — Screenshot Filename Extensions

### Symptom

A screenshot was initially saved with a double extension:

```text
filename.png.jpeg
```

A mismatch between the repository filename and the Markdown link would cause a GitHub 404 error.

### Resolution

A single screenshot format and exact filename convention were adopted:

```text
descriptive-name.jpeg
```

Files and Markdown links were checked for exact character and extension matches.

### Lesson Learned

GitHub paths are exact. Folder location, spelling, capitalisation and filename extensions must match the Markdown link precisely.

## Security Decisions

The following security decisions were made:

* Two emergency access accounts were created for redundancy.
* Emergency accounts were excluded through a dedicated security group.
* Emergency credentials were not included in screenshots or documentation.
* A trusted location was documented without revealing its IP address.
* New policies started in Report-only mode.
* Only the pilot-user MFA policy was enabled.
* Administrator privileges used for testing were removed afterward.
* Passwords, MFA QR codes, recovery data and authentication secrets were never recorded.
* Real sign-in logs were reviewed after controlled enforcement.

## Operational Recommendations

Before wider production deployment, an organisation should:

1. Monitor Report-only results over an agreed observation period.
2. Identify legacy applications and service accounts.
3. Communicate MFA changes to affected users.
4. Provide registration and recovery guidance.
5. Use a staged rollout with pilot groups.
6. Confirm emergency accounts are operational.
7. Use phishing-resistant authentication for privileged administrators.
8. Alert on emergency-account sign-ins.
9. Review exclusions regularly.
10. Document policy ownership and approval.
11. Test policy changes through a formal change-management process.
12. Review sign-in logs after each enforcement stage.

## Final Lessons

The most important lesson from this project is that successful Conditional Access implementation is not simply creating policies.

A professional deployment requires:

* Clear business requirements
* Emergency access preparation
* Careful scoping
* Staged deployment
* Simulation
* Real sign-in validation
* Monitoring
* Troubleshooting
* Accurate documentation
* Ongoing operational review
