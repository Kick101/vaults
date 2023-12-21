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
- Global (Forests)false
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

![[Pasted image 20231206222117.png]]

__SID history__
- Attribute is used in migration scenarios. If a user in one domain is migrated to another domain, a new account is created in the second domain. The original user's SID will be added to the new user's SID history attribute, ensuring that they can still access resources in the original domain.

---
### ACLs
![[Pasted image 20231206155001.png]]

__AD Attack chains:__
- "Unprivileged" users (shadow admins) having administrative access on member servers or workstations.
- Privileged users having a logon session on these workstations and member servers.
- Other forms of object-to-object control include force password change, add group member, change owner, write ACE, and full control.
#### ACL Abuse
|ACL Abuse|Command-let|
|-|-|
|ForceChangePassword|`Set-DomainUserPassword`|
|Add Members|`Add-DomainGroupMember`|
|GenericAll | `Set-DomainUserPassword` or `Add-DomainGroupMember`|
|GenericWrite| `Set-DomainObject`|
|WriteOwner| `Set-DomainObjectOwner`|
|WriteDACL | `Add-DomainObjectACL`|
|AllExtendedRights | `Set-DomainUserPassword` or `Add-DomainGroupMember`|

#### Interesting Rights
__WriteDacl__
- We can give ourselves DCSync rights

__GenericAll/GenericWrite__
- We can force change a user's password or add our account to a specific group
- (Less destructive than changing password) Set a fake SPN on the account & perform a targeted `Kerberoasting` attack or modify the account's `userAccountControl` not to require Kerberos pre-authentication and perform a targeted `ASREPRoasting attack`.
- If changing a user's password lead to domain compromise, we can `DCSync`, obtain the account's password history, and use `Mimikatz` to reset the account to the previous password using `LSADUMP::ChangeNTLM` or `LSADUMP::SetNTLM`.
- We can also perform a Kerberos Resource-based Constrained Delegation attack.

__SeDebugPrivilege__

__SeTakeOwnershipPrivilege__


##### DCSync ACLs
- Replicating Directory Changes (DS-Replication-Get-Changes)
- Replicating Directory Changes All (DS-Replication-Get-Changes-All)
- Replicating Directory Changes In Filtered Set (DS-Replication-Get-Changes-In-Filtered-Set)

---
### GPO
>- GPOs can be abused to perform attacks such as adding additional rights to a user, adding a local admin, or creating an immediate scheduled task.
>- Create a scheduled task to modify group membership, add an account, run DCSync, or send back a reverse shell connection.
>- Install targeted malware across the entire Domain.
>
>[SharpGPOAbuse](https://github.com/FSecureLABS/SharpGPOAbuse) is an excellent tool 




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
### Kerberos  Delegation
> Kerberos Delegation allows to "reuse the end-user credentials to access resources hosted on a different server".

- This is typically useful in multi-tier service or applications where Kerberos _Double Hop_ is required.
- For example, users authenticates to a web server and web server makes requests to a database server. The web server can request access to resources (all or some resources depending on the type of delegation) on the database server as the user and not as the web server's service account.
- If the user is not using Kerberos authentication to authenticate to the first hop server, Windows offers _Protocol Transition_ to transition the request to Kerberos.

![[Pasted image 20230920021133.png]]

#### General/Basic or Unconstrained Delegation
- It allows the first hop server (web server in our example) to _request access to any service on any computer in the domain_.
- DC places user's _TGT inside TGS_ (Step 4 in the diagram). When presented to the server, the TGT is extracted from TGS and stored in LSASS. This way the server can reuse the user's TGT to access any other resource as the user.
- This could be used to escalate privileges in case we can compromise the computer with unconstrained delegation and a Domain Admin connects to that machine.

#### Constrained Delegation
- It allows the first hop server (web server in our example) to _request access only to specified services on specified computers_. 
- A typical scenario where constrained delegation is used - A user authenticates to a web service without using Kerberos and the web service makes requests to a database server to fetch results based on the user's authorization.
- To impersonate the user, Service for User (S4U) extension is used which provides two extensions:
	- __Service for User to Self (S4U2self)__ - Allows a service to obtain a _forwardable TGS_ to itself(web server) on behalf of a user with just the user principal name without supplying a password. The service account must have the _TRUSTED_TO_AUTHENTICATE_FOR_DELEGATION_ - T2A4D UserAccountControl attribute.
	- __Service for User to Proxy (S4U2proxy)__ - Allows a service to obtain a TGS to a second service on behalf of a user. This is controlled by msDS-AllowedToDelegateTo attribute of the delegated computer(web server). This attribute contains a list of SPNs to which the user tokens can be forwarded i.e, computer delegated to.

##### Protocol Transition
![[Pasted image 20231219185312.png]]
- To abuse constrained delegation in above scenario, we need to have access to the websvc account. If we have access to that account, it is possible to access the services listed in msDS-AllowedToDelegateTo of the websvc account as ANY user.


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
