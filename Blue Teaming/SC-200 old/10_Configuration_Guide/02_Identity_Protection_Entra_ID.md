
# Risk Policies
### 🔄 Example Workflow

1. **User signs in** from an unusual IP
2. **Sign-in risk** is calculated as “high”
3. **Conditional Access policy**  *blocks access or forces password reset/MFA*
4. **Admin reviews** the incident in Entra portal
5. User account is marked **“at risk”** until confirmed safe


![[Pasted image 20250703144318.png|400]]

### There are two Identity risk policies

- **Sign-in risk policy**: The sign-in risk policy *detects suspicious actions that come along with the sign-in*. It's focused on the sign-in activity itself and analyzes the probability that the sign-in was performed by some other than the user.
- **User risk policy**: The user risk policy detects the *probability that a user account has been compromised* by detecting risk events that are atypical of a user's behavior.

### Identity Protection Policy Actions

These policies automatically respond to **detected risk levels**

| **Policy Type**         | **Action**                               |
| ----------------------- | ---------------------------------------- |
| **User Risk Policy**    | Require password reset or block sign-in. |
| **Sign-in Risk Policy** | Require MFA or block sign-in.            |
>For users to be able to respond to MFA prompts, they *must first register for MFA*.
## User self-remediation

- Users must be registered for both *self-service password reset and multi-factor authentication*. 
- Microsoft recommends enabling the *combined security information registration experience*. 
- Allowing users to self-remediate gets them back to a productive state more quickly without requiring administrator intervention. 
- Administrators can still see these events and investigate them after the fact.

### Choosing acceptable risk levels

>Microsoft's recommendation is to set the user risk policy threshold to **High** and the sign-in risk policy to **Medium and higher**.

*Chosen risk level will trigger the policy.*

|**Threshold Level**|**Impact on Users**|**Security Effectiveness**|**Use Case**|
|---|---|---|---|
|**High**|Fewer challenges (minimal user interruptions)|May allow some threats to go unmitigated (excludes Low & Medium risks)|Prioritize user experience|
|**Medium**|Balanced approach|Covers Medium + High risks|Common default for most organizations|
|**Low**|Frequent user challenges|Maximum protection – covers all risk levels|High-security environments (e.g., finance, admin roles)


---
# MFA Registration Policy


1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com/) using a Global administrator account. Or [Identity Protection](https://entra.microsoft.com/#view/Microsoft_AAD_IAM/IdentityProtectionMenuBlade/~/Overview)
2. Open the portal menu and then select **Identity**.
3. On the Identity men, select **Protection**.
4. On the Security blade, in the left navigation, select **Identity protection**.
5. In the Identity protection blade, in the left navigation, select **Multifactor authentication registration policy**.
6. Under **Assignments**, select **All users** and review the available options. You can select from **All users** or **Select individuals and groups** if limiting your rollout. Additionally, you can choose to exclude users from the policy.
7. Under **Controls**, notice that the **Require Microsoft Entra ID multifactor authentication registration** is selected and cannot be changed.
8. Under **Enforce Policy**, select **Enabled** and then select **Save**.

---
# Conditional Access

## 🔐 Conditional Access Policy Actions

These policies are enforced **when specific conditions are met**, such as sign-in risk, device state, location, or app being accessed.

| **Action**                        | **Description**                                                                                          |
| --------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Require MFA**                   | Enforces multi-factor authentication before granting access.                                             |
| **Block access**                  | Completely denies access under specified conditions.                                                     |
| **Grant access**                  | Allows access under defined controls (e.g., MFA, device compliance).                                     |
| **Require compliant device**      | Only allows access if the device is Intune-compliant.                                                    |
| **Require Hybrid Azure AD Join**  | Enforces that device must be domain-joined and registered in Entra ID.                                   |
| **Require approved app**          | Only permits access via apps marked as trusted (like Outlook).                                           |
| **Require app protection policy** | Applies data protection rules on mobile devices (like preventing copy/paste from corporate to personal). |
| **Sign-in frequency control**     | Forces re-authentication after a time window (e.g., every 1 hour).                                       |
| **Session controls**              | Enforces Microsoft Defender for Cloud Apps policies during session (e.g., real-time monitoring).         |

---
# Exclusions

- All of the policies allow for excluding users such as your emergency access or break-glass administrator accounts.
- All exclusions should be reviewed regularly to see if they're still applicable.

>Configured trusted network locations are used by Identity Protection in some risk detections to reduce false positives.
