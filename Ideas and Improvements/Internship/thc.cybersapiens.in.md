### Initial Recon
#### Web Technologies
![[Pasted image 20230808193612.png]]


#### Finding Endpoints
##### Wayback Machine Archive
![[Pasted image 20230808185708.png]]

##### ffuf
__Common.txt__
- admin
- .well-known/http-opportunistic 
- /cgi-sys/ [403]
- cpanel/controlpanel
- login/
- mailman [403]
- pipermail
- webmail 

__raft-medium-directories_long.txt__
- admin/error_log
- admin/welcome.php
- admin/wp-config.php [403]
- login/uploads
- login/testing.php
- login/etc
- login/errors.php
- login/server.php
- login/conn.php
- login/wp-config.php
- login/error_log
- login/mail
- 
