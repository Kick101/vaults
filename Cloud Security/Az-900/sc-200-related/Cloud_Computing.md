Cloud computing is the delivery of computing services over the internet. Computing services include common IT infrastructure such as virtual machines, storage, databases, and networking.

---
# Shared responsibility model

| **Responsibility Area**          | **Cloud Provider** | **Cloud Consumer**  | **Notes**                                                          |
| -------------------------------- | ------------------ | ------------------- | ------------------------------------------------------------------ |
| **Physical Security**            | ✅                  | ❌                   | Cloud provider manages datacenters, physical access, hardware.     |
| **Power and Cooling**            | ✅                  | ❌                   | Ensures infrastructure is powered and cooled.                      |
| **Network Connectivity**         | ✅                  | ❌                   | Provides networking between regions/zones and to the internet.     |
| **Data and Information**         | ❌                  | ✅                   | Consumer owns and secures all uploaded/stored data.                |
| **Access Management / Identity** | ❌                  | ✅                   | Consumer controls who can access the cloud resources and services. |

| **Component**             | **IaaS** (Infra as a Service) | **PaaS** (Platform as a Service) | **SaaS** (Software as a Service) | **Notes**                                                           |
| ------------------------- | ----------------------------- | -------------------------------- | -------------------------------- | ------------------------------------------------------------------- |
| **Physical Security**     | ✅ Cloud Provider              | ✅ Cloud Provider                 | ✅ Cloud Provider                 | Datacenter, power, network, and physical access                     |
| **Virtualization**        | ✅ Cloud Provider              | ✅ Cloud Provider                 | ✅ Cloud Provider                 | Hypervisor layer managed by provider                                |
| **OS (Operating System)** | ✅ Consumer                    | ✅ Cloud Provider                 | ✅ Cloud Provider                 | IaaS requires consumer to install/maintain OS                       |
| **Middleware / Runtime**  | ✅ Consumer                    | ✅ Cloud Provider                 | ✅ Cloud Provider                 | PaaS provides runtime for apps (e.g., Node.js, .NET Core)           |
| **Applications**          | ✅ Consumer                    | ✅ Consumer                       | ✅ Cloud Provider                 | In SaaS, the app (e.g., Outlook, Salesforce) is fully managed       |
| **Data**                  | ✅ Consumer                    | ✅ Consumer                       | ✅ Consumer                       | Consumer is always responsible for securing and managing their data |
| **Access & Identity**     | ✅ Consumer                    | ✅ Consumer                       | ✅ Consumer                       | User roles, MFA, RBAC, etc., are managed by consumer                |

> ✅ indicates **who is primarily responsible** for that layer.

---

### Summary:

- **IaaS** = Most control, most responsibility for consumer (e.g., Azure VMs).
- **PaaS** = Balanced responsibility (e.g., Azure App Service).
- **SaaS** = Least control, least responsibility for consumer (e.g., Microsoft 365).

![Diagram showing the responsibilities of the shared responsibility model.](https://learn.microsoft.com/en-us/training/wwl-azure/describe-cloud-compute/media/shared-responsibility-b3829bfe.svg)


---

# Cloud Models

| **Public cloud**                                                      | **Private cloud**                                                  | **Hybrid cloud**                                                  |
| --------------------------------------------------------------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------- |
| No capital expenditures to scale up                                   | Organizations have complete control over resources and security    | Provides the most flexibility                                     |
| Applications can be quickly provisioned and deprovisioned             | Data is not collocated with other organizations’ data              | Organizations determine where to run their applications           |
| Organizations pay only for what they use                              | Hardware must be purchased for startup and maintenance             | Organizations control security, compliance, or legal requirements |
| Organizations don’t have complete control over resources and security | Organizations are responsible for hardware maintenance and updates |                                                                   |
## Multi-cloud

A **multi-cloud** scenario involves using services from **multiple public cloud providers**. This could be to leverage different features, migrate between providers, or reduce dependency on one vendor. It requires managing **resources and security** across all involved platforms.

## Azure Arc

A **multi-cloud** setup uses **multiple public cloud providers** to take advantage of different features, support migration, or avoid vendor lock-in. It involves managing **security and resources** across all providers.

---
# Consumption-based model

When comparing IT infrastructure models, there are two types of expenses to consider. Capital expenditure (CapEx) and operational expenditure (OpEx).

|**Category**|**Capital Expenditure (CapEx)**|**Operational Expenditure (OpEx)**|
|---|---|---|
|**Definition**|Upfront investment in physical infrastructure|Ongoing payments for services/resources|
|**Examples**|Building datacenter, buying hardware, company vehicles|Cloud subscriptions, leasing software or equipment|
|**Cloud Usage**|Not applicable|Cloud follows an OpEx, consumption-based model|
|**Payment Model**|One-time payment|Pay-as-you-go / pay-for-what-you-use|
|**Flexibility**|Rigid – requires forecasting future needs|Flexible – scale up/down based on demand|
|**Waste Risk**|High if over/under provisioning|Low – you only pay for what you actually use|
|**Management Overhead**|High – you manage hardware, cooling, power, etc.|Low – cloud provider manages infrastructure|
## ⚙️ Cloud Pricing Model Summary

- **Pay-as-you-go**: Only pay for resources when used.
- **No upfront cost**: Avoid large initial investments.
- **Scalable**: Add or remove resources as needed.
- **Efficient**: Aligns cost with usage, avoids over/under provisioning.
- **Benefits**:
    - Predictable OpEx
    - Fast time to deploy
    - Offload hardware management
    - Focus on business logic, not infrastructure

> 🧠 **Analogy**: Cloud computing is like renting a hotel room—you use it when needed, pay for the nights you stay, and leave without maintenance worries. **CapEx** is like buying a house—you pay upfront, but you're stuck with it.


