**You can enable ASR rules by using any of these methods:**

1. Microsoft Intune
2. Mobile Device Management (MDM)
3. Microsoft Endpoint Configuration Manager
4. Group Policy
5. PowerShell

Enterprise-level management such as Intune or Microsoft Endpoint Configuration Manager is recommended. 

>Enterprise-level management will overwrite any conflicting Group Policy or PowerShell settings on startup.

---
## 1. Intune: Configure ASR via Device Configuration Profiles

1. Go to **Device Configuration *->* Profiles**  
   - Select an existing profile **or**  
   - Click **Create profile** *->* Set **Profile type** to *Endpoint protection*
2. **Configure ASR Rules:**  
   - In *Endpoint protection*, select **Windows Defender Exploit Guard**  
   - Open **Attack Surface Reduction**  
   - Set desired options for each rule
3. **Add ASR Exceptions:**  
   - Input individual files/folders manually  
   - Or use **Import** to upload a CSV  
   - CSV format:   `C:\folder,  %ProgramFiles%\folder\file, C:\path` 

4. **Finish:**  
   - Click **OK** on all panes  
   - Select **Create** or **Save** the profile

---
## 2. Intune: Configure ASR via Endpoint Security Policy

1. Go to **Endpoint Security *->* Attack surface reduction**  
   - Select an existing policy **or**  
   - Click **Create Policy** *->* Choose **Profile type: Attack surface reduction rules**

2. **Configure Rules:**  
   - In *Configuration settings*, choose **Attack Surface Reduction**
   - Set desired options for each rule

3. **Add File/Folder Lists:**  
   - Define:
     - Protected folders  
     - Apps allowed access  
     - Excluded files/paths  
   - Or **Import** a CSV `C:\folder, %ProgramFiles%\folder\file, C:\path`

4. **Finalize:**  
   - Click **Next** through config panes  
   - Select **Create** or **Save** the policy

---
## 3. Mobile Device Management: ASR Rules via CSP

To manage the attack surface reduction rules in mobile device management:

- Use the `./Vendor/MSFT/Policy/Config/Defender/AttackSurfaceReductionRules` configuration service provider (CSP) to individually enable and set the mode for each rule.
- Follow the mobile device management reference in [Attack surface reduction rules](https://learn.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-atp/attack-surface-reduction#attack-surface-reduction-rules?azure-portal=true) for using GUID values.
- **OMA-URI path:** `./Vendor/MSFT/Policy/Config/Defender/AttackSurfaceReductionRules`
- Value: 

```
 75668C1F-73B5-4CF0-BB93-3ECF5CB7CC84=2|3B576869-A4EC-4529-8536-B80A7769E899=1|D4F940AB-401B-4EfC-AADC-AD5F3C50688A=2|D3E037E1-3EB8-44C8-A917-57927947596D=1|5BEB7EFE-FD9A-4556-801D-275E5FFC04CC=0|BE9BA2D9-53EA-4CDC-84E5-9B1EEEE46550=1
```

- The values to enable, disable, or enable in audit mode are:
    - *Disable* = 0
    - *Block* (enable attack surface reduction rule) = 1
    - *Audit* = 2
- Use the `./Vendor/MSFT/Policy/Config/Defender/AttackSurfaceReductionOnlyExclusions` configuration service provider (CSP) to add exclusions.

**Example:**
- OMA-URI path: `./Vendor/MSFT/Policy/Config/Defender/AttackSurfaceReductionOnlyExclusions`
- Value: `c:\path|e:\path|c:\wlisted.exe`

---
## 4. Microsoft Endpoint Configuration Manager

To configure **Attack Surface Reduction (ASR)** rules:

1. **Assets and Compliance** *->* **Endpoint Protection** *->* **Windows Defender Exploit Guard**
2. Select:  **Home** *->* **Create Exploit Guard Policy**
3. Enter:
   - **Name**
   - **Description**
   - Select **Attack Surface Reduction**, then click **Next**
2. Choose rules to **Block** or **Audit**  
   Click **Next**
3. **Review** the settings and click **Next**
4. After creation, click **Close**

---
## Group policy

**To manage the attack surface reduction rules in Group Policy:**


---
## PowerShell

>Use `Add-MpPreference` to append or add apps to the list. Using the `Set-MpPreference` cmdlet will overwrite the existing list.

- Type _PowerShell_ in the Start menu, right-click Windows PowerShell, and select Run as administrator.
    
- Enter the following cmdlet:

```powershell
Set-MpPreference -AttackSurfaceReductionRules_Ids <rule ID> -AttackSurfaceReductionRules_Actions Enabled
```

- To enable attack surface reduction rules in audit mode, use the following cmdlet:

```powershell
Add-MpPreference -AttackSurfaceReductionRules_Ids <rule ID> -AttackSurfaceReductionRules_Actions AuditMode
```

- To turn off attack surface reduction rules, use the following cmdlet:

```powershell
Add-MpPreference -AttackSurfaceReductionRules_Ids <rule ID> -AttackSurfaceReductionRules_Actions Disabled
```

- You must specify the state individually for each rule, but you can combine rules and states in a comma-separated list.
- In the following example, the first two rules will be enabled, the third rule will be disabled, and the fourth rule will be enabled in audit mode:

```powershell
Set-MpPreference -AttackSurfaceReductionRules_Ids <rule ID 1>,<rule ID 2>,<rule ID 3>,<rule ID 4> -AttackSurfaceReductionRules_Actions Enabled, Enabled, Disabled, AuditMode
```

- You can also use the *Add-MpPreference* PowerShell verb to add new rules to the existing list.
- *Set-MpPreference* will always overwrite the existing set of rules. If you want to add to the existing set, you should use *Add-MpPreference* instead. You can obtain a list of rules and their current state by using *Get-MpPreference*.
- To exclude files and folders from attack surface reduction rules, use the following cmdlet:

```powershell
Add-MpPreference -AttackSurfaceReductionOnlyExclusions "<fully qualified path or resource>"
```

- Continue to use Add-MpPreference -AttackSurfaceReductionOnlyExclusions to add more files and folders to the list.

---
## List of attack surface reduction events

**All attack surface reduction events are located under:**
- **Applications and Services Logs** > **Microsoft** > **Windows in the Windows Event viewer**.

