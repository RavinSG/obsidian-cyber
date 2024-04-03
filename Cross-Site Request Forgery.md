---
aliases:
  - CSRF
  - XSRF
---
[[Cross-site Scripting|XSS]] attacks exploit the trust that a user has in a website to *execute code on the user’s computer*. **XSRF** attacks exploit the trust that remote sites have in a user’s system to *execute commands on the user’s behalf*. 

XSRF attacks work by making the reasonable assumption that users are often logged into many different websites at the same time. Attackers then embed code in one website that sends a command to a second website. When the user clicks the link on the first site, they are unknowingly sending a command to the second site. If the user happens to be logged into that second site, the command may succeed. 