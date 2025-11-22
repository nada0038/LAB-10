# Zero Trust Azure Landing Zone

# CloudMed Solutions Inc 

## 1. About CloudMed

CloudMed Solutions is a healthcare tech company that works with hospitals and clinics in Canada, the US, and Europe. They offer telemedicine and patient management services through their main platform, MedConnect. It lets doctors have secure virtual consultations, manage electronic medical records, and use AI to help with patient care.

Because CloudMed handles sensitive patient information, it has to follow HIPAA, GDPR, and PIPEDA. Their main Azure regions are Canada Central and West Europe. Since patient data is so sensitive, CloudMed wants a Zero Trust approach—checking every access, limiting permissions, and making sure problems can’t spread across the network.

---

## 2. Governance and Identity

### Management Groups

### How We Manage Things

- **Roles:** Admins get full access, DevOps only get what they need, Finance sees costs and budgets.  
- **Policies:** Tags are mandatory (like environment, owner, cost center), resources are limited to allowed regions, and naming rules are applied.  
- **Identity:** All users are in Azure Entra ID, MFA is required, devices must meet rules, and admin access is temporary through PIM.

---

## 3. Network Setup

CloudMed uses a hub-and-spoke design. The hub hosts shared services and security tools, while each workload is in its own spoke.

### Hub
- Azure Firewall  
- Azure Bastion  
- Private DNS Zones  
- Log Analytics Workspace  
- Shared services subnet  

### Spokes
- **App Spoke:** front-end web and mobile apps  
- **API Spoke:** backend services  
- **Data Spoke:** SQL databases and storage (private endpoints only)  

Traffic between spokes goes through the hub so everything can be checked. Workloads don’t have public IPs, and private endpoints keep data internal and secure.

---

## 4. Zero Trust Principles

- **Verify Explicitly:** MFA, conditional access, check identity/device/location, use private endpoints.  
- **Least Privilege:** Only give the access needed, use Just-in-Time admin access with PIM, separate networks by workload.  
- **Assume Breach:** No public inbound traffic, logs go to Log Analytics, Defender for Cloud monitors everything, firewall inspects traffic, data is encrypted everywhere.

**Some Examples**  
- Use Bastion to access VMs securely  
- Private Link for SQL databases  
- Policies to prevent public IPs  
- Firewall rules on all traffic  
- Conditional Access for admin accounts

---

## 5. Monitoring, Compliance, and Cost

- **Monitoring:** Log Analytics collects logs, Defender for Cloud watches for threats, and activity logs are centralized.  
- **Compliance:** Azure Policies enforce rules, dashboards show compliance, and alerts notify of any issues.  
- **Cost:** Budgets per environment, tagging required for tracking costs, Finance has view-only access.

---

## 6. Conceptual Architecture Diagram

```mermaid
class Bastion verify;
class AppSpoke verify;
class DNS least;
class APISpoke least;
class Firewall breach;
class Logs breach;
class DataSpoke breach;
graph TD
    %% Management Groups
    Root["Root Management Group"]
    CloudMed["CloudMed"]
    Platform["Platform"]
    Prod["Production"]
    Dev["Development"]

    %% Hub Components
    Hub["Hub Network"]
    Firewall["Azure Firewall<br>(Assume Breach)"]
    Bastion["Azure Bastion<br>(Verify Explicitly)"]
    DNS["Private DNS Zones<br>(Least Privilege)"]
    Logs["Log Analytics Workspace<br>(Assume Breach)"]

    %% Spokes
    AppSpoke["App Spoke<br>(Front-End Tier)<br>(Verify Explicitly)"]
    APISpoke["API Spoke<br>(Backend Tier)<br>(Least Privilege)"]
    DataSpoke["Data Spoke<br>(SQL & Storage)<br>(Assume Breach)"]

    %% Connections
    Root --> CloudMed
    CloudMed --> Platform
    CloudMed --> Prod
    CloudMed --> Dev

    CloudMed --> Hub
    Hub --> Firewall
    Hub --> Bastion
    Hub --> DNS
    Hub --> Logs

    Hub --> AppSpoke
    Hub --> APISpoke
    Hub --> DataSpoke

    %% Styles
    classDef verify fill:#d0f0fd,stroke:#0288d1,stroke-width:2px;
    classDef least fill:#d9f7be,stroke:#4caf50,stroke-width:2px;
    classDef breach fill:#ffe0b2,stroke:#fb8c00,stroke-width:2px;

    class Bastion verify;
    class AppSpoke verify;
    class DNS least;
    class APISpoke least;
    class Firewall breach;
    class Logs breach;
    class DataSpoke breach;



## Akash Nadackanal Vinod - 041156265

---------------------------------------------------------------------------------------
