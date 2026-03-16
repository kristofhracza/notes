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
[[Directory Traversal]]is a web security vulnerability allowing attackers to access unauthorised files, such as source code or system credentials, by manipulating file paths with `../` sequences. By tricking a server into traversing outside the root directory, attackers can read, modify, or delete sensitive data.
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
DNS servers contain a *Zone* file that replicates the map of the domain.
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