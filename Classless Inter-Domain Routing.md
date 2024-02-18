---
aliases:
  - CIDR
---
Classless Inter-Domain Routing (**CIDR**) is a method of assigning subnet masks to IP addresses to create a subnet. Classless addressing replaces classful addressing. 

Classful addressing was used in the 1980s as a system of grouping IP addresses into classes (*Class A to Class E*). Each class included a limited number of IP addresses, which were depleted as the number of devices connecting to the internet outgrew the classful range in the 1990s. Classless CIDR addressing expanded the number of available IPv4 addresses. 

CIDR allows cybersecurity professionals to *segment classful networks into smaller chunks*. CIDR IP addresses are formatted like IPv4 addresses, but they include a slash (“/’”) followed by a number at the end of the address, This extra number is called the IP network prefix.

 For example, *a regular IPv4 address uses the 198.51.100.0 format*, whereas a *CIDR IP address would include* the IP network prefix at the end of the address, *198.51.100.0/24*. This CIDR address encompasses all IP addresses between 198.51.100.0 and 198.51.100.255. The system of CIDR addressing reduces the number of entries in routing tables and provides more available IP addresses within networks. 