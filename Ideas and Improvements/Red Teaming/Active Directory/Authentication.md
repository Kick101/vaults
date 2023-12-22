### Kerberos Authentication
![[Pasted image 20230226153146.png | 500]]

- User sends username & a timestamp encrypted using a _key derived from their password_ to the **KDC**, installed on the Domain Controller.
- KDC sends back a **TGT** which contains a copy of session key encrypted with __krbtgt__ password hash and a copy of **Session Key** is encrypted with client's secret key. 
- The need for a ticket to get more tickets allows users to request service tickets without passing their credentials every time they want to connect to a service. 

![[Pasted image 20230226153809.png | 500]]

- User needs a **TGS** to connect to a service
- To _request a TGS_, the user will send their username and a timestamp _encrypted using the Session Key_, along with the TGT and a __Service Principal Name__ 
- KDC will _send us a TGS_ along with a **Service Session Key**. _TGS is encrypted_ using a key derived from the **Service Owner Hash**

![[Pasted image 20230226162607.png | 500]]

- The TGS can then be sent to the service to authenticate. The service will use its configured account's password hash to _decrypt the TGS_ and validate the Service Session Key.

#### Terminology
-   **Ticket Granting Ticket (TGT)** - A ticket-granting ticket is an authentication ticket used to request service tickets from the TGS for specific resources from the domain.
-   **Key Distribution Center (KDC)** - The Key Distribution Center is a service for issuing TGTs and service tickets that consist of the Authentication Service and the Ticket Granting Service.
-   **Authentication Service (AS)** - The Authentication Service issues TGTs to be used by the TGS in the domain to request access to other machines and service tickets.
-   **Service Principal Name (SPN)** - A Service Principal Name is an identifier given to a service instance to associate a service instance with a domain service account. Windows requires that services have a domain service account which is why a service needs an SPN set.
-   **KDC Long Term Secret Key (KDC LT Key)** - The KDC key is based on the KRBTGT service account. It is used to encrypt the TGT and sign the PAC.
-   **Client Long Term Secret Key (Client LT Key)** - The client key is based on the computer or service account. It is used to check the encrypted timestamp and encrypt the session key.
-   **Service Long Term Secret Key (Service LT Key)** - The service key is based on the service account. It is used to encrypt the service portion of the service ticket and sign the PAC.
-   **Session Key** - Issued by the KDC when a TGT is issued. The user will provide the session key to the KDC along with the TGT when requesting a service ticket.
-   **Privilege Attribute Certificate (PAC)** - The PAC holds all of the user's relevant information, it is sent along with the TGT to the KDC to be signed by the Target LT Key and the KDC LT Key in order to validate the user.
- __krbtgt__ kerberos ticket granting ticket
- __TGS__ are tickets that allow connection only to the specific service they were created for. It contains a copy of the Service Session Key on its encrypted contents so that the Service Owner can access it by decrypting the TGS.
- __Service Owner__ is the user or machine account that the service runs under.

#### Kerberos Double Hop Problem
we have three hosts: `Attack host` --> `DEV01` --> `DC01`. Our Attack Host is a Parrot box within the corporate network but not joined to the domain. We obtain a set of credentials for a domain user and find that they are part of the `Remote Management Users` group on DEV01. We want to use `PowerView` to enumerate the domain, which requires communication with the Domain Controller, DC01.

![[Pasted image 20231223003942.png]]

When we connect to `DEV01` using a tool such as `evil-winrm`, we connect with network authentication, so our credentials are not stored in memory and, therefore, will not be present on the system to authenticate to other resources on behalf of our user. When we load a tool such as `PowerView` and attempt to query Active Directory, Kerberos has no way of telling the DC that our user can access resources in the domain. This happens because the user's Kerberos TGT (Ticket Granting Ticket) ticket is not sent to the remote session; therefore, the user has no way to prove their identity, and commands will no longer be run in this user's context. In other words, when authenticating to the target host, the user's ticket-granting service (TGS) ticket is sent to the remote service, which allows command execution, but the user's TGT ticket is not sent. When the user attempts to access subsequent resources in the domain, their TGT will not be present in the request, so the remote service will have no way to prove that the authentication attempt is valid, and we will be denied access to the remote service.

#### Workaround #1: PSCredential Object
- Create a [[Persistence#]]
```powershell
get-domainuser -spn -credential $Cred | select samaccountname
```



---
### NTLM Authentication

![[Pasted image 20230226162802.png | 500]]

#### NTLM Pass-Through Authentication - Used to get access from a server
1.  The client _sends an authentication request_ to the server they want to access.
2.  The _server generates & sends a random 16-byte number_ as a challenge to the client.
3.  The client combines their NTLM password hash with the challenge (and other known data) to _generate a response and sends_ it back to the server for verification.
4.  The _server forwards the challenge & the response to the DC_ for verification.
5.  The DC uses the challenge to recalculate the response and compares it to the original response sent by the client. The authentication result is sent back to the server.
6.  The server forwards the authentication result to the client.

#### NTLM Authentication - Used to authenticate with the DC
- The client types the username and password into the logon window.
- Windows _generates a hash_ for the password.
- The client computer _sends a login request_ along with a domain name to the domain controller.
- The domain controller generates a _16-byte random character string called a "nonce,"_ which it sends to the client computer.
- The client computer _encrypts the nonce with a hash_ of the user password and sends it back to the domain controller.
- The domain controller retrieves the hash of the user password from the SAM and uses it to encrypt the nonce. The domain controller then _compares the encrypted value with the value received from the client_. A matching value authenticates the client, and the logon is successful.

#### Challenge Response Value Calculation
__NTLMv1__
1.  The user's password is converted from Unicode to the system's default code page.
2.  The password is padded with null characters to a length of 14 bytes.
3.  The password is split into two 7-byte halves.    
4.  Each half is used to create an MD4 hash value.    
5.  The two hash values are concatenated into a 16-byte value.    
6.  This 16-byte value is used as the _"LM hash"_ value in the response calculation.    
7.  The challenge value provided by the server is concatenated with the user's username.    
8.  This concatenated value is used to create an MD4 hash value.    
9.  This hash value is used as the _"NT hash"_ value in the response calculation.    
10.  The two hash values (LM hash and NT hash) are concatenated into a 24-byte value.    
11.  This 24-byte value is encrypted using the RC4 algorithm, using the challenge value provided by the server as the encryption key.    
12.  The resulting encrypted value is the response value that is sent back to the server.

__NTLMv2__
1.  The user's password is used to create an NT hash value, as in step 9 of the NTLMv1 calculation.
2.  The challenge value provided by the server is concatenated with the user's username and the domain name.
3.  This concatenated value is used to create a message authentication code (MAC) using the HMAC-SHA256 algorithm.
4.  The resulting MAC value is concatenated with the user's username and the domain name.
5.  This concatenated value is encrypted using the RC4 algorithm, using the NT hash value as the encryption key.
6.  The resulting encrypted value is the response value that is sent back to the server.

---
### LDAP
>`LDAP` is an open-source and cross-platform protocol used for authentication against various directory services

![[Pasted image 20230304023824.png]]

#### LDAP Authentication
1.  **Simple Authentication:** This includes anonymous authentication, unauthenticated authentication, and username/password authentication. Simple authentication means that a _username and password create a BIND request_ to authenticate to the LDAP server.

2.  **SASL Authentication:** The [Simple Authentication and Security Layer (SASL)](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) framework uses other authentication services, such as Kerberos, to bind to the `LDAP` server and then uses this authentication service (Kerberos in this example) to authenticate to `LDAP`. The `LDAP` server uses the `LDAP` protocol to send an `LDAP` message to the authorization service which initiates a series of challenge/response messages resulting in either successful or unsuccessful authentication. SASL can provide further security due to the separation of authentication methods from application protocols.
