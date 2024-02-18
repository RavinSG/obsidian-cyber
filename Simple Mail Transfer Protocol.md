---
aliases:
  - SMTP
tags:
  - Protocol
---
Simple Mail Transfer Protocol (**SMTP**) is used to transmit and route email from the sender to the recipient’s address. 

SMTP works with *Message Transfer Agent* (MTA) software, which searches DNS servers to resolve email addresses to IP addresses, to ensure emails reach their intended destination. 

SMTP uses TCP/UDP port 25 for unencrypted emails and TCP/UDP port 587 using TLS for encrypted emails. The TCP port 25 is often used by high-volume spam. SMTP helps to filter out spam by regulating how many emails a source can send at a time.