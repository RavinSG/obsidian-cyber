---
aliases:
  - LDAP
---
The figure below shows an example of an LDAP directory hierarchy for `Inc.com`, where there are two *organisational units* (OUs): security and human resources. Each of those units includes a number of entries labeled with a *common name* (CN). 

![[Screenshot 2024-04-17 at 11.24.51 AM.png]]

In addition to the structure shown in the diagram, each entry would have additional information not  shown in this simplified diagram, including a distinguished name, an email address, phone numbers, office location, and other details.

Since *directories* contain significant cant amounts of organisational data and may be used to *support a range of services, including directory-based authentication*, they must be well protected. The same set of needs often means that directory servers need to be publicly exposed to provide services to systems or business partners
who need to access the directory information. In those cases, additional security, tighter access controls, or even an entirely separate public directory service may be needed.