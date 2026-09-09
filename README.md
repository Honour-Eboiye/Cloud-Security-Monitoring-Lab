# Hybrid Cloud Security & SIEM Monitoring Lab

## Executive Summary
The objective of this deployment was to establish a secure hybrid identity architecture and centralize server management within the cloud. This involved synchronizing an on-premise Microsoft Active Directory environment with Microsoft Entra ID (Azure AD) and onboarding a local Ubuntu Linux server into Azure using Azure Arc. The foundation for centralized security monitoring was also staged, pending the resolution of subscription billing statuses.

---

## 1. Active Directory to Azure Integration

### 1.1 Deployment and Configuration
To establish a hybrid identity environment, Microsoft Entra Connect Sync was deployed directly onto the on-premise Active Directory server. The configuration process involved:
* Executing the Entra Connect Sync installation package.
* Authenticating the secure connection between the on-premise environment and the Azure cloud tenant.
* Binding the designated local domain name to the cloud directory to ensure accurate identity routing.

> **[Image: Microsoft Entra Connect Sync – Welcome / Setup Wizard Screen]**

---

### 1.2 Troubleshooting and Remediation
During the initial deployment phase, several environmental and configuration challenges were encountered. These roadblocks were systematically isolated and resolved through active troubleshooting, utilizing external technical documentation and AI-assisted research to ensure the sync engine functioned securely and correctly.

> **[Image: Microsoft Entra Connect Sync – Configuration Error: "Microsoft Entra Connect could not configure application-based authentication for this server. Setup cannot continue." / Log trace C:\ProgramData\AADConnect\trace-...]**

---

### 1.3 Verification of Synchronization
Following the deployment, the synchronization pipeline was validated within the cloud environment:
* Navigated to the **Users** blade within the Microsoft Entra ID portal.
* Configured the directory view to display the **On-premises sync enabled** column.
* Confirmed that the relevant user objects successfully populated in the cloud with the synchronization attribute reflecting a **"Yes"** status, proving the on-premise server is actively communicating with Azure.

> **[Image: Microsoft Entra ID – Users blade displaying filtered user list with 'On-premises sync enabled' set to 'Yes']**

---

## 2. Azure Arc Ubuntu Onboarding

### 2.1 Script Generation and Deployment
To centralize the management of the Linux infrastructure, the local Ubuntu server was connected to the Azure cloud utilizing Azure Arc. The deployment was executed by generating a custom onboarding bash script within the Azure portal, which was subsequently downloaded and executed directly on the Ubuntu server's terminal.

---

### 2.2 Security and Access Control (Least Privilege)
Authentication for the Azure Arc connection was performed interactively using a dedicated user account. To enforce strict security standards and the principle of least privilege, the deployment account was deliberately restricted to:
* **Reader** access scoped strictly to the target Resource Group.
* The built-in **Azure Connected Machine Onboarding** role.

> **[Image: Azure Portal – Role Assignments displaying the 'Azure Connected Machine Onboarding' role]**

This Role-Based Access Control (RBAC) configuration ensures that in the event of a credential or system compromise during the onboarding phase, the threat actor's blast radius is strictly limited. They are restricted from escalating privileges or gaining unauthorized access to broader Azure resources.

> **[Image: Azure Arc | Machines dashboard displaying 'webserver.foxsecurity.com' with Status: 'Connected']**

---

## 3. Security Telemetry and SIEM Integration

### 3.1 Unified Security Operations Platform
To provide a single pane of glass for threat hunting and alert management, Microsoft Sentinel was initialized. The workspace was accessed via the unified Microsoft Defender portal, which combines Sentinel's SIEM capabilities with Defender's XDR features.

> **[Image: Microsoft Defender – Analytics rule wizard (Scheduled rule configuration for Brute Force Attack)]**

---

### 3.2 Data Ingestion and Administrative Roadblocks
Following the successful integration of the Ubuntu server, a Data Collection Rule (DCR) was configured to govern how the machine collects, transforms, and routes log data into the Sentinel workspace.
* **Current Status:** The final step required deploying the Syslog extension (Azure Monitor Linux Agent) to the server via Azure Arc to facilitate log forwarding. However, this deployment is currently paused as the associated Azure subscription was found to be in a disabled state.
* **Next Action:** Extension installation and log collection will resume immediately once the subscription status is administratively resolved and re-enabled.

> **[Image: Azure Portal – Log Analytics Workspace 'Workspace-Fox-Sec | Insights' showing active Data Collection Rule 'Fox-Sec-Interns-DCR']**

---

## 4. Conclusion and Next Steps
Both the Active Directory hybrid integration and the Linux server onboarding were successfully architected. The foundation for centralized identity management and infrastructure telemetry is firmly in place.
