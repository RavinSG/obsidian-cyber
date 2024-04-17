
Kerberos us a protocol for authentication service requests between trusted hosts across an untrusted network like the internet. It uses authentication to shield its authentication traffic.

Kerberos users are composed of three main elements: the **primary**, which is typically the username; the **instance**, which helps to differentiate similar primaries; and **realms**, which consist of groups of users. Realms are typically separated by trust boundaries and have distinct Kerberos key distribution centres (KDCs).

The figure below demonstrates a basic Kerberos authentication flow.

![[Kerberos.png]]

1. When a client wants to use Kerberos to access a service, the client requests an authentication ticket, or ticket-granting ticket (TGT).
   
2. An authentication server checks the client's credentials and responds with the TGT, which is encrypted using the ticket-granting service's (TGS) secret key.
   
3. When the client wants to use a service, the client sends the TGT to the TGS (which is usually also the KDC) and includes the name of the resource it wants to use.
   
4. The TGS sends back a valid session key for the service, and the client presents the key to the service to access it.