## Protocols & Ports

|Protocol|Port|Purpose|Encryption Type|Notes|
|---|---|---|---|---|
|**SMTP**|25|Mail transfer (server to server)|Optional STARTTLS|Often blocked by ISPs for spam prevention|
|**SMTP**|465|Mail submission (client to server)|Implicit SSL/TLS|Deprecated but still widely used|
|**SMTP**|587|Mail submission (client to server)|**STARTTLS (Recommended)**|Modern standard for sending email|
|**IMAP**|143|Email retrieval (client to server)|STARTTLS|Upgrades to encrypted connection|
|**IMAP**|993|Email retrieval (client to server)|Implicit SSL/TLS|Always encrypted|
|**POP3**|110|Email retrieval (client to server)|STARTTLS|Upgrades to encrypted connection|
|**POP3**|995|Email retrieval (client to server)|Implicit SSL/TLS|Always encrypted|

> ##### -> **STARTTLS (Explicit TLS):** Starts with an unencrypted connection and upgrades to encrypted using the STARTTLS command.
>    
>##### **-> Implicit SSL/TLS:** Encryption is negotiated immediately upon connecting to the server.

---
## Analyze Email headers

> ##### It is important to know that when reading an email header every line can be forged, so only the **Received:** lines that are created by your service or computer should be completely trusted.

| **Header Field**  | **Description**                                                                                                                                          |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **From**          | Displays who the message is from; however, this can be easily forged and is often the least reliable.                                                    |
| **To**            | Shows to whom the message was addressed; may not include the recipient's actual address.                                                                 |
| **Return-Path**   | The email address for return mail; same as "Reply-To:".                                                                                                  |
| **Envelope-To**   | Shows that the email was delivered to the mailbox of the recipient (e.g., user@example.com).                                                             |
| **Delivery Date** | The date and time when the email was received by your mail service or email client.                                                                      |
| **Received**      | The most important and reliable part of the email header. It shows the servers the message passed through, best read from bottom to top.                 |
| **Message-ID**    | A unique string assigned by the mail system when the message is first created. Can be forged.                                                            |
| **MIME-Version**  | Indicates the use of MIME, which extends email format standards. More info: https://web.archive.org/web/20221219232959/http://en.wikipedia.org/wiki/MIME |
| **Content-Type**  | Specifies the format of the message, such as HTML or plain text.                                                                                         |
| **X-Spam-Status** | Displays a spam score given by your service or email client.                                                                                             |
| **X-Spam-Level**  | Also shows a spam score, usually as a visual scale (e.g., using asterisks).                                                                              |
| **Message Body**  | The actual written content of the email from the sender.                                                                                                 |


### Received
- The received is the most important part of the email header and is usually the most reliable. They form a list of all the servers/computers through which the message traveled in order to reach you.
- The received lines are ***best read from bottom to top.*** That is, the first "Received:" line is your own system or mail server. The last "Received:" line is where the mail originated. Each mail system has their own style of "Received:" line. A "Received:" line typically identifies the machine that received the mail and the machine from which the mail was received.

### Finding the Original Sender

The easiest way for finding the original sender is by looking for the **X-Originating-IP** header. This header is important since it tells you the IP address of the computer that had sent the email. If you cannot find the **X-Originating-IP** header, then you will have to sift through the **Received** headers to find the sender's IP address.

Once the email sender's IP address is found, you can search for it at [**http://www.arin.net/**](https://web.archive.org/web/20221219232959/http://www.arin.net/). You should now be given results letting you know to which ISP (Internet Service Provider) or webhost the IP address belongs. Now, if you are tracking a spam email, you can send a complaint to the owner of the originating IP address. Be sure to include all the headers of the email when filing a complaint.

