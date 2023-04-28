
### Source Code Viewer
__Challenge Question__
During one of our pentest engagement, we have found a client's web app where you can check source code of other websites. However, we're unable to exploit it further. Can you help us? P.S: As a proof of concept, the client is asking us to get the password of their employee "tuhin1729" & wrap it inside BugBase{}. For example, if the password is "1234567890", the flag will be BugBase{1234567890}

_URL:_ http://165.232.190.5:8081/

__Hints__
1.  They said he is using a very weak password.

#### Notes
`Server: Werkzeug/2.3.0 Python/3.8.16`
- Seems like `curl` is used
- Possible attacks: LFI, Command Injection, SSRF

__Test LFI__
- HTTP/HTTPS protocol is appended by default
- file, ftp is blocked


---
