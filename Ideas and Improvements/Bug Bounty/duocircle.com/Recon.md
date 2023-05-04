#### Tech
- WHMCS
- Clouflare
- PHP
- Apache
- Load Balancer: awselb/2.0

---

#### duocircle.com

#### halonapi.duocircle.com
#### proxy1.duocircle.com
#### release.duocircle.com
#### support.duocircle.com
#### logs.duocircle.com
- IP - 52.28.87.204
- /api -> /v2 [404]


#### portal.duocircle.com
##### Intresting
- `style` cookie -> 403
- `X-Forwarded-For: ` -> DNS interaction
- `/cart.php?gid=1--` -> 403 (Load-balancer)
- `sqltype` - Get param, cookie -> "Critical Error" "Invalid Request Input"
- `incorrect` -> "Login Details Incorrect. Please try again."
- `this` -> "Something went wrong and we couldn't process your request."
- `GLOBALS` -> "Hacking attempt"
- `X-Original-Url: /admin/login.php` -> WHMCS Panel

##### X-Original-Url Paths
- /announcements
- /download
- /index.php -> clientarea.php
- /store -> cart.php
- /api -> "result=error;message=Invalid IP 103.170.226.94"
- /upgrade -> "Method Not Allowed"
- /subscription
- /knowledgebase

__Similar reports__
- https://hackerone.com/reports/737323


##### Paths
- /cart.php
- 

##### 

#### status.duocircle.com
#### api.duocircle.com
#### proxy2.duocircle.com

#### hiring.duocircle.com

#### launchpad.duocircle.com
#### release.duocircle.com
#### send.duocircle.com
#### shell.duocircle.com
#### vendorsecurity.duocircle.com
#### 