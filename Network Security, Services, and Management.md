
### Out-of-Band Management

Access to the management interface for a network appliance or device needs to be protected so that *attackers can't seize control* of it and to ensure that *administrators can reliably gain access* when they need to. Whenever possible, network designs must include a way to do secure out-of-band management.

A separate means of accessing the administrative interface should exist. Since most devices are now managed through a network connection, modern implementations use a separate management VLAN or an entirely separate physical network for administration. Physical access to administrative interfaces is another option for out-of-band management, but in most cases physical access is reserved for emergencies because traveling to the network device to plug into it and manage it via USB, serial, or other interfaces is time consuming and far less useful for administrators than a network-based management plane.

### DNS

Domain Name System (DNS) servers and service can be an attractive target for attackers since systems rely on DNS to tell them where to send their traffic whenever they try to visit a site using a human-readable name. 

**DNS itself isn't a secure protocol**—in fact, like many of the original Internet protocols, it travels in an unencrypted, unprotected state and does not have authentication capabilities built in. Fortunately, *Domain Name System Security Extensions* (DNSSEC) can be used to help close some of these security gaps. DNSSEC provides authentication of DNS data, allowing DNS queries to be validated even if they are not encrypted.

Properly configuring DNS servers themselves is a key component of DNS security. *Preventing techniques such as zone transfers, as well as ensuring that DNS logging is turned on and that DNS requests to malicious domains are blocked*, are common DNS security techniques.

DNS filtering is used by many organisations to block malicious domains. DNS filtering uses a list of prohibited domains, subdomains, and hosts and replaces the correct response with an alternate DNS response, often to an internal website that notes that the access was blocked and what to do about the block.

### Email Security

There are three major methods of protecting email. These include **Domain Keys Identified Mail** (DKIM), **Sender Policy Framework** (SPF), and **Domain-based Message Authentication Reporting and Conformance** (DMARC).

**DIKM** allows organisations to add content to messaged to identify them as being from their domain. DIKM *signs both the body of the message and elements of the header*, helping to ensure that the message is actually from the organisation it claims it to be from. It adds a DIKM-signature header, which can be *checked against the public key that is stored in public DNS* entries for DIKM-enabled organisations.

**SPF** in an email authentication technique that allows organisations to *publish a list of their authorised email servers*. SPF records are added to the DNS information for your domain, and they specify which systems are allowed to send email from that domain. Systems not listed in SPF will be rejected.

>[!note] SPF Records
>SPF records in DNS are limited to 255 characters. This can make it tricky to use SPF for organizations that have a lot of email servers or that work with multiple external senders. In fact, SPF has a number of issues you can run into —you can read more about some of them at www.mimecast.com/content/sender-policy-framework.

**DMARC**, or Domain-based Message Authentication Reporting and Conformance, is a protocol that *uses SPF and DKIM to determine whether an email message is authentic*. Like SP and DKIM, DMARC records are published in DNS, but unlike DKIM and SPF, DMARC can be used to *determine whether you should accept a message from a sender*. Using DMARC, you can choose to reject or quarantine messages that are not sent by a DMARC-supporting sender. You can read an overview of DMARC at http://dmarc.org/overview.

In addition to email security frameworks like DKIM, SPF, and DMARC, *email security devices are used* by many organizations. These devices, often called **email security gateways**, are designed to filter both inbound and outbound email while providing a variety of security services. They typically include functions like phishing protection, email encryption, attachment sandboxing to counter malware, ransomware protection functions, URL analysis and threat feed integration, and of course support for DKIM, SPF, and DMARC checking.

[[Network Attacks]]