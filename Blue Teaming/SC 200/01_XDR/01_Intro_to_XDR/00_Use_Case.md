![Diagram the Detection of a Compromised endpoint.](https://learn.microsoft.com/en-us/training/wwl-sci/introduction-microsoft-365-threat-protection/media/compromised-endpoint.png)

## 🔒 Detection and Remediation

1. **Microsoft Defender for Endpoint (MDE)** identifies a threat on a device.
2. MDE notifies Intune that the risk level on this endpoint has changed. 
3. `Intune` evaluates it against **configured compliance policies** (e.g., “block if risk level = medium or above”).
4. When the **risk level exceeds the allowed threshold**, Intune updates the compliance status of the device in `Microsoft Entra ID`.
5. Then `Microsoft Entra ID` uses this risk status to:
	1. Blocking access to enterprise apps and services that support `continuous access evaluation` (CAE).
    2. Allowing limited, non-corporate internet use (e.g., public websites like YouTube, Wikipedia)
6. MDE remediates threat – either via automated remediation, security analyst approval of automated remediation, or analyst manual investigation of threat.
7. MDE also adds this attack information to `Microsoft Threat Intelligence` system to help other Microsoft MDE customers.
8. MDO, MDC also use this threat signals to detect and remediate threats in email, office collaboration, Azure, and more.
9. Once the threat has been remediated and cleaned up, MDE triggers Intune to update Microsoft Entra ID, and Conditional Access restores the user’s access to corporate resources.

>**Intune** acts as a **centralized control panel for managing endpoints**

---
## 🛡️ Integrated Solutions

| **Component**                         | Function                                                                                                                                                                             |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Defender for Office 365**           | Email and collaboration protection: phishing, malware, hunting, and investigation features.                                                                                          |
| **Defender for Endpoint**             | Endpoint threat protection: prevention, detection, automated response.                                                                                                               |
| **Defender XDR**                      | Correlates security signals across domains to build unified incident view.                                                                                                           |
| **Defender for Cloud Apps**           | Deep visibility and control over SaaS/PaaS apps with threat protection.                                                                                                              |
| **Defender for Identity**             | Uses AD signals to detect lateral movement, credential theft, and insider threats.                                                                                                   |
| **Defender Vulnerability Management** | Continuous asset visibility, risk-based vulnerability prioritization, and remediation tools.                                                                                         |
| **Defender for IoT**                  | Secures OT/IoT environments in sectors like manufacturing and utilities.                                                                                                             |
| **Microsoft Sentinel**                | Integrate XDR with Sentinel to stream all XDR incidents & advanced hunting events into Sentinel and keep the incidents and events synchronized between the Azure & Defender portals. |

## 🔗 Related Portals Accessible via "More Resources"

| **Portal**                        | **Purpose / Capability**                                                                            |
| --------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Microsoft Purview Portal**      | Compliance, governance, classification, and case management for Microsoft 365.                      |
| **Microsoft Entra ID**            | Identity and access management – MFA, branding, sign-ins.                                           |
| **Microsoft Entra ID Protection** | Identity risk detection and automated response for suspicious sign-in activity.                     |
| **Azure Information Protection**  | Data classification and protection for email/docs using labels, policies, and reports.              |
| **Microsoft Defender for Cloud**  | CSPM + CWPP for Azure, multi-cloud, and on-prem environments. Secures infrastructure and workloads. |

