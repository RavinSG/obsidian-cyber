---
aliases:
  - PAM
---
PAM is the management of the privileges a system's access role has. It also encompasses enforcing security policies such as password management, auditing policies and reducing the attack surface a system faces.

PAM tools can be used to *handle the administrative and privileged accounts*. PAM tools focus on ensuring that the concept of least privilege is maintained by helping administrators specify only the minimum set of privileges needed for a role or task. 

PAM tools often provide more detailed, granular controls; **increased audit capabilities**; and additional **visibility and reporting** on the state of privileged accounts.

There are three specific features of PAM tools we should know about:

- *Just-in-time* (JIT) *permissions* are permissions that are granted and revoked only when needed. This is intended to prevent users from having ongoing access when they don't need that access on an ongoing basis. Users will typically use a console to *check out* permissions, which are then removed when the task is completed or a set time period expires, which helps to **prevent privilege creep**.
  
- *Password vaulting* is commonly used as part of PAM environments to allow users to access privileged accounts without needing to know a password. Much like JIT, this allows privileged credentials to be checked out as needed. Password vaults are also commonly used to ensure that passwords are available for emergencies and outages.
  
- *Ephemeral accounts* are temporary accounts with limited lifespan. They may be used for guests or for specific purposed in an organisation when a user needs access but should not have an account on an ongoing basis.
