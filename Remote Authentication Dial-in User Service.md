---
aliases:
  - RADIUS
---
RADIUS (Remote Authentication Dial-in User Service) is one of the *most common* authentication, authorisation, and accounting (AAA) systems *for network devices*, wireless networks, and other services.

RADIUS can operate via TCP or UDP and operates in a client-server model. RADIUS sends passwords that are *obfuscated by a shared secret and MD5 hash*, meaning that its password security is not very strong. RADIUS traffic between the RADIUS network access server and the RADIUS server is typically encrypted using IPSec tunnels or other protections to protect the traffic.