---
aliases:
  - SSH
tags:
  - Protocol
---
Secure shell protocol (**SSH**) is used to *create a secure connection* with a remote system. 

This [[Application Layer]] protocol provides an alternative for secure authentication and encrypted communication. 

SSH operates over the TCP port 22 and is a replacement for less secure protocols, such as Telnet.

Note that if this is the first time we connect using SSH, we will need to *confirm the fingerprint of the SSH server’s public key to avoid man-in-the-middle (MITM) attacks*. 

In the case of SSH, unlike HTTPS, we don’t usually have a third party to check if the public key is valid, so we need to do this manually.