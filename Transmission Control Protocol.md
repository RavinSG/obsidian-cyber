---
aliases:
  - TCP
tags:
  - Protocol
---
**Transmission Control Protocol** is an internet communication protocol that allows two devices to form a connection and stream data. TCP uses a *three-way handshake* process. 

First, the device sends a synchronise (SYN) request to a server. Then the server responds with a SYN/ACK packet to acknowledge receipt of the device's request. Once the server receives the final ACK packet from the device, a TCP connection is established. 

In the TCP/IP model, TCP occurs at the [[Transport Layer]]