# Root Squashing
Feature in [[2049 - NFS Service]] that prevents remote **root** users on client machines from having the same level of permissions (root) on the NFS server.
This is done by mapping the remote users to an unprivileged account such as `nobody` or `nfsnobody` with *UID 65534*

## TLDR
Basically, don't carry system privileges over to NFS and only give the low privileged accounts.


# How run0 and the polkit can be used as a more secure alternative to sudo on Linux platforms.

## run0
Alternative to `sudo` as it does not use SUID binaries. 
It allows users to execute commands with elevated **(root)** privileges by creating a new, isolated `systemd` service, avoiding the security risks associated with `setuid` binaries. 
`run0` uses `polkit` for authentication and changes terminal colours to highlight privileged sessions.

### TLDR
It does not run commands as **root**, but gives it to a `systemd` service, which already has high privileges. It uses `polkit` for authentication

## polkit
Polkit (PolicyKit) manages system-wide privileges allowing unprivileged processes to communicate with those of higher level.
The `polkit` daemon runs as **root** to be able to mediate privileges.

### TLDR
High level *daemon* that manages privileges between processes with varying levels of privileges.

## Answer
`run0` runs commands in an isolated environment, handing the commands to services, using `polkit` for authentication.
It is more secure because it does not use SUID bits to grant privileges. Which would open up a larger attack surface.
The isolated environment does not inherit anything from the user session, hence preventing attackers to manipulate user sessions.

# What is DMARC (Domain-based Message Authentication, Reporting, and Conformance)?

## SPF (Sender Policy Framework)
Email authentication protocol that protects against spoofing and phishing by listing authorised IPs or domains permnitted.
It uses a DNS TXT record to allow receiving mail servers to verify if an incoming email comes from a trusted source

## DKIM (DomainKeys Identified Mail)
It is is an email authentication protocol that adds a digital signature to outgoing emails, allowing receiving servers to verify that the message was truly sent from the domain and was not altered in transit. It uses public-key cryptography to prevent email spoofing and phishing. 

## Answer
It is an email authentication protocol, that helps protect email domains from spoofing, phishing, and other types of abuse.
DMARC builds on two existing protocols, **SPF (Sender Policy Framework)** and **DKIM (DomainKeys Identified Mail)**, to verify the identity of the sender and the integrity of the message.


# What are the security issues with the FTP protocol?
It transmits data in plain-text.
This makes it vulnerable to attacks such as packet sniffing and Man-in-the-Middle (MitM). It would also allow unauthorised access.
Safer alternatives are SFTP or FTPS.


# What is Kerberos?
**Kerberos** is a network authentication protocol designed to provide secure user authentication over insecure networks. 
Kerberos uses tickets to allow nodes to prove their identity over non-secure networks without transmitting passwords. The protocol involves a Key Distribution Center (KDC) that includes an Authentication Server (AS) and a Ticket Granting Server (TGS).

*For more info  read [[88 - Kerberos]]*


# Directory Traversal
[[Directory Traversal]] is a web security vulnerability allowing attackers to access unauthorised files, such as source code or system credentials, by manipulating file paths with `../` sequences. By tricking a server into traversing outside the root directory, attackers can read, modify, or delete sensitive data.
It can also be used in file uploads by using the sequences in the file name for example.

# What are the four types of Application Programming Interface (API)?
- **Public APIs** can be used by anyone to access a server’s data or other services from a client application. Common uses of public APIs include retrieving traffic and weather data and managing third-party login processes. Public APIs are generally intended to let any application use a service. Public APIs tend to be widely used, great care is taken not to change them unless absolutely necessary so as not to break the functionality of applications.

- **Private APIs** are developed for internal use only and aren’t widely published. Typically, private APIs let a vendor’s applications communicate with that vendor’s servers. For example, the banking application on your phone uses private APIs to access the unique services of your specific bank.

- **Partner APIs** are developed for use between specific organisations. The details of the API are disclosed to a limited set of partners. For example, a cloud database platform may agree to partner with a set number of analytics providers. 

- **Composite APIs** are chained together for a particular function and might be a combination of public, private, and partner APIs. An example of a chained API using public and private APIs is the integration between a weather app and a fitness tracker app. The weather app’s public API provides data such as temperature and humidity to the fitness tracker app. Its private API brings data on the owner’s pace and distance covered and combines it with environmental factors to calculate calories burned.

# How would you fingerprint a database server?
Fingerprinting a database server involves identifying the Database Management System (DBMS) type, version, and configuration by analysing network service banners, probing with specific SQL queries, and mapping open ports.
- Banner grabbing (netcat,telnet or openssl)
- Nmap scan (ports and services)
- TCP/IP Stack fingerprinting
- Use of automated tools
	- Sending malfomed requests

# Explain symmetrical and asymmetrical cryptography
**Symmetric** cryptography uses a single, shared secret key for both encryption and decryption, offering high speed for large datasets. 
**Asymmetric** cryptography uses a linked pair of public and private keys—one to encrypt and the other to decrypt—enhancing security and key distribution, but at a slower processing speed

## Symmetric
- Both parties have one shared key
- It is efficient for large data, but is hard to distribute securely, as if the key is stolen data is compromised
- Algorithms: AES, DES,ChaCha20
	- **AES:** The industry standard, commonly using 128 or 256-bit keys.
	- **DES**: Older, less secure
	- **ChaCha20**: High-speed stream cipher

## Asymmetric
- Uses public and private key
- **Public** key for encryption -- shared with everyone
- **Private** key for decryption -- secret
-  Algorithms: RSA,ECC,DH
	- **RSA (Rivest-Shamir-Adleman):** Widely used for key exchange and digital signatures.
	- **CC (Elliptic Curve Cryptography):** Provides high security with smaller key sizes, making it more efficient than RSA.
	- **Diffie-Hellman (DH):** Primarily used for key exchange.

## Childish example
If Claire, the sender, and Jacqueline, the recipient, want to continually send a confidential file back and forth to each other, Claire and Jacqueline will give their unique and respective public keys to each other. Claire will then use Jacqueline’s public key to encrypt the file, since it’s intended for Jacqueline only, and send the file to Jacqueline. Upon receipt of the file, Jacqueline will use her private key – keyword, “private,” meaning no one else other than Jacqueline knows it - to decrypt the file and access its contents. No one other than Jacqueline, not even Claire, can decrypt this file, because no one other than Jacqueline knows Jacqueline’s private key. The same process applies when Jacqueline wants to send the file back to Claire. Jacqueline ties it to Claire’s public key, and Claire uses her private key to decrypt the file.


# What is the difference between SQL and no SQL database vulnerabilities?
NoSQL Injection vulnerabilities often arise from issues within database libraries and how they handle user input. These libraries may not provide proper input validation and parameterisation, leaving room for exploitation.

| Aspect | SQL Injection | NoSQL Injection |
|------|------|------|
| **Query Structure** | Uses structured queries written in SQL (Structured Query Language). | Uses various query languages or sometimes no formal query language depending on the database. |
| **Injection Techniques** | Attackers manipulate SQL query structure to alter database commands. | Attackers manipulate unstructured data or NoSQL-specific query constructs. |
| **Query Parameters** | Exploits vulnerabilities in query parameters used within SQL statements. | Targets input data that is directly used in database queries or data manipulation operations. |
| **Library Vulnerabilities** | Often related to poorly sanitized SQL queries in application code. | Often arises from weaknesses in NoSQL database libraries and how they handle user input without proper validation or parameterization. |
| **Data Manipulation** | Frequently used to access, modify, or delete data in relational databases. | Attackers manipulate document structures or data objects, which may lead to data exposure or tampering. |


# What is the Open Vulnerability and Assessment Language (OVAL)?
The Open Vulnerability and Assessment Language (OVAL) is an international, community-standardised, XML-based language used to automate the assessment and reporting of a computer system's security state. It standardises how to check for vulnerabilities, configuration issues, patches, and malicious software across various security tools

Tools and services that use OVAL for the three steps of system assessment
1. Representing system information
2. Expressing specific machine states
3. Reporting the results of an assessment


# Explain the difference between Phishing, Smishing and Vishing and what risks do they pose?

- **Phishing (Email Phishing):** Delivered via email or fraudulent websites. These usually mimic legitimate businesses to get users to click links or download attachments.
- **Smishing (SMS Phishing):** Conducted via text messages or instant messaging apps (e.g., WhatsApp). Smishing often leverages the high open rates of text messages to send urgent alerts about bank accounts or deliveries.
- **Vishing (Voice Phishing):** Occurs over phone calls or voice messages (VoIP). Scammers use voice communication—often impersonating authorities or bank officials—to manipulate victims in real-time

| Type     | Medium     | Key Tactic                          | Example                                                     |
|----------|------------|--------------------------------------|-------------------------------------------------------------|
| Phishing | Email      | Spoofed email/website.               | "Action Required: Your Amazon account has been locked."     |
| Smishing | Text/SMS   | Urgent, short message with a link.   | "Your package is pending delivery. Click here to update your address." |
| Vishing  | Phone Call | Impersonation, "Live" pressure.      | "Hello, this is fraud detection. We need your PIN to secure your account." 

## Risks
- **Identity Theft:** Stolen personal details such as; addresses, full names, etc

- **Financial Loss:** Direct theft of money through unauthorised bank transfers, fraudulent bank transfers, or stolen credit card details.

- **Data Breaches:** Compromise of login credentials, including passwords, 2FA codes, or session tokens.

- **Malware and Ransomware Installation:** Links can install malicious software (adware/spyware) or ransomware that locks files until a ransom is paid.

- **Access to Corporate Networks:** Attackers can use stolen credentials to infiltrate company systems, leading to intellectual property theft or operational disruption (e.g., stopping operations).

- **"MFA Fatigue":** Repeated 2FA notifications aimed at wearing down the victim to approve a log-in, a rising tactic in 2026.

# What tools could you use to enumerate Domain Name System (DNS) for ZONE transfer vulnerabilities?
[[53 - DNS]] servers contain a *Zone* file that replicates the map of the domain.
Only the server itself should have access to it, but if it's misconfigured anyone can request the file and get the list of all the sub-domains.

- **DiG (Domain Information Groper)**: It is frequently used to test for zone transfer vulnerabilities by attempting an AXFR query. 
	- **Command:** `dig axfr @<nameserver> <domain>`
	- **Purpose:** Directly queries the authoritative DNS server to attempt a full zone transfer. 

 - **DNSRecon**:  Python script often used for comprehensive DNS enumeration. It can automatically check all NS records for zone transfers and supports multiple output formats. 
	- **Command:** `dnsrecon -d <domain> -t axfr`
	- **Purpose:** Automates AXFR tests against name servers for a specified domain. 

-  **Nmap (DNS Enumeration Scripts)**: Nmap includes specialized Network Scripting Engine (NSE) scripts to detect misconfigured DNS servers. 
	- **Command:** `nmap --script=dns-zone-transfer -p 53 <nameserver>`
	- **Purpose:** Attempts a zone transfer (AXFR) query during the Nmap scan. 

- **DNSenum**: is a versatile tool used for deep DNS enumeration. It can gather standard records, perform zone transfers, and conduct brute-force dictionary attacks to discover hidden subdomains. 
	- **Command:** `dnsenum <domain>` 

- **Nslookup**: While older, `nslookup` is a standard utility that can be used interactively to test for zone transfers by setting the query type to AXFR.
# Explain what a Security identifier (SID) is and the different parts of a SID.
A **Security Identifier (SID)** is a unique value used by the Microsoft Windows operating system to identify **security principals** such as users, groups, and computers. Instead of using the account name internally, Windows assigns each account a SID and uses that SID in permissions, access control lists (ACLs), and security auditing.

**Example**
```
S-1-5-21-3623811015-3361044348-30300820-1013
```

1. **SID Indicator & Revision (`S-1`)**
    - `S` indicates a SID.
    - `1` is the revision level of the SID format.
        
2. **Identifier Authority (`5`)**
    - Shows who issued the SID.
    - `5` represents **NT Authority** in Windows.
        
3. **Domain or Computer Identifier (`21-3623811015-3361044348-30300820`)**
    - Identifies the **domain or local machine** where the account exists.
        
4. **Relative Identifier – RID (`1013`)**
    - Identifies the **specific user or group** within that domain.
## Visual
```
S-1-5-21-3623811015-3361044348-30300820-1013
│ │ │ │                                  │
│ │ │ │                                  └── Relative Identifier (RID)
│ │ │ └──────────────────────────────────── Domain / Computer Identifier
│ │ └────────────────────────────────────── Identifier Authority
│ └──────────────────────────────────────── Revision Level
└────────────────────────────────────────── SID indicator
```


# What are the two IPsec encoding methods and what are the pros and cons of each?
**IPsec (Internet Protocol Security)** is a **set of protocols used to secure IP network communications** by **encrypting and authenticating IP packets** as they travel across a network such as the internet. It operates at the **Network Layer (Layer 3)** of the OSI model.

- **Tunnel mode**:  It encapsulates the entire IP packet, including its header, within a new packet with a new header. This mode is akin to a protective envelope, ensuring the contents remain confidential and unaltered during their journey across public networks.
	- The **entire original IP packet (header + payload)** is encrypted.
	- A **new outer IP header** is added.

- **Transport mode**: It  secures only the payload of the IP packet, leaving the header untouched. This approach is less about hiding and more about protecting the integrity and confidentiality of the message within.
	- Only the **payload (data)** of the IP packet is encrypted/authenticated.
	- The **original IP header remains visible**.

## Pros and Cons
| IPsec Mode | Pros | Cons |
|-------------|------|------|
| **Tunnel Mode** | - Entire packet (header + payload) is encrypted<br><br>- Better security and privacy since original IP addresses are hidden<br><br>- Ideal for VPNs and site-to-site connections | - Higher overhead because a new IP header is added<br><br>- Increased bandwidth usage<br><br>- Slightly slower performance due to extra processing |
| **Transport Mode** | - Lower overhead because only the payload is encrypted<br><br>- Faster performance due to less encryption processing<br><br>- Efficient for host-to-host communication | - Original IP header remains visible<br><br>- Less privacy because source and destination addresses are exposed<br><br>- Not suitable for gateway-to-gateway VPNs 


# In key signing do you use the private or public key for signing?
In **public-key cryptography**, a **digital signature is created using the private key** and **verified using the public key**.
This way only the owner has the **private key**, but the **public key is shared**, so everyone can confirm the signature without being able to forge it.

- Private key -- Signing
- Public Key -- Verify

# What ports need to be open on a domain controller (DC)?

| Port | Protocol | Service                  | Purpose                                     |
| ---- | -------- | ------------------------ | ------------------------------------------- |
| 53   | TCP/UDP  | DNS                      | Domain name resolution for Active Directory |
| 88   | TCP/UDP  | Kerberos                 | Authentication protocol                     |
| 135  | TCP      | RPC Endpoint Mapper      | RPC service discovery                       |
| 137  | UDP      | NetBIOS Name Service     | Legacy name resolution                      |
| 138  | UDP      | NetBIOS Datagram         | Legacy network browsing                     |
| 139  | TCP      | NetBIOS Session          | Legacy file/auth traffic                    |
| 389  | TCP/UDP  | LDAP                     | Directory queries                           |
| 445  | TCP      | SMB                      | File sharing, SYSVOL access                 |
| 464  | TCP/UDP  | Kerberos Password Change | Password updates                            |
| 636  | TCP      | LDAPS                    | Secure LDAP                                 |
| 3268 | TCP      | Global Catalog           | Forest-wide directory searches              |
| 3269 | TCP      | Global Catalog over SSL  | Secure GC queries                           |


# Explain the Simple Network Management Protocol (SNMP) protocol and any weaknesses in the protocol
**[[161, 162, 10161, 10162 - SNMP | SNMP]]** is a standard protocol used for **monitoring, managing, and configuring network devices** like routers, switches, servers, printers, and other networked hardware. It’s widely used in network management systems (NMS) to collect information about devices and sometimes to control them remotely.

## Key Components
1. **Managed Devices** – Devices on the network that support SNMP (e.g., routers, switches, servers).
2. **Agents** – Software on managed devices that **collect information** and **respond to SNMP requests**.
3. **Network Management System (NMS)** – Central software that **monitors and controls** devices using SNMP.
4. **Operations:** Involves commands like GET (request data), SET (change configuration), and TRAP/INFORM (agent alerts manager of events)

## How It Works
SNMP operates using a **manager-agent model**:
- The **manager** (NMS) requests information or sends commands.
- The **agent** responds with data about the device or executes the command.


## Versions
SNMP has three main versions:
1. **SNMPv1** – Original version; simple but lacks strong security.
2. **SNMPv2c** – Improved performance; added bulk retrieval but still uses weak security (community strings).
3. **SNMPv3** – Introduces **strong security features**: authentication, encryption, and access control.

## Common Vulnerabilities and Weaknesses
- **Plain Text Community Strings:** Versions 1 and 2c use clear-text "community strings" (analogous to passwords), allowing attackers to intercept them via sniffing.
- **Weak Authentication:** The lack of secure authentication in v1 and v2c makes them susceptible to unauthorised access.
- **Default Configurations:** Many devices default to "public" (read-only) or "private" (read/write) strings, enabling easy unauthorised access to network configurations.
- **Lack of Encryption:** Data transmitted in v1 and v2c can be read and manipulated in transit.
- **UDP Vulnerabilities:** Since it uses UDP, it is susceptible to packet loss and spoofing attacks

# What are the trade-offs between symmetrical and asymmetrical cryptography?
The trade-offs between symmetric and asymmetric cryptography centre on the balance between speed/efficiency and security/key management. 
**Symmetric** cryptography uses a single shared key for both encryption and decryption, making it fast but difficult to distribute securely. 
**Asymmetric** cryptography uses a public-private key pair, enhancing security and allowing for digital signatures, but at the cost of computational speed

| Factor                    | Symmetric Cryptography                           | Asymmetric Cryptography                               |
| ------------------------- | ------------------------------------------------ | ----------------------------------------------------- |
| **Key Usage**             | Same key for encryption & decryption             | Key pair: public key (encrypt), private key (decrypt) |
| **Speed**                 | Very fast                                        | Slower, computationally intensive                     |
| **Key Size for Security** | Short (e.g., 128–256 bits)                       | Long (e.g., 2048–4096 bits)                           |
| **Key Management**        | Difficult at scale; keys must be shared securely | Easier at scale; public key can be shared openly      |
| **Secure Key Exchange**   | Problematic                                      | Built-in; secure via public key                       |
| **Digital Signatures**    | Not supported                                    | Supported (authentication & non-repudiation)          |
| **Best Use Case**         | Bulk data encryption                             | Key exchange, authentication, small data encryption   |
| **Scalability**           | Poor for large networks (needs many keys)        | Good for large networks (one key pair per user)       |


# Explain some vulnerabilities in SSL (Secure Sockets Layer) and TLS (Transport Layer Security)?
SSL and TLS vulnerabilities often stem from outdated protocols (SSL 3.0), weak cipher suites, and improper configurations, allowing attackers to perform Man-in-the-Middle (MITM) attacks, data decryption, or session hijacking

| Protocol | Key Vulnerabilities                     | Notes / Impact |
|----------|----------------------------------------|----------------|
| SSL 2.0 / 3.0 | POODLE, weak ciphers, no forward secrecy, MITM risk | Outdated, insecure; should be disabled |
| TLS 1.0 / 1.1 | BEAST, CRIME, BREACH, weak ciphers, downgrade attacks | Older TLS versions; vulnerable to known attacks |
| TLS 1.2 / 1.3 | Fewer vulnerabilities, but misconfigurations can allow weak ciphers or downgrade attacks | Recommended; use strong ciphers and forward secrecy |

# Explain the purpose of a Remote Authentication Dial-In User Service (RADUIS) server
A **Remote Authentication Dial-In User Service** (RADIUS) server provides centralised authentication, authorisation, and accounting (AAA) management for users connecting to a network, such as Wi-Fi and VPNs. It enhances security by verifying user credentials against a central databases, like Active Directory before allowing network access

## **How it works**

1. A user/device connects to a **network access server (NAS)** (like a VPN, Wi-Fi access point, or switch).

2. The NAS forwards the credentials to the **RADIUS server**.

3. The RADIUS server checks the credentials against a **central database** (like LDAP, Active Directory, or a local database).

4. The server responds with:
    - **Access-Accept** → grant access
    - **Access-Reject** → deny access
    - **Access-Challenge** → request more info (e.g., multi-factor authentication)

# Where are passwords stored on server devices?
Passwords on server devices are typically stored in hashed and encrypted formats within specialised system database.

## Windows Server

- **Local Accounts:** Stored in the **Security Account Manager (SAM)** database, located at `%SystemRoot%\system32\config\SAM`.

- **Domain Accounts:** Stored in Active Directory, specifically in the `NTDS.dit` file on Domain Controllers, typically located at `%SystemRoot%\NTDS\NTDS.dit`.

- **Cached Credentials:** When a domain user logs in, the password hash is cached in the registry under `HKEY_LOCAL_MACHINE\SECURITY`.

- **Windows Vault/Credential Manager:** Encrypted credentials for apps, websites, and network resources are stored under `%Systemdrive%\Users[Username]\AppData\Local\Microsoft\Vault`.

- **Service Accounts:** Passwords for services are stored in the registry under `HKEY_LOCAL_MACHINE\SECURITY\Policy\Secrets`. 

## Linux Server
- **User Passwords:** Stored in the `/etc/shadow` file. This file is secured so that only the root user can read it.

- **Storage Method:** Passwords are never stored in plain text. They are hashed using algorithms like SHA512, bcrypt, or yescrypt, typically with a salt to prevent rainbow table attacks.



# Explain Structured Query Language injection (SQLi)

SQL Injection (SQLi) is a vulnerability where untrusted input is concatenated directly into a SQL query, allowing an attacker to manipulate the query's logic and break the boundary between data and code.
The attack exploits applications that use **string concatenation** to build queries by injecting SQL reserved characters (like `'`, `--`, or `;`).
If the code is vulnerable the attacker will be able to query data from the database, or even get an RCE vulnerability.





# If password hashes cannot be cracked can the hashes be used in any way and how would you do this?
Uncracked password hashes can still be used to impersonate users through a technique called **Pass-the-Hash (PtH)**. Attackers do not need the original password if they have the hash; they can inject it into authentication protocols to log in as that user, as the system validates the hash itself rather than the password.

## How to Use Uncracked Hashes (Pass-the-Hash):
- **Extract Hashes:** Attackers gain initial access to a system (e.g., Windows) and extract hashes from memory (LSASS) or the SAM database.

- **Authenticate Directly:** Using tools like Mimikatz, an attacker uses the hash as the credentials in protocols like NTLM or Kerberos, skipping the plaintext password entirely.

- **Lateral Movement:** This allows attackers to move between systems in a network without needing to break the encryption or hash strength

# Explain how a Cross-Site Request Forgery (CSRF) exploit works
[[CSRF - Cross Site Request Forgery]])** is an attack that forces an end user to execute (submit) unwanted actions on a web application in which they’re currently authenticated. With a little help of social engineering (such as sending a link via email or chat), an attacker may trick the users of a web application into executing actions of the attacker’s choosing, such as stealing cookies, or changing user passwords



# Explain session hijacking (with respect to web application testing)
In web application testing, **session hijacking** is an attack where an adversary gains unauthorised access to a user's active session by stealing or predicting their unique **session ID**.

## Key Attack Techniques

- **Cross-Site Scripting (XSS):** Injecting malicious JavaScript into a page that, when executed by a victim's browser, sends their session cookies directly to the attacker's server.

- **Session Sniffing (Side-Jacking):** Using packet sniffers (like Wireshark) on unencrypted networks, such as public Wi-Fi, to capture session IDs transmitted in "the clear".

- **Session Prediction:** Analysing how an application generates IDs to guess future valid tokens, especially if they are sequential or based on predictable data like timestamps.

# What is Internet Information Service (IIS)?
Internet Information Services (IIS) is a flexible, secure, and manageable web server created by Microsoft for hosting websites, services, and applications on Windows operating systems. It supports HTTP, HTTPS, FTP, and SMTP, enabling the delivery of static and dynamic content (like ASP.NET and PHP) to users.



# Explain baseboard management controller (BMC) Hacking and the Intelligent Platform Management Interface (IPMI)

A Baseboard Management Controller (BMC) is a specialised service processor on a server's motherboard that provides out-of-band management, allowing administrators to monitor, power cycle, or reconfigure systems remotely, even if they are turned off. The Intelligent Platform Management Interface (IPMI) is the standardised protocol used by BMCs to communicate

**TLDR**: BMC --> Chip on server's motherboard that you can use for management | IPMI --> Protocol used to communicate with BMC


# What are the pros and cons of using the cloud vs on prem (in terms of cyber security)?
Cloud security offers superior, up-to-date protection, rapid scaling, and lower upfront costs, but relies on third-party security, while on-premise provides full control over data sovereignty, which is ideal for compliance but requires significant internal resources .

| Aspect                | Cloud                                 | On-Premises                        |
| --------------------- | ------------------------------------- | ---------------------------------- |
| Responsibility        | Shared (provider + customer)          | Full control (all on you)          |
| Patch Management      | Automatic (provider-managed)          | Manual (you must maintain)         |
| Misconfiguration Risk | High (common cause of breaches)       | Moderate (depends on team skill)   |
| Physical Security     | Provider handles it                   | You are responsible                |
| Scalability (Defense) | High (elastic, handles large attacks) | Limited (hardware-dependent)       |
| Visibility/Control    | Limited at lower levels               | Full visibility and control        |
| Expertise Required    | Lower (built-in tools available)      | Higher (in-house expertise needed) |
| Default Security      | Strong baseline                       | Varies widely                      |


# What technical controls are available in the cloud to help with cyber security?
## Identity & Access Management (IAM)
- Role-based access control (RBAC)
- Multi-factor authentication (MFA)
- Least privilege policies
- Single sign-on (SSO)

## Encryption Controls
- Encryption at rest and in transit
- Customer-managed keys (CMK)
- Hardware Security Modules (HSMs)

## Network Security
- Virtual networks (VPCs/VNets)
- Security groups and firewall rules
- Network ACLs
- Private endpoints
- DDoS protection

## Logging & Monitoring
- API and activity logging
- Network flow logs
- Centralised logging
- Alerts and anomaly detection

## Configuration Management
- Continuous compliance checks
- Secure configuration baselines
- Policy enforcement
- Infrastructure as Code scanning