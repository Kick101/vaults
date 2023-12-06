### Objects
__Security Principals__ can be authenticated by the domain and can be assigned privileges over resources like files or printers.
#### Users
- People: employees, managers
- Services: these users will _only have service needed privileges_ to run
#### Machines
Every computer that joins the Active Directory domain, a machine object will be created and assigned an account with limited rights within the domain itself.
- _Machine accounts are local administrators_
- Machine account name is the computer's name followed by a dollar sign. Ex: `DC01` -> `DC01$`

---
### Groups
#### Group Types
- Local
- Domain Local 
- Global (Forests)
- Universal (Stored in Global Catalog)

#### Security Groups

![[Pasted image 20230225013725.png]]


#### Domain Groups
|Group|Description|
|-|-|
|_Enterprise Admins_|Administrative rights across all domains/forests, stored in root domain|
|Schema Admins|Manages Domain schema, stored in root domain|
|_Domain Admins_|Specific to domain, enterprise admins is a member of this |
|Domain Users|Non administrative access|
|Domain Guests|Not added to local guest group. Cannot logon|
|Domain Computers|All computers except DC|
|_Domain Controllers_|All writable DCs except read only DCs|
|Read-Only Domain Controllers|Computer account of RODC|
|Enterprise RODC|Stored in root domain, empty by default|
|Allowed RODC Password Replication|Allows passwords to be cached on a RODC|
|_DNS Admins_|DNS administration|
|DNS Update Proxy|DNS updates for other clients|
|DHCP Administrators|DHCP administration|
|DHCP User|Read only access to DHCP Server|
|_Group Policy Creator Owners_|Modify GPO in the domain, admin is a member of this|
|_Cert Publishers_|Members can publish certificates for users & computers|
|_RAS & IAS Servers_|Allows access to remote access properties of a user|
#### Default Local Groups
|Group|Description|
|-|-|
|Administrators|Full control|
|Users|Change settings related to them|
|Power Users|Legacy support|
|Guests|kiosks|
|Backup Operators|Backup & restore files regardless of file permissions|
|Remote Desktop Users|Remote desktop access|
|Offer Remote Assistance Helpers||
|Network configuration operators|Change network settings, change/renew DHCP config|
|Performance Monitor Users|Monitor performance counters|
|IIS_IUSRS|Internet information services, run IIS|
|Replicator|Used on DC for replication|
|Distributed COM Users|Distributed n/w components|
|Cryptographic Operators|System Level cryptographic operations|


---
### Organizational Units (OUs)
_Container objects_ that allow you to classify users and machines. OUs are mainly used to define sets of users with similar _policing_ requirements. 
>A user can only be a part of a _single OU at a time_.

#### Default OUs
- __Builtin:__ Contains default groups available to any Windows host.
- **Computers:** Any machine joining the network will be put here by default.
- **Domain Controllers:** Contains the DCs in your network.
- **Users:** Default users and groups that apply to a domain-wide context.
- **Managed Service Accounts:** Holds accounts used by services in your Windows domain.
- __ForeignSecurityPrinciples:__ Accounts that bridge domains

---
### Managing Users
#### Deleting OUs and users
- To delete the OU, we need to enable the **Advanced Features** in the View menu
- Go to properties of OU and uncheck "Protect object from accidental deletion" under Object tab
#### Delegation
- Delegation allows you to grant users specific privileges to perform advanced tasks on OUs without needing a Domain Administrator to step in.
- Ex: Help desk privileges to reset other low-privilege users' passwords

__Current user privileges__
- Privileges differ b/w elevated and non-elevated console even though we're admin.
```powershell
whoami /priv
```

####  NT AUTHORITY\\SYSTEM
Local System account - `NT AUTHORITY\SYSTEM` is a built-in account in Windows operating systems, _used by the service control manager_.
- This account has _more privileges than a local administrator_ account and is used to run most Windows services and third party services.
- Having SYSTEM-level access within a domain environment is nearly equivalent to a domain user account. The only limitation is  Kerberoasting.

__Ways to gain SYSTEM-level access on a host:__
-   _Remote Windows exploits_ such as EternalBlue or BlueKeep.
-   _Abusing a service_ running in the context of the SYSTEM account.
-   _Abusing SeImpersonate privileges_ using [RottenPotatoNG](https://github.com/breenmachine/RottenPotatoNG) against older Windows systems, [Juicy Potato](https://github.com/ohpe/juicy-potato), or [PrintSpoofer](https://github.com/itm4n/PrintSpoofer) if targeting [Windows 10/Windows Server 2019](https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/).
-   _Local privilege escalation_ flaws in Windows operating systems such as the [Windows 10 Task Scheduler 0day](https://blog.0patch.com/2019/06/another-task-scheduler-0day-another.html).
-   _PsExec_ with the `-s` flag

__By gaining SYSTEM acct on a domain-joined host__
-   Enumerate domain users and groups, local administrator access, domain trusts, ACLs, user and computer properties, using `BloodHound`, and `PowerView`/`SharpView`.
-   Perform Kerberoasting / ASREPRoasting attacks.
-   Run tools such as [Inveigh](https://github.com/Kevin-Robertson/Inveigh) to gather Net-NTLM-v2 hashes or perform relay attacks.
-   Perform token impersonation to hijack a privileged domain user account.
-   Carry out ACL attacks.

---
### Managing Computers
__Types__
- Workstations
- Servers
- Domain Controllers

Classifying computers from _Computers OU_ allows us to configure policies for each OU.

---
### Group Policies
**Security Filtering** 
- It allows us to apply GPOs on specific users/computers under an OU. 
- By default, they will apply to the *Authenticated Users* group, which includes all users/PCs.

__Settings__
- Each GPO has configurations that apply to computers only and configurations that apply to users only.

#### GPO Distribution
- GPOs are distributed via a _network share_ called `SYSVOL`, which is stored in the DC.
- _SYSVOL share_ points by default to the `C:\Windows\SYSVOL\sysvol\` directory on each of the DCs
- Once a change has been made to any GPOs, it might take up to 2 hours for computers to catch up.

_Sync immediately_
```powershell
gpupdate /force
```

#### Restrict Access to Control Panel GPO
- Create new GPO "_Restrict Access to Control Panel_"
- Edit _User Configuration_ -> _Administrative Templates_ -> _Control Panel_ -> _Prohibit access to Control Panel and PC settings_

#### Auto Lock Screen GPO
- _Computer Configuration_ -> _Windows Settings_ -> _Security Settings_ -> _Local Policies_ -> _Security Operations_ -> _Interactive logon: Machine inactivity limit_

#### Apply GPO to OU
- Drag and drop each GPO to OUs

---
### Trees, Forests
__Tree__ is a group of domains that share a contiguous namespace and a common schema.
Ex: thm.local -> uk.thm.local & us.thm.local
- **Enterprise Admins** group will grant a user administrative privileges over all of an enterprise's domains.

__Forest__ is the union of several trees with different namespaces into the same network.
Ex: THM tree & MHT tree

---
### Trust
__One-way trust__
- If Domain A trusts Domain B then user from Domain B can access resources in Domain A
- Domain B denies access to user from Domain A

__Two-way trust__ - Mutual trust
- By default, joining several domains under a tree or a forest will form a two-way trust relationship.

__Transitive Trust__
- Trust extends from one domain to another domain

__Non-transitive Trust__
- Trust doesn't extend from one domain to another 

__External Forest Trust__
- Domain from forest A trusts a domain or root domain in forest B which is non-transitive

---
### ACLs
|**ACL**|**Description**|
|-|-|
|`Discretionary Access Control List (DACL)`|This defines which security principals are granted or denied access to an object.|
|`System Access Control Lists (SACL)`|These allow administrators to log access attempts made to secured objects.|
__AD Attack chains:__
-   "Unprivileged" users (shadow admins) having administrative access on member servers or workstations.
-   Privileged users having a logon session on these workstations and member servers.
-   Other forms of object-to-object control include force password change, add group member, change owner, write ACE, and full control.
#### ACL Abuse
-   ForceChangePassword abused with `Set-DomainUserPassword`
-   Add Members abused with `Add-DomainGroupMember`
-   GenericAll abused with `Set-DomainUserPassword` or `Add-DomainGroupMember`
-   GenericWrite abused with `Set-DomainObject`
-   WriteOwner abused with `Set-DomainObjectOwner`
-   WriteDACL abused with `Add-DomainObjectACL`
-   AllExtendedRights abused with `Set-DomainUserPassword` or `Add-DomainGroupMember`

#### Interesting Rights
- `GenericWrite` - we can leverage this to force change a user's password or add our account to a specific group
- `WriteDacl` - 

---
### User-Account-Control (UAC) Attributes
These values are not to be confused with the Windows User Account Control technology.

![[Pasted image 20230304114246.png]]

- _PASSWD_NOTREQD_ - 32
```powershell
Get-ADUser -Filter {adminCount -gt 0} -Properties admincount,useraccountcontrol | select Name,useraccountcontrol
```

>[UAC attribute values](https://academy.hackthebox.com/storage/resources/Convert-UserAccountControlValues.zip)

---
### Default Security Posture

A basic AD user account with _no added privileges can be used to enumerate_ the majority of objects contained within AD, including but not limited to:
-   Domain Computers
-   Domain Users
-   Domain Group Information
-   Default Domain Policy
-   Domain Functional Levels
-   Password Policy
-   Group Policy Objects (GPOs)
-   Kerberos Delegation
-   Domain Trusts
-   Access Control Lists (ACLs)

Many attacks exist that merely leverage AD misconfigurations, bad practices, or poor administration, such as:
-   Kerberoasting / ASREPRoasting
-   NTLM Relaying
-   Network traffic poisoning
-   Password spraying
-   Kerberos delegation abuse
-   Domain trust abuse
-   Credential theft
-   Object control

---
