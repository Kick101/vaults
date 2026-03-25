- Protect and Limit DA
- Isolate admin workstation
- Secure local admins
- Time bound and just enough administration
- Isolate admins in a separate forest and breach containment using Tiers & ESAE
---
### Protect and Limit DA
- Reduce the number of Domain Admins in your environment.
- Do not allow or limit login of DAs to any other machine other than the Domain Controllers. If logins to some servers is necessary, do not allow other administrators to login to that machine.
- (Try to) Never run a service with a DA. Credential theft protections which we are going to discuss soon are rendered useless in case of a service account.
- Set "Account is sensitive and cannot be delegated" for DAs.

__Protected Users Group__
- Protected Users is a group introduced in Server 2012 R2 for "better protection against credential theft" by not caching credentials in insecure ways. A user added to this group has following major device protections:
	- Cannot use CredSSP and WDigest - No more cleartext credentials caching.
	- NTLM hash is not cached.
	- Kerberos does not use DES or RC4 keys. No caching of clear text cred or long term keys.
- If the domain functional level is Server 2012 R2, following DC protections are available:
	- No NTLM authentication.
	- No DES or RC4 keys in Kerberos pre-auth.
	- No delegation (constrained or unconstrained)
	- No renewal of TGT beyond initial four hour lifetime - Hardcoded, unconfigurable "Maximum lifetime for user ticket" and "Maximum lifetime for user ticket renewal"
 
 __Protected Users Group__
- Needs all domain control to be at least Server 2008 or later (because AES keys).
- Not recommended by MS to add DAs and EAs to this group without testing "the potential impact" of lock out.
- No cached logon ie.e no offline sign-on.
- Having computer and service accounts in this group is useless as their credentials will always be present on the host machine.

---
### Isolate Administrative workstations
__Privileged Administrative Workstations (PAWs)__
- A hardened workstation for performing sensitive tasks like administration of domain controllers, cloud infrastructure, sensitive business functions etc.
- Can provides protection from phishing attacks, OS vulnerabilities, credential replay attacks.
- Admin Jump servers to be accessed only from a PAW, multiple strategies
	- Separate privilege and hardware for administrative and normal tasks.
	- Having a VM on a PAW for user tasks.

__LAPS (Local Administrator Password Solution)__
- Centralized storage of passwords in AD with periodic randomizing where read permissions are access controlled.
- Computer objects have two new attributes - ms-mcs-AdmPwd attribute stores the clear text password and ms-mcs-AdmPwdExpirationTime controls the password change.
- Storage in clear text, transmission is encrypted.
- Note - With careful enumeration, it is possible to retrieve which users can access the clear text password providing a list of attractive targets!

---
### Time Bound Administration 
__JIT__
- Just In Time (JIT) administration provides the ability to grant time-bound administrative access on per-request bases.
- Check out Temporary Group Membership! (Requires Privileged Access Management Feature to be enabled which can't be turned off later)
```powershell
Add-ADGroupMember -Identity 'Domain Admins' -Members newDA -MemberTimeToLive (New-TimeSpan -Minutes 60)
```

__JEA__
- JEA (Just Enough Administration) provides role based access control for PowerShell based remote delegated administration.
- With JEA non-admin users can connect remotely to machines for doing specific administrative tasks.
- For example, we can control the command a user can run and even restrict parameters which can be used.
- JEA endpoints have PowerShell transcription and logging enabled.

---
### Tier Model
– Tier 0 - Accounts, Groups and computers which have privileges across the enterprise like domain controllers, domain admins, enterprise admins. .
– Tier 1 - Accounts, Groups and computers which have access to resources having significant amount of business
value. A common example role is server administrators who maintain these operating systems with the ability to impact all enterprise services.
– Tier 2 - Administrator accounts which have administrative control of a significant amount of business value that is hosted on user workstations and devices. Examples include Help Desk and computer support administrators because they can impact the integrity of almost any user data.
- Control Restrictions - What admins control.
-  Logon Restrictions - Where admins can log-on to

	