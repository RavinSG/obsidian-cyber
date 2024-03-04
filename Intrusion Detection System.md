---
aliases:
  - IDS
---
An intrusion detection system (**IDS**) is an application that *monitors system activity and alerts on possible intrusions*. An IDS provides continuous monitoring of network events to help protect against security threats or attacks. The goal of an IDS is to *detect potential malicious activity* and generate an alert once such activity is detected. An IDS does not stop or prevent the activity. Instead, security professionals will investigate the alert and act to stop it, if necessary. 

For example, an IDS can send out an alert when it identifies a suspicious user login, such as an unknown IP address logging into an application or a device at an unusual time. But, an IDS will not stop or prevent any further actions, like blocking the suspicious user login. 

### Host-based intrusion detection system

A host-based intrusion detection system (**HIDS**) is an application that monitors the activity of the host on which it's installed. A HIDS is installed as an agent on a host. A host is also known as an *endpoint*, which is any device connected to a network like a computer or a server. 

Typically, HIDS agents are installed on all endpoints and used to monitor and detect security threats. A HIDS monitors internal activity happening on the host to identify any unauthorised or abnormal behaviour. If anything unusual is detected, such as the installation of an unauthorised application, the HIDS logs it and sends out an alert. 

In addition to monitoring inbound and outbound traffic flows, HIDS can have *additional capabilities, such as monitoring file systems, system resource usage, user activity*, and more. 

### Network-based intrusion detection system

A network-based intrusion detection system (**NIDS**) is an application that collects and monitors network traffic and network data. NIDS software is installed on devices located at specific parts of the network that you want to monitor. The NIDS application inspects network traffic from different devices on the network. If any malicious network traffic is detected, the NIDS logs it and generates an alert.

![[NIDS.png]]

Using a combination of **HIDS** and **NIDS** to monitor an environment can provide a multi-layered approach to intrusion detection and response. HIDS and NIDS tools provide a different perspective on the activity occurring on a network and the individual hosts that are connected to it. This helps provide a comprehensive view of the activity happening in an environment.

[[Detection Techniques]]