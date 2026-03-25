### 🎯 Core Capabilities

1. **Automated Risk Detection & Remediation**
    - Detects suspicious sign-ins and user behaviors using real-time threat intelligence
    - Applies automated policies to block, require MFA, or force password reset
2. **Investigation Tools**
    - Interactive risk reports via the **Microsoft Entra portal**
    - Allows manual investigation of high-risk users or events
3. **Data Export**
    - Exports risk events to third-party tools or SIEM (e.g., Microsoft Sentinel)
    - Enables deeper correlation and custom alerting

### 📊 Intelligence Backing

- Leverages **6.5+ trillion signals/day** across:
    - **Microsoft Entra ID (Azure AD)** (enterprise)
    - **Microsoft Account (MSA)** (consumer)
    - **Xbox network** (gaming)
- Signals include:
    - Atypical travel
    - Anonymous IP addresses
    - Leaked credentials
    - Malware-linked IPs

### 🛡 Conditional Access Integration

Identity Protection risks can **trigger Conditional Access policies**, such as:

- Block access for high-risk users
- Require MFA for medium-risk sign-ins
- Force password reset

>Identity = Account + Attributes + Authentication Capability

---
# Risk detection and remediation

Identity Protection identifies risks in the following classifications:

|**Risk detection type**|**Description**|
|---|---|
|Anonymous IP address|Sign in from an anonymous IP address (for example: Tor browser, anonymizer VPNs).|
|Atypical travel|Sign in from an atypical location based on the user's recent sign ins.|
|Malware-linked IP address|Sign in from a malware-linked IP address.|
|Unfamiliar sign in properties|Sign in with properties we've not seen recently for the given user.|
|Leaked credentials|Indicates that the user's valid credentials have been leaked.|
|Password spray|Indicates that multiple usernames are being attacked using common passwords in a unified brute-force manner.|
|Microsoft Entra threat intelligence|Microsoft's internal and external threat intelligence sources have identified a known attack pattern.|
|New country|This detection is discovered by Microsoft Defender for Cloud Apps (MDCA).|
|Activity from anonymous IP address|This detection is discovered by MDCA.|
|Suspicious inbox forwarding|This detection is discovered by MDCA.|

## Permissions

**Identity Protection** requires users be a *Security Reader, Security Operator, Security Administrator, Global Reader Administrator* in order to access.

| **Role**               | **Can do**                                                                                                            | *Can't do*                                                                                             |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Security Administrator | Full access to Identity Protection                                                                                    | Reset password for a user                                                                              |
| Security Operator      | View all Identity Protection reports and Overview screen, Dismiss user risk, confirm safe sign-in, confirm compromise | Configure or change policies, Reset password for a user, Configure alerts                              |
| Security Reader        | View all Identity Protection reports and Overview screen                                                              | Configure or change policies, Reset password for a user, Configure alerts, Give feedback on detections |

>Currently, the Security Operator role cannot access the *Risky sign-ins* report. Conditional Access Administrators can also create policies that factor in sign-in risk as a condition.

---
# Risk Reports: Investigate & Remediate

## Investigate risk

Identity Protection provides three reports to **investigate** identity risks:
- **risky users**
- **risky sign-ins**
- **risk detections**

>All three reports allow for downloading of events in `.CSV`. The **risky users & risky sign-ins** reports allow for downloading the most recent 2,500 entries, while the **risk detections** report allows for downloading the most recent 5,000 records.

We can find the three reports in the: *Microsoft Entra admin center -> Identity -> Protection - Identity Protection*

### Navigating the reports

![[Pasted image 20250703201501.png]]

### Risky users

**It reports:**

- Which users are at risk, have had risk remediated, or have had risk dismissed?
- Details about detections.
- History of all risky sign-ins.
- Risk history.

**Action on these events. Administrators can choose to:**

- Reset the user password.
- Confirm user compromise.
- Dismiss user risk.
- Block user from signing in.
- Investigate further using Azure ATP.

### Risky sign-ins

The risky sign-ins report contains filterable data for up to the past 30 days.

**It reports:**

- Which sign-ins are classified as at risk, confirmed compromised, confirmed safe, dismissed, or remediated.
- Real-time and aggregate risk levels associated with sign-in attempts.
- Detection types triggered.
- Conditional Access policies applied.
- MFA details.
- Device information.
- Application information.
- Location information.

**Action on these events. Administrators can choose to:**

- Confirm sign-in compromise.
- Confirm sign-in safe.

### Risk detections

The risk detections report contains filterable data for up to the *past 90 days*.

**It reports:**

- Information about each risk detection including type.
- Other risks triggered at the same time.
- Sign-in attempt location.

Administrators can then choose to return to the user's risk or sign-ins report to take actions based on information gathered.

The risk detection report also *provides a clickable link to the detection* in the Microsoft Defender for Cloud Apps (MDCA) portal where you can view additional logs and alerts.

---
## Remediation

All active risk detections contribute to the calculation of a value called _user risk level_ (low, medium, high). 

>Some risk detections are marked by Identity Protection as "Closed (system)" because the events were no longer determined to be risky.

**Administrators have the following options to remediate:**

- Self-remediation with risk policy.
- Manual password reset.
- Dismiss user risk.
- Close individual risk detections manually.

### Self-remediation with risk policy

If you allow users to self-remediate, with *MFA* & *SSPR* (self-service password reset) in your risk policies. 

- Users must have previously registered for MFA and SSPR in order to use when risk is detected.
- Some detections don't raise risk to the level where a user self-remediation would be required, but administrators should still evaluate these detections. 
- Administrators determine that additional measures are necessary, such as *blocking access from locations* or lowering the acceptable risk in their policies.

### Manual password reset

If requiring a password reset using a user risk policy isn't an option, administrators can close all risk detections for a user with a manual password reset.

##### Resetting a password options

**Generate a temporary password** - By generating a temporary password, you can immediately bring an identity back into a safe state. This method requires contacting the affected users since they need to know what the temporary password is. The user is prompted to change the password to something new during the next sign-in.

**Require the user to reset password** - Requiring the users to reset passwords enables self-recovery without contacting help desk or an administrator. This method only applies to users who are registered for MFA and SSPR.

### Dismiss user risk

If a password reset isn't an option for you because, for example, the user has been deleted, you can choose to dismiss user risk detections.

When you select **Dismiss user risk**, all events are closed and the affected user is no longer at risk. 

>However, because this method doesn't affect the existing password, it doesn't bring the related identity back into a safe state.

### Close individual risk detections manually

By closing individual risk detections manually, you can lower the user risk level. Typically, risk detections are closed manually in response to a related investigation, such as when talking to a user reveals that an active risk detection isn't required anymore.

**You can choose to take any of the following actions:**

- Confirm user compromised.
- Dismiss user risk.
- Confirm sign-in safe.
- Confirm sign-in compromised.

---
## Unblocking based on user risk policy

- **Reset password** - You can reset the user's password.
- **Dismiss user risk** - The user risk policy blocks a user if the configured user risk level for blocking access has been reached. You can reduce a user's risk level by dismissing user risk or manually closing reported risk detections.
- **Exclude the user from policy** - If you think that the current configuration of your sign-in policy is causing issues for specific users, you can exclude the users from it.
- **Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable the policy.

## Unblocking based on sign-in risk policy

- **Sign in from a familiar location or device** - A common reason for blocked suspicious sign-ins are sign-in attempts from unfamiliar locations or devices. Your users can quickly determine whether this reason is the blocking reason by trying to sign in from a familiar location or device.
- **Exclude the user from policy** - If you think that the current configuration of your sign-in policy is causing issues for specific users, you can exclude the users from it.
- **Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable the policy.

---
## PowerShell preview

Using the Microsoft *Graph PowerShell SDK Preview* module, organizations can manage risk using PowerShell. The preview modules and sample code are located in the [Azure GitHub repo](https://github.com/AzureAD/IdentityProtectionTools).

---
## Risk Investigation: Graph API

### Connect to Microsoft Graph

There are *four steps* to accessing Identity Protection data: 
- retrieve your domain name
- create a new app registration
- configure API permissions
- configure a valid credential.

### 1. Retrieve your domain name

1. Sign in to the Microsoft Entra admin center.
2. Browse to **Identity**, then open Settings, and select **Domain names**.
3. Take note of the `.onmicrosoft.com` domain. You'll need this information in a later step.

### 2. Create a new app registration

1. In the Microsoft Entra admin center, browse to **Identity and Applications**, then **App registrations**.
2. Select **New registration**.
3. On the **Create** page, perform the following steps:
    1. In the **Name** textbox, type a name for your application (for example: Microsoft Entra Risk Detection API).
    2. Under **Supported account types**, select the type of accounts that will use the APIs.
    3. Select **Register**.
4. Copy the **Application ID**.

### 3. Configure API permissions

1. From the **Application** you created, select **API permissions**.
2. On the **Configured permissions** page, in the toolbar on the top, select **Add a permission**.
3. On the **Add API access** page, choose **Select an API**.
4. On the **Select an API** page, select **Microsoft Graph**, and then select **Select**.
5. On the **Request API permissions** page:
    1. Select **Application permissions**.
    2. Select the checkboxes next to IdentityRiskEvent.Read.All and IdentityRiskyUser.Read.All.
    3. Select **Add permissions**.
6. Select **Grant admin consent for domain.**

### 4. Configure a valid credential

1. From the **Application** you created, select **Certificates and secrets**.
2. Under **Client secrets**, select **New client secret**.
    1. Give the client secret a **Description** and set the expiration time period according to your organizational policies.
    2. Select **Add**.

### Get Auth Token

At this point, you should have:

- The name of your tenant's domain
- The Application (client) ID
- The client secret or certificate

**Get Auth Token**

Send a post request to `https://login.microsoft.com` with the following parameters in the body:

- grant_type: `client_credentials`
- resource: `https://graph.microsoft.com`
- client_id:
- client_secret:

```powershell
$ClientID      = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here

$ClientSecret  = "<your client secret here>"    # Should be a ~44 character string; insert your info here

$tenantdomain  = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

$loginURL      = "https://login.microsoft.com"

$resource      = "https://graph.microsoft.com"

$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

$oauth        = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

Write-Output $oauth

if ($oauth.access_token -ne $null) {

$headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

$url = "https://graph.microsoft.com/v1.0/identityProtection/riskDetections"

Write-Output $url

$myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {

Write-Output $event

}

} else {

Write-Host "ERROR: No Access Token"

}
```

**Query API with Auth Token**

```http
https://graph.microsoft.com/v1.0/identityProtection/riskDetections

Authorization="<token_type> <access_token>"
```



### Get all of the offline risk detections (riskDetection API)

With Identity Protection sign-in risk policies, you can apply conditions when risk is detected in real time. But what about detections that are discovered offline? To understand what detections occurred offline and, thus, wouldn't have triggered the sign-in risk policy, you can query the `riskDetection` API.

```http
GET https://graph.microsoft.com/v1.0/identityProtection/riskDetections?$filter=detectionTimingType eq 'offline'
```

### Get all of the users who successfully passed an MFA challenge triggered by risky sign-ins policy (riskyUsers API)

To understand the value Identity Protection risk-based policies have on your organization, you can query all of the users who successfully passed an MFA challenge triggered by a risky sign-ins policy. This information can help you understand which users Identity Protection has falsely detected as a risk and which of your legitimate users are performing actions that the AI deems risky.

```http
GET https://graph.microsoft.com/v1.0/identityProtection/riskyUsers?$filter=riskDetail eq 'userPassedMFADrivenByRiskBasedPolicy'
```

---
# Workload Identities

A workload identity is an identity that allows an application or service principal access to resources, sometimes in the context of a user. These workload identities differ from traditional user accounts as they:

- Can’t perform multifactor authentication.
- Often have no formal lifecycle process.
- Need to store their credentials or secrets somewhere.

### Requirements to use workload identity protection

To make use of workload identity risk, including the new Risky workload identities (preview) blade and the Workload identity detections tab in the Risk detections blade, in the Azure portal you must have the following.

- Microsoft Entra ID Premium P2 licensing
- Logged in user must be assigned either:
    - Security administrator
    - Security operator
    - Security reader

### What types of risks are detected?

| **Detection name**                              | **Detection type** | **Description**                                                                                                                                                                                                                                                                                                                |
| ----------------------------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Microsoft Entra threat intelligence             | Offline            | This risk detection indicates some activity that is consistent with known attack patterns based on Microsoft's internal and external threat intelligence sources.                                                                                                                                                              |
| Suspicious Sign-ins                             | Offline            | This risk detection indicates sign-in properties or patterns that are unusual for this service principal.                                                                                                                                                                                                                      |
|                                                 |                    | The detection learns the baselines sign-in behavior for workload identities in your tenant in between 2 and 60 days, and fires if one or more of the following unfamiliar properties appear during a later sign-in: IP address / ASN, target resource, user agent, hosting/non-hosting IP change, IP country, credential type. |
| Unusual addition of credentials to an OAuth app | Offline            | This detection is discovered by Microsoft Defender for Cloud Apps. This detection identifies the suspicious addition of privileged credentials to an OAuth app. This can indicate that an attacker has compromised the app, and is using it for malicious activity.                                                            |
| Admin confirmed account compromised             | Offline            | This detection indicates an admin has selected 'Confirm compromised' in the Risky Workload Identities UI or using riskyServicePrincipals API. To see which admin has confirmed this account compromised, check the account’s risk history (via UI or API).                                                                     |
| Leaked Credentials (public preview)             | Offline            | This risk detection indicates that the account's valid credentials have been leaked. This leak can occur when someone checks in the credentials in public code artifact on GitHub, or when the credentials are leaked through a data breach.                                                                                   |

### Add conditional access protection

Using **Conditional Access for workload identities**, you can block access for specific accounts you choose when Identity Protection marks them "at risk." Policy can be applied to single-tenant service principals that have been registered in your tenant. Third-party SaaS, multi-tenanted apps, and managed identities are out of scope.

---
# License requirements

Microsoft Entra Identity Protection requires a **Microsoft Entra ID Premium P2** license.

|**Capability**|**Details**|**Microsoft Entra ID Free / Microsoft 365 Apps**|**Microsoft Entra ID Premium P1**|**Microsoft Entra ID Premium P2**|
|---|---|---|---|---|
|Risk policies|User risk policy (via Identity Protection)|No|No|Yes|
|Risk policies|Sign-in risk policy (via Identity Protection or Conditional Access)|No|No|Yes|
|Security reports|Overview|No|No|Yes|
|Security reports|Risky users|Limited information. Only users with medium and high risk are shown. No details drawer or risk history.|Limited information. Only users with medium and high risk are shown. No details drawer or risk history.|Full access|
|Security reports|Risky sign ins|Limited information. No risk detail or risk level is shown.|Limited information. No risk detail or risk level is shown.|Full access|
|Security reports|Risk detections|No|Limited information. No details drawer.|Full access|
|Notifications|Users at risk detected alerts|No|No|Yes|
|Notifications|Weekly digest|No|No|Yes|
||MFA registration policy|No|No|Yes|