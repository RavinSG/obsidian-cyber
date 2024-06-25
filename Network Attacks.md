
### On-Path Attacks

An on-path (**man-in-the-middle** (MitM)) attack occurs when an attacker causes traffic that should be sent to its intended recipient to be relayed through a system or device the attacker controls. Once the attacker has traffic flowing through that system, *they can eavesdrop or even alter the communications* as they wish.

![[Man in the Middle.png]]

An on-path attack can be used to conduct SSL stripping, an attack that in modern implementations removes TLS encryption to read the contents of traffic that is intended to be sent to a trusted endpoint. A typical SSL stripping attack occurs in three phases:

1) A user sends an HTTP request for a web page.
2) The server responds with a redirect to the HTTPS version of the page.
3) The user sends an HTTPS request for the page they were redirected to, and the website loads.

A SSL stripping attack uses an on-path attack when the HTTP request occurs, *redirecting the rest of the communications through a system that an attacker controls*, allowing the communication to be read or possibly modified. Although SSL stripping attacks can be conducted on any network, one of the most common implementations is through an **open wireless network**, where the attacker can control the wireless infrastructure and thus modify traffic that passes through their access point and network connection.

>[!fail] Stopping SSL Stripping and HTTPS On-Path Attacks
>
>Protecting against SSL stripping attacks can be done in a number of ways, including configuring systems to expect certificates for sites to be issued by a known certificate authority and thus preventing certificates for alternate sites or self-signed certificates from working. Redirects to secure websites are also a popular target for attackers since unencrypted requests for the HTTP version of a site could be redirected to a site of the attacker's choosing to allow for an on-path attack. The HTTP Strict Transport Security (HSTS) security policy mechanism is intended to prevent attacks like these that rely on protocol downgrades and cookie jacking by forcing browsers to connect only via HTTPS using TLS. Unfortunately, HSTS only works after a user has visited the site at least once, allowing attackers to continue to leverage on-path attacks.


A final on-path attack variant is the browser-based on-path attack (man-in-the-browser (**MitB** or **MiB**)). This attack relies on a *Trojan that is inserted into a user's browser*. The Trojan is then able to access and modify information sent and received by the browser. Since the browser receives and decrypts information, a browser-based on-path attack **can successfully bypass TLS encryption** and other browser security features, and it can also access sites with open sessions or that the browser is authenticated to, allowing a browser-based on-path attack to be a very powerful option for an attacker. Since browser-based on-path attacks *require a Trojan to be installed*, either as a browser plug-in or a proxy, system-level security defences like antimalware tools and system configuration management and monitoring capabilities are best suited to preventing them.

On-path attack indicators are typically changed network gateways or routes, although sophisticated attackers might also compromise network switches or routers to gain access to and redirect traffic.

### Domain Name System Attacks

**Domain hijacking** changes the registration of a domain, either through technical means like a *vulnerability* with a domain registrar or control of a system belonging to an authorised user, or through nontechnical means such as *social engineering*. The end result of domain hijacking is that the domain's settings and configuration can be changed by an attacker, allowing them to intercept traffic, send and receive email, or otherwise take action while appearing to be the legitimate domain holder. 

Many domains end up in hands other than those of the intended owner because they are *not properly renewed*. Detecting domain hijacking can be difficult if you are simply a user of systems and services from the domain, but *domain name owners can leverage security tools* and features provided by domain registrars to both protect and monitor their domains.

**DNS poisoning** can be accomplished in multiple ways. One form is another form of the on-path attack where an *attacker provides a DNS response while pretending to be an authoritative DNS server*. Vulnerabilities in DNS protocols or implementations can also permit DNS poisoning, but they are rarer. DNS poisoning *can also involve poisoning the DNS cache on systems*. Once a malicious DNS entry is in a system's cache, it will continue to use that information until the cache is purged or updated. This means that **DNS poisoning can have a longer-term impact**, even if it is discovered and blocked by an IPS or other security device. DNS cache poisoning may be noticed by users or may be detected by network defences like an IDS or IPS, but it can be difficult to detect if done well.

When domain hijacking isn't possible and DNS cannot be poisoned, another option for attackers is **URL redirection**. URL redirection can take many forms, depending on the vulnerability that attackers leverage, but one of the most common is to insert *alternate IP addresses into a system's hosts file*. The hosts file is checked when a system looks up a site via DNS and will be used first, making a modified hosts file a powerful tool for attackers who can change it. Modified hosts files can be manually checked, or they can be monitored by system security antimalware tools that know the hosts file is a common target.

Although DNS attacks can provide malicious actors with a way to attack your organization, you can also *leverage DNS-related information to help defend against attacks*. **Domain reputation services** and tools provide information about whether a domain is a trusted email sender or sends a lot of spam email. In addition, individual organisations may assign domain reputation scores for email senders using their own email security and anti-spam tools.

### Credential Replay Attacks

Credential replay attacks are a form of network attack that requires the attacker to be able to *capture valid network data and to re-send it or delay it* so that the attacker's own use of the data is successful. The most common version of this has been to **re-send authentication hashes**; however, most modern implementations of authentication systems use session IDs and encryption to largely prevent replay attacks.

Common indicators of replay attacks are on-path attack indicators like **modified gateways or routes**.

### Distributed Denial-of-Service Attacks

A distributed denial-of-service is conducted from multiple locations, networks, or systems, making it difficult to stop and hard to detect. At the same time, the distributed nature of the DDoS means that it may bring significant resources to bear on a targeted system or network, potentially overwhelming the target through its sheer size.

#### Network DDOS

Malicious actors commonly use large-scale botnets to conduct network DDoS attacks, and *commercial services exist* that conduct DDoS attacks and DDoS-like behaviour for stress- and load-testing purposes. All of this means that organisations need to have a plan in place to detect and handle network DDoS attacks.

In many cases, an organisation's *Internet service provider (ISP) may provide a DDoS prevention service*, either by default or as an additional subscription option. Knowing whether your ISP provides the capability and under what circumstances it will activate or can be turned on can be a critical network defence for your organization. If your ISP does not provide DDoS prevention, a second option is to **ensure** that your **network border security devices have DDoS prevention capabilities**.

**Volume-based** network DDoS attacks focus on the sheer amount of traffic causing a denial-of-service condition. Some volume-based DDoS attacks rely on *amplification techniques that leverage flaws or features in protocols* and services to create significantly more traffic than the attacker sends. Volume-based attack examples include **UDP** and **ICMP floods**:

- **UDP floods** take advantage of the fact that *UDP doesn't use a three-way handshake* like TCP does, allowing UDP floods to be executed simply by sending massive amounts of traffic that the target host will receive and attempt to process. Since UDP is not rate limited or otherwise protected and does not use a handshake, UDP floods can be conducted with minimal resources on the attacking systems.
  
- ICMP is rate limited in many modern operating systems. **ICMP floods**, sometimes called ping floods, send massive numbers of ICMP packets, with each requesting a response. ICMP floods *require more aggregate bandwidth on the side of the attacker than the defender* has, which is why a distributed denial-of-service via ICMP may be attempted. Many organisations *rate-limit or block ping* at network ingress points to prevent this type of attack, and they may rate-limit ICMP between security zones as well.

**Protocol-based** network DDoS attacks focus on the underlying protocols used for networking. **SYN floods** send the first step in a three-way handshake and do not respond to the SYN-ACK that is sent back, thus consuming TCP stack resources until they are exhausted. These attacks are one of the most common modern protocol-based network DDoS attacks. Older attacks targeted vulnerable TCP stacks with attacks like the **Ping of Death**, which sent a ping packet too large for many to handle, and **Smurf attacks**, which leveraged ICMP broadcast messages with a spoofed sender address, causing systems throughout the broadcast domain to send traffic to the purported sender and thus overwhelming it. 

*Fragmented packets, packets with all of their TCP flags turned on* (Christmas Tree or Xmas attacks), and a variety of other attacks have leveraged flaws and limitations in how the networking was implemented in operating systems. Security professionals need to know that the features of network protocols and the specific implementations of those protocols may be leveraged as part of an attack and that they may need to identify those attacks.