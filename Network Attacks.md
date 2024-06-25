
### On-Path Attacks

An on-path (**man-in-the-middle** (MitM)) attack occurs when an attacker causes traffic that should be sent to its intended recipient to be relayed through a system or device the attacker controls. Once the attacker has traffic flowing through that system, *they can eavesdrop or even alter the communications* as they wish.

![[Man in the Middle.png]]

An on-path attack can be used to conduct SSL stripping, an attack that in modern implementations removes TLS encryption to read the contents of traffic that is intended to be sent to a trusted endpoint. A typical SSL stripping attack occurs in three phases:

1) A user sends an HTTP request for a web page.
2) The server responds with a redirect to the HTTPS version of the page.
3) The user sends an HTTPS request for the page they were redirected to, and the website loads.

A SSL stripping attack uses an on-path attack when the HTTP request occurs, *redirecting the rest of the communications through a system that an attacker controls*, allowing the communication to be read or possibly modified. Although SSL stripping attacks can be conducted on any network, one of the most common implementations is through an **open wireless network**, where the attacker can control the wireless infrastructure and thus modify traffic that passes through their access point and network connection.

>[!fail] Stopping SSL Stripping and HTTPS On-Path Attacks
>
>Protecting against SSL stripping attacks can be done in a number of ways, including configuring systems to expect certificates for sites to be issued by a known certificate authority and thus preventing certificates for alternate sites or self-signed certificates from working. Redirects to secure websites are also a popular target for attackers since unencrypted requests for the HTTP version of a site could be redirected to a site of the attacker's choosing to allow for an on-path attack. The HTTP Strict Transport Security (HSTS) security policy mechanism is intended to prevent attacks like these that rely on protocol downgrades and cookie jacking by forcing browsers to connect only via HTTPS using TLS. Unfortunately, HSTS only works after a user has visited the site at least once, allowing attackers to continue to leverage on-path attacks.


A final on-path attack variant is the browser-based on-path attack (man-in-the-browser (**MitB** or **MiB**)). This attack relies on a *Trojan that is inserted into a user's browser*. The Trojan is then able to access and modify information sent and received by the browser. Since the browser receives and decrypts information, a browser-based on-path attack **can successfully bypass TLS encryption** and other browser security features, and it can also access sites with open sessions or that the browser is authenticated to, allowing a browser-based on-path attack to be a very powerful option for an attacker. Since browser-based on-path attacks *require a Trojan to be installed*, either as a browser plug-in or a proxy, system-level security defences like antimalware tools and system configuration management and monitoring capabilities are best suited to preventing them.

On-path attack indicators are typically changed network gateways or routes, although sophisticated attackers might also compromise network switches or routers to gain access to and redirect traffic.

### Domain Name System Attacks
