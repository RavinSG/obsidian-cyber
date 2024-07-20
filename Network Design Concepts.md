### Physical Isolation

Physical isolation is the idea of separating devices so that there is no connection between them. This is commonly known as an *air-gapped* design because there is air between them instead of a network cable or connection. Physical isolation requires physical presence to move data between air-gapped systems, preventing remote attackers from bridging the gap.

### Logical Segmentation

Logical segmentation is done *using software or settings rather than a physical separation* using difference deices. Virtual local area networks (VLANs) are a common method of providing logical segmentation. [[Virtual Local Area Network|VLAN]] tags are applied to packets that are part of a VLAN, and systems on that virtual network segment see them like they would in a physical network segment. Attacks against logical segmentation attempt to bypass the software controls that separate traffic to allow traffic to be sent or received from other regions.

### High Availability

High availability (HA) is the ability of a service, system, network, or other element of infrastructure to be *consistently available without downtime*. High availability designs will typically allow for upgrades, patching, system or service failures, changes in load, and other events without interruption of services. High availability targets are set when systems are designed and then various solutions like *clustering, load balancing, and proxies are used* to ensure that the solution reaches availability targets. HA design typically focuses on the ability to reliably switch between components as needed, elimination of single points of failure that could cause an outage, and the ability to detect and remediate or work around a failure if it does happen.

### Implementing Secure Protocols

Implementation of secure protocols is a common part of ensuring that communication and services are secure. Examples of secure protocols are the use of HTTPS (TLS) instead of unencrypted HTTP, using SSH instead of Telnet, and wrapping other services using TLS. Protocol selection for most organisations will *default to using the secure protocol* if it exists and is supported, and security analysts will typically note the use of insecure protocols as a risk if they are discovered. Port selection is related to this, although default ports for protocols are normally preselected. Some protocols use different ports for the secure version of the protocol, like the use of port 443 for HTTPS instead of 80 for HTTP. Others like Microsoft's SQL Server use the same port and rely on client requests for TLS connections on TCP 1433. Finally, selection of the transport method, including protocol versions, is important when selecting secure protocols. *Downgrade attacks* and simply using insecure versions of protocols can lead to data exposures, so selecting and requiring appropriate versions of protocols like TLS is an important configuration decision.

### Reputation Services
Reputation describes *services and data feeds that track IP addresses, domains, and hosts that engage in malicious activity*. Reputation services allow organisations to monitor or block potentially malicious actors and systems, and reputation systems are often combined with threat feeds and log monitoring to provide better insight into potential attacks.

### Software-Defined Networking
Software-defined networking (SDN) uses *software-based network configuration to control networks*. SDN designs rely on controllers that manage network devices and configurations, centrally managing the software-defined network. This allows networks to be dynamically tuned based on performance metrics and other configuration settings, and to be customised as needed in a flexible way. SDN can be leveraged as part of security infrastructure by allowing *dynamic configuration of security zones* to remove or isolate systems when they need to systems based on authorisation or to quarantined.

### SD-WAN

Software-defined wide area network (SD-WAN) is a virtual wide area network design that can *combine multiple connectivity services* for organisations. SD-WAN is commonly used with technologies like Multiprotocol Label Switching (**MPLS**), **4G** and **5G**, and **broadband** networks. SD-WAN can help by providing high availability and allowing for networks to route traffic based on application requirements while controlling costs by using less expensive connection methods when possible.

### Secure Access Service Edge

Secure Access Service Edge combines *virtual private networks, SD-WAN, and cloud-based security* tools like firewalls, *cloud access security brokers (CASBs), and zero-trust networks* to provide secure access for devices regardless of their location. SASE is deployed to ensure that endpoints are secure, that data is secure in transit, and that policy-based security is delivered as intended across an organisation's infrastructure and services.