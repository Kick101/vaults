#### Recon
- Run init-recon.sh
- Shodan Recon
- Do  Search Engines recon (sensitive files, 3rd party integrations, parameter urls)
- Do GitHub Recon
- Do Cloud recon

#### App analysis
- Check Ports
- Content Discovery
	- Dot files (OneListForAll-2.4.1.1/dict/dotfiles_short.txt)
	- Backup files (OneListForAll-2.4.1.1/dict/zip_short.txt, bak_short.txt, bak_asset_short.txt)
	- SQLdatabase files (OneListForAll-2.4.1.1/dict/sql_short.txt)
	- Directories (OneListForAll-2.4.1.1/dict/raft-large-directories_long.txt)
	- Subdomain backups, domain backups (example.org.tar.xz) (All Txts/backup-extensions.txt)
	- Files (OneListForAll-2.4.1.1/dict/raft-large-files_long.txt, )
	- onelistforallshort.txt
	- Backdoors (OneListForAll-2.4.1.1/dict/backdoors_short.txt)
	- LogFiles (OneListForAll-2.4.1.1/dict/log_short.txt)
- Attack on parameters
	- Paste parameter urls on burp scan
	- Manual testing
---
### Functionality Attacks
#### Registration Page
1. Input Validation:
    - Test for any input validation bypasses, such as HTML/JavaScript injection or SQL injection.
2. User Enumeration:
    - Attempt to register with a known username or email address to determine if the system reveals information about existing users.
    - Verify if error messages or response times differ when registering with valid or invalid credentials.
3. Account Lockout Mechanism:
    - Verify if the registration page implements an account lockout mechanism to prevent brute force attacks.
    - Test the effectiveness of the lockout mechanism by exceeding the maximum number of allowed registration attempts.
4. Email Verification:
    - Verify if the verification link is secure and not prone to tampering or account takeover.
5. Privacy and Data Protection:
    - Ensure that the registration page collects and stores personal data in compliance with applicable data protection laws (e.g., GDPR).
    - Verify if there are appropriate consent mechanisms in place for data collection and usage.
6. CAPTCHA bypass
7. Session fixation
8. Bypassing email verification
9. Check for duplicate registration/Overwrite existing user
10. Check for reuse existing usernames
11. Check for insufficient email verification process
12. Overwrite default web pages by username registrations. => If user profile: /user then create a user acct with some endpoint like (/feedback - username: feedback)
---
#### Login Page
1. Input Validation
2. Account Lockout Mechanism
3. User Enumeration
4. Check if user credentials are transmitted over SSL or not
5. Test remember me functionality
6. Default creds
---
#### Captcha Bypass  
1. Remove the param value or remove the entire parameter  
2. Response manipulation  
3. Re-use captcha value  
4. Use any token with same length(+1/-1)  
5. Change request method & remove captcha  
6. Change body to JSON or vice-versa.  
7. Check source code for captcha value  
8. Same captcha value with different sessionIDs  
9. Try with http headers with local IP  
10. Check if the value is inside a cookie.  
11. Try OCR
---
#### SSO Bypass
- If internal.company.com -> auth.company.com.  Internal.company.com/FUZZ
- If company.com/internal -> SSO. Try To Insert public Before internal e.g. company.com/public/internal
- Try To Craft SAML Request With Token And Send It To The Server And Figure Out How Server Interact With This
- If There Is AssertionConsumerServiceURL In Token Request Try To Insert Your Domain e.g. http://me.com As Value To Steal The Token
- If There Is AssertionConsumerServiceURL In Token Request Try To Do FUZZ On Value Of AssertionConsumerServiceURL If It Is Not Similar To Origin
- If There Is Any UUID, Try To Change It To UUID Of Victim Attacker e.g. Email Of Internal Employee Or Admin Account etc
- 

---
#### MFA / OTP / Backup code Bypass
1. Response Manipulation
2. 2FA Code Leakage in Response
3. 2FA Code doesn't expire
4. Lack of Brute-Force Protection
5. Missing 2FA Code Integrity Validation
6. With null or 000000
7. Try John token in Jane acct
8. Force browse to next page with referrer header to the MFA page
9. Search the 2FA code in response/js files using burp search.
10. Enabling 2FA doesn't expire previous sessions.
11. No 2FA required for disabling 2FA.
12. Password can be reset via forgot password without 2FA.
13. Information disclosure about victim
14. Test backup code. [blog](http://c0d3g33k.blogspot.com/2018/02/how-i-bypassed-2-factor-authentication.html)
15. Force browse all authenticated endpoints without MFA verification
16. OTP Bypass in JSON: `{"code":["0000","0001",..,"9999"]}`
17. CSRF on 2FA Disabling: No CSRF Protection on disabling 2FA, also there is no auth confirmation.
18. 2FA gets disabled on password change/email change
19. Clickjacking on 2FA Disabling Page

---
#### Password Reset
- Failure to invalidate session on Logout and Password reset
- Check if forget password reset link/code uniqueness
- If reset link has another param such as date and time, then. Change date and time value in order to make active & valid reset link
- Add only spaces in new password and confirmed password. Then Hit enter and see the result
-  Ask for two password reset link and use the older one from user's email
- Check if active session gets destroyed upon changing the password or not?
- Send continuous forget password requests so that it may send sequential tokens
##### Password Reset Token Leak Via Referrer
1. Request password reset to your email address
2. Click on the password reset link
3. Don't change password
4. Click any 3rd party websites(eg: Facebook, twitter)
5. Intercept the request in Burp Suite proxy
6. Check if the referer header is leaking password reset token.

##### Password Reset Poisoning
1. Intercept the password reset request in Burp Suite
2. Add or edit the following headers in Burp Suite : `Host: attacker.com`, `X-Forwarded-Host: attacker.com`
3. Forward the request with the modified header
```http
POST https://example.com/reset.php HTTP/1.1
Host: attacker.com
```
1. Look for a password reset URL based on the *host header* like : `https://attacker.com/reset-password.php?token=TOKEN`

##### Password Reset Via Email Parameter
__parameter pollution__
email=victim@mail.com&email=hacker@mail.com

__array of emails__
{"email":["victim@mail.com","hacker@mail.com"]}

__carbon copy__
email=victim\@mail.com%0A%0Dcc:hacker\@mail.com
email=victim\@mail.com%0A%0Dbcc:hacker\@mail.com

__separator__
email=victim\@mail.com,hacker\@mail.com
email=victim\@mail.com%20hacker\@mail.com
email=victim\@mail.com|hacker\@mail.com

##### Weak Password Reset Token
- Check if reset link does get expire or not if its not used by the user for certain amount of time
- can be guessed? 
- The following variables might be used by the algorithm.
* Timestamp
* UserID
* Email of User
* Firstname and Lastname
* Date of Birth
* Cryptography
* Number only
* Small token sequence (<6 characters between [A-Z,a-z,0-9])
* Token reuse
* Token expiration date

##### Leaking Password Reset Token
1. Trigger a password reset request using the API/UI for a specific email e.g: test\@mail.com
2. Inspect the server response and check for `resetToken`
3. Then use the token in an URL like `https://example.com/v3/user/password/reset?resetToken=[THE_RESET_TOKEN]&email=[THE_MAIL]`

##### Password Reset Via Username Collision
1. Register on the system with a username identical to the victim's username, but with white spaces inserted before and/or after the username. e.g: `"admin "`
2. Request a password reset with your malicious username.
3. Use the token sent to your email and reset the victim password.
4. Connect to the victim account with the new password.

The platform CTFd was vulnerable to this attack. 
See: [CVE-2020-7245](https://nvd.nist.gov/vuln/detail/CVE-2020-7245)

##### Account takeover due to unicode normalization issue
- Victim account: `demo@gmail.com`
- Attacker account: `demⓞ@gmail.com`

##### Pre-Auth account takeover
- 