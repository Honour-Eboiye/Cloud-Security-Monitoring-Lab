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
```mermaid
flowchart TD
    A[Azure Cloud Environment] --> B[Identity]
    A --> C[Network]
    A --> D[Governance]

    B --> B1["Entra ID<br/>RBAC / MFA"]
    C --> C1["NSG<br/>Access Rules"]
    D --> D1["Azure Policy<br/>Enforcement"]

    B1 --> E[Cloud Resources]
    C1 --> E
    D1 --> E

    E --> F[Virtual Machine]
    E --> G[Storage Account]

    F --> H[Security Monitoring]
    G --> H
    H --> H1["KQL Queries"]
    H --> H2["Microsoft Defender for Cloud"]

```


------------------------------------------------------------------------

## Security Controls

### 1. Identity & Access Management

**Objective:** Apply least privilege and stronger authentication.

**Implemented:** - Microsoft Entra ID - Azure RBAC - Least-privilege
permissions - MFA

A test user was given restricted storage access and read-only VM access,
and MFA was validated.



![Image showing RBAC role assignment / restricted permissions](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/identity/Screenshot%202026-08-03%20232425.png)
*RBAC role assignment / restricted permissions.*


![An Image Showing MFA validation without exposing personal information.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/identity/Screenshot%202026-07-24%20132541.png)
*MFA validation*


------------------------------------------------------------------------

### 2. Network Security

**Objective:** Reduce unnecessary network exposure to the VM.

An Azure Network Security Group was configured to restrict inbound
access to a trusted source. An unauthorized connection attempt was
blocked.


``` mermaid
flowchart LR
    Internet((Internet)) --> NSG[Network Security Group]
    NSG -- Allowed Rule --> VM[Virtual Machine]
    NSG -- Denied Rule --> Block[Traffic Blocked]
    VM --> Subnet[Private Subnet]
    Block --> NSGLog[NSG Flow Logs]
    NSG --> NSGLog

```



![AN Image Showing the relevant NSG inbound rule.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/network/Deliverable%204.png)
*Relevant NSG inbound rule.*



![An Image Showing Unauthorized SSH connection was blocked.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/network/Deliverable%205.png)
*Unauthorized connection was blocked.*


------------------------------------------------------------------------

### 3. Storage Security & Data Protection

**Objective:** Reduce the risk of unintended public access to cloud
data.

**Implemented:** - Public access restrictions - Encryption at rest -
Time-bound SAS access - Granular permissions

![A SAS with public access restrictions and encryption settings.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/cee972c4f74ca79d7692ddbae063abd9b78c3dcf/storage/SAS%20GENERATION.png)
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

``` mermaid
flowchart LR
    Internet((Internet)) --> NSG[Network Security Group]
    NSG -- Allowed Rule --> VM[Virtual Machine]
    NSG -- Denied Rule --> Block[Traffic Blocked]
    VM --> Subnet[Private Subnet]
    Block --> NSGLog[NSG Flow Logs]
    NSG --> NSGLog

```


![Image showing blocked deployment result as a result of not adhering to organisation's policy.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/ddcafad123c7f9d9a9e5b4372c4592a6c0191294/governance/Screenshot%202026-07-30%20153821.png)

*Blocked deployment result as a result of not adhering to organisation's policy.*



------------------------------------------------------------------------

### 5. Security Monitoring & Investigation

**Objective:** Use security telemetry to investigate authentication
activity.

The project included: - Security event monitoring - Failed
authentication analysis - KQL-based investigation - Microsoft Defender
for Cloud recommendations

``` mermaid
flowchart LR
    Sources["Sign-in Logs<br/>NSG Flow Logs<br/>Activity Logs"] --> LA[Log Analytics Workspace]
    LA --> KQL[KQL Queries]
    LA --> Defender[Microsoft Defender for Cloud]
    KQL --> Investigate[Auth Log Investigation]
    Defender --> Recs[Security Recommendations]
    Investigate --> Report[Findings / Evidence]
    Recs --> Report

```


### The KQL query.
<br>
![Image of Failed-authentication events.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/cee972c4f74ca79d7692ddbae063abd9b78c3dcf/monitoring/KQL%20Query.png)

![Image of Failed-authentication events.](https://github.com/Honour-Eboiye/Cloud-Security-Monitoring-Lab/blob/cee972c4f74ca79d7692ddbae063abd9b78c3dcf/network/SSH%20RULE%20IMPLEMENTATED.png)

*Failed-authentication events.*



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

``` mermaid
flowchart LR
    Sources["Sign-in Logs<br/>NSG Flow Logs<br/>Activity Logs"] --> LA[Log Analytics Workspace]
    LA --> KQL[KQL Queries]
    LA --> Defender[Microsoft Defender for Cloud]
    KQL --> Investigate[Auth Log Investigation]
    Defender --> Recs[Security Recommendations]
    Investigate --> Report[Findings / Evidence]
    Recs --> Report

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

------------------------------------------------------------------------


## AI-Assisted Learning

ChatGPT was used as a supporting learning and troubleshooting aid during
the project to help explain unfamiliar concepts, reason through
implementation approaches, and troubleshoot configuration issues.

Implementation decisions and security claims were validated through
hands-on configuration, testing, documentation, and project evidence.
