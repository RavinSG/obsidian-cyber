
Network interception attacks work by *intercepting network traffic and stealing valuable information* or *interfering with the transmission* in some way.

Malicious actors can use hardware or software tools to **capture and inspect data in transit**. This is referred to as packet sniffing. In addition to seeing information that they are not entitled to, malicious actors can also intercept network traffic and alter it. 

These attacks can cause damage to an organisation’s network by *inserting malicious code modifications or altering the message and interrupting network operations*. For example, an attacker can intercept a bank transfer and change the account receiving the funds to one that the attacker controls.

### IP Spoofing

After a malicious actor has [[Packet Sniffing|sniffed packets]] on the network, they can impersonate the IP and MAC addresses of authorised devices to perform an IP spoofing attack. Firewalls can prevent IP spoofing attacks by configuring it to refuse unauthorised IP packets and suspicious traffic. 

#### On-path Attack
An on-path attack happens when a *hacker intercepts the communication between two devices or servers that have a trusted relationship*. The transmission between these two trusted network devices could contain valuable information like usernames and passwords that the malicious actor can collect. An on-path attack is sometimes referred to as a man-in-the middle (**MITM**) attack because the hacker is hiding in the middle of communications between two trusted parties.

Or, it could be that the *intercepted transmission contains a DNS system look-up*. If a malicious actor intercepts a transmission containing a DNS lookup, they could spoof the DNS response from the server and redirect a domain name to a different IP address, perhaps one that contains malicious code or other threats. The most important way to protect against an on-path attack is to encrypt your data in transit, e.g. using [[Transport Layer Security|TLS]]. 

#### Smurf Attack
A smurf attack is a network attack that is performed when an attacker *sniffs an authorised user’s IP address and floods it with packets*. Once the spoofed packet reaches the broadcast address, it is sent to all of the devices and servers on the network. 

In a smurf attack, IP spoofing is combined with another denial of service ([[Denial of Service Attack|DoS]]) technique to *flood the network with unwanted traffic*. For example, the spoofed packet could include an Internet Control Message Protocol (ICMP) ping. As you learned earlier, ICMP is used to troubleshoot a network. But if too many ICMP messages are transmitted, the ICMP echo responses overwhelm the servers on the network and they shut down. This creates a denial of service and can bring an organisation’s operations to a halt.

An important way to *protect against* a smurf attack is to *use an advanced firewall* that can monitor any unusual traffic on the network. Most next generation firewalls (NGFW) include features that detect network anomalies to ensure that oversized broadcasts are detected before they have a chance to bring down the network.