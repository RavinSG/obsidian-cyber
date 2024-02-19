
Each device, tool, or security strategy put in place by security analysts further protects—or hardens—the network until the network owner is satisfied with the level of security. This approach of adding layers of security to a network is referred to as **defence in depth**.

### [[Firewall]]

Most firewalls are similar in their basic functions. Firewalls *allow or block traffic based on a set of rules*. As data packets enter a network, the packet header is inspected and allowed or denied based on its port number. NGFWs are also able to inspect packet payloads. Each system should have its own firewall, regardless of the network firewall.

### [[Intrusion Detection System]]

An intrusion detection system (**IDS**) is an application that monitors system activity and alerts on possible intrusions. An IDS alerts administrators based on the signature of malicious traffic.

The IDS is configured to detect known attacks. IDS systems often *sniff data packets* as they move across the network and *analyse them for the characteristics of known attacks*. Some IDS systems review not only for signatures of known attacks, but also for anomalies that could be the sign of malicious activity. When the IDS discovers an anomaly, it sends an alert to the network administrator who can then investigate further.

The **limitations** to IDS systems are that they can *only scan for known attacks or obvious anomalies*. New and sophisticated attacks might not be caught. The other limitation is that the IDS *doesn’t actually stop the incoming traffic* if it detects something awry. It’s up to the network administrator to catch the malicious activity before it does anything damaging to the network. 

![[IDS.png]]

When combined with a firewall, an IDS adds another layer of defense. The IDS is p*laced behind the firewall and before entering the LAN*, which allows the IDS to analyze data streams after network traffic that is disallowed by the firewall has been filtered out. This is done to **reduce noise in IDS alerts**, also referred to as false positives.

### [[Intrusion Prevention System]]

An intrusion prevention system (**IPS**) is an application that *monitors system activity for intrusive activity and takes action to stop the activity*. It offers even more protection than an IDS because it actively stops anomalies when they are detected, unlike the IDS that simply reports the anomaly to a network administrator.

An IPS searches for signatures of known attacks and data anomalies. An IPS reports the anomaly to security analysts and blocks a specific sender or drops network packets that seem suspect. 

![[IPS.png]]

However, one potential limitation is that it is inline: *If it breaks, the connection between the private network and the internet breaks*. Another limitation of IPS is the possibility of false positives, which can result in legitimate traffic getting dropped.

### Full packet capture devices

Full packet capture devices can be incredibly useful for network administrators and security professionals. These devices allow you to *record and analyse all of the data* that is transmitted over your network. They also aid in investigating alerts created by an IDS. 

### [[Security Information and Event Management]]

A security information and event management system (**SIEM**) is an application that collects and analyses log data to monitor critical activities in an organization. SIEM tools *work in real time to report suspicious activity* in a centralised dashboard. SIEM tools additionally analyse network log data sourced from **IDSs, IPSs, firewalls, VPNs, proxies, and DNS logs**. SIEM tools are a way to aggregate security event data so that it all appears in one place for security analysts to analyse. This is referred to as a *single pane of glass*.