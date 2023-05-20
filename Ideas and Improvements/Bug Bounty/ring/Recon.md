#### Scope
-   Ring
    -   ring.com/*
    -   api.ring.com/*
    -   fw.ring.com/*
    -   app.ring.com/*
    -   admin.ring.com/*
    -   nw.ring.com/* [426]
    -   oauth.ring.com/*
    -   billing.ring.com/*
    -   [http://prd-ring-web-us.prd.rings.solutions](http://prd-ring-web-us.prd.rings.solutions)
-   Blink
    -   \*.blinkforhome.com/*
    -   \*.immedia-semi.com/*
---
#### Intresting
https://backend.admin.ring.com/auth/sign_in

#### backend.admin.ring.com
- /api/ [302]
- X-Forwarded-Host: 127.0.0.1
- _401_
	- /devices `"authorization":"Nil JSON web token"`
	- /comments
	- /jobs
	- /tags
	- /admin_users
	- /invitations
	- /permissions
	- /retailers
	- /ringtones
- /ping `"ping":"successfull"`
- /maintenance `Title: Admin app`
- `fuzz.xml` -> xml resp, `fuzz.json` -> json resp

