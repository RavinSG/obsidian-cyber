---
aliases:
  - DNS
tags:
  - Protocol
---
Domain Name System (**DNS**) is a protocol that translates internet domain names into IP addresses. 

When a client computer wishes to access a website domain using their internet browser, a query is sent to a dedicated DNS server. The DNS server then looks up the IP address that corresponds to the website domain. 

DNS normally *uses UDP on port 53*. However, if the DNS reply to a request is large, it will switch to using the TCP protocol. 

In the TCP/IP model, DNS occurs at the [[Application Layer]]