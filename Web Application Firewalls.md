---
aliases:
  - WAFs
---
WAFs function similarly to network firewalls, but they work at the Application layer. A WAF sits in front of a web server, as shown below, and receives all network traffic headed to that server. 

It then *scrutinises the input headed to the application*, performing input validation before passing the input to the web server. This prevents malicious traffic from ever reaching the web server and acts as an important component of a layered defence against web application vulnerabilities.

![[Web Application Firewall.png]]