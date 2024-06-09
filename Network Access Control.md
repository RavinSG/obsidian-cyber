---
aliases:
  - NAC
---
Network segmentation helps divide networks into logical security zones, but *protecting networks from unauthorised access is also important*. That's where network access control (**NAC**), sometimes called network admissions control, comes in. 

NAC technologies focus on *determining whether a system or device should be allowed to connect* to a network. If it passes the requirements set for admission, NAC places it into an appropriate zone.

To accomplish this task, NAC can use a software agent that is installed on the computer to perform security checks. Or the process may be agentless and run from a browser or by another means without installing software locally. Capabilities vary, and software agents typically have a greater ability to *determine the security state of a machine by validating patch levels, security settings, antivirus versions, and other settings* and details before admitting a system to the network. 

Some NAC solutions also track user behaviour, allowing for systems to be removed from the network if they engage in suspect behaviours.

Since NAC has the ability to validate security status for systems, it **can be an effective policy enforcement tool**. If a system does not meet security objectives, or if it has an issue, the system can be placed into a quarantine network. There the system can be remediated and rechecked, or it can simply be prohibited from connecting to the network.

NAC checks can occur before a device is allowed on the network (pre-admission) or after it has connected (post-admission). The combination of agent or agentless solutions and pre-or postadmission designs is driven by security objectives and the technical capabilities of an organisation's selected tool. 

**Agent-based NAC** requires installation and thus *adds complexity and maintenance, but it provides greater insight and control*. 

**Agentless** installations are *lightweight and easier to handle* for users whose machines may not be centrally managed or who have devices that may not support the NAC agent. However, agentless installations *provide less detail*. 

Pre-admission NAC decisions keep potentially dangerous systems off a network; postadmission decisions can help with response and prevent failed NAC access attempts from stopping business.

### NAC and 802.1X

802.1X is a standard for *authenticating devices connected to wired and wireless networks*. It uses centralised authentication using [[Extensible Authentication Protocol|EAP]]. 802.1X authentication requires a supplicant, typically a user or client device that wants to connect. 

The supplicant connects to the network and the *authentication server sends a request for it to identify itself*. The supplicant respond and an authentication process commences. If the supplicant's credentials are accepted, the system is allowed access to the network, often with appropriate rules applied such as placing the supplicant in a network zone or VLAN.

802.1X is frequently used for port-based authentication or port security, which is used at the network access switch level to authorise ports or to leave them as unauthorised. Thus, if devices want to connect to a local area network, they need to have an 802.1X supplicant and complete an authentication process.
