---
aliases:
  - WPA
tags:
  - Protocol
---
Wi-Fi Protected Access (**WPA**) was developed in 2003 to improve upon [[Wired Equivalent Privacy|WEP]], address the security issues that it presented, and replace it. WPA was always *intended to be a transitional measure* so backwards compatibility could be established with older hardware.

The flaws with WEP were in the protocol itself and how the encryption was used. WPA addressed this weakness by using a protocol called **Temporal Key Integrity Protocol** (TKIP). WPA encryption algorithm uses larger secret keys than WEPs, making it more difficult to guess the key by trial and error.

WPA also includes a *message integrity check* that includes a message *authentication tag with each transmission*. If a malicious actor attempts to alter the transmission in any way or resend at another time, WPA’s message integrity check will identify the attack and reject the transmission.

Despite the security improvements of WPA, **it still has vulnerabilities**. Malicious actors can use a **key reinstallation attack** (or KRACK attack) to decrypt transmissions using WPA. Attackers can insert themselves in the WPA authentication handshake process and insert a new encryption key instead of the dynamic one assigned by WPA. If they set the new key to all zeros, it is as if the transmission is not encrypted at all.

Because of this significant vulnerability, WPA was replaced with an updated version of the protocol called [[WPA2]]. 