[Free e-mail spoofer](https://emkei.cz/)
[E-mail Spoof test](https://caniphish.com/free-phishing-tools/email-spoofing-test)

---
## SPF (Sender Policy Framework)
SPF is a `DNS TXT` record that __lists all the servers (IPs) authorized to send emails from a particular domain__.
- SPF checks whether the **sending server (IP address)** is authorized to send emails on behalf of the domain used in the **envelope sender (MAIL FROM)**.  
- It does **not** verify the person or the visible `From:` address.

Any IP can send mail behalf of their domain.

```txt
v=spf1 +all
```


>🔒 **Enforcement** of SPF results (e.g., reject/quarantine) requires a DMARC policy — SPF by itself is a passive check.

---
## DKIM (DomainKeys Identified Mail)
DKIM is an email authentication method that **adds a digital signature to outgoing emails** to verify that:
- The message **wasn't altered in transit**, and
- It was **sent by a domain authorized to send it** (via cryptographic proof).

### 🔧 How it works:

1. The sending mail server **digitally signs** parts of the email (headers, body hash) using a **private key**.
2. The receiving mail server retrieves the **public key** from the sender's domain’s `DNS TXT` record (e.g., `default._domainkey.example.com`).    
3. It uses that public key to **verify the signature**.
	- If the contents were modified, or the signature is forged, verification fails.
4. **DKIM signs the email** on behalf of the sending domain, not per user.
	- Signature is stored in the header: `DKIM-Signature`

```txt
DKIM-Signature: v=1; a=rsa-sha256; d=example.com; s=default; 
 h=from:subject:date:to:message-id;
 bh=HASHED_BODY;
 b=SIGNATURE;
```

---
## DMARC (Domain-based Message Authentication, Reporting, and Conformance)

>The idea behind DMARC is to reject emails that 'pretend' to originate from your organization

- DMARC uses SPF and DKIM and gives **policies** on how to deal with error cases. 
- DMARC DNS record contains instructions on what to do when SPF / DKIM test failed. 
- Emails that failed can either be rejected or quarantined. If that happens, DMARC can be configured to send a report back.

| Mechanism | What it checks                                 | What DMARC checks for                    |
| --------- | ---------------------------------------------- | ---------------------------------------- |
| **SPF**   | IP ↔ `MAIL FROM` domain match                  | Does `MAIL FROM domain == From: domain`? |
| **DKIM**  | Valid cryptographic signature from `d= domain` | Does `d= domain == From: domain`?        |


---
## Spoof email using Python's `smtplib`

__On Ubuntu, install sendmail__

```
sudo apt install sendmail  
```

```python
import smtplib  
from email.message import EmailMessage

msg = EmailMessage()

msg.set_content("You've been a good boy")
msg["Subject"] = "Ho-ho-ho"

# The fake sender address
msg["From"] = "[santa.clause@christm.as](mailto:santa.clause@christm.as)"

# The actual receiver
msg["To"] = "victim@example.com"

# The attacker's address
msg.add_header("reply-to", "[phishy@phising.com](mailto:phishy@phising.com)")

# Send the message via our own SMTP server.  

s = smtplib.SMTP("localhost")  
s.send_message(msg)  
s.quit()
```

