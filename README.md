# Cloud Security Monitoring Lab

> A hands-on Azure lab demonstrating cloud security controls,
> governance, security monitoring, and investigation.

## Executive Summary

This project demonstrates how I applied practical **Cloud Security +
Security Operations** concepts in an Azure environment.

The lab focused on: - Identity and access management - Network
security - Storage security - Governance and policy enforcement -
Security monitoring - Authentication log investigation with KQL -
Microsoft Defender for Cloud recommendations

The goal was not only to configure controls, but to **validate their
security effect with hands-on tests and evidence**.

> **Note:** Azure subscription availability limited some later
> end-to-end monitoring work. Incomplete components are identified as
> future improvements rather than presented as completed.

------------------------------------------------------------------------

## Architecture

``` text
                    AZURE CLOUD ENVIRONMENT
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
       Identity        Network          Governance
       Entra ID           NSG           Azure Policy
       RBAC / MFA     Access Rules       Enforcement
          │                │                │
          └────────────────┼────────────────┘
                           ▼
                    Cloud Resources
                    ┌──────┴──────┐
                    ▼             ▼
                   VM          Storage
                    │             │
                    └──────┬──────┘
                           ▼
                 Security Monitoring
                     KQL / Defender
```

### Architecture Evidence

📸 **Screenshot slot --- `evidence/01-architecture.png`**

> Show the actual Azure architecture/environment if available. Do not
> use a generic diagram as evidence of implementation.

------------------------------------------------------------------------

## Security Controls

### 1. Identity & Access Management

**Objective:** Apply least privilege and stronger authentication.

**Implemented:** - Microsoft Entra ID - Azure RBAC - Least-privilege
permissions - MFA

A test user was given restricted storage access and read-only VM access,
and MFA was validated.
*RBAC role assignment / restricted permissions.*
![Image showing RBAC role assignment / restricted permissions](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/identity/Screenshot%202026-08-03%20232425.png)

*MFA validation*
![An Image Showing MFA validation without exposing personal information.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/identity/Screenshot%202026-07-24%20132541.png)


------------------------------------------------------------------------

### 2. Network Security

**Objective:** Reduce unnecessary network exposure to the VM.

An Azure Network Security Group was configured to restrict inbound
access to a trusted source. An unauthorized connection attempt was
blocked.

``` text
Trusted Source ──────► ALLOWED ──────► VM
Untrusted Source ─────► BLOCKED
```

*Relevant NSG inbound rule.*
![AN Image Showing the relevant NSG inbound rule.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/network/Deliverable%204.png)


*Unauthorized connection was blocked.*
![An Image Showing Unauthorized SSH connection was blocked.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/network/Deliverable%205.png)


------------------------------------------------------------------------

### 3. Storage Security & Data Protection

**Objective:** Reduce the risk of unintended public access to cloud
data.

**Implemented:** - Public access restrictions - Encryption at rest -
Time-bound SAS access - Granular permissions

![Public access restrictions and encryption settings.]()
*Public access restrictions and encryption settings.*

> **Warning:** Never publish SAS tokens, access keys, passwords,
> or other secrets. Redact them from screenshots before uploading to
> GitHub (as the SAS token in the image above is no longer active).

------------------------------------------------------------------------

### 4. Governance & Policy Enforcement

**Objective:** Prevent defined non-compliant configurations from being
deployed.

Azure Policy was used as a governance control. A controlled policy
violation was tested and the deployment was blocked.

``` text
Deployment
    ↓
Azure Policy
    ↓
Policy Check
  ↙       ↘
Pass      Fail
 ↓          ↓
Allow      Block
```

*Blocked deployment result as a result of not adhering to organisation's policy.*
![Image showing the policy assignment was blocked blocked deployment result.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/governance/Screenshot%202026-07-30%20153821.png)

------------------------------------------------------------------------

### 5. Security Monitoring & Investigation

**Objective:** Use security telemetry to investigate authentication
activity.

The project included: - Security event monitoring - Failed
authentication analysis - KQL-based investigation - Microsoft Defender
for Cloud recommendations

``` text
Security Event
      ↓
Log / Telemetry
      ↓
KQL Query
      ↓
Authentication Events
      ↓
Investigation
      ↓
Risk Assessment
```

*The KQL query.*
![Image of the KQL query that was used]()

*Failed-authentication events.*
![Image of Failed-authentication events.]()

![Image od Relevant Defender for Cloud recommendations.]()
*Relevant Defender for Cloud recommendations.*

------------------------------------------------------------------------

## Detection Use Case

### Failed Authentication Investigation

**Scenario:** Failed authentication events occur against a cloud
workload.

Investigation questions: - Which account was targeted? - What source
generated the events? - When did the activity occur? - How many failures
occurred? - Was there a successful login afterward? - Is the activity
consistent with normal behavior? - Could it indicate brute-force or
credential abuse?

**Telemetry:** SecurityEvent, Syslog, authentication events

**Investigation tool:** KQL

------------------------------------------------------------------------

## Security Findings & Remediation

  -----------------------------------------------------------------------
  Finding                 Security Impact         Remediation
  ----------------------- ----------------------- -----------------------
  Unnecessary public      Increased attack        Restricted access
  exposure                surface                 

  Storage exposure        Potential unauthorized  Disabled public access
                          data access             and strengthened
                                                  storage controls

  Broad permissions       Greater impact from     Applied RBAC / least
                          compromised accounts    privilege

  Unrestricted inbound    Increased attack        Applied NSG
  access                  surface                 restrictions

  Non-compliant           Security policy         Applied Azure Policy
  configuration           violation               enforcement

  Failed authentication   Potential credential    Investigated
  activity                abuse                   authentication events
                                                  with KQL
  -----------------------------------------------------------------------



------------------------------------------------------------------------

## Security Workflow

``` text
DESIGN → SECURE → GOVERN → MONITOR → DETECT → INVESTIGATE → REMEDIATE → IMPROVE
```

The project helped me connect cloud infrastructure security with
security operations rather than treating them as separate areas.

------------------------------------------------------------------------

## Limitations

Azure subscription availability affected the later stages of the planned
monitoring pipeline.

Therefore, this project distinguishes between: - **Controls implemented
and validated** - **Monitoring/investigation activities demonstrated** -
**Future components**

The project does not claim incomplete end-to-end components as fully
operational.

------------------------------------------------------------------------

## Future Improvements

-   Expand KQL detection rules
-   Add alert-driven incident workflows
-   Complete end-to-end Linux/security telemetry ingestion
-   Add automated response with appropriate safeguards
-   Add additional cloud attack scenarios
-   Integrate incident/ticket management
-   Expand service-principal and workload-identity monitoring

------------------------------------------------------------------------

## Technologies

**Cloud:** Microsoft Azure

**Identity:** Microsoft Entra ID, Azure RBAC, MFA

**Network Security:** Azure NSG, TCP/IP

**Data Protection:** Azure Storage, encryption at rest, SAS

**Governance:** Azure Policy

**Monitoring & Detection:** KQL, SecurityEvent, Syslog, Microsoft
Defender for Cloud

**Security Concepts:** Least Privilege, Defense in Depth, Threat
Detection, Security Monitoring, Incident Investigation

------------------------------------------------------------------------

## Evidence Structure

``` text
evidence/
├── identity/
│   ├── rbac.png
│   └── mfa.png
│
├── network/
│   ├── nsg-rules.png
│   └── blocked-connection.png
│
├── storage/
│   └── storage-hardening.png
│
├── governance/
│   └── policy-block.png
│
└── monitoring/
    ├── kql-query.png
    ├── query-results.png
    └── defender-recommendations.png
```

Only include screenshots that **actually prove the claim being made**.
If a screenshot is not available or does not clearly demonstrate an
implemented control, remove that slot rather than adding a generic
image.

------------------------------------------------------------------------

## AI-Assisted Learning

ChatGPT was used as a supporting learning and troubleshooting aid during
the project to help explain unfamiliar concepts, reason through
implementation approaches, and troubleshoot configuration issues.

Implementation decisions and security claims were validated through
hands-on configuration, testing, documentation, and project evidence.
