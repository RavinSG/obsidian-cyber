
There many different types of network appliances that we should consider as part of a network design. Special-purpose hardware devices, virtual machine and cloud-based software appliances, and hybrid models in both open source and proprietary commercial versions are used by organisations.

**Hardware appliances** can offer the advantage of being *purpose-built, allowing very high-speed traffic handling* capabilities or other capabilities. 

**Software and virtual machine appliances** can be *easily deployed and can be scaled* based on needs, whereas **cloud appliances** can be *dynamically created, scaled, and used* as needed.

### Jump Servers

Administrators and other staff need ways to **securely operate in security zones with different security levels**. Jump servers, sometimes called jump boxes, are a common solution. *A jump server is a secured and monitored system used to provide that access*. It is typically configured with the tools required for administrative work and is frequently accessed with SSH, RDP, or other remote desktop methods. 

Jump boxes should be *configured to create and maintain a secure audit trail*, with copies maintained in a separate environment to allow for incident and issue investigations.

### Load Balancing

Load balancers are used to distribute traffic to multiple systems, provide redundancy, and allow for ease of upgrades and patching. Load balancers *typically present a virtual IP* (VIP), which clients send service requests to on a service port. The load balancer then distributes those requests to servers in a pool or group. Two major modes of operation are common for load balancers:

- **Active/active** load balancer designs distribute the load among multiple systems that are online and in use at the same time.
  
- **Active/passive** load balancer designs bring backup or secondary systems online when an active system is removed or fails to respond properly to a health check. This type of environment is more likely to be found as part of [[Disaster Recovery Plan|disaster recovery]] or business continuity environments, and it may offer less capability from the passive system to ensure some functionality remains.

Load balancers rely on a variety of *scheduling* or *load-balancing algorithms* to choose where traffic is sent to. Here are a few of the most common options:

- **Round-robin** sends each request to servers by working through a list, with each server receiving traffic in turn.
  
- **Least connection** sends traffic to the server with the fewest number of active connections.
  
- **Agent-based** adaptive balancing monitors the load and other factors that impact a server's ability to respond and updates the load balancer's traffic distribution based on the agent's reports.
  
- **Source IP hashing** uses a hash of the source IP to assign traffic to servers. This is essentially a randomisation algorithm using client-driven input.

In addition to these, *weighted algorithms* take into account a weighting or score. Weighted algorithms include the following:

- **Weighted least connection** uses a least connection algorithm combined with a predetermined weight value for each server.
  
- **Fixed weighted** relies on a preassigned weight for each server, often based on capability or capacity.
  
- **Weighted response time** combines the server's current response time with a weight value to assign it traffic.

Finally, load balancers *may need to establish persistent sessions*. This helps servers provide a smoother experience, with consistent information maintained about the client, rather than requiring that the entire load-balanced pool be made aware of the client's session. Of course,
sticky sessions also mean that *load will remain on the server that the session started with*, which requires caution in case too many long-running sessions run on a single server and a load-balancing algorithm is in use that doesn't watch this.

### Proxy Servers

Proxy servers accept and forward requests, **centralising the requests** and allowing actions to be taken on the requests and responses. They can *filter or modify traffic and cache data*, and since they centralise requests, they can be used to *support access restrictions by IP address* or similar requirements. There are two types of proxy servers:

- **Forward proxies** are placed between clients and servers, and they accept requests from clients and send them forward to servers. Since forward proxies conceal the original client, they *can anonymise traffic or provide access to resources that might be blocked by IP* address or geographic location. They are also frequently used to allow access to resources such as those that libraries subscribe to.
  
- **Reverse proxies** are placed between servers and clients, and they are used to *help with load balancing and caching of content*. Clients can thus query a single system but have traffic load spread to multiple systems or sites.

### Web Filters

Web filters, sometimes called content filters, are centralised proxy devices or agent-based tools that allow or block traffic based on content rules. These can be as simple as conducting *Uniform Resource Locator (URL) scanning* and blocking specific URLs, domains, or hosts, or they may be complex, with *pattern matching, IP reputation, and other elements* built into the filtering rules. Like other technologies, they can be configured with allow or deny lists as well as rules that operate on the content or traffic they filter.

When *deployed as hardware devices or virtual machines*, they are typically a **centralised** proxy, which means that traffic is routed through the device. In *agent-based deployments*, the agents are installed on devices, meaning that the proxy is **decentralised** and can operate wherever the device is rather than requiring a network configured to route traffic through the centralised proxy.

Regardless of their design, they typically provide *content categorisation* capabilities that are used for URL filtering with common categories, including adult material, business, child-friendly material, and similar broad topics. This is often tied to block rules that stop systems from visiting sites that are in an undesired category or that have been blocked due to reputation, threat, or other reasons.

Proxies frequently have content filtering capabilities, but content filtering and URL filtering can also be part of other network devices and appliances such as firewalls, network security appliances, and IPSs.

### Data Protection

Ensuring that data isn't extracted or inadvertently sent from a network is where a data loss prevention (DLP) solution comes into play. [[Data Loss Prevention|DLP]] solutions frequently *pair agents on systems with filtering capabilities at the network border, email servers, and other likely exfiltration points*. DLP systems can use pattern-matching capabilities or can rely on tagging, including the use of metadata to identify data that should be flagged. Actions taken by DLP systems can include *blocking traffic, sending notifications, or forcing identified data to be encrypted* or otherwise securely transferred rather than being sent by an unencrypted or unsecure mode.

### Intrusion Detection and Intrusion Prevention Systems

Network-based intrusion detection systems (IDSs) and intrusion prevention systems (IPSs) are used to detect threats and, in the case of IPS, to block them.

They rely on one or more of three different detection methods to identify unwanted and potentially malicious traffic:

- **Signature-based** detections rely on a known hash or signature matching to detect a threat.
  
- **Heuristic, or behaviour-based**, detections look for specific patterns or sets of actions that match threat behaviours.
  
- **Anomaly-based** detection establishes a baseline for an organization or network and then flags when out-of-the-ordinary behaviour occurs.

Although an *IPS needs to be deployed in line* where it can interact with the flow of traffic to stop threats, *both IDSs and IPSs can be deployed in a passive mode as well*. Passive modes can detect threats but cannot take action—which means that an IPS placed in a passive deployment is effectively an IDS.

![[Inline vs Passive IPS.png]]

### Firewalls

Firewalls are one of the most common components in network design. They are deployed as network appliances or on individual devices, and many systems implement a simple firewall or firewall-like capabilities. 

There are two basic types of firewalls:

- **Stateless** firewalls (sometimes called packet filters) filter every packet based on data such as the source and destination IP and port, the protocol, and other information that can be gleaned from the packet's headers. They are the most basic type of firewall.
  
- **Stateful** firewalls (sometimes called dynamic packet filters) pay attention to the state of traffic between systems. They can make a decision about a conversation and allow it to continue once it has been approved rather than reviewing every packet. They track this information in a state table, and use the information they gather to allow them to see entire traffic flows instead of each packet, providing them with more context to make security decisions.

Along with stateful and stateless firewalls, additional terms are used to describe some firewall technologies. **Next-generation firewall** (NGFW) devices are far more than simple firewalls. In fact, they might be more accurately described as all-in-one network security devices in many cases. The general term has been used to describe network security devices that include a range of capabilities such as deep packet inspection, IDS/IPS functionality, antivirus and antimalware, and other functions. Despite the overlap between NGFWs and UTM devices, NGFWs are typically faster and capable of more throughput because they are more focused devices.

Unified threat management (**UTM**) devices frequently include firewalls, IDS/IPS, anti-malware, URL and email filtering and security data loss prevention, VPN, and security monitoring and analytics capabilities. The line between UTM and NGFW devices can be confusing, and the market continues to narrow the gaps between devices as each side offers additional features.

UTM appliances are frequently deployed at network boundaries, particularly for an entire organization or division. Since they have a wide range of security functionality, *they can replace several security devices* while providing a single interface to manage and monitor them. They also typically provide a management capability that can handle multiple UTM devices at once, allowing organisations with several sites or divisions to deploy UTM appliances to protect each area while still managing and monitoring them centrally.

Finally, web application firewalls (**WAFs**) are security devices that are designed to *intercept, analyse, and apply rules to web traffic*, including tools such as database queries, APIs, and other web application tools. In many ways, a WAF is easier to understand if you think of it as a firewall combined with an intrusion prevention system. They provide deeper inspection of the traffic sent to web servers looking for attacks and attack patterns, and then apply rules based on what they see. This allows them to block attacks in real time, or even modify traffic sent to web servers to remote potentially dangerous elements in a query or request.