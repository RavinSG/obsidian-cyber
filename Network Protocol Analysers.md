---
aliases:
  - packet sniffers
---
A **network protocol analyser**, also known as a **packet sniffer**, is a tool designed to capture and analyse data traffic in a network. 

This means that the tool keeps a record of all the data that a computer within an organisation's network encounters. 

They are commonly used as investigative tools to monitor networks and identify suspicious activity. There are a wide variety of network protocol analysers available, but some of the most common analysers include:

- SolarWinds NetFlow Traffic Analyzer
- ManageEngine OpManager
- Azure Network Watcher
- Wireshark
- [[tcpdump]]

### How network protocol analysers work

Network protocol analysers use both software and hardware capabilities to capture network traffic and display it for security analysts to examine and analyse. Here’s how:

- First, packets must be collected from the network via the Network Interface Card (**NIC**), which is hardware that connects computers to a network, like a router. NICs receive and transmit network traffic, but by default they only listen to network traffic that’s addressed to them. To capture all network traffic that is sent over the network, a NIC must be switched to a mode that has access to all visible network data packets. In wireless interfaces this is often referred to as *monitoring mode*, and in other systems it may be called *promiscuous mode*. This mode enables the NIC to have access to all visible network data packets, but it won’t help analysts access all packets across a network. A network protocol analyser must be positioned in an appropriate network segment to access all traffic between different hosts. 

- The network protocol analyser collects the network traffic in raw binary format. Binary format consists of 0s and 1s and is not as easy for humans to interpret. The network protocol analyser takes the binary and converts it so that it’s displayed in a human-readable format, so analysts can easily read and understand the information.  

### Capturing packets

Packet sniffing is the practice of capturing and inspecting data packets across a network. A **packet capture** (p-cap) is a file containing data packets intercepted from an interface or network. Packet captures can be viewed and further analysed using network protocol analysers. For example, you can filter packet captures to only display information that's most relevant to your investigation, such as packets sent from a specific IP address.

P-cap files can come in many formats depending on the packet capture library that’s used. Each format has different uses and network tools may use or support specific packet capture file formats by default. You should be familiar with the following libraries and formats:

- **Libpcap** is a packet capture library designed to be used by Unix-like systems, like Linux and MacOS®. Tools like tcpdump use Libpcap as the default packet capture file format. 

- **WinPcap** is an open-source packet capture library designed for devices running Windows operating systems. It’s considered an older file format and isn’t predominantly used.

- **Npcap** is a library designed by the port scanning tool Nmap that is commonly used in Windows operating systems.

- **PCAPng** is a modern file format that can simultaneously capture packets and store data. Its ability to do both explains the “ng,” which stands for “next generation.”