- What is AD?
- What is the need of it?
- What are the components of the AD?
- What are steps of AD testing?
- What are the common easy Attacks on AD?

---

**1. What is AD?**
   - Active Directory (AD) is a directory service developed by Microsoft for Windows domain networks. 
   - It is used to store and organize information about network resources (such as computers, users, and printers) and to manage access to these resources. 
   - Active Directory provides a centralized and standardized way to manage and authenticate users and devices in a network.

**2. What is the need for AD?**
   - Active Directory serves several key purposes:
     - **Centralized Authentication:** It allows users to log in with a single set of credentials across the network.
     - **Authorization:** It controls access to network resources based on user permissions.
     - **Management:** It simplifies the management of network resources by providing a centralized database of information.
     - **Security:** It enforces policies and helps secure the network by managing user access and permissions.

**3. Components of AD:**
   - Key components of Active Directory include:
     - **Domains:** Logical groupings of objects within a network.

![[Pasted image 20240102214958.png]]

 - __Forests:__  is the union of several trees with different namespaces into the same network.
![[Pasted image 20240102214936.png]]


 - __Trees:__ is a group of domains that share a contiguous namespace and a common schema. Ex: gitam.local -> vizag.gitam.local & hyd.gitam.local
     - __Trust:__ If Domain A trusts Domain B then user from Domain B can access resources in Domain A
     - **Domain Controllers:** Servers that manage security authentication requests.
     - **Organizational Units (OUs):** Containers for organizing and managing resources within a domain.
     - **Users, Groups, and Computers:** Objects representing people, groups, and devices in the network.
     - **Group Policy Objects (GPOs):** Policies that define settings and configurations for users and computers.

**4. AD Testing Methodology:**
![[Pasted image 20240102214330.png]]


**5. Common Attacks on AD:**
 - **Pass-the-Hash (PtH) Attacks:** Exploiting hashed password values.
 - **Pass-the-Ticket (PtT) Attacks:** Exploiting Kerberos tickets.
 - **Brute Force Attacks:** Attempting to guess passwords.
 - **Golden Ticket Attacks:** Forging Kerberos tickets with extended privileges.
 - **Credential Stuffing:** Using known username/password pairs from other breaches.



---






