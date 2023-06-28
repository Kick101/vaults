### Auth Pages
https://support.bigcommerce.com/BCSLogin
https://support.bigcommerce.com/login
https://login.bigcommerce.com/login
https://oauth.bigcommerce.com/
https://login.bigcommerce.com/deep-links/marketplace/apps/5073
https://investors.bigcommerce.com/user/register [403]
https://support.bigcommerce.com/s/login/?language=en_US&startURL=%2Fidp%2Flogin%3Fapp%3D0sp4O0000004Cle%26RelayState%3D%252Fprm%252FEnglish%252Fs%252Fassets%26binding%3DHttpPost%26inresponseto%3D_5b97042e-6b3b-48d2-8fc2-0f67d5b70800
https://partners.bigcommerce.com/English/register_email.aspx


### Paths

/api/v2/coupons/1 [https://npm.runkit.com/bigcommerce-node-v2]

### IPs
https://64.183.182.118/ [Connect] [Session Key] 
https://209.170.205.68 [page not found]
https://206.71.93.68/ [IIS]

### Interesting
https://64.183.182.118/appliance/login [no cve]
https://64.183.182.118/login [forbidden] [User Administration]
- `Sec-CH-UA: "Opera";v="81", " Not;A Brand";v="99", "Chromium";v="95"`
- `_method` - GET & POST
- `max-forwards`
- 

#### 64.183.182.118
/console
/index [session key]
/rep -> login
/stats [403]

__From main.js__
/login
- /my_account [forbidden]
	- /profile
		-  /change-email
		- /change-display-names
		- /set-extended-availability
		- /rep-avatar
	- /security
		- /generate-mfa-secret
		- /store-mfa-secret
		- /remove-mfa-secret
		- /change-password
	- /discover
		- /discover-via-pinned-clients
		- /accounts
		- /user-push-agents
		- /discovery-jobs
		- /discovery-progress/:Job-ID
	- /vault_account
		- /available_users
		- /can-checkout-shared-accounts
		- /all-ids
- downloads
		- /console
		- /drivers
- search-index.json
- 

---
### Six Big Questions
- How does the app pass data?
    - Is it REST? Is it Parameter?
- How/Where does the app talk about users?
    - Referenced in cookies? Headers? Parameters? uid? uuid? email? username?
    - How users are referenced by the app and by the API?
- Does the site have multi-tenancy or user levels?
    - Admin/User, Admin/User/Guest levels
- Does the site have a unique threat model?
    - API Keys, specific to the app & it's users
- Has there been past security research & vulns?
    - Try to bypass the patch, know what issues they're facing
- How does the app handle against vulnerabilities?
    - XSS, CSRF, Code Injections