
### Using Secure Protocols

Secure protocols have places in many parts of your network and infrastructure. Security professionals need to be able to recommend the right protocol for each of the following scenarios:

- Voice and video rely on a number of common protocols. *Videoconferencing* tools often rely on **HTTPS**, but secure versions of the **Session Initiation Protocol** (SIP) and the **Realtime Transport Protocol** (RTP) exist in the form of **SIPS** and **SRTP**, which are also used to ensure that communications traffic remains secure.
  
- A secure version of the *Network Time Protocol* (NTP) exists and is called **NTS**, but NTS has not been widely adopted. Like many other protocols you will learn about in this chapter, *NTS relies on TLS*. Unlike other protocols, NTS does not protect the time data. Instead, it focuses on authentication to make sure that the time information is from a trusted server and has not been changed in transit.
  
- Email and web traffic relies on a number of secure options, including **HTTPS**, **IMAPS**, **POPS**, and security protocols like Domain-based Message Authentication Reporting and Conformance (**DMARC**), DomainKeys Identified Mail (**DKIM**), and Sender Policy Framework (**SPF**).
  
- *File Transfer Protocol* (FTP) has largely been replaced by a combination of **HTTPS** file transfers and **SFTP** or **FTPS**, depending on organisational preferences and needs.
  
- Directory services like *LDAP* can be moved to **LDAPS**, a secure version of LDAP.
  
- Remote access technologies-including shell access, which was once accomplished via *telnet* and is now almost exclusively done via **SSH**-can also be secured. Microsoft's **RDP** is encrypted by default, but other remote access tools may use other protocols, including **HTTPS**, to ensure that their traffic is not exposed.
  
- *Domain name resolution* remains a security challenge, with multiple efforts over time that have had limited impact on DNS protocol security, including **DNSSEC** and DNS reputation lists.
  
- *Routing and switching protocol* security can be complex, with protocols like Border Gateway Protocol (BGP) lacking built-in security features. Therefore, attacks such as BP hijacking attacks and other routing attacks remain possible. Organisations cannot rely on a secure protocol in many cases and need to design around this lack.
  
- Network address allocation using DHCP does not offer a secure protocol, and network protection against DHCP attacks relies on detection and response rather than a secure protocol.
- Subscription services such as cloud tools and similar services frequently leverage HTTPS but may also provide other secure protocols for their specific use cases. The wide variety of possible subscriptions and types of services means that these services must be assessed individually with an architecture and design review, as well as data flow reviews all being part of best practices to secure subscription service traffic if options are available.

### Secure Protocols

| Unsecure Protocol | Original Port   | Secure Protocol Option(s) | Secure Port                                      | Notes     |
| ----------------- | --------------- | ------------------------- | ------------------------------------------------ | --------- |
| DNS               | UDP/TCP 53      | DNSSEC                    | UDP/TCP 53                                       |           |
| FTP               | TCP 21 (and 20) | FTPS                      | TCP 21 in explicit mode and 990 in implicit mode | Using TLS |
| FTP               | TCP 21 (and 20) | SFTP                      | TCP 22 (SSH)                                     | Using SSH |
| HTTP              | TCP 80          | HTTPS                     | TCP 443                                          | Using TLS |
| IMAP              | TCP 143         | IMAPS                     | TCP 993                                          | Using TLS |
| LDAP              | UDP and TCP 389 | LDAPS                     | TCP 636                                          | Using TLS |
| POP3              | TCP 110         | POP3                      | TCP 995 - Secure POP3                            | Using TLS |
| RTP               | UDP 16384-32767 | SRTP                      | UDP 5004                                         |           |
| SNMP              | UDP 161 and 162 | SNMPv3                    | UDP 161 and 162                                  |           |
| Telnet            | TCP 23          | SSH                       | TCP 22                                           |           |

- Domain Name System Security Extension (DNSSEC) focuses on **ensuring that DNS information is not modified or malicious**, but it *doesn't provide confidentiality* like many of the other secure protocols listed here do. DNSSEC uses digital signatures, allowing systems that query a DNSSEC-equipped server to validate that the server's signature matches the DNS record. DNSSEC can also be used to build a chain of trust for IPSec keys, SSH fingerprints, and similar records.
  
- Simple Network Management Protocol, version 3 (SNMPv3) improves on previous versions of SNMP by providing authentication of message sources, message integrity validation, and confidentiality via encryption. It supports multiple security levels, **but only the authPriv level uses encryption**, meaning that *insecure implementations of SNMPv3 are still possible*. Simply using SNMPv3 does not automatically make SNMP information secure.
  
- Secure Shell (SSH) is a protocol used for remote console access to devices and is a **secure alternative to telnet**. SSH is also *often used as a tunnelling protocol* or to support other uses like SFTP. SSH can use SSH keys, which are used for authentication. As with many uses of certificate or key-based authentication, a lack of a password or weak passwords as well as poor key handling can make SSH far less secure in use.
  
- Hypertext Transfer Protocol over SSL/TLS (HTTPS) relies on TLS in modern implementations but is often called SSL despite this. Like many of the protocols discussed here, the underlying HTTP protocol relies on TLS to provide security in HTTPS implementations.
  
- Secure Real-Time Protocol (SRTP) is a secure version of the Real-time Protocol, a protocol designed to provide audio and video streams via networks. SRTP uses encryption and authentication to attempt to reduce the likelihood of successful attacks, including replay and denial-of-service attempts. RTP uses paired protocols, RTP and RTCP. RTCP is the control protocol that monitors the quality of service (QoS) and synchronisation of streams, and RTCP has a secure equivalent, SRTP, as well.
  
- Secure Lightweight Directory Application Protocol (LDAPS) is a TLS-protected version of LDAP that offers confidentiality and integrity protections.

### Email-Related Protocols

Although many organisations have moved to web-based email, email protocols like Post Office Protocol (**POP**) and Internet Message Access Protocol (**IMAP**) remain in use for mail clients. Secure protocol options that *implement TLS as a protective layer exist for both*, resulting in the deployment of **POPS** and **IMAPS**.

**Secure/Multipurpose Internet Mail Extensions** (S/MIME) provides the ability to encrypt and sign MIME data, the format used for email attachments. Thus, the content and attachments for an email can be protected, while providing authentication, integrity, nonrepudiation, and confidentiality for messages sent using S/MIME.

Unlike many of the other protocols discussed here, *S/MIME requires a certificate* for users to be able to send and receive S/MIME-protected messages. A locally generated certificate or one from a public certificate authority (CA) is needed. *This requirement adds complexity for S/MIME* users who want to communicate securely with other individuals, because certificate management and validation can become complex. *For this reason, S/MIME is used less frequently*, despite broad support by many email providers and tools.

### IPSec

IPSec (Internet Protocol Security) is more than just a single protocol. In fact, IPSec is an entire suite of security protocols used to *encrypt and authenticate IP traffic*.

- **Authentication Header** (AH) uses hashing and a shared secret key to ensure integrity of data and validates senders by authenticating the IP packets that are sent. AH can ensure that the IP payload and headers are protected.
  
- **Encapsulating Security Payload** (ESP) operates in either transport mode or tunnel mode. In tunnel mode, it provides integrity and authentication for the entire packet; in transport mode, it only protects the payload of the packet. If ESP is used with an authentication header, this can cause issues for networks that need to change IP or port information.
