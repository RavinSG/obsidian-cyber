---
aliases:
  - NAT
tags:
  - Protocol
---
The devices on a local home or office network each have a [[Private IP Address|Private IP Addresses]] that they use to communicate directly with each other. In order for the devices with private IP addresses to communicate with the public internet, they need to have a [[Public IP Address|public IP address]]. 

Otherwise, responses will not be routed correctly. Instead of having a dedicated public IP address for each of the devices on the local network, the router can *replace a private source IP address with its public IP address and perform the reverse operation for responses*. 

This process is known as Network Address Translation (**NAT**) and it generally requires a router or firewall to be specifically configured to perform NAT. 

NAT is a part of layer 2 ([[Internet Layer]]) and layer 3 ([[Transport Layer]]) of the TCP/IP model.