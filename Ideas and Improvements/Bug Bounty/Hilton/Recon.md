#### Scope
```txt
*.hilton.com 
*.hilton.com.tr
*.hilton.io
*.hiltonbusinessonline.com
*.hiltonjapan.co.jp
*.hiltonmanage.com

hiltonlocalbiz.com
hiltonhawaiianvillage.jp


121.200.237.36/29
167.187.0.0/16
192.251.123.0/24
192.251.124.0/24
192.251.125.0/24
192.251.126.0/24
203.79.37.2/29
62.216.152.46/29
82.196.42.196/28
```

>All subdomains of hilton.com that resolve to IP addresses belonging to the Rackspace organization are considered out of scope. In addition, the application eis.hilton.com is out of scope.
>
>"Test-Hackerone" must be prepended to the First and Last name for all Honors accounts.

---
#### TECH
- SSO: SailPoint

---
### Honors
A: 1833761529
B: 1833802471, guestId: 1675194808

__Auth Handling__
- accessToken, username(honor Id), guestId saved in wso2AuthToken Cookie
- wso2AuthToken got in login response as JSON

__Test Cases__
- Leak Honors account numbers

---
### hiltonlocalbiz.com
#### Tech 
IIS, Windows, ASP.NET

- 

_Error_
>GraphQL introspection is not allowed, but the query contained __schema or __type


---
#### API
- /graphql/customer?appName=dx-guests-ui&operationName=createGuest


---
### hilton.io
#### subs
- developer.hilton.io
- developer-s.hilton.io

#### 403
- apipm.pep-s.hilton.io
- apipm.pep.hilton.io

#### 404
```txt
apic.hilton.io
apic-prv.hilton.io
apic-s.hilton.io
apic-t.hilton.io
api.hilton.io
apip.hilton.io
apip-prv.hilton.io
api-prv.hilton.io
apip-s.hilton.io
apip-t.hilton.io
api-s.hilton.io
api-t.hilton.io
kapic.hilton.io
kapic-s.hilton.io
kapic-t.hilton.io
kapi.hilton.io
kapip.hilton.io
kapip-s.hilton.io
kapip-t.hilton.io
kapi-s.hilton.io
kapi-t.hilton.io
m.hilton.io
m.stg.hilton.io
m.test.hilton.io
```

