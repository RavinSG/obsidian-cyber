---
aliases:
  - SSO
---


## Google Cert

Single sign-on (**SSO**) is a technology that combines several different logins into one. More companies are turning to SSO as a solution to their authentication needs for three reasons:

- *SSO improves the user experience* by eliminating the number of usernames and passwords people have to remember.
- *Companies can lower costs* by streamlining how they manage connected services.
- *SSO improves overall security* by reducing the number of access points attackers can target.

This technology became available in the mid-1990s as a way to **combat password fatigue**, which refers to people’s tendency to reuse passwords across services. Remembering many different passwords can be a challenge, but using the same password repeatedly is a major security risk. SSO solves this dilemma by shifting the burden of authentication away from the user.

### How SSO works

SSO works by automating how trust is established between a user and a service provider. Rather than placing the responsibility on an employee or customer, SSO solutions *use trusted third-parties* to prove that a user is who they claim to be. This is done through the **exchange of encrypted access tokens** between the identity provider and the service provider.

Similar to other kinds of digital information, these access tokens are exchanged using specific protocols. SSO implementations commonly rely on two different authentication protocols: **LDAP** and **SAML**. LDAP, which stands for *[[Lightweight Directory Access Protocol]]*, is mostly used to transmit information on-premises; SAML, which stands for *[[Security Assertion Markup Language]]*, is mostly used to transmit information off-premises, like in the cloud.

![[SSO.png]]

### Limitations of SSO

Usernames and passwords alone are not always the most secure way of protecting sensitive information. SSO provides useful benefits, but there’s still the risk associated with using **one form of authentication**. For example, a lost or stolen password could expose information across multiple services. This is solved by using [[Multi-factor Authentication]]