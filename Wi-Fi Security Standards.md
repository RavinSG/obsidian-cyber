
[[Wi-Fi]] networks rely on security and certification standards to help keep them secure. In fact, modern *wireless devices can't even display the Wi-Fi trademark without being certified* to a current standard like WPA2 or WPA3.

WPA2, or Wi-Fi Protected Access 2, is a widely deployed and used standard that provides two major usage modes:

- **WPA2-Personal**, which uses a pre-shared key and is thus *often called WPA2-PSK*. This allows clients to authenticate without an authentication server infrastructure.
  
- **WPA2-Enterprise** relies on a [[Remote Authentication Dial-in User Service|RADIUS]] authentication server as part of an 802.1X implementation for authentication. Users can thus have unique credentials and be individually identified.

WPA2 introduced the use of the **Counter Mode Cipher Block Chaining Message Authentication Code Protocol** (CCMP). CCMP uses Advanced Encryption Standard ([[Advanced Encryption Standard|AES]]) encryption to provide confidentiality, delivering much stronger encryption than older protocols like the Wired Equivalent Privacy ([[Wired Equivalent Privacy|WEP]]) protocol, which was used prior to WPA2. In addition to confidentiality, CCMP provides *authentication for the user and access control capabilities*. You'll note that user authentication is provided but **not network authentication**—that is an important addition in WPA3.

**Wi-Fi Protected Access 3** (WPA3), the *replacement for WPA2*, has been required to be supported in all Wi-Fi devices since the middle of 2020. WPA3 deployments are increasingly common as WPA3 supplants WPA2 in common usage. *WPA3 improves on WPA2 in a number of ways* depending on whether it is used in Personal or Enterprise mode. 

WPA3-Personal provides additional protection for password-based authentication, using a process known as **Simultaneous Authentication of Equals** (SAE). *SAE replaces the pre-shared keys used in WPA2* and requires interaction between both the client and the network to validate both sides. That interaction *slows down brute-force attacks* and makes them less likely to succeed. WPA3-Personal also *implements perfect forward secrecy*, which ensures that the traffic sent between the client and network is secure even if the client's password has been compromised.

>[!note] Perfect Forward Secrecy
> Perfect forward secrecy uses a process that *changes the encryption keys on an ongoing basis* so that a single exposed key won't result in the entire communication being exposed. Systems using perfect forward secrecy can refresh the keys they are using throughout a session at set intervals or every time a communication is sent.


WPA3-Enterprise provides stronger encryption than WPA2, with an *optional 192-bit security mode*, and adds authenticated encryption and additional controls for deriving and authenticating keys and encrypting network frames. WPA3 thus offers numerous security advantages over existing WPA2 networks.

>[!info] WPA3
>As WPA3 slowly expands in usage, it is important to note the security improvements it brings. *WPA3-Personal replaces the WPA2-PSK* authentication mode SAE (simultaneous authentication of equals) and implements perfect forward secrecy to keep traffic secure. WPA3-Enterprise continues to use RADIUS but improves the encryption and key management features built into the protocol, and *provides greater protection for wireless frames*. Open Wi-Fi networks also get an upgrade with the Wi-Fi Enhanced Open certification, which uses **opportunistic wireless encryption** (OWE) to provide encrypted Wi-Fi on open networks when possible—a major upgrade from the unencrypted open networks used with WPA2.


## [[Wireless Authentication]]
