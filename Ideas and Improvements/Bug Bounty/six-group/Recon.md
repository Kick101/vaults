#### Program Info
__Creds__
bb1101_gui_test_sup_02 [SIX Bug Bounty Supplier 1 (1101)]
CrqY0FYjRmqs

__Roles__
Technical Distributor, Technical Supplier - Can only login via API

__APIs__
test.six-dochub.com
- /api/soap/v3_0/dochub.wsdl (3.0)
- /api/soap/v3_1/dochub.wsdl (3.1)
- /api/rest/documentation

__Scope__
- https://test.six-dochub.com/
- https://www.bolsasymercados.es/

---
#### Tech
- Apache
- VueJS
- JWT
- 

---
#### test.six-dochub.com
- /en/widgets/FUZZ - "Page not found"
- /api/internal-perl-rest/auth - `X-Requested-With: XMLHttpRequest`
-  Private IP in 
```js
NODE_ENV:"production",VUE_APP_ENV:"PROD",VUE_APP_PROXY_PERL_API:"172.17.133.10",VUE_APP_PROXY_JAVA_API:"172.17.133.10:8090",BASE_URL:"/
```
- `/api/internal-perl-rest/connections?supplier_id=1101&channel_types[]=2&statuses[]=3&statuses[]=5&distributor_id=FUZZ` -> distributor_id
- 

##### JWT Test
__X-Login-Token Header__ -> Pre-auth token
- `/api/internal-perl-rest/auth/login-token`
- exp, jti

__access-token Cookie__ -> session management
-  `/api/internal-perl-rest/auth`
- jti, sid, exp

__Authorization Header__ -> API Access
- `/api/internal-perl-rest/auth`
- roles, client{id, fkis_customer_id, entitlements[]}, name, exp, jti, session_id (sid), id

##### REST API
/api/rest/v2/documents/latest/search-requests
/api/rest/v2/documents/archived/search-requests
/api/rest/v1/instruments/isin/isinValue}/pib/content-requests
/api/rest/v1/documents/latest/documentId}/content-requests
/api/rest/v1/documents/latest/search-requests
/api/rest/v1/documents/archived/archiveId}/content-requests
/api/rest/v1/documents/archived/search-requests
/api/rest/v1/authentication/authorization-tokens
/api/rest/v1/instruments/valor/identifier}/six-status-information
/api/rest/v1/instruments/isin/identifier}/six-status-information
/api/rest/v1/healthcheck/ping


#### bolsasymercados.es



