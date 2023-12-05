| **Command** | **Description** |
| --------------|-------------------|
| `xfreerdp /v:<target IP address> /u:htb-student /p:<password>` | RDP to lab target |
| `Get-DomainPolicy` | View the domain password policy |
| `.\SharpView.exe ConvertTo-SID -Name sally.jones`            | Convert a username to a SID |
| `.\SharpView.exe Convert-ADName -ObjectName S-1-5-21-2974783224-3764228556-2640795941-1724` | Convert a SID to a username |
| `Get-DomainUser harry.jones  \| ConvertFrom-UACValue -showall` | List all UAC values |
| `.\SharpView.exe Get-Domain` | View information about the current domain |
| `.\SharpView.exe Get-DomainOU` | List all OUs |
| `.\SharpView.exe Get-DomainUser -KerberosPreauthNotRequired` | Find ASREPRoastable users |
| `Get-DomainComputer ` | Get a listing of domain computers |
| `.\SharpView.exe Get-DomainGPO  \| findstr displayname` | List all GPO names |
| ` Get-DomainGPO -ComputerIdentity WS01` | List GPOs on a specific host |
| `Test-AdminAccess -ComputerName SQL01` | Test local admin access on a remote host |
| `.\SharpView.exe Get-NetShare -ComputerName SQL01` | Enumerate open shares on a remote computer |
| `Find-DomainUserLocation` | Find machines where domain users are logged in |
| `Get-DomainTrust` | View a list of domain trusts |
| `(Get-DomainUser).count` | Count all domain users |
| `.\SharpView.exe Get-DomainUser -Help` | Get help about a SharpView function |
| `Get-DomainUser -Properties samaccountname,description \| Where {$_.description -ne $null}` | Find non-blank user description fields |
| `.\SharpView.exe Get-DomainUser -SPN` | Find users with SPNs set |
| `Find-ForeignGroup` | Find foreign domain users |
| `Get-DomainGroup -Properties Name` | List domain groups |
| `.\SharpView.exe Get-DomainGroupMember -Identity 'Help Desk'` | Get members of a domain group |
| `.\SharpView.exe Get-DomainGroup -AdminCount` | List protected groups |
| `.\SharpView.exe Find-ManagedSecurityGroups` | List managed security groups |
| `Get-NetLocalGroup -ComputerName WS01` |  Get local groups on a host |
| `.\SharpView.exe Get-NetLocalGroupMember -ComputerName WS01` | Get members of a local group |
| `.\SharpView.exe Get-DomainComputer -Unconstrained` | Find computers that allow unconstrained delegation |
| `Get-DomainComputer -TrustedToAuth` | Find computers set with constrained delegation |
| `Get-DomainObjectAcl -Identity harry.jones` | Enumerate ACLs on a user |
| `Find-InterestingDomainAcl` | Find objects in the domain with modification rights over non built-in objects| 
| `Get-PathAcl "\\SQL01\DB_backups"` | Find the ACLs set on a directory |
| ` gpresult /r /S WS01` | Get a report of all GPOs applied to a host |
| ` Get-DomainGPO  \| Get-ObjectAcl` | Find GPO permissions |
| `Get-DomainTrustMapping` | Enumerate trusts for our domain/reachable domains |

---
#### Misc Functions
| Command Name                   | Description                                                    |
| ------------------------------| -------------------------------------------------------------- |
| Export-PowerViewCSV            | thread-safe CSV append                                         |
| Resolve-IPAddress              | resolves a hostname to an IP                                   |
| ConvertTo-SID                  | converts a given user/group name to a security identifier (SID) |
| Convert-ADName                 | converts object names between a variety of formats             |
| ConvertFrom-UACValue           | converts a UAC int value to human readable form                |
| Add-RemoteConnection           | pseudo "mounts" a connection to a remote path using the specified credential object |
| Remove-RemoteConnection        | destroys a connection created by New-RemoteConnection           |
| Invoke-UserImpersonation       | creates a new "runas /netonly" type logon and impersonates the token |
| Invoke-RevertToSelf            | reverts any token impersonation                                |
| Get-DomainSPNTicket            | request the kerberos ticket for a specified service principal name (SPN) |
| Invoke-Kerberoast              | requests service tickets for kerberoast-able accounts and returns extracted ticket hashes |
| Get-PathAcl                    | get the ACLs for a local/remote file path with optional group recursion |

#### Domain/LDAP Functions
| Cmdlet                         | Description |
| ----------------------------- | ----------- |
| Get-DomainDNSZone             | enumerates the Active Directory DNS zones for a given domain |
| Get-DomainDNSRecord           | enumerates the Active Directory DNS records for a given zone |
| Get-Domain                    | returns the domain object for the current (or specified) domain |
| Get-DomainController          | return the domain controllers for the current (or specified) domain |
| Get-Forest                    | returns the forest object for the current (or specified) forest |
| Get-ForestDomain              | return all domains for the current (or specified) forest |
| Get-ForestGlobalCatalog       | return all global catalogs for the current (or specified) forest |
| Find-DomainObjectPropertyOutlier | finds user/group/computer objects in AD that have 'outlier' properties set |
| Get-DomainUser                | return all users or specific user objects in AD |
| New-DomainUser                | creates a new domain user (assuming appropriate permissions) and returns the user object |
| Set-DomainUserPassword        | sets the password for a given user identity and returns the user object |
| Get-DomainUserEvent           | enumerates account logon events (ID 4624) and Logon with explicit credential events |
| Get-DomainComputer            | returns all computers or specific computer objects in AD |
| Get-DomainObject              | returns all (or specified) domain objects in AD |
| Set-DomainObject              | modifies a given property for a specified active directory object |
| Get-DomainObjectAcl           | returns the ACLs associated with a specific active directory object |
| Add-DomainObjectAcl           | adds an ACL for a specific active directory object |
| Find-InterestingDomainAcl     | finds object ACLs in the current (or specified) domain with modification rights set to non-built in objects |
| Get-DomainOU                  | search for all organization units (OUs) or specific OU objects in AD |
| Get-DomainSite                | search for all sites or specific site objects in AD |
| Get-DomainSubnet              | search for all subnets or specific subnets objects in AD |
| Get-DomainSID                 | returns the SID for the current domain or the specified domain |
| Get-DomainGroup               | return all groups or specific group objects in AD |
| New-DomainGroup               | creates a new domain group (assuming appropriate permissions) and returns the group object |
| Get-DomainManagedSecurityGroup| returns all security groups in the current (or target) domain that have a manager set |
| Get-DomainGroupMember         | return the members of a specific domain group |
| Add-DomainGroupMember         | adds a domain user (or group) to an existing domain group, assuming appropriate permissions to do so |
| Get-DomainFileServer          | returns a list of servers likely functioning as file servers |
| Get-DomainDFSShare            | returns a list of all fault-tolerant distributed file systems for the current (or specified) domain |

#### GPO functions
| Command                               | Description |
| ------------------------------------- | ----------- |
| Get-DomainGPO                         | returns all GPOs or specific GPO objects in AD |
| Get-DomainGPOLocalGroup               | returns all GPOs in a domain that modify local group memberships through 'Restricted Groups' or Group Policy preferences |
| Get-DomainGPOUserLocalGroupMapping    | enumerates the machines where a specific domain user/group is a member of a specific local group, all through GPO correlation |
| Get-DomainGPOComputerLocalGroupMapping| takes a computer (or GPO) object and determines what users/groups are in the specified local group for the machine through GPO correlation |
| Get-DomainPolicy                      | returns the default domain policy or the domain controller policy for the current domain or a specified domain/domain controller |
#### Computer Enumeration Functions
| Command                              | Description                                                                                |
|--------------------------------------|--------------------------------------------------------------------------------------------|
| Get-NetLocalGroup                    | Enumerates the local groups on the local (or remote) machine                                |
| Get-NetLocalGroupMember              | Enumerates members of a specific local group on the local (or remote) machine               |
| Get-NetShare                         | Returns open shares on the local (or remote) machine                                        |
| Get-NetLoggedon                      | Returns users logged on the local (or remote) machine                                       |
| Get-NetSession                       | Returns session information for the local (or remote) machine                               |
| Get-RegLoggedOn                      | Returns who is logged onto the local (or remote) machine through enumeration of registry keys|
| Get-NetRDPSession                    | Returns remote desktop/session information for the local (or remote) machine                |
| Test-AdminAccess                     | Tests if the current user has administrative access to the local (or remote) machine        |
| Get-NetComputerSiteName              | Returns the AD site where the local (or remote) machine resides                             |
| Get-WMIRegProxy                      | Enumerates the proxy server and WPAD contents for the current user                           |
| Get-WMIRegLastLoggedOn               | Returns the last user who logged onto the local (or remote) machine                         |
| Get-WMIRegCachedRDPConnection        | Returns information about RDP connections outgoing from the local (or remote) machine       |
| Get-WMIRegMountedDrive               | Returns information about saved network mounted drives for the local (or remote) machine    |
| Get-WMIProcess                       | Returns a list of processes and their owners on the local or remote machine                 |
| Find-InterestingFile                 | Searches for files on the given path that match a series of specified criteria              |
#### Threaded 'Meta'-Functions
The 'meta' functions can be used to _find where domain users are logged in_, look for _specific processes on remote hosts_, find domain _shares, find files on domain shares_, and test where our current user has local admin rights.

| Command                              | Description |
|--------------------------------------|-------------|
| Find-DomainUserLocation              | finds domain machines where specific users are logged into |
| Find-DomainProcess                   | finds domain machines where specific processes are currently running |
| Find-DomainUserEvent                 | finds logon events on the current (or remote domain) for the specified users |
| Find-DomainShare                     | finds reachable shares on domain machines |
| Find-InterestingDomainShareFile      | searches for files matching specific criteria on readable shares in the domain |
| Find-LocalAdminAccess                | finds machines on the local domain where the current user has local administrator access |
| Find-DomainLocalGroupMember          | enumerates the members of specified local group on machines in the domain |


#### Domain Trust Functions
The domain trust functions provide us with the tools we need to enumerate information that can be used to mount cross-trust attacks.

| Command | Description |
|---------|-------------|
| Get-DomainTrust | returns all domain trusts for the current domain or a specified domain |
| Get-ForestTrust | returns all forest trusts for the current forest or a specified forest |
| Get-DomainForeignUser | enumerates users who are in groups outside of the user's domain |
| Get-DomainForeignGroupMember | enumerates groups with users outside of the group's domain and returns each foreign member |
| Get-DomainTrustMapping | enumerates all trusts for the current domain and then enumerates all trusts for each domain it finds |
