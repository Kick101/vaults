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
Honors Account Number is 1833761529
1833802471, guestId: 1675194808

- Leak Honors account numbers?

---
### hiltonlocalbiz.com
#### Tech 
IIS, Windows, ASP.NET

#### API
- /graphql/customer?appName=dx-guests-ui&operationName=createGuest
- 

_Error_
>GraphQL introspection is not allowed, but the query contained __schema or __type







