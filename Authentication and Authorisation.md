
When a subject wants to claim an [[Identity]], they need to prove that the identity is theirs. That means they **need to authenticate**.

**Authorisation** *verifies what you have access to*. When combined, authentication and authorisation first verify who you are, and then allow you to access resources, systems, or other objects based on what you are authorised to use.

### Authentication and Authorisation Technologies

- [[Extensible Authentication Protocol]]
- [[Challenge Handshake Authentication Protocol]]
- [[802.1X]]
- [[Terminal Access Controller Access Control System Plus]]
- [[Kerberos]]

### Single Sign-on

Single sign-on ([[Single Sign-on|SSO]]) systems allow a user to log in with a *single identity* and then *use multiple systems* or services *without re-authenticating*. SSO systems provide significant advantages because they simplify user interactions with authentication and authorisation systems, but they require a **trade-off in the number of identity-based security boundaries** that are in place.

This means that many organisations end up implementing single sign-on for many systems but may require *additional authentication* steps or use of an *additional privileged account* for high-security environments.

Directory services like the *Lightweight Directory Access Protocol* ([[Lightweight Directory Access Protocol|LDAP]]) are commonly deployed as part of an identity management infrastructure and offer hierarchically organised information about the organization. They are frequently used to make available an organisational directory for email and other contact information.

Internet-based systems and architectures often rely on a number of core technologies to accomplish authentication and authorisation that can also be used for single sign-on. These include the following:

- Security Assertion Markup Language ([[Security Assertion Markup Language|SAML]]) is an XML-based open standard for **exchanging authentication and authorisation information**. SAML is often used. *between identity providers and service providers* for web-based applications. Using SAML means that service provides can accept SAML assertions from a range of identity providers, making is a common solution for federated environments.
  
- [[OpenID]] is a open standard for decentralised authentication. OpenID identity providers can be leveraged for third-party sites using established identities. A common example of this is the "Log in with Google" functionality that many websites provide. Microsoft, Amazon, and many other organisations are OpenID identity providers (IdPs). Relying parties (RPs) redirect authentication requests to the IdP and then receive a response with an assertion that the user is who they claim to be due to successful authentication, and the user is logged in using the OpenID for that user.
  
- [[OAuth]] is an open standard for authorisation used by any websites. OAuth provides a method for users to determine what information to provide to third part applications and sites without sharing credentials. This can be experienced with tools like Google Drive plug-ins that request access to your files and folders, or when you use a web conferencing tool that requests access to a Google calendar with a list of permissions it need or wants.