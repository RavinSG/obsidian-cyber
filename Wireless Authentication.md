
Although the security protocols and standards that a network uses are important, it is also critical to control access to the network itself. Organisations have a number of choices when it comes to choosing how they provide access to their networks:

- **Open networks**, which do not require authentication but that often *use a captive portal to gather some information* from users who want to use them. Captive portals redirect traffic to a website or registration page before allowing access to the network. Open networks *do not provide encryption*, leaving user data at risk unless the traffic is sent via secure protocols like HTTPS.
  
- **Use of preshared keys** (PSKs) requires a passphrase or key that is shared with anybody who wants to use the network. This allows traffic to be encrypted but *does not allow users to be uniquely identified*.
  
- **Enterprise authentication** relies on a RADIUS server and utilises an Extensible Authentication Protocol ([[Extensible Authentication Protocol|EAP]]) for authentication.

### Wireless Authentication Protocols

**802.1X** is an IEEE standard for access control and is *used for both wired and wireless devices*. In wireless networks, 802.1X is used to integrate with [[Remote Authentication Dial-in User Service|RADIUS]] servers, allowing enterprise users to authenticate and gain access to the network. Additional actions can be taken based on information about the users, such as *placing them in groups or network zones*, or *taking other actions based on attributes* once the user has been authenticated.

Wi-Fi enterprise networks rely on IEEE 802.1X and various versions of **Extensible Authentication Protocol** (EAP). [[Extensible Authentication Protocol|EAP]] is used by 802.1X as part of the authentication process when devices are authenticating to a RADIUS server. There are many EAP variants because EAP was designed to be extended, as the name implies. Here are common EAP variants that you should be aware of:

- **Protected EAP** (*PEAP*) authenticates servers using a certificate and wraps EAP *using a TLS tunnel to keep it secure*. Devices on the network use unique encryption keys, and Temporal Key Integrity Protocol (*TKIP*) is implemented to replace keys on a regular basis.
  
- **EAP-Flexible Authentication via Secure Tunnelling** (*EAP-FAST*) is a Cisco-developed protocol that improved on vulnerabilities in the Lightweight Extensible Authentication Protocol (*LEAP*). EAP-FAST is focused on *providing faster re-authentication while devices are roaming*. EAP-FAST works around the public key exchanges that slow down PEAP and EAP-TLS by using a shared secret (symmetric) key for re-authentication. EAP-FAST can use either preshared keys or dynamic keys established using public key authentication.
  
- **EAP-Transport Layer Security** (*EAP-TLS*) implements certificate-based authentication as well as mutual authentication of the device and network. It *uses certificates on both client and network devices to generate keys* that are then used for communication. EAP-TLS is used less frequently due to the certificate management challenges for deploying and managing certificates on large numbers of client devices.
  
- **EAP-Tunnelled Transport Layer Security** (*EAP-TTLS*) extends EAP-TLS, and unlike EAP-TLS, it *does not require that client devices have a certificate* to create a secure session. This removes the overhead and management effort that EAP-TLS requires to distribute and manage endpoint certificates while still providing TLS support for devices. A concern for EAP-TTLS deployments is that *EAP-TTLS can require additional software to be installed on some devices*, whereas PEAP, which provides similar functionality, does not. EAP-TTLS does provide support for some less secure authentication mechanisms, meaning that there are times where it may be implemented due to specific requirements.

When organisations want to work together, [[Remote Authentication Dial-in User Service|RADIUS]] (Remote Authentication Dial-in User Service) *servers can be federated to allow individuals from other organisations* to authenticate to remote networks using their home organisation's accounts and credentials. Federating RADIUS servers like this *requires trust to be established between the RADIUS servers* as part of a federation. RADIUS servers can be federated in a single organization as well if there are multiple RADIUS domains.