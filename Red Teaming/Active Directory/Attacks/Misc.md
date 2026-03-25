### Create Password Lists

__Get Only passwords of length__
```bash
 cat new.pass| awk 'length($0) > 7 && length($0) < 13' | tee passwords.txt
```

#### Password Set Times
- Target Old passwords
- In most organizations, administrators have multiple accounts. If you see the administrator changing his "user account" around the same time as his "Administrator Account", they are highly likely to use the same password for both accounts. 
- If several passwords are at once then _maybe password is the same across those accounts_. For one user, guess passwords like "Password2020", for a different user, try company name "Freight2020!".
- Avoid passwords that wouldn't make sense, such as "Winter2019." when password was set in July of 2019.
- If you see a _several passwords set at the same time_, this indicates they were set by the Help Desk and may be the same. 

```powershell
Get-DomainUser -Properties samaccountname,pwdlastset,lastlogon -Domain InlaneFreight.local | select samaccountname, pwdlastset, lastlogon | Sort-Object -Property pwdlastset
```
__Certain Date__
```powershell
Get-DomainUser -Properties samaccountname,pwdlastset,lastlogon -Domain InlaneFreight.local | select samaccountname, pwdlastset, lastlogon | where { $_.pwdlastset -lt (Get-Date).addDays(-90) }
```

