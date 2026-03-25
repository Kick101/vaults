#### Policies
__AWS__
- aws.amazon.com/security/penetration-testing
__Azure__
- docs.microsoft.com/en-us/azure/security/azure-security-pen-testing
- microsoft.com/en-us/msrc/pentest-rules-of-engagement?rtc=1


  

---
### Methodology
#### SANS Cloud Penetration Testing
- Recon / Data Collection
- Data Analysis for Discovery
- Scanning / Mapping
- Vulnerability / Discovery
- Exploitation / Elevation
- Post Exploit Recon
- Persistent / Pivot
- Lessons Learned / Remediation

#### Recon

__Nmap NSE__
```sh
nmap -A -p- --script=couch*, docker*, http*, mongo*, redis*, voldemort*, memcached* -iL IPs.txt
```

---
### AWS

---
### Azure
