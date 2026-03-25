Let's install pfSense as a VM and learn about it.

## Objectives
- Learn to utilize pfSense to protect internal network.
- Services configuration: DHCP, DNS, SSH, VPN
- Firewall rules
- PFBlockerNG: GeoBlocking, DNSBL
- IDPS: snort testing

---
## Setup 
- Download & install `pfSense` on your hypervisor (I use **Virt-Manager**) from this video or any other but watch videos from *2025*: https://www.youtube.com/watch?v=Y-Dj8lHmXy8
- pfSense requires a WAN interface to be configured. The LAN interface can be skipped and configured later.

---
## Access Admin interface from WAN interface
- By default you *cannot access* admin interface from WAN subnet like from your host machine, for that quickest way is to disable firewall *temporarily*.
- I *ssh*-ed here, we will see later how to enable that.
- Command to disable firewall.
```bash
pfctl -d
```
- Then go to that WAN IP of the pfSense on the browser to access it.

![[Pasted image 20250522163757.png]]

---
## Enabling SSH Access

To manage pfSense remotely via SSH, you need to enable and configure the SSH service.
### Steps to Enable SSH

1. **Log in to the Web GUI**
   - **Default :** Username: `admin`, Password: `pfsense`

1. **Navigate to SSH Settings**

   - Go to: **System** → **Advanced** → **Admin Access**

![[Pasted image 20250522164811.png]]
- Scroll down to the **Secure Shell** section
- Check the box: **"Enable Secure Shell"**
![[Pasted image 20250522164933.png]]
- Scroll all the way and click "Save"

### Configure pfSense Firewall to Allow SSH
-  Go to: **Firewall** → **Rules** → **WAN**

![[Pasted image 20250522170126.png]]

- Click on "Add"
- Do as image suggests and click "Save", change *WAN to LAN* as per your preference. 

![[Pasted image 20250522170352.png]]
- Click on "**Apply Changes**". Most important one, I was troubleshooting without applying changes for some time :)

![[Pasted image 20250522170743.png]]

4. **Access via Terminal**
- Type your *admin password* when prompted.

```bash
ssh admin@<pfSense-IP>
```

### SSH via Key
If you want to configure ssh key then follow this, mind you, you still need to enable SSH in firewall as shown above.
Video suggested: https://www.youtube.com/watch?v=oakOE2iDkhU

#### Create SSH keys in Linux
```bash
ssh-keygen -t ed25519 -C "admin@pfSense.local"
```

![[Pasted image 20250522172911.png]]

#### Configure pfSense with SSH key
- Go to: **1. System -> User Manager -> Users**
- Click on "Pencil" icon

![[Pasted image 20250522173054.png]]

- Scroll down to : **Keys** 
- Paste your **Public Key** here

![[Pasted image 20250522173148.png]]

#### Access via terminal
```bash
ssh -i .ssh/id_ed25519 admin@$IP
```


---
## DHCP Configuration

- Go to: **Services -> DHCP Server**
- Choose which interface you want to configure DHCP, below image is showing LAN config.
- Specify **Address Pool Range** excluding the IP which the pfSense configured on.
- Scroll down and click "Save".

![[Pasted image 20250523140441.png]]

- You can check DHCP leases here: **Status -> DHCP Leases**
	
![[Pasted image 20250523141733.png]]

---
## DNS

### Set DNS Servers

- Go to:  **System** > **General Setup**
- Scroll to the **DNS Server Settings** section.
- Enter your preferred DNS servers ( `1.1.1.1`, `8.8.8.8`).
- Also change **DNS Resolution Behaivor** as you need.
	- **Default:** Uses local DNS server on the pfSense and fallback to remote servers.
- Check the box **"Allow DNS server list to be overridden by DHCP/PPP on WAN ..."** if you want WAN-provided DNS. Otherwise, ***uncheck*** it to use your own *configured* DNS servers.
- Click **Save**.

![[Pasted image 20250523081256.png]]


### DNS Forwarder[¶](https://docs.netgate.com/pfsense/en/latest/services/dns/forwarder.html#dns-forwarder "Link to this heading")

- The DNS Forwarder in pfSense® software utilizes the `dnsmasq` daemon, which is a caching DNS forwarder.
- Unlike the DNS Resolver, the DNS Forwarder can only act in a forwarding role as it does not support acting as a resolver.
- The DNS Forwarder uses [DNS Servers](https://docs.netgate.com/pfsense/en/latest/config/general.html#general-dns-servers) configured at **System > General Setup** and those obtained automatically from an ISP for dynamically configured WAN interfaces (DHCP, PPPoE, etc).
- This service is disabled by default. The [DNS Resolver](https://docs.netgate.com/pfsense/en/latest/services/dns/resolver.html) (`unbound`) is the default DNS service.
- Use *DNS Resolver* -- it supports both forwarding and resolving.

### DNS Resolver[¶](https://docs.netgate.com/pfsense/en/latest/services/dns/resolver.html#dns-resolver "Link to this heading")
The DNS Resolver in pfSense® software utilizes `unbound`, which is a validating, recursive, caching DNS resolver that *supports DNSSEC, DNS over TLS*, and a wide variety of options. It can act in either a DNS resolver or forwarder role.

- Go to: **Services -> DNS Resolver -> General Settings**
- Enable options are seen in the below image.
- Enable **SSL/TLS** for encrypted DNS queries from *local clients* like LAN.
- Use appropriate **Outgoing Network Interfaces** like WAN to resolve DNS quires rather than sending request to all the interfaces.
	1.  Imagine Unbound (not restricted) sends a DNS query **out through LAN**, where clients also use **pfSense as their DNS**.
		- The request could circle back into Unbound — a **loop**.
		- This can cause: *Timeouts, Failed resolutions, High CPU usage*
	2. Sending DNS out through the organization's `GUEST` network (public Wi-Fi) could:
		- **Leak DNS** into untrusted networks
		- Be sniffed or redirected
		- Allow MITM attacks if **DNSSEC** isn’t used

![[Pasted image 20250523100306.png]]

#### DNS over TLS (DoT)
- DoT requires **Forwarding Mode** and **SSL/TLS outgoing** enabled on DNS Resolver.

![[Pasted image 20250523104340.png]]

> pfSense (Unbound) encrypts DNS queries **to an upstream DNS provider** like Cloudflare, Google, or Quad9 — over **port 853**.


- But **Unbound's native recursive resolution** (non-forwarding mode) means:

> pfSense contacts **root DNS servers directly** (e.g: `.`, `.com`, `google.com` servers) — and **they do not support DNS over TLS**.

#### DNS over HTTPS (DoH)
- **DNS over HTTPS (DoH)** is a protocol that encrypts DNS queries using **HTTPS (port 443)** instead of plain DNS (port 53) or DNS over TLS (DoT).
- **pfSense does not natively support DoH** in the DNS Resolver (Unbound) or DNS Forwarder (dnsmasq).
- To use DoH **on pfSense**, you need to run a **local DoH proxy** that talks to an upstream *DoH provider* (like Cloudflare or Google).

#### Domain Name System Security Extensions (DNSSEC)

DNSSEC adds **authentication** to DNS. It **does not encrypt** DNS queries (like DoT or DoH) — instead, it ensures that the DNS response:
- ✅ **Was not tampered with**
- ✅ **Came from the legitimate DNS source**
- ❌ **Does not provide privacy**

__How it works__
- DNS records are **digitally signed** by domain owners.
- DNS resolvers (like pfSense’s Unbound) **validate** the signature.
- If the signature is invalid or missing, the DNS query **fails**, protecting you from forged or poisoned responses. Prevents **spoofing**.

### Testing configurations
#### Testing DNSSEC
- Run this command from one of the LAN machines set up with pfSense.
- `10.10.10.1` - is my pfSense LAN IP/interface.
```bash
dig  @10.10.10.1 +dnssec dnssec-failed.org
```
- Output should be like this. Notice `status: SERVFAIL`.
- Check with a domain that has DNSSEC enabled like: cloudflare.com

```txt
; <<>> DiG 9.18.37 <<>> dnssec-failed.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 3270
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1432
;; QUESTION SECTION:
;dnssec-failed.org.             IN      A

;; Query time: 2520 msec
;; SERVER: 10.10.10.1#53(10.10.10.1) (UDP)
;; WHEN: Fri May 23 11:37:55 IST 2025
;; MSG SIZE  rcvd: 46
```

### Testing DoT [¶](https://docs.netgate.com/pfsense/en/latest/recipes/dns-over-tls.html#testing-dns-over-tls "Link to this heading")
**From the docs:** [¶](https://docs.netgate.com/pfsense/en/latest/recipes/dns-over-tls.html#enable-dns-over-tls-for-forwarded-queries "Link to this heading")
>**DNSSEC** is not generally compatible with forwarding mode, with or without DNS over TLS. 


- Change **DNS Resolution Behavior** to "*Use local DNS, fallback to DNS Servers*". This seems to work in my rigorous testing.
- Test via **Diagnostics > DNS Lookup**
- Check for states using port `853` going in **Diagnostics -> States -> States**
- Refer to the image below, you should be good.


![[Pasted image 20250523135432.png]]

---
## PFBlockerNG: GeoBlocking, DNSBL

#### How does it work
__IP-Based Blocking__
- Downloads **lists of IP addresses** known for malicious activity (malware, bots, spam, etc.), or from specific countries (**GeoIP**).
- Creates **aliases** (groups of IPs).
- Uses **pfSense firewall rules** to block or reject traffic **to/from** those IPs.

**DNSBL**
- Runs a **local DNS resolver** with **unbound** (built into pfSense).
- *Intercepts* DNS queries for known ad/tracker/malware domains.
- Returns a **fake IP (like 10.10.2.1, the IP you configured for Virtual IP)** instead of the real IP.
- This "blackholes" the domain — no connection can be made.
- __Example__
	- our device asks: “What’s the IP of `ads.evilsite.com`?”
	- pfBlockerNG responds: “It's `10.10.10.1`.” (a dead end)
	- So the ad never loads, and no malicious content gets through.

### Configuration

**DNSBL (DNS Blackhole List — blocks ads, malware, trackers)**

- Go to : **Firewall -> pfBlockerNG -> DNSBL**
- Check **Enable DNSBL**.
- Set **Listen Interfaces** for outbound and inbound on **PFBlockerNG -> IP** :  **IP Interface/Rules Configuration** section
- Check **Permit Firewall Rules** (to auto-create rules to access **DNSBL Webserver (block page)**.).
- **Global Logging/Blocking Mode:**
	- *No Global mode*: Logging & blocking depends per `block list` settings
	- *DNSBL mode*: Domains are sinkholed to the DNSBL VIP(Virtual IP) and logged.
	- *Null blocking (no logging)*: **'0.0.0.0'** will be used instead of the DNSBL VIP and not logged.
- Save settings and reload in **Update** tab.

__IP Blocking__
- Go to : **pfBlockerNG -> IP**
- **ASN Configuration:** Enable *ASN Reporting* for ASN of the logged IP and populate *ASN IPinfo Token* with it's token.
- **MaxMind GeoIP configuration:** Populate its fields for *GeoBlocking*
- **Kill States:** After each update, this **removes any active connections** involving blocked IPs.

__GeoBlocking__
Video suggested: https://www.youtube.com/watch?v=q1X0K-wzlTg&t=106s

1. Go to : **IP -> GeoIP** 
2. Configure *action* of the preferred continent.
3. Click pencil icon on one of the listed continent like: Asia. 
4. Select countries with `ctrl + click`. Example: China, Pakistan, Turkiye.
![[Pasted image 20250524211455.png]]
5. Configure the *List Action*, and "Save" and reload.

> Here pakistan.gov.pk website is blocked.

![[Pasted image 20250525004924.png]]

> ⚠️ Note: Requires **MaxMind GeoIP key** (free with account).


**Threat Intel Feeds**
1. Go to **Feeds** tab.
2. Check feeds like:
    - `easylist`, `adaway`, `malwaredomains`, etc.
3. Click *plus icon* to add, then go to **Update** tab and run **Force Reload - All**
4. ping/curl an IP from one of the IPs from feeds that are active. 
5. And check alerts generated in **Reports -> Alerts**
6. We can see **ASN** info as well that I configured in **pfBlockerNG -> IP** with https://ipinfo.io/ API.

![[Pasted image 20250524144316.png]]

---
## VPN: OpenVPN

Video suggested: https://www.youtube.com/watch?v=I61t7aoGC2Q&t=148s

We need to configure these: **Certificate Authority** to sign certificates, **Server Certificate** (used by OpenVPN) to prove its identity, and the **client certificate**. It's better to view walk-through videos to configure these than step by step instructions. 

### 🔄 How OpenVPN Works 

1. **Startup**
    - Server starts and listens on port 1194.
    - Client starts and connects to server.
2. **(Optional) TLS Auth Check**
    - Before any TLS handshake is accepted, server checks HMAC on packet using `ta.key`.
3. **TLS Handshake**
    - Client and server exchange certificates.
    - Each verifies the other's certificate via the CA.
    - TLS keys are negotiated.
4. **Key Exchange**
    - A secure session is established using TLS with DH or ECDH.
    - A shared symmetric key is derived for encryption.
5. **VPN Tunnel Created**
    - A virtual interface (`tun0`) is created on both ends.
    - All traffic is encrypted and routed through this interface.
6. **Data Encryption**
    - All traffic between client and server is encrypted with the symmetric key.
7. **Client Receives IP**
    - Server assigns an internal VPN IP to the client (e.g., 10.10.3.2).
    - Client can access internal services or route internet traffic via VPN.


### OpenVPN Client Export Utility

- Install this from **Syatem -> Package Manager -> Available Packages → Search for `openvpn-client-export`**
- Configure **Host Name Resolution** to public IP for remote access over public network.
- Create a VPN user here: **System -> User Manager -> Users**
- Add user certificate: **Create Certificate for User** section. And "Save".

![[Pasted image 20250526120732.png]]
- Go to: **VPN -> OpenVPN -> Client Export Utility**
- The user you created should be visible here. Export the desired option.

![[Pasted image 20250526121353.png]]

**The exported `.ovpn` config includes:**

- The **client certificate**    
- The **client private key**
- The **CA certificate** (so the client can verify the server)
- Optionally, the **TLS key** (if used)

### Test VPN

- Image should be self explanatory. 

![[Pasted image 20250526123625.png]]

### Security Tips

- Use **4096-bit keys** for long-term security
- Disable *compression* (can lead to *VORACLE* attacks)
- Use **TLS-Crypt** instead of TLS-Auth for additional privacy
- Restrict users with firewall rules or user groups

---
## IDPS: Snort

Video suggested: https://www.youtube.com/watch?v=SapAcfHbQSE&t=192s

### Test snort
- Custom rule
```snort
alert tcp any any -> any 80 (msg:"TEST ALERT - Visit to example.com"; content:"example.com"; http_header; sid:1000001; rev:1;)
```

- curl from one of the LAN machines:
```bash
curl http://example.com
```

- Go to: **Services -> Snort -> Alerts**

![[Pasted image 20250527131729.png]]


---
## Proxy


---
## Reverse Proxy

---
## Secure Admin Interface


---
## VLAN

---
## Load Balancing


---
## Captive Portal

---
## CARP


---








