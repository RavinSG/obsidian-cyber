---
aliases:
  - DoS
  - DDos
---
An attack that targets a network or server and floods it with network traffic. The objective of a denial of service attack, or a **DoS** attack, is to *disrupt normal business operations by overloading an organisation's network*. 

The goal of the attack is to send so much information to a network device that it crashes or is unable to respond to legitimate users. This means that the organization won't be able to conduct their normal business operations, which can cost them money and time. A network crash can also leave them vulnerable to other security threats and attacks.

### Distributed Denial of Service (DDoS)

**DDoS** is a kind of DoS attack that *uses multiple devices or servers in different locations* to flood the target network with unwanted traffic. Use of numerous devices makes it more likely that the total amount of traffic sent will overwhelm the target server. Remember, DoS stands for denial of service. So it doesn't matter what part of the network the attacker overloads; if they overload anything, they win. 

An attacker can also craft a very careful packet that caused a router to *spend extra time processing the request*. The overall traffic volume wont overload the router; the specifics within the packet will.

### DoS Attack Types

#### SYN Flood
This is a type of **DoS** attack that *simulates the TCP connection* and floods the server with SYN packets. The first step in the handshake is for the device to send a SYN, or synchronise, request to the server. Then, the server responds with a SYN/ACK packet to acknowledge the receipt of the device's request and leaves a port open for the final step of the handshake. Once the server receives the final ACK packet from the device, a TCP connection is established. Malicious actors can take advantage of the protocol by flooding a server with SYN packet requests for the first part of the handshake. But if the *number of SYN requests is larger than the number of available ports* on the server, then the *server will be overwhelmed* and become unable to function.

#### ICMP Flood
An ICMP flood attack is a type of DoS attack performed by an attacker repeatedly sending ICMP packets to a network server. This *forces the server to send an ICMP packet*. This eventually uses up all the bandwidth for incoming and outgoing traffic and causes the server to crash. 

#### Ping of Death
A ping of death attack is a type of DoS attack that is caused when a hacker pings a system by *sending it an oversized ICMP packet* that is bigger than 64 kilobytes, the maximum size for a correctly formed ICMP packet. Pinging a vulnerable network server with an oversized ICMP packet will overload the system and cause it to crash.